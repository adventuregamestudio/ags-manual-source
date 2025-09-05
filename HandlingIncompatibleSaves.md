## Handling incompatible saves

This article overviews a engine feature that is available since **AGS 3.6.2**, which allows to restore older saves which become incompatible after certain changes done to a game.

Please note that this solution may be only applied in a limited number of cases. The problem of saves compatibility itself is thoroughly discussed in the dedicated article: [Game saves compatibility](GameSavesCompatibility). Following is a summary of what you may do if you like to use this feature:

1. Add new game objects **at the end** of their lists. This includes adding more controls to existing GUIs, adding more options to Dialogs, and more loops and frames to existing Views.
2. Add new script variables **after** all the previously existing variables.
3. Add new script modules. Note that their *order* is not relevant, but their *names* are - they should not match any existing or previously existing scripts.
4. Add new rooms. Note that their numbers should not match any existing or previously existing ones.
5. Add new assets, such as sprites, audio clips, etc. These may be added freely as necessary, although you should remember that reusing sprite numbers may lead to glitches after restoring older saves, and might require additional fixing things in script, after restoring a save.
6. Add or remove plugins. Note that if you replace a plugin with another of same name (maybe another variant or version of the same plugin), and that plugin writes data to saves, then it is responsibility of plugin to correctly read that data back.

What you may NOT do:

1. Can't remove existing objects or variables. If you no longer need any, then hide or disable them, but do not remove nor change their order.
2. Can't change an order of existing objects or order of variables in script. The reason is that the old save will load data into the wrong objects. Strictly speaking, you may still try fixing up the objects in script after loading such older save, but this is not always reliable, and it's easier not to get into such trouble in the first place.
3. Can't insert new objects in the middle of their list, nor variables in between of existing variables. For the same reason as stated above.

### How it works

The engine can load saves if they have *equal or less* count of Characters, Dialogs, GUI, controls on each given GUI, variables in script, and so forth, compared to the current game. Please be aware that *only quantity* of data is compared. Engine still cannot distinguish *the order* in which the items were saved. This means that if you change their order (such as order of IDs of items, or order of variables in a script), then engine won't be able to detect that, and will apply loaded items from the save into wrong positions.

There are two exceptions to this rule at the moment: script modules and plugins. Script modules and plugins are identified by their names (at least in saves made by 3.6.2 version of the engine or later), and so long as the name matches the script then data will be correctly loaded from save. Engine also skips the data for a plugin if one is not present in the game anymore.

The loading of "incompatible" saves is not done automatically though, it has to be triggered by adding a certain function to your scripts. This function is called ["validate_restored_save"](ValidateRestoredSave) (please read further below).

When that old save is restored, the matching objects and variables have their states overwritten by the data from the save.

But what happens to the newer objects or variables, ones that are not found in a save? All of these are going to be *reset to their initial states*, the ones they have set in the compiled game. This is achieved by *reloading game data* prior to restoring a save. As a side effect this makes restoring such saves take more time than usual.

Please remember that the engine can do only that much: load what is available from the save. It's up to you to fixup anything after save is restored, such as correcting restored objects or initializing objects that were not restored. The good place to do this is, for example, an ["on_event" function](Globalfunctions_Event#on_event) which receives eEventRestoreGame event.

### Prescanning save slots

If engine is told to load an incompatible save and fails it then will usually just stop the game. This is quite frustrating for the players.
In version 3.6.2 there's a new script command called [`Game.ScanSaveSlots()`](Game#gamescansaveslots). This function lets you *test* a range of saves and return only validated ones. When checking the saves it will also try calling "validate_restored_save" for saves with fewer data in them.
Scanning saves allows you to take precaution and avoid displaying invalid saves in game menus, or reject loading a save before it crashes the game.

### Validating restored saves

In order to "validate" an "incompatible" save either when restoring one or when prescanning one you must add a function called "validate_restored_save" into one of your game scripts. The function is defined as:

```ags
function validate_restored_save(RestoredSaveInfo* saveInfo)
{
}
```

Whenever engine meets a save that is not entirely compatible with the current game, it will check if this function is present, and then run it. The function receives a RestoredSaveInfo object as a parameter, and this struct contains brief summary of the restored game, such as its description text, the version of the engine it was saved in, and numbers of global game objects (Characters, Inventory items, and so on) found inside.

RestoredSaveInfo has a boolean property named "Cancel", which must be set in this function to tell whether you confirm or cancel the use of this save. By default, Cancel begins set to "true", meaning that if you don't do anything then the engine will cancel restoring this save.

The purpose of this function is to let you decide whether save may still be allowed to load or not.

For more detailed explanation, please see ["validate_restored_save"](ValidateRestoredSave).
