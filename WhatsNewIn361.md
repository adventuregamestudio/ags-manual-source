## What's new in AGS 3.6.1

3.6.1 is a *relatively* minor update, focusing on performance and convenience of existing features. There are no breaking changes to the game settings or script, but there are few things that should be mentioned.

### Log Panel in the Editor

Editor now has a Log Panel, which may be shown by a command in "Window" menu. This panel is quite useful when testing your game, because it prints the engine logs, including the messages printed from script using [System.Log](System#systemlog) function. For more information on configuring this panel please refer to [the corresponding page](EditorLogPanel).

### New sprite compression option

Back in 3.6.0 we added LZW to sprite compression choice, because LZW was already been in use for room backgrounds. In 3.6.1 release there's a new option called "Deflate", which enables compression type of the same name. This compression is known to be used in such file formats as GZIP and PNG, and provides even more saved disk space than LZW.

### Expanded interaction callbacks

All the interaction event callbacks for Characters, Inventory Items and so on now have extended parameter list with a pointer to a interacted object and a cursor mode that triggered interaction, somewhat similar to GUI event callbacks. They now look like this:

    function Ego_Interact(Character *thisCharacter, CursorMode mode)
    function oObject1_Look(Object *thisObject, CursorMode mode)

and so forth. There are few exceptions that do not include CursorMode, as the events are run from different actions, such as Hotspot's "WalkOn" and "MouseOver" events, and also all of the Region's callbacks. These include only activated object's pointer.

The reason to have these is to let you use a single script function for multiple interaction modes and/or multiple objects, when necessary. In such case you can use the pointer to access activated object and "mode" parameter to know which mode was used to trigger the event. If the function is only used for a single object / interaction, then you may ignore these and keep using real object's script name as you always do.

Note that you do not have to add these parameters to existing functions after you loaded old game into the new Editor. Your game will run fine even if these parameters are not declared there.

### Disabled automatic SetRestartPoint

Previously AGS engine was saving a restart point (save slot 999) automatically right after a "game_start" script function call. Starting with 3.6.1 this will not be done. Instead, you should add "SetRestartPoint()" call into your game script yourself, if you need that.

This was done to let users have an option to NOT have this default save. For example, some games may have custom save files, or just don't use this save slot, so it's not good to force them have it too without any way to disable it.

When you load older projects into 3.6.1 editor, AGS will *insert* a "SetRestartPoint()" call at the end of your "game_start" function in GlobalScript, to keep this behavior. If you don't like it be there you may safely remove this line.

### Discontinued Source control integration

This feature theoretically allowed you to put your game files under source control, using either Microsoft's SourceSafe utility if one is installed on PC, or a custom AGS plugin written for this purpose. It is not known to us whether this functionality was ever known and used by any AGS users, but it was not properly maintained throughout the last decade, and hence is probably broken anyway. Today it's easier to let people use their own source control tool (like Git or Mercurial) without integration with AGS Editor. Therefore this feature is completely removed from the program now.
