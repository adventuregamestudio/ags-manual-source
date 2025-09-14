## Backing up your game

You will no doubt want to back up your game files, and should do so
regularly during your game development. But which files should you back
up?

When taking a backup, make sure you copy **ALL** the files listed below:

-   `Game.agf` - this is the main data file for your game and contains almost all the game settings. Without it, your game is lost.
-   `acsprset.spr` - this is your game's sprite file, containing all the sprites from the sprite manager.
    You can alternatively backup your sprites sources and restore from the editor _File_ -> _Restore all sprites from source_ menu.
-   `Rooms\` - this directory contains a directory for each room, with each containing a xml file for the room data, a `.asc` room script, and png files for masks and backgrounds.
-   `*.asc`, `*.ash` - these are your script files, and contain all of your scripting handywork.
-   `*.trs` - translation source files. They contain any translations that you've had done.
-   `Fonts\`- should contain any `*.wfn` or `*.ttf` font you have imported.
-   `Speech\` - this depends if your game has any speech audio files, if it does you will need to backup this directory.

Also remember to back up any **sound**, **music** and **video** files you are using.
