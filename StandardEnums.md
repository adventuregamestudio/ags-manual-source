## Standard Enumerated Types

AGS has several [enumerated types](ScriptKeywords#enum) in its standard header.
These are
used in calls to various commands, and will usually pop up automatically
in autocomplete. However, for times where autocomplete doesn't do the
job, having a manual reference is invaluable:

---

### `Alignment`

```ags
enum Alignment {
    eAlignNone          = 0,
    eAlignTopLeft       = 1,
    eAlignTopCenter     = 2,
    eAlignTopRight      = 4,
    eAlignMiddleLeft    = 8,
    eAlignMiddleCenter  = 16,
    eAlignMiddleRight   = 32,
    eAlignBottomLeft    = 64,
    eAlignBottomCenter  = 128,
    eAlignBottomRight   = 256,
    eAlignHasLeft       = 73,
    eAlignHasRight      = 292,
    eAlignHasTop        = 7,
    eAlignHasBottom     = 448,
    eAlignHasHorCenter  = 146,
    eAlignHasVerCenter  = 56
};
```

The Alignment enumeration consists of values that could be summed up in one integer variable to form a more complex combination of alignments. Although that ability is not used anywhere in practice yet, but may be in future. Additionally, it provides a set of masks that could be applied bitwise to check if alignment variable contains one of the distinct directions, for example:

```ags
if (align & eAlignHasLeft)
    // some code here
```

will execute some code if *align* variable contains "Left" in any combination (eAlignTopLeft, eAlignMiddleLeft or eAlignBottomLeft).

*Compatibility:* new version of Alignment enum was introduced in **AGS 3.5.0**. Previous Alignment was renamed into HorizontalAlignment.

*Used by:* [`Button.TextAlignment`](Button#buttontextalignment)

---

### `AudioFileType`

```ags
enum AudioFileType {
    eAudioFileOGG,
    eAudioFileMP3,
    eAudioFileWAV,
    eAudioFileVOC,
    eAudioFileMIDI,
    eAudioFileMOD
};
```

*Used by:* [`AudioClip.FileType`](AudioClip#audioclipfiletype)

---

### `AudioPriority`

```ags
enum AudioPriority {
    eAudioPriorityVeryLow = 1,
    eAudioPriorityLow = 25,
    eAudioPriorityNormal = 50,
    eAudioPriorityHigh = 75,
    eAudioPriorityVeryHigh = 100
};
```

*Used by:* [`AudioClip.Play`](AudioClip#audioclipplay),
[`AudioClip.PlayFrom`](AudioClip#audioclipplayfrom),
[`AudioClip.PlayQueued`](AudioClip#audioclipplayqueued)

---

### `BlockingStyle`

```ags
enum BlockingStyle {
    eBlock,
    eNoBlock
};
```

*Used by:* [`Character.Animate`](Character#characteranimate),
[`Character.FaceCharacter`](Character#characterfacecharacter),
[`Character.FaceLocation`](Character#characterfacelocation),
[`Character.FaceObject`](Character#characterfaceobject),
[`Character.Move`](Character#charactermove),
[`Character.Walk`](Character#characterwalk),
[`Character.WalkStraight`](Character#characterwalkstraight),
[`Object.Animate`](Object#objectanimate),
[`Object.Move`](Object#objectmove)

---

### `eCDAudioFunction`

```ags
enum eCDAudioFunction {
    eCDIsDriverPresent,
    eCDGetPlayingStatus,
    eCDPlayTrack,
    eCDPausePlayback,
    eCDResumePlayback,
    eCDGetNumTracks,
    eCDEject,
    eCDCloseTray,
    eCDGetCDDriveCount,
    eCDSelectActiveCDDrive
};
```

*Used by:* [`CDAudio`](Multimedia#cdaudio)

---

### `CharacterDirection`

```ags
enum CharacterDirection {
    eDirectionDown = 0,
    eDirectionLeft,
    eDirectionRight,
    eDirectionUp,
    eDirectionDownRight,
    eDirectionUpRight,
    eDirectionDownLeft,
    eDirectionUpLeft,
    eDirectionNone = SCR_NO_VALUE
};
```

*Used by:* [`Character.ChangeRoom`](Character#characterchangeroom),
[`Character.FaceDirection`](Character#characterfacedirection)

---

### `CursorMode`

```ags
enum CursorMode {
    eModeXXXX,
    eModeXXXX,
    // ...
};
```

The CursorMode enumeration is generated automatically based on your
mouse cursors. The cursor mode name is taken, all its spaces are
removed, and *eMode* is added to the front.

*Used by:* [`IsInteractionAvailable`](Globalfunctions_General#isinteractionavailable),
[`Room.ProcessClick`](Room#roomprocessclick),
[`Mouse.ChangeModeGraphic`](Mouse#mousechangemodegraphic),
[`Mouse.ChangeModeHotspot`](Mouse#mousechangemodehotspot),
[`Mouse.DisableMode`](Mouse#mousedisablemode),
[`Mouse.EnableMode`](Mouse#mouseenablemode),
[`Mouse.IsModeEnabled`](Mouse#mouseismodeenabled),
[`Mouse.UseModeGraphic`](Mouse#mouseusemodegraphic),
[`Mouse.Mode`](Mouse#mousemode),
[`InventoryItem.IsInteractionAvailable`](InventoryItem#inventoryitemisinteractionavailable),
[`InventoryItem.RunInteraction`](InventoryItem#inventoryitemruninteraction),
[`Hotspot.IsInteractionAvailable`](Hotspot#hotspotisinteractionavailable),
[`Hotspot.RunInteraction`](Hotspot#hotspotruninteraction),
[`Object.IsInteractionAvailable`](Object#objectisinteractionavailable),
[`Object.RunInteraction`](Object#objectruninteraction),
[`Character.IsInteractionAvailable`](Character#characterisinteractionavailable),
[`Character.RunInteraction`](Character#characterruninteraction)

---

### `CutsceneSkipType`

```ags
enum CutsceneSkipType {
    eSkipESCOnly,
    eSkipAnyKey,
    eSkipMouseClick,
    eSkipAnyKeyOrMouseClick,
    eSkipESCOrRightButton,
    eSkipScriptOnly
};
```

*Used by:* [`StartCutscene`](Globalfunctions_General#startcutscene)

---

### `DialogOptionsNumbering`

```ags
enum DialogOptionsNumbering {
  eDialogOptNumbers_Disabled  = -1,
  eDialogOptNumbers_KeysOnly  = 0,
  eDialogOptNumbers_Display   = 1
};
```

*Used by:* [`Dialog.OptionsNumbering`](Dialog#dialogoptionsnumbering)

---

### `DialogOptionSayStyle`

```ags
enum DialogOptionSayStyle {
    eSayUseOptionSetting,
    eSayAlways,
    eSayNever
};
```

*Used by:* [`Dialog.DisplayOptions`](Dialog#dialogdisplayoptions)

---

### `DialogOptionState`

```ags
enum DialogOptionState {
    eOptionOff,
    eOptionOn,
    eOptionOffForever
};
```

*Used by:* [`Dialog.GetOptionState`](Dialog#dialoggetoptionstate),
[`Dialog.SetOptionState`](Dialog#dialogsetoptionstate)

---

### `Direction`

```ags
enum Direction {
    eForwards,
    eBackwards
};
```

*Used by:* [`Character.Animate`](Character#characteranimate),
[`Object.Animate`](Object#objectanimate)

---

### `EngineValueID`

```ags
enum EngineValueID
{
    ENGINE_VALUE_UNDEFINED = 0,         // formality...
    ENGINE_VALUE_SI_VALUENAME,          // get engine value's own name, by its index
    ENGINE_VALUE_S_ENGINE_NAME,
    ENGINE_VALUE_S_ENGINE_VERSION,      // N.N.N.N (with an optional custom tag)
    ENGINE_VALUE_S_ENGINE_VERSION_FULL, // full, with bitness, endianess and any tag list
    ENGINE_VALUE_S_DISPLAY_MODE_STR,
    ENGINE_VALUE_S_GFXRENDERER,
    ENGINE_VALUE_S_GFXFILTER,
    ENGINE_VALUE_I_SPRCACHE_MAXNORMAL,  // sprite cache capacity limit (in KB)
    ENGINE_VALUE_I_SPRCACHE_NORMAL,     // sprite cache capacity filled (in KB)
    ENGINE_VALUE_I_SPRCACHE_LOCKED,     // amount of locked sprites (in KB)
    ENGINE_VALUE_I_SPRCACHE_EXTERNAL,   // amount of external sprites, that means dynamic sprites (in KB)
    ENGINE_VALUE_I_TEXCACHE_MAXNORMAL,  // texture cache capacity limit (in KB)
    ENGINE_VALUE_I_TEXCACHE_NORMAL,     // texture cache capacity filled (in KB)
    ENGINE_VALUE_I_FPS_MAX,             // max fps, this is set by SetGameSpeed
    ENGINE_VALUE_I_FPS,                 // real average fps (updates along with the game run)
    ENGINE_VALUE_LAST                   // in case user wants to iterate them
};
```

The `EngineValueID` follows the pattern `ENGINE_VALUE_<I,II,S,SI>_NAME`, where

- `I` - integer,
- `S` - string,
- `II` - indexed integer,
- `SI` - indexed string.

*Compatibility:* supported by **AGS 3.6.2** and higher.

*Used by:* [`System.GetEngineInteger`](System#systemgetengineinteger),
[`System.GetEngineString`](System#systemgetenginestring)

---

### `EventType`

```ags
enum EventType {
    eEventLeaveRoom,
    eEventEnterRoomBeforeFadein,
    eEventGotScore,
    eEventGUIMouseDown,
    eEventGUIMouseUp,
    eEventAddInventory,
    eEventLoseInventory,
    eEventRestoreGame,
    eEventEnterRoomAfterFadein,
    eEventLeaveRoomAfterFadeout,
    eEventGameSaved
};
```

*Compatibility:* the `eEventEnterRoomAfterFadein` is supported by **AGS 3.6.0** and later versions.
The `eEventLeaveRoomAfterFadeout` and `eEventGameSaved` is supported by **AGS 3.6.1** and later versions.

*Passed into:* on_event

---

### `FileMode`

```ags
enum FileMode {
    eFileRead,
    eFileWrite,
    eFileAppend
};
```

*Used by:* [`File.Open`](File#fileopen)

---

### `FileSeek`

```ags
enum FileSeek {
    eSeekBegin = 0,
    eSeekCurrent = 1,
    eSeekEnd = 2
};
```

*Used by:* [`File.Seek`](File#fileseek)

---

### `FileSortStyle`

```ags
enum FileSortStyle
{
    eFileSort_None,
    eFileSort_Name,
    eFileSort_Time
};
```

*Compatibility:* supported by **AGS 3.6.2** and higher.

*Used by* [`File.GetFiles`](File#filegetfiles),
[`ListBox.FillDirList`](ListBox#listboxfilldirlist)

---

### `eFlipDirection`

```ags
enum eFlipDirection {
    eFlipLeftToRight,
    eFlipUpsideDown,
    eFlipBoth
};
```

*Used by:* [`DynamicSprite.Flip`](DynamicSprite#dynamicspriteflip)

---

### `FontType`

```ags
enum FontType {
    eNullFont
    eFontXXXX,
    eFontXXXX,
    // ...
};
```

The FontType enumeration is generated automatically based on your fonts.
The font name is taken, all its spaces are removed, and *eFont* is added
to the front.

The `eNullFont` is a special value that can be used in place of an existing font,
and instead will draw no text, and return 0 size for the text.

*Compatibility:* the `eNullFont` is supported by **AGS 3.6.2** and later versions.

*Used by:* [`Button.Font`](Button#buttonfont),
[`DrawingSurface.DrawMessageWrapped`](DrawingSurface#drawingsurfacedrawmessagewrapped),
[`DrawingSurface.DrawString`](DrawingSurface#drawingsurfacedrawstring),
[`DrawingSurface.DrawStringWrapped`](DrawingSurface#drawingsurfacedrawstringwrapped),
[`Game.NormalFont`](Game#gamenormalfont),
[`Game.SpeechFont`](Game#gamespeechfont),
[`GetTextHeight`](Globalfunctions_General#gettextheight),
[`GetTextWidth`](Globalfunctions_General#gettextwidth),
[`Label.Font`](Label#labelfont),
[`ListBox.Font`](ListBox#listboxfont),
[`TextBox.Font`](TextBox#textboxfont),
[`Overlay.CreateTextual`](Overlay#overlaycreatetextual),
[`Overlay.SetText`](Overlay#overlaysettext)

---

### `GUIPopupStyle`

```ags
enum GUIPopupStyle {
    eGUIPopupNormal = 0,
    eGUIPopupMouseYPos = 1,
    eGUIPopupModal = 2,
    eGUIPopupPersistent = 3
};
```

*Compatibility:* supported by **AGS 3.5.0** and higher.

*Used by:* [`GUI.PopupStyle`](GUI#guipopupstyle)

---

### `HorizontalAlignment`

```ags
enum HorizontalAlignment {
    eAlignLeft          = 1,
    eAlignCenter        = 2,
    eAlignRight         = 4
};
```

Note that HorizontalAlignment's values match the first values of Alignment enumeration (eAlignTopLeft, eAlignTopCenter, eAlignTopRight).

*Compatibility:* replaced old Alignment enumeration in **AGS 3.5.0**.

*Used by:*
[`Character.LockViewAligned`](Character#characterlockviewaligned), [`DrawingSurface.DrawStringWrapped`](DrawingSurface#drawingsurfacedrawstringwrapped), [`Label.TextAlignment`](Label#labeltextalignment), [`ListBox.TextAlignment`](ListBox#listboxtextalignment), [`Speech.TextAlignment`](Speech#speechtextalignment)

---

### `InputType`

```ags
enum InputType
{
    eInputNone     = 0x00000000,
    eInputKeyboard = 0x02000000,
    eInputMouse    = 0x04000000,
    eInputAny      = 0xFF000000
};
```

The InputType enumeration consists of values that could be summed up in one integer variable to form a combination of input types. This may be used to store several "types" in one integer, pass them into a function, or return from a function. You may use bitwise operation to check if a variable contains one of the distinct types, for example:

```ags
if (type & eInputKeyboard)
    // some code here
```

will execute some code if *type* variable contains *at least* "eInputKeyboard".

*Compatibility:* supported by **AGS 3.6.0** and higher.

*Used by:* [Wait functions](Globalfunctions_Wait)

---

### `LocationType`

```ags
enum LocationType {
    eLocationNothing,
    eLocationHotspot,
    eLocationCharacter,
    eLocationObject
};
```

*Returned by:* [`GetLocationType`](Globalfunctions_General#getlocationtype)

---

### `LogLevel`

```ags
enum LogLevel
{
    eLogAlert = 1,
    eLogFatal = 2,
    eLogError = 3,
    eLogWarn = 4,
    eLogInfo = 5,
    eLogDebug = 6
};
```


*Compatibility:* supported by **AGS 3.6.0** and higher.

*Used by:* [`System.Log`](System#systemlog)

---

### `MouseButton`

```ags
enum MouseButton {
    eMouseLeft,
    eMouseRight,
    eMouseMiddle,
    eMouseLeftInv,
    eMouseMiddleInv,
    eMouseRightInv,
    eMouseWheelNorth,
    eMouseWheelSouth
};
```

*Used by:* [`Mouse.IsButtonDown`](Mouse#mouseisbuttondown)

*Passed into:* on_mouse_click

---

### `eOperatingSystem`

```ags
enum eOperatingSystem {
    eOSDOS,
    eOSWindows,
    eOSLinux,
    eOSMacOS,
    eOSAndroid,
    eOSiOS,
    eOSPSP,
    eOSWeb,
    eOSFreeBSD
};
```

*Used by:* [`System.OperatingSystem`](System#systemoperatingsystem)

---

### `RenderLayer`

```ags
enum RenderLayer {
    eRenderLayerNone      = 0x00000000,
    eRenderLayerEngine    = 0x00000001,
    eRenderLayerCursor    = 0x00000002,
    eRenderLayerUI        = 0x00000004,
    eRenderLayerRoom      = 0x00000008,
    eRenderLayerAll       = 0xFFFFFFFF
};
```

The `RenderLayer` enum lets specify which elements should be rendered in a screen capture.
This allows you to, as an example, create a screenshot of only the room layer and not include any GUI or cursor on top.
The Engine layer is for things like the in-engine FPS counter.

The `RenderLayer` enum can be combined like flags, using bitwise operators.

*Compatibility:* supported by **AGS 3.6.2** and higher.

*Used by:* [`DynamicSprite.CreateFromScreenShot`](DynamicSprite#dynamicspritecreatefromscreenshot),
[`SetGameOption`](Globalfunctions_General#setgameoption)

---

### `RepeatStyle`

```ags
enum RepeatStyle {
    eOnce,
    eRepeat
};
```

*Used by:* [`Button.Animate`](Button#buttonanimate),
[`Character.Animate`](Character#characteranimate),
[`Object.Animate`](Object#objectanimate)

---

### `RestoredSaveResult`

```ags
enum RestoredSaveResult
{
    eRestoredSave_ClearData   = 0x01,
    eRestoredSave_MissingData = 0x08,
    eRestoredSave_ExtraData   = 0x10,
    eRestoredSave_Prescan     = 0x20
};
```

The `RestoredSaveResult` enum is used to specify the result of reading a save.

The `RestoredSaveResult` enum can be combined like flags, using bitwise operators.

*Compatibility:* supported by **AGS 3.6.2** and higher.

*Used by:* [`validate_restored_save`](ValidateRestoredSave)

---

### `RoundDirection`

```ags
enum RoundDirection {
    eRoundDown,
    eRoundNearest,
    eRoundUp
};
```

*Used by:* [`FloatToInt`](Maths#floattoint)

---

### `SaveGameSortStyle`

```ags
enum SaveGameSortStyle
{
    eSaveGameSort_None,
    eSaveGameSort_Number,
    eSaveGameSort_Time,
    eSaveGameSort_Description
};
```

*Compatibility:* supported by **AGS 3.6.2** and higher.

*Used by:* [`Game.GetSaveSlots`](Game#gamegetsaveslots),
[`Game.ScanSaveSlots`](Game#gamescansaveslots),
[`ListBox.FillSaveGameList`](ListBox#listboxfillsavegamelist),
[`ListBox.FillSaveGameSlots`](ListBox#listboxfillsavegameslots)

---

### `SaveComponentSelection`

```ags
enum SaveComponentSelection
{
    eSaveCmp_None           = 0,
    eSaveCmp_Audio          = 0x00000002,
    eSaveCmp_Dialogs        = 0x00000008,
    eSaveCmp_GUI            = 0x00000010,
    eSaveCmp_Cursors        = 0x00000040,
    eSaveCmp_Views          = 0x00000080,
    eSaveCmp_DynamicSprites = 0x00000100,
    eSaveCmp_Plugins        = 0x00002000
};
```

The `SaveComponentSelection` enum lets specify which parts of the save should be skipped when writing or restoring a game save.
This allows you to, as an example, keep certain things persistent in your game, unaffected by player saving and loading their story progress.

The `SaveComponentSelection` enum can be combined like flags, using bitwise operators.

*Compatibility:* supported by **AGS 3.6.2** and higher.

*Used by:* [`SetGameOption`](Globalfunctions_General#setgameoption),
[`RestoredSaveInfo`](RestoredSaveInfo)

---

### `SkipSpeechStyle`

```ags
enum SkipSpeechStyle {
    eSkipNone         = -1,
    eSkipKeyMouseTime = 0,
    eSkipKeyTime      = 1,
    eSkipTime         = 2,
    eSkipKeyMouse     = 3,
    eSkipMouseTime    = 4,
    eSkipKey          = 5,
    eSkipMouse        = 6
};
```

*Used by:* [`Speech.SkipStyle`](Speech#speechskipstyle)

---

### `SortDirection`

```ags
enum SortDirection
{
    eSortNoDirection,
    eSortAscending,
    eSortDescending
};
```

*Compatibility:* supported by **AGS 3.6.2** and higher.

*Used by* [`ListBox.FillDirList`](ListBox#listboxfilldirlist),
[`ListBox.FillSaveGameList`](ListBox#listboxfillsavegamelist),
[`ListBox.FillSaveGameSlots`](ListBox#listboxfillsavegameslots)

---

### `SortStyle`

```ags
enum SortStyle {
    eNonSorted = 0,
    eSorted = 1
};
```

*Compatibility:* supported by **AGS 3.5.0** and higher.

*Used by:* [`Dictionary.Create`](Dictionary#dictionarycreate),
[`Set.Create`](Set#setcreate)

---

### `eSpeechStyle`

```ags
enum eSpeechStyle {
    eSpeechLucasarts,
    eSpeechSierra,
    eSpeechSierraWithBackground,
    eSpeechFullScreen
};
```

*Used by:* [`Speech.Style`](Speech#speechstyle)

---

### `StopMovementStyle`

```ags
enum StopMovementStyle
{
    eKeepMoving = 0,
    eStopMoving = 1
};
```

*Used by:* [`Character.LockView`](Character#characterlockviewaligned),
[`Character.LockViewFrame`](Character#characterlockviewoffset)

---

### `StringCompareStyle`

```ags
enum StringCompareStyle {
    eCaseInsensitive = 0,
    eCaseSensitive = 1
};
```

*Compatibility:* supported by **AGS 3.5.0** and higher.

*Used by:* [`Dictionary.Create`](Dictionary#dictionarycreate),
[`Set.Create`](Set#setcreate),
[`String.CompareTo`](String#stringcompareto),
[`String.EndsWith`](String#stringendswith),
[`String.Replace`](String#stringreplace),
[`String.StartsWith`](String#stringstartswith)

---

### `TransitionStyle`

```ags
enum TransitionStyle {
    eTransitionFade,
    eTransitionInstant,
    eTransitionDissolve,
    eTransitionBoxout,
    eTransitionCrossfade
};
```

*Used by:* [`SetScreenTransition`](Globalfunctions_Screen#setscreentransition),
[`SetNextScreenTransition`](Globalfunctions_Screen#setnextscreentransition)

---

### `VideoSkipStyle`

```ags
enum VideoSkipStyle {
    eVideoSkipNotAllowed,
    eVideoSkipEscKey,
    eVideoSkipAnyKey,
    eVideoSkipAnyKeyOrMouse
};
```

*Used by:* [`PlayVideo`](Multimedia#playvideo)

---

### `eVoiceMode`

```ags
enum eVoiceMode {
    eSpeechTextOnly,
    eSpeechVoiceAndText,
    eSpeechVoiceOnly
};
```

*Used by:* [`Speech.VoiceMode`](Speech#speechvoicemode)

---

### `WalkWhere`

```ags
enum WalkWhere {
    eAnywhere,
    eWalkableAreas
};
```

*Used by:* [`Character.Move`](Character#charactermove),
[`Character.Walk`](Character#characterwalk),
[`Object.Move`](Object#objectmove)
