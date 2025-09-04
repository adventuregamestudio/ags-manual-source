## What's new in AGS 3.6.2

3.6.2 is a second update for the 3.6 version, this time it focuses on convenience of user interface and filling certain gaps in the existing scripting API. There are no breaking changes to the game settings or script.

One major addition that should be noted in particular is the minimal support for loading game saves made in older versions of your game (seek for it below in the corresponding section).

### Event handler functions can be in any script module

Previously in AGS when you tied an object or room event to a function in script, such function could only be located in GlobalScript.
With 3.6.2 you can now select which script module to use. This selection is made on Events tab of a Property Grid, where you will see a "Script Module" dropdown list.
This allows you to neatly organize event functions in your project scripts, and not clutter GlobalScript with all kinds of things.

Note that GUI Controls inherit script module of their parent GUI, and everything in room (room objects, hotspots, etc.) will always use the same room script.

Please be aware that changing this selection does not automatically move any existing functions from one module to another: you will have to move them by hand. But any new functions generated for events by clicking "..." button will end up in the selected script module.

### Expanded voice clip names

Previously the voice clips had to be named with only first 4 letters from a Character's script name, e.g. "ROGE1.ogg" in case character's name is "cRoger". This could have become a problem if you have several characters which begin with the same 4 letters.

Starting with 3.6.2 the voice clips are to be named using full script name (except the preceding 'c' symbol) and an extra dot between name and a number. This dot is added to be able to distinguish clip numbers from any numbers found in the character name itself.

For example:
* If Character's script name is "cRoger" then the voice clips have to be called like "Roger.1.ogg";
* If a script name is "cVeryLongCharacterName" then they have to be called like "VeryLongCharacterName.1.ogg";
* If a script name is "cRandomGuy5" then they have to be called like "RandomGuy5.1.ogg" (notice that extra dot makes the clip number stand out).

The classic behavior may be re-enabled by a switch in "Backwards Compatibility" section of "General Settings". This is useful in case you are upgrading an older project and do not want to rename all the voice clips.

### Dynamic array's Length attribute

Dynamic arrays do now have a Length pseudo-attribute which allows to get their length without storing it in another variable.

For a simple example:
```ags
int[] CreateSomeArray(int how_long)
{
    return new int[how_long];
}

function game_start()
{
    int array[] = CreateSomeArray(100);
    Display("My array has a length of %d", array.Length);
}
```

In addition, you are now allowed to create empty dynamic arrays (of 0 length). This may be useful if you must return a dynamic array that has no elements but do not want to return "null" and bother with null pointer checks.

### Restricting the data read or written in a save

Engine now allows you to specify which sort of data you do not want to put into saves, or load from them (regardless of whether they have it or not).

This is not directly related to restoring old saves, but may as well be used to workaround compatibility issues when doing so. It has other uses as well.

Restricting of data is done using `SetGameOption()` command with `OPT_SAVECOMPONENTSIGNORE` option id. The list of data that you can skip is defined by the `SaveComponentSelection` enumeration in script. This enum includes following values, that may be combined together:

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

There are 3 general uses for this feature:
1. Reducing save file size. This is the simplest case. If your game has an excess use of DynamicSprites, that are created at runtime, you may wish to not write them into the save file to reduce the usage of disk space (in case player makes a lot of saves). Instead, these sprites can be recreated after a game is restored, or whenever they are required.
2. When you design your game in such way that certain things should not be reset when restoring a save. For instance, if you exclude Audio from saves, then anything currently playing will continue to play even after loading a saved game.
3. Reduce number of things that may make saves incompatible. If something is not a part of game save, then changing that cannot make older saves invalid. There are things in game that may be reinitialized or recreated in script rather than loading their states from a save. Good example are GUIs, Views and DynamicSprites.

### Loading old saves feature

Since the early days of AGS there have been this problem that if you change something in your game, there's a high chance that these changes will make previously made saves incompatible. This is not a big issue while you're developing the game, but may become one after you release it, since often you may have to patch or update your game. The problem is thoroughly discussed in the dedicated article: [Game saves compatibility](GameSavesCompatibility).

The new 3.6.2 engine introduces a number of options that let solve, or at least partially compensate this problem.

### Support for loading saves with *less* content for any given type of data

This means that the engine can load saves if they have equal or less count of Characters, Dialogs, GUI, controls on each given GUI, variables in script, and so forth, compared to the current game. Please be aware that *only quantity* of data is compared. Engine still cannot distinguish the order in which the items were saved. This means that if you change their order (such as order of IDs of items, or order of variables in a script), then engine won't be able to detect that, and will apply loaded items from the save into wrong positions.

There are two exceptions to this rule at the moment: script modules and plugins. Script modules and plugins are identified by their names, and so long as the name matches the script data will be correctly loaded from save. Engine also skips the data for module or plugin if one is not present in the game anymore.

The loading of "incompatible" saves is not done automatically though, it has to be triggered by adding a certain function to your scripts. This function is called ["validate_restored_save"](ValidateRestoredSave) (please read further below).

An extra note should be made for those of the recently added objects that are *not* found in a save. All of these are going to be *reset to their initial states*, the ones they have when player launches your game.

Please remember that the engine can do only that much: load what is available from the save. It's up to you to fixup anything after save is restored, such as correcting restored objects or initializing objects that were not restored. The good place to do this is, for example, an ["on_event" function](Globalfunctions_Event#on_event) which receives eEventRestoreGame event.

### Validating restored saves

In order to "validate" an "incompatible" save either when restoring one or when prescanning one (see explanation of prescanning below) you must add a function called "validate_restored_save" into one of your game scripts. The function is defined as:

```ags
function validate_restored_save(RestoredSaveInfo* saveInfo)
{
}
```

Whenever engine meets a save that is not entirely compatible with the current game, it will check if this function is present, and then run it. The function receives a RestoredSaveInfo object as a parameter, and this struct contains brief summary of the restored game, such as its description text, the version of the engine it was saved in, and numbers of global game objects (Characters, Inventory items, and so on) found inside.

RestoredSaveInfo has a boolean property named "Cancel", which must be set in this function to tell whether you confirm or cancel the use of this save. By default, Cancel begins set to "true", meaning that if you don't do anything then the engine will cancel restoring this save.

The purpose of this function is to let you decide whether save may still be allowed to load or not.

For more detailed explanation, please see ["validate_restored_save"](ValidateRestoredSave).

### Prescanning save slots

If engine is told to load an incompatible save and fails it then will usually just stop the game. This is quite frustrating for the players.
In version 3.6.2 there's a new script command called [`Game.ScanSaveSlots()`](Game#gamescansaveslots). This function lets you *test* a range of saves and return only validated ones. When checking the saves it will also try calling "validate_restored_save" for saves with fewer data in them.
Scanning saves allows you to take precaution and avoid displaying invalid saves in game menus, or reject loading a save before it crashes the game.
