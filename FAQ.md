## Frequently Asked Questions

This section of the manual is very rarely updated. Please visit the [AGS Forums](https://www.adventuregamestudio.co.uk/forums/) or [AGS Discord server](https://discord.gg/BSvN5ZF) if you need more answers on beginner questions.

---

**Q. Can I make commercial games with AGS?**

A. Yes, absolutely. AGS is completely free to use for creating any kind of games, whether commercial or not.

---

**Q. Can I make non-adventure games with AGS (platformer, strategy, card game, roguelike, ...)?**

A. Yes, and such games were made with AGS in the past. But you should know that AGS was created as a point'n'click adventure Engine and Editor, and the way it works prioritizes this genre. In order to make something different you will have to apply considerably bigger effort.

---

**Q. What resolution should I make my game in?**

A. AGS games are not different from any other games. Think on what resolution would match your chosen game design and graphic style. Experiment, make few test games to find out what's best fit.<br>
It's important to know the difference between "native resolution" and "window resolution". For example, you may make a game with a resolution of 320 x 200, but AGS allows players to stretch the game window to any size, scaling your game up (or down, although that usually is not a good thing), without any extra effort from you. So if you want to use "old-school" low-res graphics you don't have to worry about how to display your game on larger monitors.

---

**Q. I just started making a new game, but Editor throws an error at me: "The game is set to start in room -1 which does not exist". What does that mean?**

A. Each game has a main character, aka "player character", and starts in the room this character is assigned to. Open your main character for editing and make sure its "StartingRoom" property is valid (of course we assume that you've created at least one room already).

---

**Q. On my screen, I can't move the main character. Wherever I click to
move him, he just stands there.**

A. If the main character isn't on a walkable area, he will not be able
to move. Load the room in the editor, and check that the location where
the character starts is on a walkable area.

---

**Q. I have a function for a Room event or an object interaction, but commands inside do not work.**

A. In AGS the Room events and Object (Character, GUI, and so on) interactions must be "wired" to the script using the event grid. This is true for all events, so for example a 'room_Load' script function won't trigger if it's not wired on the room events.

Open the Editor, load the room or select the object, then under the object properties, in events, the field should be filled with the function to execute. If it's empty, clicking on the three dots button (...) the respective script will be open with the new function for you to fill the details. If it has a value, clicking on the same button will direct you to it on the respective script file. Optionally, you may type or copy the existing function's name into the field. But it's usually easier to click on the three dots button of the event you want to implement, which will generate the function in the script file for you.

---

**Q. I click on a GUI button, but nothing happens. It doesn't seem to register clicks!**

A. There are some different immediate suspects here.
- Is the button set to Run Script? Go to the GUI Editor, select the button, and make sure that in its properties window, "Click Action" is set to "Run Script".
- Is your script wired to the button's event? In the button properties, in events, the "OnClick" field should be filled with the function to execute.
- Is the GUI *and* the button clickable? Make sure that the "Clickable" property is set to "True" for both of them.
- Alternatively, if you are creating many GUIs, check if you have created a transparent, clickable GUI that is on top of the GUI you are clicking.

---

**Q. Mouse clicks do not seem to work in my game, no matter what I click on!**

A. Make sure that there's no big transparent GUI covering the game screen. In AGS transparent pixels on GUIs do not let clicks through, unless you set that GUI's Clickable property to "false". If that's the case, but that GUI has interactive controls on it (such as buttons), then either shrink GUI to only have enough size for those controls to fit, or make several smaller GUIs and organize controls on them.

---

**Q. When I enter a certain room, I just get a black screen.**

A. Make sure that you haven't used a blocking script command such as "Display", "Character.Say", "Character.Walk" or similar in the
"Enters room before fade-in" event for that room. Remember that this
event happens BEFORE the screen fades in.

To make sure, when you get the black screen, try pressing enter, or
clicking the left mouse button. If nothing happens then something more
serious may have happened. If this is the case, while running the game from the Editor, switch to the Editor and click on "Pause" button on toolbar: that will stop game execution and may allow you to trace which line of script it has stuck on.

---

**Q. How do I make a main menu for my game?**

A. There are various ways to do this, depending on how do you want your menu to work, but in general there's no dedicated "menu screen" feature in AGS, so you have to use any combination of common items. You could begin with creating a new room, assign a plain coloured background, and place objects to act like menu options. Or you could create a GUI with buttons. But you also may do a full scene with animating objects, characters and minigames, and force player to solve a puzzle in order to start a "real" game.

---

**Q. Can I hide player character in particular room (main menu, intro, credits screen)?**

A. Open your room in the Room Editor and set its "ShowPlayerCharacter" property to "False".<br>
If you want to hide player character only for some time, you may set that character's Transparency to "100" in script, or just move him/her out of the room borders, for example, by assigning x position to a large negative value.

---

**Q. The character isn't drawn behind my walk-behind areas!**

A. You need to define the baseline for these areas, baselines determine the order things (characters, objects and walk-behinds) are arranged in the room.

---

**Q. There's a bunch of identical script commands I need to execute on several events, is there a way to not copy/paste same 5 (20, 50, 100) lines everywhere I need them?**

A. Sure, create your own custom function in script, put these commands there, and call this functions each time you want these commands executed. Refer to [Scripting Tutorial](ScriptingTutorialPart2#your-own-functions) for more on this subject.

---

**Q. I created a script function, but I get "function is undefined" error trying to call it elsewhere.**

A. If you have a function written in script file `my_script_A.asc`, and you want to call it from a different script file `my_script_B.asc`, you need to correctly declare it as import on its header `my_script_A.ash`. Additionally, make sure the function is declared/defined above your call in script or placed in a higher script module, move function up if necessary.

---

**Q. Can I use script commands on my dialogues?**

A. Yes, you can. The dialog lines that start with at least one space character are interpreted as plain script, and the ones that don't are treated as special dialog commands. All special dialog commands are actually converted into real AGS script before compiling into your game.

---

**Q. How do I change object's "Name" or "Description" at runtime so that I could display a different one after a game event?**

A. At the time of writing this answer AGS does not let you change these values, as well as few other properties at runtime (this may be fixed in future versions). The common solution is to use "Custom Properties", which you define in the Editor and may modify in script using SetProperty and SetTextProperty functions for the respective object type.

---

**Q. How do I keep animations running while I am showing a message on screen using Display function?**

A. "Display" command pauses whole game, use an Overlay or GUI with a label instead. You may make them look similar to Display by defining background and border colours, or assigning a sprite. Then use "Wait" or "WaitMouseKey" command to halt the game while this Overlay or GUI is on screen.

---

**Q. How do I do an action when animation reaches certain frame?**

A. In repeatedly_execute function test whether an object is [`animating`](Object#objectanimating), and what it's current View, Loop and Frame properties (Characters and Buttons have similar values). Execute an action when these properties match your condition.

---

**Q. How do I put a variable on a GUI label?**

A. [`String.Format`](String#stringformat) script function allows to create a string with variable printed in it. You may then use that string anywhere, such as assign it to a label's Text property, for example:

    myLabel.Text = String.Format("Coins: %d / %d", current_coins, total_coins);

---

**Q. My game EXE file seems to have disappeared.**

A. For some reason AGS Editor deletes a game exe from Compiled/Windows folder each time when you make a test run (F5). To get this compiled exe back, simply build the game again by using the "Build EXE" command on the Build menu.

---

**Q. I change settings in Default Setup pane in the Editor, but in game nothing changes.**

A. Default Setup affects only default configuration. If you ran game's own setup program at least once and saved, there's already personal configuration written in your user documents, which overrides defaults. Also some of the settings are written by the game itself on exit. Change the setting by running setup program as well, or delete user config file to reset everything to defaults.

---

**Q: I loaded my old game in a newer version of AGS, and now I get errors about some AGS functions are "undefined".**

A: These functions were probably deprecated. Check the ["What's in new versions and upgrading"](WhatsInNewVersionsAndUpgrading) topic in the manual to learn about possible breaking changes and how can you adjust your script to the new standard.<br>
There's also a [big table of outdated functions and properties](ObsoleteScriptAPI) where you may find suggested replacements.<br>
But if you're in a real hurry, go to game's General Settings and set "Script compatibility level" to the version you were making game in previously (that usually works).

---

**Q. How should I name my views, objects, ...**

A. When you are looking into your objects in the Room Editor, they are obviously an object. When you are referencing them on script, you can get lost. It's a good and common practice when using AGS to precede the name of an element with a lower case letter of its type, make it easier to find them in the Editor with auto complete (ctrl+space) and the consistency will make easier to remember. So for a view, something like `vRogerWalking` is a good name, and the `v` at start tells you it's a view; A character with real name Joe, is a good idea to have the script name be cJoe, now the `c` reminds of character; a GUI for a lever puzzle could be called `gLeverPuzzle`; An object that is a flower as `oFlower`; A hotspot of the exit door as hExitDoor.

---

**Q. Can I create games that use a text parser, similar to the classic Sierra adventure games, such as King's Quest and Space Quest?**

A: Yes, AGS allows for the development of games that use text parser input. It is possible to create games that use an "always on" text parser (used in AGI games, such as Kings Quest I) as well as a "pop up" text parser (used in SCI games such as Space Quest 3). See more in the [Text Parser](TextParser) topic in the manual.
