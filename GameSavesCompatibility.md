## Game saves compatibility

AGS games have a built-in save system that is very convenient most of the time as it automatically writes down states of the game and all objects in it, allowing to restore saved game to exactly same condition it was saved in.

While seems convenient and suitable in a common case, these saves are essentially "save states", meaning that they contain a "dump" of all the game objects, regardless of whether they are related to the game progress or not.

And then, it has one serious technical flaw. When game data is written to a save file the game objects and variables are written as lists, where separate items are not identified in any way rather than an order in that list. (This is also related to how objects are refering to each other in game internally - by using numeric IDs.) Because of that, **adding, removing or changing an order of objects in game** may make older saves incompatible, or completely unusable and impossible to load.
Simply changing the IDs of game objects in the project tree or changing the order of the variables in script will cause previous saves to glitch, as old data will be restored into wrong objects or variables.

This may not be the biggest issue while the game is in development, as you have multiple ways to make the game "teleport" the player character to the desired scene instead of using a previously made save. But it becomes a problem after you released your game to the public, as any update or patch that involves adding new objects or variables will render the player's saves unstable or unusable.

Starting with **AGS 3.6.2** engine now has a minimal support for restoring saves made in previous versions of the game, with a different amount of data. This feature is restricted in a way that it only can work properly if you have *added new objects to the end of their lists* (or new script variables after existing ones in each current script module). Furthermore, it does not load the old saves with mismatching data automatically, but requires you to add [a special function](ValidateRestoredSave) into game script, that is going to "validate" saves and fixup data if necessary. For more information on this particular feature please see a respective article: [Handling incompatible saves](HandlingIncompatibleSaves).

Following article explores which changes to your game are safe and which are not in terms of keeping your game compatible with older saves, and which known methods exist to work around this problem.

### Changes in game that MAY break saves

Changing the number (adding or removing), as well as reording of almost all game objects will affect compatibility of previously made saves. Following types of objects contribute to this:
* Audio Types,
* Characters,
* Dialogs (but not options in existing Dialog, for technical reasons),
* Global Variables,
* GUIs and controls on existing GUIs,
* Inventory items,
* Mouse cursor types,
* Views, loops and frames in them (because frames may be changed in script and their properties are added to the save state),
* Script modules (when changing the number of them by adding or removing scripts)

In addition to that, changing the *total size* of variables declared in the global scope of each script (*NOT* local function variables) will break older save states.<br>
To elaborate on what "total size" is, imagine you have this declared in a script:

```ags
int a;
int b;
int c;
```

This adds up to make the total size of script variables 3 integers (or 3 * 4 bytes = 12 bytes). Now, if you change this to

```ags
int vars[3];
```

even though there's now only one variable, this also gives a total size of 3 integers, and won't break save states.
(Here we omit the question whether it will still make sense to restore older save with such a change in the script.)

What is the effect of different changes?

1. Adding a game object to the end of the list - may be handled with the use of ["validate_restored_save"](ValidateRestoredSave) script function. If one is not written in your script, then your game will refuse to load saves with *less* objects.
2. Inserting a game object in the middle of the list - will cause save data to load into wrong objects. This may in theory be also handled by "validate_restored_save" but is going to be more difficult in case of some object types, and impossible in case of others.
3. Changing an order of game objects in their list (e.g. by changing ID, or changing order of variables in script): will cause save data to load into wrong objects, and if it's the only change then the engine won't be able to detect a mismatch, and won't run "validate_restored_save" in script.
4. Removing a game object - will invalidate old saves: your game will refuse to load saves with *more* objects compared to current game.
5. Replacing one game object with another - if you keep the object ID (order), then the saves might load, but this new object will get data from the old object that it replaced. If it's the only change then the engine won't be able to detect a mismatch, and won't run "validate_restored_save" in script.

To summarize: if you want to keep old saves compatible:
1. DON'T remove existing objects or variables. If you don't use them - hide or disable them, but keep them in game.
2. DON'T change the order of existing objects in their lists, or global variables in script. 
3. If you need more objects then add them to the end of the list. If you need more variables in script then add them after existing ones.
4. If you have added more objects or variables, then you *must* write ["validate_restored_save"](ValidateRestoredSave) function in your script, or the game will refuse to load older saves. Please read its article for explanation on how to implement one.

### Changes in game that DON'T break saves

Parts of the game which may be safely added or removed:
* Rooms, adding new ones. Removing a room is *not* safe, as loading a state saved in a no longer existing room will crash the game.
* Adding room objects. Removing a room object is not entirely safe, as although these will not be present when restoring an older save, but if there are any references to them e.g. stored in variables, these will become invalid and can cause crashes.
* Adding more background frames to existing rooms, but probably not removing them (*needs to be checked*).
* Editing room backgrounds or area masks is fully safe; even if you draw areas of different ID (color).
* Adding or removing dialog options in existing Dialogs. Please note that replacing one option with another might still lead to glitches, as the old option's state (on/off and visited) might get applied over a new one.
* Custom properties. Adding is safe. If you remove existing ones, their values may still load from the older save but will not be accessible.
* Adding or removing plugins. Some plugins write their data to saves, but if they are missing in a new version of a game, then their data will be simply skipped.

Adding or removing any kind of plain resources, such as
* Sprites
* Fonts,
* Audio clips,
* Voice-over clips,
* Video files,
* Translations

**IMPORTANT:** Removing sprites is only safe if you fix all objects that could have them assigned upon restoring a save. The same goes for clips assigned to View Frames, and fonts used on GUIs.

In scripts:
* User types (structs) may be added; but if you change the size of a regular struct while having variables of that type in your script - that would also change the size of these variables, and may break saves. Managed structs *may* be changed in size without breaking a save, but this requires a special approach ([see below](GameSavesCompatibility#an-issue-of-dynamic-objects)).
* Macros,
* Functions and attributes, and generally - function code itself,
* For the same reasons - changing existing dialog scripts,
* Local variables (inside functions) may be added and removed freely because they are not saved, because AGS does not allow the game to save while it is inside a script function

### An issue of dynamic objects

The pointers which are global variables are part of script's total variable size, so adding or removing them in script will affect old saves compatibility as usual.
The *managed objects themselves* are not stored in the script's variable memory, but in their own memory pool. They are also completely written to the save file, and loading this save will restore the original managed object with its original size. Therefore, changing their sizes in a new version of the game doesn't break previous saves on itself.
However, there's a number of potential problems with that and these have to be resolved if you want to maintain save compatibility.

Consider a simple dynamic array:

```ags
int dyn_arr[];

void game_start() {
    dyn_arr = new int[100];
}
```

Let's assume you had this in game version 1, made a save, then increased the dynamic array's size in script to 200:

```ags
dyn_arr = new int[200];
```

What will happen if you now compile game version 2 and then restore the old save? The game will restore the dynamic array with the previous size of 100. This means that if your new script will now try to access elements in the array beyond 100 (thinking that this array has 200 elements now), that will result in an "index out of range" error.
Unfortunately at the time of writing this AGS manual page, you can't access the length of a dynamic arrays directly in script. But you can store their length somewhere else, for example, in a variable:

```ags
int dyn_arr[];
int arr_size;

void game_start() {
    dyn_arr = new int[100];
    arr_size = 100;
}
```

There are other ways of fixing this, for example you could store the array length in its first element. You just will have to remember it's there when working with the array; but that's a different topic.
In any case, having the array length stored, if you ever change that array's size and restore an older save, that length variable will also be restored and will tell you the correct size of the array.
If you still need the array to be exactly 200 elements in size in the new version of the game you may resize it after restoring a save. This is explained further in the ["Solutions" section](GameSavesCompatibility#solution-7-extending-dynamic-arrays-and-managed-structs).

Less likely, but if you instead reduce the array's size, then the array restored from the older save will be bigger in size than necessary, but that's much less of a problem and may be ignored.

This is what happens with changes in dynamic arrays, but what about *changes in custom managed structs*? Assume in game version 1 you have:

```ags
managed struct MyStruct {
    int a;
    int b;
};

MyStruct* var;

void game_start() {
    var = new MyStruct;
}
```

Then in game version 2 you decided to add another variable:

```ags
managed struct MyStruct {
    int a;
    int b;
    int c;
};
```

If you load an older save from version 1 while running version 2, created objects of this type will load but will be one variable less in size. Trying to use the additional variable in script will result in an error. This is similar to the array case.
The solution here is similar to the array solution: upon restoring the older save recreate all managed objects (they will be of the correct size), copy valid content from restored objects into them, and reassign pointers to these recreated objects. Again, this is explained more in the ["Solutions" section](GameSavesCompatibility#solution-7-extending-dynamic-arrays-and-managed-structs).

And again, if you remove a variable instead:

```ags
managed struct MyStruct {
    int a;
};
```

in this case the older save will be restored, and the old variants of MyStruct will also be loaded. They will contain all the removed variables, but you no longer will be able to access them in script because they are no longer declared so the script is not aware of their existence.

Finally, there's another potential problem. Let's look at this variant:

```ags
managed struct MyStruct {
    int a;
    // int b;
    int c;
};
```

The `b` variable was removed, so variable `c` now follows `a`. If you load an older save however, the old MyStruct objects contain variable `b`, and its value will be assigned to `c` instead of `b`, as it took its place in the struct.

For that reason, if save compatibility is essential, it is recommended to only *extend* managed types and not cut out existing data.

---

### Solution 0: Code your own save system

Yes, this may sound like a crazy suggestion (that's why it's under number 0), but it's a real possibility. And this may be a much more suitable solution for the game genres other than "adventure" (arcades, strategy games and so forth). This is a whole separate topic though, so we won't go into much detail here. But if you want to experiment here are some points to get you started:

* AGS supports writing and reading custom files. See [File functions](File) for reference.
* Consider using simpler save states, more like checkpoints. If you can live without restoring literally everything to the smallest bit, maybe you can only save the most important game variables, items that the player possesses, the state of the accessible puzzles.
* Learn to describe the game state using just a few variables and restore the game and rooms from these. For example, if your variable says that "puzzle A is solved", you may know that Room Objects A and B are invisible, item C is in the player's inventory, and non-player character D moved to room 2. This approach allows you to rebuild the game state in script just from a few variables the game reads from a custom file. But of course you have to plan this ahead well.

### Solution 1: Handle incompatible saves in script

This option exists since **AGS 3.6.2**. It will manage to handle cases when the new version of the game has more objects of any kind added to the end of the list, and involves writing a script function that will test incompatible saves, decide whether game is still allowed to load them, and fixes game data if necessary.

Because there's more to this, this solution is explained in a separate article: [Handling incompatible saves](HandlingIncompatibleSaves).

### Solution 2: Reduce types of data stored in saves

Starting with **AGS 3.6.2** engine allows to restrict certain types of data and objects from being written in saves (and read from them). Only some types of data can be restricted this way, and the restriction is for the whole type, not individual objects.

While this has multiple purposes, one of the uses is to reduce types of data that affect save compatibility. For example, if you remove Views or GUIs from saves, then changing them in game will not affect older saves at all.

This restriction can be done using [`SetGameOption`](Globalfunctions_General#setgameoption) command with `OPT_SAVECOMPONENTSIGNORE` option id. The list of data that you can skip is defined by the [`SaveComponentSelection`](StandardEnums#savecomponentselection) enumeration in script. This enum includes following values, that may be combined together:

```ags
    eSaveCmp_Audio,
    eSaveCmp_Dialogs,
    eSaveCmp_GUI,
    eSaveCmp_Cursors,
    eSaveCmp_Views,
    eSaveCmp_DynamicSprites,
    eSaveCmp_Plugins
```

An example of use:

```ags
function game_start()
{
    SetGameOption(OPT_SAVECOMPONENTSIGNORE, eSaveCmp_Audio + eSaveCmp_GUI + eSaveCmp_DynamicSprites);
}
```

Above will *exclude* audio playback state, GUI state, and Dynamic Sprites from the saves, and don't read them back from a save even if it happens to contain them.

### Solution 3: Reusing game objects

If you need to urgently patch your released game but realize you are going to break previous saves by doing that, you may try reusing existing game objects.

Characters may switch Views and play different roles in other rooms. Unused Room Objects are perhaps more rare, but their Graphic or View may be switched too and can act as something else. Characters may be used as room elements too, except they cannot be simply assigned a sprite, but require a View.
GUIs may be reconfigured on the fly, if you have enough suitable controls on them.
View Frames may be assigned different sprites, even [Dynamic Sprites](DynamicSprite), which you can paint upon with script functions to display something completely different.

Global variables may be reused for other purposes if you find a way to indicate what meaning they have at the moment and how they should be used in your script in various circumstances.

### Solution 4: Dummy object reserve

If you are planning changes after your game's release, there's one very straightforward yet ugly solution: create a number of extra objects of every type (Characters, GUIs, and so on) that you don't use right now but which could be used in case of emergency for patching the game.

In your script, you can allocate big global arrays of ints and other types as a reserve for future fixes and updates, then use elements of those arrays whenever you need an extra variable.

### Solution 5: New rooms

If you must change the content of a room but do not want to break saves at all costs you may create a duplicate room with a new number and updated contents, then script changing to this new room if the player restores a save made in the old room.

This is done like this, for example:

```ags
void on_event(EventType evt, int data) {
    if (evt == eEventRestoreGame) {
        if (player.Room == OLD_ROOM_NUMBER) {
            player.ChangeRoom(NEW_ROOM_NUMBER);
        }
    }
}
```

### Solution 6: String, Dictionary and Set

You may use String variables (or even one String) to store almost any amount of additional data without adding new variables. Strings may be [formatted](StringFormats) to include numbers too. You may create for example a comma-separated list of values, then parse it back by iterating over characters, cutting it into substrings and converting it back to the wanted types. That will involve some advanced scripting but can be used as a last resort.

Since AGS 3.5.0 there's also a [`Dictionary`](Dictionary) type and a [`Set`](Set) type available. Those types may serve as an easier alternative in this solution. It's easy to check which variables (keys) they contain. You can even store the "game version" inside them as one of the elements and check for that value after restoring a save to know which version of your game saved it. They may be used as a universal global storage, for example, for story variables, expanding them between game updates.

### Solution 7: Extending dynamic arrays and managed structs

As mentioned earlier in this article, any managed object is not restricted to change because its full content is read from the save. This allows you to use managed structs and dynamic arrays as infinite reserve for variables.
Upon loading an old save you would need to test the length or another kind of "version" of that array and resize it: create a new one, copy the old restored contents, fill up the rest with default values, replace the pointer variable.

Consider following example:

```ags
#define GAME_VER_001_LENGTH 10
#define GAME_VER_002_LENGTH 20

int GameVersion;
int MyVariables[];

void game_start() {
    GameVersion = 2;
    MyVariables = new int[GAME_VER_002_LENGTH];
}

void on_event(EventType evt, int data) {
    if (evt == eEventRestoreGame) {
        // detect old save
        if (GameVersion == 1) {
            // allocate bigger array suited for latest version of the game
            int new_vars[] = new int[GAME_VER_002_LENGTH];
            // copy restored array with old data into our new array
            for (int i = 0; i < GAME_VER_001_LENGTH; i++) {
                new_vars[i] = MyVariables[i];
            }
            // set default values for the rest (replace with your code as appropriate)
            for (int i = GAME_VER_001_LENGTH; i < GAME_VER_002_LENGTH; i++) {
                new_vars[i] = 0;
            }
            // finally replace pointer and version number
            MyVariables = new_vars;
            GameVersion = 2;
        }
    }
}
```

A similar solution may be used for managed structs, although it may be bit more complicated to script but essentially it is the same thing.

```ags
managed struct MyStruct {
    // variables from version 1
    int a;
    int b;
    // variables from version 2
    int c;
    int d;
}

int GameVersion;
MyStruct MyObj;

void game_start() {
    GameVersion = 2;
    MyObj = new MyStruct;
}

void on_event(EventType evt, int data) {
    if (evt == eEventRestoreGame) {
        // detect old save
        if (GameVersion == 1) {
            // allocate new managed object suited for latest version of the game
            MyStruct new_obj = new MyStruct;
            // copy restored object with old data into our new object
            new_obj.a = MyObj.a;
            new_obj.b = MyObj.b;
            // set default values for the rest (replace with your code as appropriate)
            new_obj.c = 0;
            new_obj.d = 0;
            // finally replace pointer and version number
            MyObj = new_obj;
            GameVersion = 2;
        }
    }
}
```
