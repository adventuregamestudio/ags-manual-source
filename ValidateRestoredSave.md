## validate_restored_save

```ags
validate_restored_save(RestoredSaveInfo* info)
```

Starting with version 3.6.2 the engine potentially allows to restore older saves which contents do not directly match the current game. To be precise - it allows to restore saves which have *less* data of any kind than the current game: less Characters, less GUI, less controls on particular GUI, less script variables, and so forth. But would such saves be restored fully automatically, that would result in unexpected game behavior, and would come unexpected to the game developers.

For that reason we have introduced a function named "validate_restored_save", which you may write in your game script if you would like to support restoring older saves with less data. If you do not plan that in your game, then you do not have to add one.

The purpose of "validate_restored_save" is to receive a struct [`RestoredSaveInfo`](RestoredSaveInfo) which contains a summary about the save, and lets game developer decide whether to accept or cancel this save.

This function may be called in two cases:
* when a save is being restored by the engine, and data mismatch was detected;
* when [`Game.ScanSaveSlots`](Game#gamescansaveslots) is called by you.

If we take a look at the `RestoredSaveInfo` struct, most of its properties tell the numbers of different kinds of data: `CharacterCount`, `GUICount`, and so on, - they are self-descriptive for the most part.

Besides these there are several properties that have special meaning:

*RestoredSaveInfo.IsPrescan* - tells whether "validate_restored_save" was called as a part of save scanning. This means that no restored data was applied to the game, and only a save summary was gathered for validation.

*RestoredSaveInfo.HasExtraData* and *RestoredSaveInfo.HasMissingData* - these indicate if the save contains mismatching data.

*RestoredSaveInfo.Cancel* - by setting this value to `true` or `false` you decide whether to accept or cancel the save. Note that if there's any data mismatch, then Cancel will be set to `true` by default: that means that if you do not set it yourself, then the save will be cancelled automatically.

*RestoredSaveInfo.RetryWithoutComponents* - by setting this value to a combination of [SaveComponentSelection values](StandardEnums#savecomponentselection) you can instruct the engine to reload the same save, only this time it should skip certain types of data completely. This choice may be the last resort when the save is so outdated that some parts of it cannot be applied.

In the most common case in this function you inspect the provided save's summary and set RestoredSaveInfo.Cancel to either `true` or `false`, judging by the summary values. What else you do in this function is completely up to you. For example, you may modify certain things in game after restoring an old save. Or you may schedule this for later, when you receive [`eEventRestoreGame`](Globalfunctions_Event#on_event).

Following is a relatively simple example of how such function could be implemented:

```ags
function validate_restored_save(RestoredSaveInfo* info)
{
    // We do not support saves with extra data.
    if (info.HasExtraData)
    {
        info.Cancel = true;
        return;
    }

    // If there's no missing data, then simply accept it and return.
    if (!info.HasMissingData)
    {
        info.Cancel = false;
        return;
    }
    
    // Now we are dealing with saves with less data than the current game has.
    // Let's suppose that we only let load saves if they have 5+ characters in them.
    // If they have less - that's some old WIP game version which we no longer want.
    if (info.CharacterCount < 5)
    {
        info.Cancel = true;
        return;
    }
    
    // Set Cancel to false, so that the save gets accepted now.
    info.Cancel = false;
    
    // If this is a "prescan" run, then this save is not getting loaded right away,
    // so we should not upgrade any game data, so return.
    if (info.IsPrescan)
    {
        return;
    }
    
    // Now we know that this save with less data was loaded into the game.
    // Suppose that in the older versions of the game we had character cNPC
    // standing in room 1, but in the newer vesions we need to have it in room 2,
    // so update its location.
    if (cNPC.Room == 1)
        cNPC.ChangeRoom(2);

    // etc...
}
```

*Compatibility:* `validate_restored_save` is supported by **AGS 3.6.2** and later versions.

*See also:* [`RestoredSaveInfo`](RestoredSaveInfo),
[`Game.ScanSaveSlots`](Game#gamescansaveslots)

