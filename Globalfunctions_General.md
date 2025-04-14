## Global functions (general section)

### `AbortGame`

```ags
AbortGame(string message, ...)
```

Aborts the game and returns to the operating system.

The standard AGS error dialog is displayed, with the script line numbers
and call stack, along with *message* (which can include `%d` and `%s`
Display-style tokens).

You can use this function rather than QuitGame if you are writing some
debugging checks into your script, to make sure that the user calls your
functions in the correct way.

This command should ideally never be called in the final release of a
game.

Example:

```ags
function MakeWider(int newWidth) {
    if (newWidth < 10)
        AbortGame("newWidth expects a width of at least 10!");
}
```

will abort the game if MakeWider is called with a parameter less than 10.

SeeAlso: [`QuitGame`](Globalfunctions_General#quitgame)

---

### `CallRoomScript`

```ags
CallRoomScript (int value)
```

Calls the `on_call` function in the current room script. This is useful
for things like the text parser, where you want to check for general
game sentences, and then ask the current room if the sentence was
relevant to it.

The on_call function will be called in the current room script, with
its `value` parameter having the value you pass here. This allows it to
distinguish between different tasks. If you need to pass more values, or values of different types, you may have to resort to [Global variables](GlobalVariables).

If the current room has no on_call function, nothing will happen. No
error will occur.

You write the on_call function into the room script (\"Edit script\"
button on Room Settings pane), similar to the way you do dialog_request
in the global script:

```ags
function on_call (int value) {
    if (value == 1) {
        // Check text input
        if (Parser.Said("get apple"))
            Display("No, leave the tree alone.");
    }
}
```

The function doesn't get called immediately; instead, the engine will
run it in due course, probably during the next game loop, so you can't
use any values set by it immediately.

Once the on_call function has executed (or not if there isn't one),
the game.roomscript_finished variable will be set to 1, so you can
check for that in your repeatedly_execute script if you need to do
something afterwards.

SeeAlso: [The text parser documentation](TextParser), [`on_call`](Globalfunctions_Event#on_call)

---

### `ClaimEvent`

```ags
ClaimEvent()
```

This command is used in a room script or script module's
*on_key_press* or *on_mouse_click* function, and it tells AGS not to
run the global script afterwards.

For example, if your room script responds to the player pressing the
space bar, and you don't want the global script's on_key_press to
handle it as well, then use this command.

This is useful if you have for example a mini-game in the room, and you
want to use some keys for a different purpose to what they normally do.

The normal order in which scripts are called for *on_key_press* and
*on_mouse_click* is as follows:

-   room script
-   script modules, in order
-   global script

If any of these scripts calls ClaimEvent, then the chain is aborted at
that point.

Example:

```ags
if (keycode == ' ') {
    Display("You pressed space in this room!");
    ClaimEvent();
}
```

prevents the global script on_key_press from running if the player
pressed the space bar.

SeeAlso: [Script events](Globalfunctions_Event)

---

### `CopySaveSlot`

```ags
void CopySaveSlot (int old_slot, int new_slot)
```

Copies the save game from one slot to another, overwriting a save if one was already present at a new slot.

Example:

```ags
CopySaveSlot(2, 5);
```

Copies the save slot 2 (which we should have saved earlier), over the save slot 5.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:* [`MoveSaveSlot`](Globalfunctions_General#movesaveslot),
[`DeleteSaveSlot`](Globalfunctions_General#deletesaveslot),
[`SaveGameSlot`](Globalfunctions_General#savegameslot),
[`RestoreGameSlot`](Globalfunctions_General#restoregameslot),
[`SaveGameDialog`](Globalfunctions_General#savegamedialog)

---

### `Debug`

```ags
Debug (int command, int data)
```

This function provides all the debug services in the system. It performs
various different tasks, depending on the value of the COMMAND
parameter. If debug mode is off, then this function does nothing. This
allows you to leave your script unaltered when you distribute your game,
so you just have to turn off debug mode in the AGS Editor.

The DATA parameter depends on the command - pass 0 if it is not used.
All the valid values for the COMMAND parameter are listed below along
with what they do:

    0   All inventory - gives the current player character one of every
        inventory item. This is useful for testing so that you don't have to
        go and pick up items every time you test part of the game where they
        are required.
    1   Display interpreter version - the engine will display its version
        number and build date.
    2   The viewport fills with the visible room mask, where DATA allows you 
        to select which mask: 
        0 - none, 1 - hotspots, 2 - walkbehinds, 3 - walkable areas, 4 - regions. 
        This command also works as a toggle switch, calling it with the same mask 
        index will turn it off.
        For the walkable areas, they are drawn with blocking areas at characters 
        feet removed. This is useful if you think the path-finder is not working 
        properly.
    3   Teleport - displays a dialog box asking for what room you want to go
        to, and then calls ChangeRoom to teleport you there. Useful for skipping
        parts of the game or going to a specific point to test something.
    4   Show FPS - toggles whether the current frames per second is displayed
        on the screen. Pass DATA as 1 to turn this on, 0 to turn it off.
    5   Character Walk Path - shows walking path for the character.
        Pass DATA matching desired Character ID. 
        Call Debug(5,n) again to toggle it off.

_Compatibility:_ Using Debug(2,n) for different masks and Debug(5,n) as toggle is only supported by **AGS 3.6.0** and later versions.

*See also:* [Debugging features](Debuggingfeatures),
[`System.RuntimeInfo`](System#systemruntimeinfo),
[`System.DisplayFPS`](System#systemdisplayfps)

---

### `DeleteSaveSlot`

```ags
DeleteSaveSlot (int slot)
```

Deletes the save game in save slot number SLOT. If there were no save then nothing happens.

Example 1:

```ags
DeleteSaveSlot (130);
```

deletes save game slot 130 (which we should have saved earlier).

Example 2:

```ags
for (int i = 999; i >= 0; i--)
{
    DeleteSaveSlot(i);
}
```

Deletes all the saves.

**IMPORTANT:** before **AGS 3.6.2**, when deleting slots in the range of 1 to 50, this function used to move the highest slot found in that range to fill the freed slot. For example, if there were slots 1, 2, 3, 4, 5 and DeleteSaveSlot removes slot 3, then save in slot 5 will be renamed to become slot 3. This behavior has since been removed!

Example 3:

```ags
void DeleteSaveSlotAndPull(int slot)
{
  DeleteSaveSlot(slot);
  int maxSaveGames = 50;
  for (int i = maxSaveGames; i > slot; i--) {
    if(Game.GetSaveSlotDescription(i) != null) {
      // pulls the highest save game to fill in the gap
      MoveSaveSlot(i, slot);
      break;
    }
  }
}
```

This reproduces the old behavior of DeleteSaveSlot that has been removed.

*Compatibility:* Since **AGS 3.6.2** and later versions, this function no longer moves the highest slot found in 50-1 range to fill the deleted slot.

*See also:* [`RestoreGameSlot`](Globalfunctions_General#restoregameslot),
[`SaveGameSlot`](Globalfunctions_General#savegameslot),
[`CopySaveSlot`](Globalfunctions_General#copysaveslot),
[`MoveSaveSlot`](Globalfunctions_General#movesaveslot)

---

### `DisableInterface`

```ags
DisableInterface ()
```

Disables the player interface. This has two main effects:
- all GUI (both parent and controls) will be disabled and not react on mouse events.
- mouse clicks will not be sent through to the \"on_mouse_click\" function.

While disabled, GUIs may change their looks depending on the setting ["When player interface is disabled, GUI should"](GeneralSettings#visual).

**NOTE:** AGS keeps a count of the number of times DisableInterface is
called. Every call to DisableInterface must be matched by a later call
to EnableInterface, otherwise the interface will get permanently
disabled.

Example:

```ags
DisableInterface();
```

will disable the user's interface.

*See also:* [`EnableInterface`](Globalfunctions_General#enableinterface),
[`IsInterfaceEnabled`](Globalfunctions_General#isinterfaceenabled)

---

### `EnableInterface`

```ags
EnableInterface ()
```

Re-enables the player interface, which was previously disabled with the
DisableInterface function. Everything which was disabled is returned to
normal.

Example:

```ags
EnableInterface();
```

will enable the user's interface.

*See also:* [`DisableInterface`](Globalfunctions_General#disableinterface),
[`IsInterfaceEnabled`](Globalfunctions_General#isinterfaceenabled)

---

### `EndCutscene`

```ags
EndCutscene()
```

Marks the end of a cutscene. If the player skips the cutscene, the game
will fast-forward to this point. This function returns 0 if the player
watched the cutscene, or 1 if they skipped it.

*See also:* [`SkipCutscene`](Globalfunctions_General#skipcutscene), [`StartCutscene`](Globalfunctions_General#startcutscene),
[`Game.InSkippableCutscene`](Game#gameinskippablecutscene),
[`Game.SkippingCutscene`](Game#gameskippingcutscene)

---

### `GetFontHeight`

```ags
int GetFontHeight (int font)
```

Returns the given font's height, in pixels. This value may be used, for
example, to calculate arrangement of text and GUI elements on screen.

Example:

```ags
int h = GetFontHeight(eFontSpeech);
```

will store the speech font's height in the variable.

*Compatibility:* Supported by **AGS 3.4.1** and later versions.

*See also:* [`GetFontLineSpacing`](Globalfunctions_General#getfontlinespacing)

---

### `GetFontLineSpacing`

```ags
int GetFontLineSpacing (int font)
```

Returns the step between two lines of text for the specified font. If
this value equals font's height, then each next line is rendered right
after previous one with no space in between. If the line spacing is
lower than font's height, then the lines of text are partially
overlapping.

**NOTE:** this is the distance between the **top** of the first line and
the **top** of the next line, and **not** distance between bottom of
first line and top of next one. If you need to calculate the **gap**
between the lines, then subtract [font's height](Globalfunctions_General#getfontheight) from
the line spacing value.

Example:

```ags
int h = GetFontHeight(eFontSpeech);
int spacing = GetFontLineSpacing(eFontSpeech);
int gap = spacing - h;
```

will calculate the gap between two lines of text, that are drawn using
speech font.

*Compatibility:* Supported by **AGS 3.4.1** and later versions.

*See also:* [`GetFontHeight`](Globalfunctions_General#getfontheight)

---

### `GetGameOption`

```ags
GetGameOption (option)
```

Gets the current setting of one of the game options, originally set in
the AGS Editor Game Settings pane.

OPTION specifies which option to get, and its current value is returned.

The valid values for settable OPTIONs are listed in
[`SetGameOption`](Globalfunctions_General#setgameoption).

Following is the list of **READ-ONLY** options:

Option | Values
--- | ---
OPT_DEBUGMODE | Tells if the game was compiled in Debug mode (0 or 1). See also: [`game.debug_mode`](Gamevariables).
OPT_TWCUSTOM | Which GUI number to use when displaying text windows. For changing this option use [`SetTextWindowGUI`](Globalfunctions_General#settextwindowgui).
OPT_LETTERBOX | Tells if the game is run in deprecated "letterbox" mode.
OPT_GUIALPHABLEND | Tells which blending algorithm is used when drawing translucent GUI and controls over each other (0=classic, 1=additive, 2=proper).
OPT_NATIVECOORDINATES | Tells if the game uses coordinates in its true resolution (1), or low-res coordinates (0). **OBSOLETE**, and for diagnostic purposes only.
OPT_SPRITEALPHABLEND | Tells which blending algorithm is used when drawing images on DrawingSurface (0=classic, 1=proper).
OPT_DIALOGOPTIONSAPI | Tells the API version of [Custom Dialog options Rendering](CustomDialogOptions).
OPT_BASESCRIPTAPI | Tells the Script API version this game was compiled with.
OPT_SCRIPTCOMPATLEV | Tells the Script Compatibility level this game was compiled with.
OPT_CLIPGUICONTROLS | Tells if the GUI controls clip their contents when drawn.
OPT_GAMETEXTENCODING | Tells the text encoding the game was compiled in. Value corresponds to the code page number (65001=utf-8, other=ASCII or ANSI mode).
OPT_KEYHANDLEAPI | Tells the API version of the key handling this game was compiled with.

Example:

```ags
if (GetGameOption(OPT_PIXELPERFECT) == 1) {
    Display("pixel-perfect click detection is on!");
}
```

*See also:* [`SetGameOption`](Globalfunctions_General#setgameoption)

---

### `GetGameParameter`

The *GetGameParameter* function is now obsolete.

It has been replaced with the following functions and properties:

[`Game.SpriteWidth`](Game#gamespritewidth) (was gp_spritewidth)<br>
[`Game.SpriteHeight`](Game#gamespriteheight) (was gp_spriteheight)<br>
[`Game.GetLoopCountForView`](Game#gamegetloopcountforview) (was
GP_NUMLOOPS)<br>
[`Game.GetFrameCountForLoop`](Game#gamegetframecountforloop) (was
GP_NUMFRAMES)<br>
[`Game.GetRunNextSettingForLoop`](Game#gamegetrunnextsettingforloop)
(was GP_ISRUNNEXTLOOP)<br>
[`Game.GetViewFrame`](Game#gamegetviewframe) (was GP_FRAMExxx,
GP_ISFRAMEFLIPPED)<br>
[`Game.GUICount`](Game#gameguicount) (was gp_numguis)<br>
[`Room.ObjectCount`](Room#roomobjectcount) (was gp_numobjects)<br>
[`Game.CharacterCount`](Game#gamecharactercount) (was
GP_NUMCHARACTERS)<br>
[`Game.InventoryItemCount`](Game#gameinventoryitemcount)(was
GP_NUMINVITEMS)

---

### `GetGameSpeed`

```ags
GetGameSpeed ()
```

Returns the current game speed (number of cycles per second).

Example:

```ags
if (GetGameSpeed() > 40) {
    SetGameSpeed(40);
}
```

will always keep the game speed at 40 cycles per second (in case the
user has raised it )

*See also:* [`SetGameSpeed`](Globalfunctions_General#setgamespeed)

---

### `GetGlobalInt`

```ags
GetGlobalInt (int index)
```

Returns the value of global int INDEX.

**NOTE:** GlobalInts are now considered obsolete. Consider using
[global variables](GlobalVariables) instead, which allow you to
name the variables.

Example:

```ags
if (GetGlobalInt(20) == 1) {
    // code here
}
```

will execute the code only if Global Integer 20 is 1.

*See also:* [Global variables](GlobalVariables),
[`SetGlobalInt`](Globalfunctions_General#setglobalint),
[`Game.GlobalStrings`](Game#gameglobalstrings)

---

### `GetGraphicalVariable`

```ags
GetGraphicalVariable (string variable_name);
```

Returns the value of the interaction editor VARIABLE_NAME variable.
This allows your script to access the values of variables set in the
interaction editor.

**NOTE:** This command is obsolete, and is only provided for backwards
compatibility with AGS 2.x. When writing new code, use
[global variables](GlobalVariables) instead.

Example:

```ags
if (GetGraphicalVariable("climbed rock")==1)
{
    // Code here.
}
```

will execute the code only if interaction variable \"climbed rock\" is
1.

*See also:* [Global variables](GlobalVariables),
[`GetGlobalInt`](Globalfunctions_General#getglobalint),
[`SetGraphicalVariable`](Globalfunctions_General#setgraphicalvariable)

---

### `GetLocationType`

```ags
LocationType GetLocationType(int x, int y)
```

Returns what type of room thing is seen under the given screen coordinates (x, y): whether it is a character, object, hotspot or nothing at all. This may be useful, for example, if you want to process a mouse click differently depending on what the player clicks on.

It's important to know that this will work only if there's a room viewport found on screen at that point, otherwise this function will fail and return "nothing".

Also, this function is "blocked" by any interactable non-room object, such as GUI, and will return "nothing" as well if one is found under these screen coordinates.

The value returned is one of the following:

    eLocationNothing    nothing, GUI or inventory
    eLocationHotspot    a hotspot
    eLocationCharacter  a character
    eLocationObject     an object

**NOTE:** The co-ordinates are SCREEN co-ordinates, NOT ROOM co-ordinates. This means that this function is suitable for use with the mouse cursor position variables.

**NOTE:** When looking up for an object under coordinates, GetLocationType is affected by the game setting ["Pixel-perfect click detection"](GeneralSettings#visual). It's possible to change this behavior in script by changing OPT_PIXELPERFECT option (see [SetGameOption](Globalfunctions_General#setgameoption)).

Example:

```ags
if (GetLocationType(mouse.x,mouse.y) == eLocationCharacter)
    mouse.Mode = eModeTalk;
```

will set the cursor mode to talk if the cursor is over a character.

*See also:* [`Hotspot.GetAtScreenXY`](Hotspot#hotspotgetatscreenxy),
[`Game.GetLocationName`](Game#gamegetlocationname),
[`Object.GetAtScreenXY`](Object#objectgetatscreenxy)

---

### `GetPlayerCharacter`

```ags
GetPlayerCharacter ()
```

**THIS COMMAND IS NOW OBSOLETE.**<br>
The recommended replacement is to use the player character's ID
property, as follows:

Example:

```ags
Display("The player character number is %d", player.ID);
```

*See also:* [`Character.ID`](Character#characterid)

---

### `GetTextHeight`

```ags
GetTextHeight(string text, FontType font, int width)
```

Calculates the height on the screen that drawing TEXT in FONT within an
area of WIDTH would take up.

This allows you to work out how tall a message displayed with a command
like [`DrawMessageWrapped`](DrawingSurface#drawingsurfacedrawmessagewrapped)
will be. WIDTH is the width of the area in which the text will be
displayed.

The height is returned in screen pixels, so it can be
used with the screen display commands.

**NOTE:** There's a historical mistake in this function, which was kept for backwards compatibility until today. When GetTextHeight calculates line wrapping within a given width, it reduces "width" parameter by 1. For a correct result, pass "width + 1" instead (where "width" is the value that you like to have).

Example:

```ags
int height = GetTextHeight("The message on the GUI!", Game.NormalFont, 100);
gBottomLine.SetPosition(0, 200 - height);
```

will move the BOTTOMLINE GUI so that it can display the text within the
screen.

*See also:* [`GetTextWidth`](Globalfunctions_General#gettextwidth),
[`DrawingSurface.DrawString`](DrawingSurface#drawingsurfacedrawstring)

---

### `GetTextWidth`

```ags
GetTextWidth(string text, FontType font)
```

Returns the width on the screen that drawing TEXT in FONT on one line
would take up.

This could be useful if you manually need to center or right-align some
text, for example with the raw drawing routines.

The width is returned in screen pixels, so it can be used
with the screen display commands.

Example:

```ags
DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
surface.DrawingColor = 14;
int width = GetTextWidth("Hello!", Game.NormalFont);
surface.DrawString(160 - (width / 2), 100, Game.NormalFont, "Hello!");
surface.Release();
```

will print \"Hello!\" onto the middle of the background scene.

*See also:* [`GetTextHeight`](Globalfunctions_General#gettextheight),
[`DrawingSurface.DrawString`](DrawingSurface#drawingsurfacedrawstring)

---

### `GetTimerPos`

```ags
int  GetTimerPos(int timerID)
```

Returns the current time value for the specified timer `timerID`.
It will return 0 if the timer is not running, and 1 if it's expiring.

This function will throw an error if `timerID` is invalid, i.e., less than 1 or greater than 20.

**NOTE:** the time left will return 1 until a call to `IsTimerExpired` is done on the same `timerID`, 
or alternatively, `SetTimer(timerID, 0)` is used to stop the timer.

Example:

```ags
#define TIMER_ID 1

function room_AfterFadeIn()
{
    SetTimer(TIMER_ID, 10);
}

function room_RepExec()
{
    int timeLeft = GetTimerPos(TIMER_ID);
    if (timeLeft == 0)
    {
        System.Log(eLogInfo, "Timer %d is not running", TIMER_ID);
    }
    else if (timeLeft == 1)
    {
        System.Log(eLogInfo, "Timer %d is about to expire!", TIMER_ID);
        // since we don't use IsTimerExpired, we must stop the timer ourselves
        // or the timer will be kept at 1 tick value
        SetTimer(TIMER_ID, 0);
    }
}
```

This will wait 10 game ticks and then log that the timer is about to expire
and keep logging that the timer is not running.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:* [`SetTimer`](Globalfunctions_General#settimer),
[`IsTimerExpired`](Globalfunctions_General#istimerexpired)

---

### `GetTranslation`

```ags
const string GetTranslation(string original)
```

Gets the translated equivalent of the supplied string. You do not
normally need to use this since the game translates most things for you.
However, if you have used an InputBox or other form of user input, and
want to compare the user's input to a particular string, it cannot be
translated automatically. So, you can do this instead.

Example:

```ags
String buffer = Game.InputBox("Enter the password:");
if (buffer.CompareTo(GetTranslation("secret")) == 0) {
    // it matched the current translation of "secret"
}
```

If there is no translation for the supplied string, it will be returned
unchanged, so it is always safe to use this function.

*See also:*
[`Game.ChangeTranslation`](Game#gamechangetranslation),
[`Game.TranslationFilename`](Game#gametranslationfilename),
[`IsTranslationAvailable`](Globalfunctions_General#istranslationavailable),
[Translation Manual](Translations)

---

### `GiveScore`

```ags
GiveScore (int score)
```

Adds SCORE to the player's score. The accumulated score is stored in `game.score` variable, from which it may be read to know current score.
Using GiveScore function is preferable to directly modifying the variable since it will play the score sound, update any
status lines and call the [on_event](Globalfunctions_Event#on_event) script callback with eEventGotScore event parameter.

Note that SCORE can be negative, in which case the score sound is NOT played.

Example:

```ags
GiveScore(5);
```

will give 5 points to the player's score.

*See also:* [Game variables](Gamevariables#game-variables), [`Game.DoOnceOnly`](Game#gamedoonceonly)

---

### `InventoryScreen`

```ags
InventoryScreen ()
```

This command is obsolete.

**This command was used for displaying a default inventory window in
previous versions of AGS, but is no longer supported.**

Instead of using this command, you should create your own Inventory GUI.

---

### `IsGamePaused`

```ags
IsGamePaused ()
```

Returns *true* if the game is currently paused, or *false* otherwise.
The game is paused when either the icon bar interface has been popped
up, or a \"script-only\" interface has been displayed with
GUI.Visible=true. While the game is paused, no animations or other
updates take place.

Example:

```ags
if (IsGamePaused()) UnPauseGame();
```

will unpause the game if it's paused.

*See also:* [`PauseGame`](Globalfunctions_General#pausegame), [`UnPauseGame`](Globalfunctions_General#unpausegame),
[`GUI.Visible`](GUI#guivisible)

---

### `IsInteractionAvailable`

```ags
int IsInteractionAvailable(int x, int y, int mode)
```

Checks whether there is an interaction defined inside a room for clicking on the screen at (X, Y) in cursor mode MODE.
Please note that x and y are *screen* coordinates, not room coordinates.

This function is very similar to Room.ProcessClick, except that rather than carry out any interactions it encounters, it simply returns 1 if something would have happened, or 0 if unhandled_event would have been run.

Function will fail and return 0 if there's no room viewport on screen at the given coordinates. On the other hand it ignores any non-room objects such as GUI, and "clicks through" any GUI that covers room at this location.

This function is useful for enabling options on a verb-coin style GUI, for example.

**NOTE:** When looking up for an object under coordinates, IsInteractionAvailable is affected by the game setting ["Pixel-perfect click detection"](GeneralSettings#visual). It's possible to change this behavior in script by changing OPT_PIXELPERFECT option (see [SetGameOption](Globalfunctions_General#setgameoption)).

Example:

```ags
if (IsInteractionAvailable(mouse.x, mouse.y, eModeLookat) == 0)
    Display("looking here would not do anything.");
```

*See also:*
[`InventoryItem.IsInteractionAvailable`](InventoryItem#inventoryitemisinteractionavailable),
[`Hotspot.IsInteractionAvailable`](Hotspot#hotspotisinteractionavailable),
[`Object.IsInteractionAvailable`](Object#objectisinteractionavailable),
[`Character.IsInteractionAvailable`](Character#characterisinteractionavailable),
[`Room.ProcessClick`](Room#roomprocessclick)

---

### `IsInterfaceEnabled`

```ags
IsInterfaceEnabled()
```

Returns 1 if the player interface is currently enabled, 0 if it is
disabled. There are following reasons why it may be disabled:
- DisableInterface function was called in script;
- the game is currently running any blocking action (such as Display, Say or Walk);
- the game is in a certain state, such as displaying dialog options.

Example:

```ags
if (IsInterfaceEnabled())
    DisableInterface();
```

will disable the user interface if it's enabled.

*See also:* [`DisableInterface`](Globalfunctions_General#disableinterface),
[`EnableInterface`](Globalfunctions_General#enableinterface)

---

### `IsKeyPressed`

```ags
IsKeyPressed(eKeyCode)
```

Tests whether the supplied key on the keyboard is currently pressed down
or not. You could use this to move an object while the player holds an
arrow key down, for instance.

KEYCODE is one of the [Key codes](Keycodes), with some
limitations: since it tests the raw state of the key, you CANNOT pass
the Ctrl+(A-Z) or Alt+(A-Z) codes (since they are key combinations). You
can, however, use some extra codes which are listed at the bottom of the
section.

Returns 1 if the key is currently pressed, 0 if not.

**NOTE:** The numeric keypad can have inconsistent keycodes between
IsKeyPressed and on_key_press. With IsKeyPressed, the numeric keypad
always uses keycodes in the 370-381 range. on_key_press, however,
passes different values if Num Lock is on since the key presses are
interpreted as the number key rather than the arrow key.

Example:

```ags
if (IsKeyPressed(eKeyUpArrow))
    cEgo.Walk(cEgo.x, cEgo.y+3);
```

will move the character EGO upwards 3 pixels when the up arrow is
pressed.

*See also:* [`Mouse.IsButtonDown`](Mouse#mouseisbuttondown)

---

### `IsTimerExpired`

```ags
bool IsTimerExpired(int timer_id)
```

Checks whether the timer TIMER_ID has expired. If the timeout set with
SetTimer has elapsed, returns *true*. Otherwise, returns *false*.

Note that this function will only return *true* once - after that, the
timer is placed into an OFF state where it will always return *false*
until restarted.

Example:

```ags
if (IsTimerExpired(1)) {
    Display("Timer 1 expired");
}
```

will display a message when timer 1 expires.

*See also:* [`SetTimer`](Globalfunctions_General#settimer),
[`GetTimerPos`](Globalfunctions_General#gettimerpos)

---

### `IsTranslationAvailable`

```ags
IsTranslationAvailable ()
```

Finds out whether the player is using a game translation or not.

Returns 1 if a translation is in use, 0 if not.

*See also:*
[`Game.ChangeTranslation`](Game#gamechangetranslation),
[`Game.TranslationFilename`](Game#gametranslationfilename),
[`GetTranslation`](Globalfunctions_General#gettranslation),
[Translation Manual](Translations)

---

### `MoveCharacterToHotspot`

**This function is now obsolete. Use Character.Walk instead**

```ags
MoveCharacterToHotspot (CHARID, int hotspot)
```

Moves the character CHARID from its current location to the walk-to
point for the specified hotspot. If the hotspot has no walk-to point,
nothing happens.

This is a blocking call - control is not returned to the script until
the character has reached its destination.

Example:

```ags
MoveCharacterToHotspot(EGO,6);
```

will move the character EGO to the hotspot's 6 \"walk to point\".

*See also:* [`Hotspot.WalkToX`](Hotspot#hotspotwalktox),
[`Hotspot.WalkToY`](Hotspot#hotspotwalktoy),
[`Character.Walk`](Character#characterwalk),
[`MoveCharacterToObject`](Globalfunctions_General#movecharactertoobject)

---

### `MoveCharacterToObject`

**This function is now obsolete. Use Character.Walk instead**

```ags
MoveCharacterToObject (CHARID, int object)
```

Moves the character CHARID from its current location to a position just
below the object OBJECT. This is useful for example, if you want the man
to pick up an object. This is a blocking call - control is not returned
to the script until the character has reached its destination.

Example:

```ags
MoveCharacterToObject (EGO, 0);
object[0].Visible = false;
```

Will move the character EGO below object number 0, then turn off object 0.

*See also:* [`Character.Walk`](Character#characterwalk),
[`MoveCharacterToHotspot`](Globalfunctions_General#movecharactertohotspot)

---

### `MoveSaveSlot`

```ags
void MoveSaveSlot (int old_slot, int new_slot)
```

Moves the save game from one slot to another, overwriting a save if one was already present at a new slot.
This is the same as renaming the file used by the old slot with the name it would have (or that has),
the file in the new slot.

Example:

```ags
MoveSaveSlot(2, 5);
```

Moves the save slot 2 (which we should have saved earlier), over the save slot 5.
The save slot 2 won't be found anymore in its previous slot.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:* [`CopySaveSlot`](Globalfunctions_General#copysaveslot),
[`DeleteSaveSlot`](Globalfunctions_General#deletesaveslot),
[`SaveGameSlot`](Globalfunctions_General#savegameslot),
[`RestoreGameSlot`](Globalfunctions_General#restoregameslot),
[`SaveGameDialog`](Globalfunctions_General#savegamedialog)

---

### `PauseGame`

```ags
PauseGame ()
```

Stops AGS processing movement and animations. This has the same effect on the game as happens when a modal GUI is popped up.

When the game is paused, game cycles will continue to run but no animations or movement will be performed, and timers will not count down. Apart from that, your scripts will continue to run as normal.

To be precise, following is paused by this function:
* Timers (ones set by [SetTimer](Globalfunctions_General#settimer)),
* Character walking and animating, idle view timing,
* Object moving and animating,
* Overlay timing (for auto removal),
* Speech timing and animation.

As you may notice, GUI and Audio are not paused at all. PauseGame was historically purposed to pause the Room, while having some kind of the GUI menus and background music running. If you want to also suspend (and later unsuspend) these, you would have to script that yourself.

**NOTE:** `PauseGame()` works as a counter, so if you call it twice, you will need to call `UnPauseGame()` game twice too to resume game. To avoid this behavior make sure to only pause once:

```ags
if (!IsGamePaused()) PauseGame();
```

Game processing will not resume until you call the UnPauseGame function as needed.

Example:

```ags
if (IsKeyPressed(32)) PauseGame();
```

will pause the game if the player presses the space bar

*See also:* [`UnPauseGame`](Globalfunctions_General#unpausegame), [`IsGamePaused`](Globalfunctions_General#isgamepaused)

---

### `QuitGame`

```ags
QuitGame(int ask_first)
```

Exits the game and returns to the operating system.

If ASK_FIRST is zero, it will exit immediately. If ASK_FIRST is not
zero, it will first display a message box asking the user if they are
sure they want to quit.

Example:

```ags
QuitGame(0);
```

will quit the game without asking the player to confirm.

*See also:* [`AbortGame`](Globalfunctions_General#abortgame)

---

### `Random`

```ags
Random (int max)
```

Returns a random number between 0 and MAX. This could be useful to do
various effects in your game. MAX must be a positive value in range
0-32767.

**NOTE:** Because of the way Random is implemented in AGS, the return
value will never be higher than 32767.

**NOTE:** The range returned is inclusive - i.e. if you do Random(3);
then it can return 0, 1, 2 or 3.

Example:

```ags
int ran=Random(2);
if (ran==0) cEgo.ChangeRoom(1);
else if (ran==1) cEgo.ChangeRoom(2);
else cEgo.ChangeRoom(3);
```

will change the current room to room 1,2 or 3 depending on a random
result.

---

### `RestartGame`

```ags
RestartGame ()
```

Restarts the game from the beginning.

Example:

```ags
if (IsKeyPressed(365)) RestartGame();
```

will restart the game if the player presses the F7 key.

*SeeAlso:* [`SetRestartPoint`](Globalfunctions_General#setrestartpoint)

---

### `RestoreGameDialog`

```ags
RestoreGameDialog (int min_slot = 1, int max_slot = 100)
```

Displays the restore game dialog, where the player can select a
previously saved game position to restore.

You can also pass `min_slot` and `max_slot` to select the range of save slots
that will be shown in the dialog.

The dialog is not displayed immediately; instead, it will be displayed
when the script function finishes executing.

Example:

```ags
if (IsKeyPressed(363)) RestoreGameDialog();
```

will bring up the restore game dialog if the player presses the F5 key.

*Compatibility:* Optional *min_slot* and *max_slot* parameters are supported only by **AGS 3.6.2** and later versions.

*See also:* [`RestoreGameSlot`](Globalfunctions_General#restoregameslot),
[`SaveGameDialog`](Globalfunctions_General#savegamedialog)

---

### `RestoreGameSlot`

```ags
RestoreGameSlot (int slot)
```

Restores the game position saved into slot number SLOT. If this slot number does not exist, an error message is displayed to
the player but the game continues. To avoid the error, use the
GetSaveSlotDescription function to see if the position exists before
restoring it.

**NOTE:** The game will not be restored immediately; instead, it will be
restored when the script function finishes executing.

Example:

```ags
RestoreGameSlot(30);
```

will restore game slot 30 if this slot number exists.

*See also:*
[`Game.GetSaveSlotDescription`](Game#gamegetsaveslotdescription),
[`RestoreGameDialog`](Globalfunctions_General#restoregamedialog),
[`SaveGameSlot`](Globalfunctions_General#savegameslot)

---

### `RunAGSGame`

```ags
RunAGSGame (string filename, int mode, int data)
```

Quits the current game, and loads up FILENAME instead. FILENAME must be
an AGS game EXE or AC2GAME.AGS file, and it must be in the current
directory.

MODE specifies various options about how you want to run the game.
Currently, the supported values are:

    0   Current game is completely exited, new game runs as if it had been launched separately
    1   GlobalInt values are preserved and are not set to 0 for the new game.

DATA allows you to pass an integer through to the next game. The value
you pass here will be accessible to the loaded game by it reading the
game.previous_game_data variable.

The save game slots are shared between the two games, and if you load a
save slot that was saved in the other game, it will automatically be
loaded.

Bear in mind that because the games must be in the same folder, they
will also share the audio.vox, speech.vox and so forth. This is a
limitation of this command.

**NOTE:** The game you run will be loaded at the same resolution and
color depth as the current game; if you mismatch color depths some
nasty results will occur.

**NOTE:** The game you want to launch must have been created with the
same point-version of AGS as the one you are launching it from. (version
2.xy - the X must be the same version between the two games).

Example:

```ags
RunAGSGame ("MyGame.exe", 0, 51);
```

will run the MyGame game, passing it the value 51.

---

### `SaveGameDialog`

```ags
SaveGameDialog (int min_slot = 1, int max_slot = 100)
```

Displays the save game dialog, where the player can save their current
game position. If they select to save, then the game position will be
saved.

You can also pass `min_slot` and `max_slot` to select the range of save slots
that will be shown in the dialog.

**NOTE:** The dialog will not be displayed immediately; instead, it will
be shown when the script function finishes executing.

Example:

```ags
if (keycode == 361) SaveGameDialog();
```

will bring up the save game dialog if the player presses the F3 key.

*Compatibility:* Optional *min_slot* and *max_slot* parameters are supported only by **AGS 3.6.2** and later versions.

*See also:* [`RestoreGameDialog`](Globalfunctions_General#restoregamedialog),
[`SaveGameSlot`](Globalfunctions_General#savegameslot)

---

### `SaveGameSlot`

```ags
SaveGameSlot (int slot, string description, optional int sprite)
```

Saves the current game position to the save game number specified by
SLOT, using DESCRIPTION as the textual description of the save position.
Be careful using this function, because you could overwrite one of the
player's save slots if you aren't careful.

You can optionally pass a number of an arbitrary sprite to write into this
save, instead of a standard screenshot.

**NOTE:** The game will not be saved immediately; instead, it will be
saved when the script function finishes executing.

Example:

```ags
SaveGameSlot(30, "save game");
```

will save the current game position to slot 30 with the description
\"Save game\".

*Compatibility:* Optional *sprite* parameter is supported only by **AGS 3.6.2** and later versions.

*See also:* [`DeleteSaveSlot`](Globalfunctions_General#deletesaveslot),
[`RestoreGameSlot`](Globalfunctions_General#restoregameslot),
[`SaveGameDialog`](Globalfunctions_General#savegamedialog)

---

### `SaveScreenShot`

```ags
SaveScreenShot (string filename)
```

Takes a screen capture and saves it to disk. The FILENAME must end in
either \".BMP\" or \".PCX\", as those are the types of files which can
be saved. Returns 1 if the shot was successfully saved, or 0 if an
invalid file extension was provided.

**NOTE:** The screenshot will be saved to the Saved Games folder.

Example:

```ags
String input = Game.InputBox("Type the filename:");
String filename = String.Format("%s.pcx", input);
SaveScreenShot(filename);
```

will prompt the player for a filename and then save the screenshot with
the filename the player typed.

*See also:*
[`DynamicSprite.SaveToFile`](DynamicSprite#dynamicspritesavetofile)

---

### `SetAmbientLightLevel`

```ags
void SetAmbientLightLevel(int light_level);
```

Sets an ambient light level that affects all objects and characters in
the room that have their UseRoomAreaLighting property set to true.

The light level is from **-100 to 100**, where 0 means that no
adjustment will be applied to sprites.

In 8-bit games you cannot use positive light level for brightening
effect, but you may still use negative values to produce darkening
effect.

To turn light level off, call this command again but pass the
*light_level* as 0.

**NOTE:** This function overrides any specific region light levels or
tints on the screen, but does NOT override individual character and
object light levels.

**NOTE:** Setting an ambient light level will disable ambient RGB tint,
if there was one previously set.

Example:

```ags
SetAmbientLightLevel(50);
```

will apply light level 50 to every character and object on screen (which
do not have individual light levels).

*Compatibility:* Supported by **AGS 3.4.0** and later versions.

*See also:* [`SetAmbientTint`](Globalfunctions_General#setambienttint),
[`Character.SetLightLevel`](Character#charactersetlightlevel),
[`Object.SetLightLevel`](Object#objectsetlightlevel),
[`Region.LightLevel`](Region#regionlightlevel)

---

### `SetAmbientTint`

```ags
SetAmbientTint(int red, int green, int blue, int saturation, int luminance)
```

Tints all objects and characters on the screen that have their UseRoomAreaLighting property set to true to (RED, GREEN, BLUE)
with SATURATION percent saturation.

This allows you to apply a global tint to everything on the screen. The
RED, GREEN and BLUE parameters are from 0-255, and specify the color of
the tint.

The SATURATION parameter defines how much the tint is applied, and is
from 0-100. A saturation of 100 will completely re-colorize the sprites
to the supplied color, and a saturation of 1 will give them a very
minor tint towards the specified color.

The LUMINANCE parameter allows you to adjust the brightness of the
sprites at the same time. It ranges from 0-100. Passing 100 will draw
the sprites at normal brightness. Lower numbers will darken the images
accordingly, right down to 0 which will draw everything black.

The tint applied by this function is global. To turn it off, call this
command again but pass the saturation as 0.

**NOTE:** This function only works in hi-color games and with hi-color
sprites.

**NOTE:** This function overrides any specific region light levels or
tints on the screen.

Example:

```ags
SetAmbientTint(0, 0, 250, 30, 100);
```

will tint everything on the screen with a hint of blue.

*See also:* [`SetAmbientLightLevel`](Globalfunctions_General#setambientlightlevel),
[`Character.Tint`](Character#charactertint),
[`Object.Tint`](Object#objecttint),
[`Region.Tint`](Region#regiontint)

---

### `SetGameOption`

```ags
SetGameOption (option, int value)
```

Changes one of the game options, originally set in the AGS Editor Game Settings pane.

OPTION specifies which option to change, and VALUE is its new value.

Valid *modifiable* OPTIONs are listed below:

Option | Values
--- | ---
OPT_WALKONLOOK | Walk to hotspot in look mode (0 or 1)
OPT_DIALOGOPTIONSGUI | Dialog options on GUI (0=none, otherwise GUI name/number)
OPT_DIALOGOPTIONSGAP | Pixel gap between options (0=none, otherwise num pixels)
OPT_WHENGUIDISABLED | When GUI is disabled, 0=grey out, 1=go black, 2=unchanged, 3=turn off
OPT_ALWAYSSPEECH | Always display text as speech (0 or 1)
OPT_PIXELPERFECT | Pixel-perfect click detection (0 or 1)
OPT_NOWALKMODE | Don't automatically move character in Walk mode (0 or 1)
OPT_FIXEDINVCURSOR | Don't use inventory graphics as cursors (0 or 1)
OPT_TURNBEFOREWALK | Characters turn before walking (0 or 1)
OPT_HANDLEINVCLICKS | Handle inventory clicks in script (0 or 1)
OPT_MOUSEWHEEL | Enable mouse wheel support (0 or 1)
OPT_DIALOGNUMBERED | Number dialog options (-1=disabled, 0=shortcuts only, 1=drawn numbers)
OPT_DIALOGUPWARDS | Dialog options go upwards on GUI (0 or 1)
OPT_CROSSFADEMUSIC | Crossfade music tracks (0=no, 1=slow, 2=slow-ish, 3=medium, 4=fast). **OBSOLETE**, reliably affects only old-style audio functions, such as `PlayMusic`. Configure [Audio Types](MusicAndSound#audio-in-the-editor) instead.
OPT_ANTIALIASFONTS | Anti-alias rendering of TTF fonts (0 or 1)
OPT_THOUGHTGUI | Thought uses bubble GUI (GUI name/number)
OPT_TURNWHENFACING | Characters turn to face direction (0 or 1)
OPT_LIPSYNCTEXT | Whether lip-sync text reading is enabled (0 or 1)
OPT_RIGHTTOLEFT | Right-to-left text writing (0 or 1)
OPT_MULTIPLEINV | Display multiple inv items multiple times (0 or 1)
OPT_SAVEGAMESCREENSHOTS | Save screenshots into save games (0 or 1)
OPT_PORTRAITPOSITION | Speech portrait side (0=left, 1=right, 2=alternate, 3=xpos)
OPT_RUNGAMEINDLGOPTS | Run game loops while dialog options are displayed  (0 or 1)
OPT_WALKSPEEDABSOLUTE | Whether character and object moving speeds depend on relative walkable mask's resolution (0=scale with mask resolution, 1=always in room resolution).
OPT_SCALECHAROFFSETS | Character's offset properties (such as [`Character.z`](Character#characterz)) are scaled with the character's Scaling (0 or 1).
OPT_SAVEGAMESCREENSHOTLAYER | The layer to select when savingsave screenshots into save game ([`RenderLayer`](StandardEnums#renderlayer)).

The game settings which are not listed here either are read-only, deprecated and have a separate
command to change them (such as Speech.Style), or unusable in the contemporary engine.

For the list of read-only options see [`GetGameOption`](Globalfunctions_General#getgameoption).

This command returns the old value of the setting.

Example:

```ags
SetGameOption (OPT_PIXELPERFECT, 0);
```

will disable pixel-perfect click detection.

*See also:* [`GetGameOption`](Globalfunctions_General#getgameoption),
[`Speech.Style`](Speech#speechstyle),
[`SetTextWindowGUI`](Globalfunctions_General#settextwindowgui)

---

### `SetGameSpeed`

```ags
SetGameSpeed (int new_speed)
```

Sets the maximum game frame rate to NEW_SPEED frames per second, or as
near as possible to that speed. The default frame rate is 40 fps, but
you can speed up or slow down the game by using this function. Note that
this speed is also the rate at which the [repeatedly_execute](RepExec) functions
are triggered.

The NEW_SPEED must lie between 10 and 1000. If it does not, it will be
rounded to 10 or 1000. Note that if you set a speed which the player's
computer cannot handle, then it will go as fast as possible.

NOTE: Because the mouse cursor is repainted at the game frame rate, at
very low speeds, like 10 to 20 fps, the mouse will appear to be jumpy
and not very responsive.

NOTE: If you set the [`System.VSync`](System#systemvsync) property to
*true*, the game speed will be capped at the screen's refresh rate, so
you will be unable to set it higher than 60-85 (depending on the
player's screen refresh).

Example:

```ags
SetGameSpeed(60);
```

will set the game speed to 60 fps.

*See also:* [`GetGameSpeed`](Globalfunctions_General#getgamespeed)

---

### `SetGlobalInt`

```ags
SetGlobalInt (int index, int value)
```

Sets the global int INDEX to VALUE. You can then retrieve this value
from any other script using GetGlobalInt.

There are 500 available global variables, from index 0 to 499.

**NOTE:** GlobalInts are now considered obsolete. Consider using
[global variables](GlobalVariables) instead, which allow you to
name the variables.

Example:

```ags
SetGlobalInt(10,1);
```

will set the Global Integer 10 to 1.

*See also:* [Global variables](GlobalVariables),
[`GetGlobalInt`](Globalfunctions_General#getglobalint)

---

### `SetGraphicalVariable`

```ags
SetGraphicalVariable(string variable_name, int value);
```

Sets the interaction editor VARIABLE_NAME variable to VALUE. This
allows your script to change the values of variables set in the
interaction editor.

**NOTE:** This command is obsolete, and is only provided for backwards
compatibility with AGS 2.x. When writing new code, use
[global variables](GlobalVariables) instead.

Example:

```ags
SetGraphicalVariable("climbed rock", 1);
```

will set the interaction editor \"climbed rock\" variable to 1.

*See also:* [Global variables](GlobalVariables),
[`GetGraphicalVariable`](Globalfunctions_General#getgraphicalvariable)

---

### `SetMultitaskingMode`

```ags
SetMultitaskingMode (int mode)
```

Allows you to set what happens when the user switches away from your
game.

If MODE is 0 (the default), then if the user Alt+Tabs out of your game,
or clicks on another window, the game will pause and not continue until
they switch back into the game.

If MODE is 1, then the game will continue to run in the background if
the user switches away (useful if, for example, you are just making some
sort of jukebox music player with AGS).

Note that mode 1 does not work with some graphics cards in full-screen
mode, so you should only rely on it working when your game is run in
windowed mode.

**Cross-Platform Support**

Windows: **Yes**<br>
Linux: **Yes**<br>
MacOS: **Yes**

Example:

```ags
SetMultitaskingMode (1);
```

will mean that the game continues to run in the background.

---

### `SetRestartPoint`

```ags
SetRestartPoint ()
```

Changes the game restart point to the current position. This means that
from now on, if the player chooses the Restart Game option, it will
return here.

This function is useful if the default restart point doesn't work
properly in your game - just use this function to move it.

**NOTE:** The restart point cannot be set while a script is running \--
therefore, when you call this it will actually set the restart point at
the next game loop where there is not a blocking script running in the
background.

*SeeAlso:* [`RestartGame`](Globalfunctions_General#restartgame)

---

### `SetTextWindowGUI`

```ags
SetTextWindowGUI (int gui)
```

Changes the GUI used for text windows to the specified GUI. This
overrides the \"text windows use GUI\" setting in the editor.

You can pass -1 as the GUI number to go back to using the default white
text box.

Example:

```ags
SetTextWindowGUI (4);
```

will change `TextWindow` GUI 4 to be used for displaying text windows in future.

---

### `SetTimer`

```ags
SetTimer (int timer_id, int timeout)
```

Starts timer TIMER_ID ticking - it will tick once every game loop
(normally 40 times per second), until TIMEOUT loops, after which it will
stop. You can check whether the timer has finished by calling the
IsTimerExpired function.

Pass TIMEOUT as 0 to disable a currently running timer.

There are 20 available timers, with TIMER_IDs from 1 to 20.

**NOTE:** the timer will not tick while the game is paused.

Example:

```ags
SetTimer(1,1000);
```

will set the timer 1 to expire after 1000 game cycles.

Example 2:

When you have a hard time keeping track of the timers only by number you can use [Macros](Preprocessor#define) to replace the descriptive name with the time number everywhere the descriptive name is use. To better keep track of these macros you could put them on top of the global script.

```ags
#define Delay_CustomAnimation 1

SetTimer(Delay_CustomAnimation, 2000);
```

this "names" timer 1 and sets it to expire after 2000 game cycles

*See also:* [`IsTimerExpired`](Globalfunctions_General#istimerexpired),
[`GetTimerPos`](Globalfunctions_General#gettimerpos)

---

### `SkipCutscene`

```ags
SkipCutscene()
```

Explicitly commences skipping current cutscene. If game is not in a cutscene sequence or cutscene is already being skipped this function will do nothing.

SkipCutscene will work regardless of the StartCutscene parameters, but is most useful when you do not want to rely on built-in skipping controls and are coding your own. In the latter case make sure to start cutscene in **eSkipScriptOnly** mode to prevent any standard input interference.

Example:

```ags
if (Game.InSkippableCutscene && IsKeyPressed(eKeySpace)) {
    SkipCutscene();
}
```

will check if game is inside a cutscene, and if player has pressed a space bar then commands to skip it.

*Compatibility:* Supported by **AGS 3.5.0** and later versions.

*See also:* [`EndCutscene`](Globalfunctions_General#endcutscene), [`SkipUntilCharacterStops`](Globalfunctions_General#skipuntilcharacterstops), [`StartCutscene`](Globalfunctions_General#startcutscene), [`Game.InSkippableCutscene`](Game#gameinskippablecutscene),
[`Game.SkippingCutscene`](Game#gameskippingcutscene)

---

### `SkipUntilCharacterStops`

```ags
SkipUntilCharacterStops(CHARID)
```

Skips through the game until the specified character stops walking, a
blocking script runs, or a message box is displayed.

The purpose of this command is to mimic the functionality in games such
as The Longest Journey, where the player can press ESC to instantly get
the character to its destination. It serves as a handy feature to allow
you to give the player character a relatively slow walking speed,
without annoying the player by making them wait ages just to get from A
to B.

If the specified character is not moving when this function is called,
nothing happens.

Example: (in on_key_press)

```ags
if (keycode == eKeyEscape) SkipUntilCharacterStops(EGO);
```

This means that if the player presses ESC, the game will skip ahead
until EGO finishes moving, or is interrupted by a Display command or a
blocking cutscene.

*See also:* [`StartCutscene`](Globalfunctions_General#startcutscene)

---

### `StartCutscene`

```ags
StartCutscene(CutsceneSkipType)
```

Marks the start of a cutscene. Once your script passes this point, the
player can choose to skip a portion by pressing a key or the mouse
button. This is useful for things like introduction sequences, where you
want the player to be able to skip over an intro that they've seen
before.

The CutsceneSkipType determines how they can skip the cutscene:

    eSkipESCOnly
      by pressing ESC only
    eSkipAnyKey
      by pressing any key
    eSkipMouseClick
      by clicking a mouse button
    eSkipAnyKeyOrMouseClick
      by pressing any key or clicking a mouse button
    eSkipESCOrRightButton
      by pressing ESC or clicking the right mouse button
    eSkipScriptOnly
      only by calling SkipCutscene script function

**NOTE:** eSkipScriptOnly may be useful if you are coding your own cutscene skipping method in script and would like to disable built-in ones.

You need to mark the end of the cutscene with the EndCutscene command.

Be **very careful** with where you place the corresponding EndCutscene
command. The script **must** pass through EndCutscene in its normal run
in order for the skipping to work - otherwise, when the player presses
ESC the game could appear to hang.

*Compatibility:* eSkipScriptOnly cutscene mode is supported by **AGS 3.5.0** and later versions.

*See also:* [`EndCutscene`](Globalfunctions_General#endcutscene), [`SkipCutscene`](Globalfunctions_General#skipcutscene), [`SkipUntilCharacterStops`](Globalfunctions_General#skipuntilcharacterstops),
[`Game.InSkippableCutscene`](Game#gameinskippablecutscene),
[`Game.SkippingCutscene`](Game#gameskippingcutscene)

---

### `UpdateInventory`

```ags
UpdateInventory ()
```

Updates the on-screen inventory display. If you add or remove inventory
items manually (i.e. by using the InventoryQuantity array rather than the
AddInventory/LoseInventory functions), the display may not get updated.
In this case call this function after making your changes, to update
what is displayed to the player.

Note that using this function will reset the order that items are
displayed in the inventory window to the same order they were created in
the editor.

*See also:* [`Character.AddInventory`](Character#characteraddinventory),
[`Character.LoseInventory`](Character#characterloseinventory),
[`Character.InventoryQuantity`](Character#characterinventoryquantity)

---

### `UnPauseGame`

```ags
UnPauseGame ()
```

Resumes the game.

Example:

```ags
if (IsGamePaused() == 1)
    UnPauseGame();
```

will unpause the game if it is paused.

**NOTE:** Because PauseGame works as a counter, if you called it more
than once, this won't work. To ignore this behavior, unpause as much
as needed with the below snippet.

```ags
while (IsGamePaused()) UnPauseGame();
```

*See also:* [`PauseGame`](Globalfunctions_General#pausegame), [`IsGamePaused`](Globalfunctions_General#isgamepaused)
