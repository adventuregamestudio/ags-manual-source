## RestoredSaveInfo

The RestoredSaveInfo instance is received as an argument by the [`validate_restored_save`](ValidateRestoredSave) script function (if you have one written in your script). This struct contains a summary of the read game's save, either when restoring one, or when running [`Game.ScanSaveSlots`](Game#gamescansaveslots), and lets you tell the engine whether to accept or cancel (skip) this save.

*Compatibility:* The RestoredSaveInfo struct is supported by **AGS 3.6.2** and later versions.

---

### `RestoredSaveInfo.AudioClipTypeCount`

```ags
int RestoredSaveInfo.AudioClipTypeCount;
```

Gets the number of Audio Types present in this save.

Example:

```ags
function validate_restored_save(RestoredSaveInfo* info)
{
  System.Log(eLogInfo, "The save has %d audio types.", info.AudioClipTypeCount);
}
```

will write the number of audio types stored in the save being restored or scanned to the game log.

---

### `RestoredSaveInfo.Cancel`

```ags
bool RestoredSaveInfo.Cancel;
```

Gets/sets whether this save should be cancelled. You must set this in `validate_restored_save` to tell AGS whether to cancel or accept the save (naturally, Cancel = true would mean to cancel it, and Cancel = false would mean to accept). Default value depends on whether AGS detected any data mismatches in the save.

Example:

```ags
function validate_restored_save(RestoredSaveInfo* info)
{
  if (info.HasExtraData)
  {
    info.Cancel = true;
  }
  else
  {
    info.Cancel = false;
  }
}
```

will cancel the saves that contain extra data and accept all the others.

---

### `RestoredSaveInfo.CharacterCount`

```ags
int RestoredSaveInfo.CharacterCount;
```

Gets the number of Characters present in this save.

Example:

```ags
function validate_restored_save(RestoredSaveInfo* info)
{
  // version 1.1 added two characters, who appear only later in the story,
  // so the saves made with version 1.0 can be loaded without them
  if (!info.HasExtraData && info.CharacterCount >= Game.CharacterCount - 2)
  {
    info.Cancel = false;
  }
}
```

will accept the saves that lack at most the two characters added by an update of the game, instead of letting the engine cancel them because of the missing data. The new characters will keep their initial state.

---

### `RestoredSaveInfo.CursorCount`

```ags
int RestoredSaveInfo.CursorCount;
```

Gets the number of Cursors present in this save.

Example:

```ags
function validate_restored_save(RestoredSaveInfo* info)
{
  if (info.CursorCount < Game.MouseCursorCount)
  {
    System.Log(eLogInfo, "The save has fewer cursors than the game.");
  }
}
```

will write a note to the game log if the save contains fewer cursors than the current game does.

---

### `RestoredSaveInfo.Description`

```ags
String RestoredSaveInfo.Description;
```

Gets this save's description string. This is the same string which was used in a call to [`SaveGameSlot`](Globalfunctions_General#savegameslot).

Example:

```ags
function validate_restored_save(RestoredSaveInfo* info)
{
  System.Log(eLogInfo, "Checking the save '%s'.", info.Description);
}
```

will write the description that the save was given when it was made to the game log.

---

### `RestoredSaveInfo.DialogCount`

```ags
int RestoredSaveInfo.DialogCount;
```

Gets the number of Dialogs present in this save.

Example:

```ags
function validate_restored_save(RestoredSaveInfo* info)
{
  if (info.DialogCount < Game.DialogCount)
  {
    System.Log(eLogInfo, "The save has fewer dialogs than the game.");
  }
}
```

will write a note to the game log if the save contains fewer dialogs than the current game does.

---

### `RestoredSaveInfo.EngineVersion`

```ags
String RestoredSaveInfo.EngineVersion;
```

Gets the version of the AGS engine which wrote this save, represented as a "N.N.N.N" string. This property is meant for informational purposes only.

Example:

```ags
function validate_restored_save(RestoredSaveInfo* info)
{
  System.Log(eLogInfo, "This save was written by the engine version %s.", info.EngineVersion);
}
```

will write the version of the AGS engine that wrote the save to the game log.

---

### `RestoredSaveInfo.GameVersion`

```ags
String RestoredSaveInfo.GameVersion;
```

Gets the version of the game which wrote this save, represented as a "N.N.N.N" string. This value is taken from the respective property of the "Info" section of the General Settings.

Example:

```ags
function validate_restored_save(RestoredSaveInfo* info)
{
  if (info.GameVersion.StartsWith("0."))
  {
    info.Cancel = true; // the saves made by the beta versions are not supported
  }
}
```

will refuse the saves made by any version of the game starting with "0.", such as the beta versions, even if their data matches the current game.

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

---

### `RestoredSaveInfo.GlobalScriptDataSize`

```ags
int RestoredSaveInfo.GlobalScriptDataSize;
```

Gets the size of the global script's data present in this save. This corresponds to the total number of bytes used by all the global script's variables.

Example:

```ags
function validate_restored_save(RestoredSaveInfo* info)
{
  System.Log(eLogInfo, "The global script's data takes %d bytes in the save.", info.GlobalScriptDataSize);
}
```

will write the size of the global script's variables stored in the save to the game log.

---

### `RestoredSaveInfo.GUICount`

```ags
int RestoredSaveInfo.GUICount;
```

Gets the number of GUIs present in this save.

Example:

```ags
function validate_restored_save(RestoredSaveInfo* info)
{
  if (info.GUICount < Game.GUICount)
  {
    System.Log(eLogInfo, "The save has fewer GUIs than the game.");
  }
}
```

will write a note to the game log if the save contains fewer GUIs than the current game does.

---

### `RestoredSaveInfo.GUIControlCount`

```ags
int RestoredSaveInfo.GUIControlCount[int index];
```

Gets the number of controls on each GUI present in this save.

Example:

```ags
function validate_restored_save(RestoredSaveInfo* info)
{
    if (info.GUIControlCount[gMainMenu.ID] < 10)
    {
        info.Cancel = true; // cancel the save
    }
    
    info.Cancel = false; // keep the save
}
```

This will check how many gui controls were on GUI called gMainMenu when the save was made, and if it's less than 10, then cancel this save.

---

### `RestoredSaveInfo.HasExtraData`

```ags
bool RestoredSaveInfo.HasExtraData;
```

Gets whether this save has extra data, which means any data that is not applicable to the current game. This property is reserved for the future, and normally you should never get `validate_restored_save` called when there's extra data, as AGS cannot deal with such situation, and will error earlier.

Example:

```ags
function validate_restored_save(RestoredSaveInfo* info)
{
  if (info.HasExtraData)
  {
    info.Cancel = true;
  }
}
```

will cancel the saves that contain data not applicable to the current game.

---

### `RestoredSaveInfo.HasMissingData`

```ags
bool RestoredSaveInfo.HasMissingData;
```

Gets whether this save has any data missing, which means that it does not have something that the current game has: has less characters or less script variables, for example. In such case these items will be reset to their default states when the save is restored.

It's up to you to decide whether you want to continue with this save, and whether any fixups are necessary to be done after restoring it.

Example:

```ags
function validate_restored_save(RestoredSaveInfo* info)
{
  if (info.HasMissingData && !info.HasExtraData)
  {
    // the save comes from an older version of the game, which had less content;
    // the new things will simply start in their initial state
    info.Cancel = false;
    System.Log(eLogInfo, "Loading an older save, made with version %s.", info.GameVersion);
  }
}
```

will accept the saves that lack some of the data the current game has (but not those with extra data), and write a note about it to the game log.

---

### `RestoredSaveInfo.InventoryItemCount`

```ags
int RestoredSaveInfo.InventoryItemCount;
```

Gets the number of Inventory Items present in this save.

Example:

```ags
function validate_restored_save(RestoredSaveInfo* info)
{
  if (info.InventoryItemCount < Game.InventoryItemCount)
  {
    System.Log(eLogInfo, "The save has fewer inventory items than the game.");
  }
}
```

will write a note to the game log if the save contains fewer inventory items than the current game does.

---

### `RestoredSaveInfo.IsPrescan`

```ags
bool RestoredSaveInfo.IsPrescan;
```

Gets whether this save was only prescanned, and not loaded into the game. This is useful when you are using [`Game.ScanSaveSlots`](Game#gamescansaveslots), and would like to distinguish cases when `validate_restored_save` is called for scanning or actual save restoration.

For example, you may want to fixup your game after restoring an older save with less data, but not do that in case of scanning one.

Example:

```ags
function validate_restored_save(RestoredSaveInfo* info)
{
    if (info.CharacterCount < 5)
    {
        if (!info.IsPrescan)
        {
            // Suppose that in the newer version of the game we have a character
            // cNewCharacter, and if player was visiting room 10, then we want it
            // to appear in the same room
            if (info.Room == 10)
                cNewCharacter.ChangeRoom(10);
        }
    }
    
    info.Cancel = false; // keep the save
}
```

---

### `RestoredSaveInfo.Slot`

```ags
int RestoredSaveInfo.Slot;
```

Gets this save's slot number.

Example:

```ags
function validate_restored_save(RestoredSaveInfo* info)
{
  System.Log(eLogInfo, "Checking the save in the slot %d.", info.Slot);
}
```

will write the slot number of the save being checked to the game log.

---

### `RestoredSaveInfo.RetryWithoutComponents`

```ags
SaveComponentSelection RestoredSaveInfo.RetryWithoutComponents;
```

Gets/sets whether this game's save should be reloaded again without particular components. By setting this value to a combination of [SaveComponentSelection values](StandardEnums#savecomponentselection) you can instruct AGS to reload the same save, only this time it should skip certain types of data completely. This choice may be the last resort when the save is so outdated that some parts of it cannot be applied.

Example:

```ags
function validate_restored_save(RestoredSaveInfo* info)
{
    if ((info.GUICount > 0) && (info.GUICount < Game.GUICount))
    {
        info.RetryWithoutComponents += eSaveCmp_GUI;
    }
    
    if ((info.ViewCount > 0) && (info.ViewCount < Game.ViewCount))
    {
        info.RetryWithoutComponents += eSaveCmp_Views;
    }
    
    info.Cancel = false; // keep the save (but reload without some components)
}
```

will check if restored save has less GUI or View data, and requests to reload this save without restoring GUIs or Views respectively, in attempt to avoid any conflicts with the current game.

---

### `RestoredSaveInfo.Room`

```ags
int RestoredSaveInfo.Room;
```

Gets the room number this save was made in.

Example:

```ags
function validate_restored_save(RestoredSaveInfo* info)
{
  System.Log(eLogInfo, "The save was made in the room %d.", info.Room);
}
```

will write the number of the room in which the save was made to the game log.

---

### `RestoredSaveInfo.ScriptModuleCount`

```ags
int RestoredSaveInfo.ScriptModuleCount;
```

Gets the number of script modules present in this save (not counting the global script, nor the room scripts).

Example:

```ags
function validate_restored_save(RestoredSaveInfo* info)
{
  System.Log(eLogInfo, "The save contains data of %d script modules.", info.ScriptModuleCount);
}
```

will write the number of script modules stored in the save to the game log.

---

### `RestoredSaveInfo.ScriptModuleDataSizes`

```ags
int RestoredSaveInfo.ScriptModuleDataSizes[int index];
```

Gets the size of each of the script module's data present in this save (except for the global script). This corresponds to the total number of bytes used by all the respective script's variables.

Example:

```ags
function validate_restored_save(RestoredSaveInfo* info)
{
  for (int i = 0; i < info.ScriptModuleCount; i++)
  {
    System.Log(eLogInfo, "Module %d data takes %d bytes.", i, info.ScriptModuleDataSizes[i]);
  }
}
```

will write the size of the variables stored in the save for each script module to the game log.

---

### `RestoredSaveInfo.ScriptModuleNames`

```ags
String RestoredSaveInfo.ScriptModuleNames[int index];
```

Gets the name of each of the script modules in this save (except for the global script).

Example:

```ags
function validate_restored_save(RestoredSaveInfo* info)
{
  for (int i = 0; i < info.ScriptModuleCount; i++)
  {
    System.Log(eLogInfo, "The save contains the script module '%s'.", info.ScriptModuleNames[i]);
  }
}
```

will write the names of all the script modules stored in the save to the game log.

---

### `RestoredSaveInfo.ViewCount`

```ags
int RestoredSaveInfo.ViewCount;
```

Gets the number of Views present in this save.

Example:

```ags
function validate_restored_save(RestoredSaveInfo* info)
{
  if (info.ViewCount < Game.ViewCount)
  {
    System.Log(eLogInfo, "The save has fewer views than the game.");
  }
}
```

will write a note to the game log if the save contains fewer views than the current game does.

---

### `RestoredSaveInfo.ViewFrameCount`

```ags
int RestoredSaveInfo.ViewFrameCount[int view];
```

Gets the number of frames in each View present in this save.

Example:

```ags
function validate_restored_save(RestoredSaveInfo* info)
{
  if (info.ViewCount > VPLAYERWALK)
  {
    System.Log(eLogInfo, "The view VPLAYERWALK has %d frames in the save.", info.ViewFrameCount[VPLAYERWALK]);
  }
}
```

will write to the game log how many frames the view VPLAYERWALK has in the save, provided that the view is present there.

---

### `RestoredSaveInfo.ViewLoopCount`

```ags
int RestoredSaveInfo.ViewLoopCount[int view];
```

Gets the number of loops in each View present in this save.

Example:

```ags
function validate_restored_save(RestoredSaveInfo* info)
{
  if (info.ViewCount > VPLAYERWALK)
  {
    System.Log(eLogInfo, "The view VPLAYERWALK has %d loops in the save.", info.ViewLoopCount[VPLAYERWALK]);
  }
}
```

will write to the game log how many loops the view VPLAYERWALK has in the save, provided that the view is present there.
