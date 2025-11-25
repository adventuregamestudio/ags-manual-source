## Global event handlers

In the AGS game there is a number of events, like starting the game, or pressing the key, that may be handled in script by adding a function with *predefined name*. These are called "global event handlers". If you add ones to your script, they will be called automatically whenever corresponding event occurs. All of these are optional, and may be omited if you don't need them.

You may you add multiple functions of the same kind to multiple [script modules](ScriptModules), in which case they will be called in the order of modules in your game project. It's possible to stop propagating the event (skip remaining modules) by using [`ClaimEvent`](Globalfunctions_General#claimevent) function (refer to its article for explanation).

Some of these functions may also be added to the room scripts. These are: `on_event`, `on_key_press`, `on_mouse_click`, `on_text_input`, `repeatedly_execute_always`, `late_repeatedly_execute_always`.

Some functions will only work in the room script: `on_call`.

*NOTE:* for historical reasons, function `repeatedly_execute` does not work in the room script, but there's a corresponding room event that may be linked to a custom function in the editor.

---

### `dialog_request`

```ags
dialog_request (int parameter)
```

Called when a dialog script line "run-script" is processed. PARAMETER
is the value of the number following the "run-script" on that line of
the dialog script.

---

### `game_start`

```ags
game_start ()
```

Called at the start of the game, before the first room is loaded. You
would typically use this to set up the initial positions of
characters, and to modify initial visibility of GUIs.

**Note:** When this function is called it is too early to run
animations or do anything else which relies on a room being loaded

---

### `interface_click`

```ags
interface_click (int interface, int button)
```

Called when the player clicks on a button on a GUI which has its
action set as "Run script". INTERFACE is the number of the GUI which
they clicked on. BUTTON is the object number of the button within this
GUI.

---

### `on_event`

```ags
on_event (EventType event, int data1, int data2, int data3, int data4)
```

Called whenever certain game events occur. The values of DATA1 to DATA4 arguments depend
on which event has occurred (see below).

**IMPORTANT:** DATA2 to DATA4 parameters are supported since **AGS 3.6.2**. If you are using **AGS 3.6.1** or lower, then only one `event` and a single `data` (or `data1`) arguments should be declared.

The possible values of event are:

    eEventEnterRoomBeforeFadein
            called just before the room's 'Enter Before Fade-in' event occurs
            DATA1 = new room number
    eEventEnterRoomAfterFadein
            called just before the room's 'Enter After Fade-in' event occurs
            DATA1 = room number
    eEventLeaveRoom
            called just after the room's 'Player Leaves Room' event occurs, before the fade-out
            DATA1 = room number they are leaving
    eEventLeaveRoomAfterFadeout
            called when leaving a room, right after fade-out, but while room is still in memory
            DATA1 = room number they are leaving
    eEventGUIMouseDown
            called when a mouse button is pressed down over a GUI
            DATA1 = GUI number
            DATA2 = mouse button id; see [MouseButton enum](StandardEnums#mousebutton)
            DATA3 = mouse x position, *relative* to this exact GUI
            DATA4 = mouse y position, *relative* to this exact GUI
    eEventGUIMouseUp
            called when a mouse button is released over a GUI
            DATA1 = GUI number
            DATA2 = mouse button id; see [MouseButton enum](StandardEnums#mousebutton)
            DATA3 = mouse x position, *relative* to this exact GUI
            DATA4 = mouse y position, *relative* to this exact GUI
    eEventAddInventory
            called when the player has just added an inventory item
            DATA1 = inventory item number that was added
    eEventLoseInventory
            called when the player has just lost an inventory item
            DATA1 = inventory item number that was lost
    eEventRestoreGame
            called after a saved game has been restored
            DATA1 = save slot number
    eEventGameSaved
            called after a game was saved
            DATA1 = save slot number
    eEventDialogStart
            called once the dialog state is started, but not when another topic is run
            DATA1 = dialog ID
    eEventDialogStop
            called once it exits the dialog state
            DATA1 = the last dialog ID before exiting dialog state, may be different from one in eEventDialogStart
    eEventDialogRun
            called anytime a dialog entry is ran
            DATA1 = dialog ID
            DATA2 = entry ID, where starting entry (@S) is 0, and user-made options begin with 1
    eEventDialogOptionsOpen
            called when dialog options appear
            DATA1 = dialog ID
    eEventDialogOptionsClose
            called when dialog options close
            DATA1 = dialog ID
            DATA2 = entry ID, the option chosen

**NOTE:** when you write this function in your script, you do not have to declare it with all 4 "data" arguments, you may put only first 1 or 2 instead, and the engine will correctly discard unused parameters without error. This also helps when you upgrade older projects to the latest version of AGS.

For example:
```ags
void on_event(int event, int data1, int data2)
{
    if (event == eEventEnterRoomBeforeFadein)
    {
        // do something on each room enter, before screen fades in
    }
    else if (event == eEventRestoreGame)
    {
        // do something each time right after a save was restored
    }
}
```

*Compatibility:*
 - The `data2`, `data3` and `data4` arguments are only supported since AGS 3.6.2.
 - `eEventEnterRoomAfterFadein` event type is only supported since AGS 3.6.0.
 - `eEventLeaveRoomAfterFadeout` event type  is only supported since AGS 3.6.1.
 - `eEventGameSaved` event type  is only supported since AGS 3.6.1.
 - `eEventDialogStart` event type  is only supported since AGS 3.6.2.
 - `eEventDialogStop` event type  is only supported since AGS 3.6.2.
 - `eEventDialogRun` event type  is only supported since AGS 3.6.2.
 - `eEventDialogOptionsOpen` event type  is only supported since AGS 3.6.2.
 - `eEventDialogOptionsClose` event type  is only supported since AGS 3.6.2.
 - `eEventGUIMouseDown` event type has 4 parameters since AGS 3.6.2, and only 1 parameter before that.
 - `eEventGUIMouseUp` event type has 4 parameters since AGS 3.6.2, and only 1 parameter before that.

---

### `on_key_press`

```ags
on_key_press (eKeyCode keycode, int mod)
```

Called whenever a key is pressed on the keyboard. `keycode` holds the value of the key, while `mod` holds a combination of modifiers pressed alongside with that key. A list of these values is [available here](Keycodes).

**IMPORTANT:** `mod` argument is supported since **AGS 3.6.0**. If you are using **AGS 3.5.1** or lower, then only `keycode` argument should be declared.

The `mod` argument is optional and may be omited. The `mod` contains *set of flags*, and is slightly more complicated than keycode: as you should not use regular comparison (`==`, `!=`) with it, but a bitwise operator (`&`):

```ags
// these two conditions check that ctrl was pressed (these commands are equivalent)
if (mod & eKeyModCtrl)
if (mod & eKeyModCtrl != 0)


// these two conditions check that ctrl was NOT pressed (again, two variants of the same check)
if (!(mod & eKeyModCtrl))
if (mod & eKeyModCtrl == 0)
```

The `on_key_press` function can also be defined in individual room
scripts. This allows the room script to intercept a key-press first,
and then decide whether to pass it on to the global script or not. See
the [`ClaimEvent`](Globalfunctions_General#claimevent) function for more
details.

Starting with version 3.6.0 AGS supports two "key handling" modes: a new-style and old-style (backwards compatible). This mode is selected in the [General Settings](GeneralSettings#backwards-compatibility) with "Use old-style key handling" option in the "Backwards compatibility" group.

**New key mode:**
 - all keys are passed into the `on_key_press` function as-is, one by one: for example, if you press Ctrl + Z, then you get **two** `on_key_press` calls, one with `eKeyCtrlLeft` and another with `eKeyZ` argument.

**Old (classic) key mode:**
 - lone mod keys (Ctrl, Alt, Shift) are not passed into the `on_key_press`; you may only test if these are pressed using [IsKeyPressed](Globalfunctions_General#iskeypressed).
 - alphabet keys + ctrl combinations are merged into one key code for the `on_key_press` callback: e.g. `eKeyCodeCtrlA` and similar key codes.

*Compatibility:* The `mod` argument is only supported since AGS 3.6.0.

---

### `on_mouse_click`

```ags
on_mouse_click (MouseButton button, int mx, int my)
```

Called when the player clicks a mouse button. BUTTON is either
`eMouseLeft`, `eMouseRight`, or `eMouseMiddle`, depending on which
button was clicked. The `mx` and `my` parameters contain the mouse's position at the time of a click.

**IMPORTANT:** `mx` and `my` arguments are supported since **AGS 3.6.2**. If you are using **AGS 3.6.1** or lower, then only `button` argument should be declared.

**NOTE:** it is an old tradition to use global variables `mouse.x` and `mouse.y` in this function to get the mouse's position. But that method is not  entirely reliable, as `on_mouse_click` may be called with certain delay, when the mouse cursor has already moved a bit. Starting with AGS 3.6.2 we advise to use this function's arguments `mx` and `my` instead.

If 'Handle inventory clicks in script' is enabled in the game options,
this function can also be called with `eMouseLeftInv`,
`eMouseMiddleInv` or `eMouseRightInv`, which indicate a left, middle
or right click on an inventory item, respectively.

If 'Enable mouse wheel support' is enabled, this function can also be
called with eMouseWheelNorth or eMouseWheelSouth, which indicate the
user moving the mouse wheel north or south, respectively.

The `on_mouse_click` function can also be defined in individual room
scripts. This allows the room script to intercept a mouse-click first,
and then decide whether to pass it on to the global script or not. See
the [`ClaimEvent`](Globalfunctions_General#claimevent) function for
more details.

*Compatibility:* The `mx` and `my` arguments are only supported since AGS 3.6.2.

---

### `on_text_input`

```ags
on_text_input(int ch)
```

Called when the player's key presses form a printable character. The difference between this and `on_key_press` is that not every key corresponds to the printable char, and some chars may be created by pressing multiple keys (which also depends on the current system language).

The `ch` argument contains a unicode character code, and may be used with the [String functions](String).

For example:

```ags
MyLabel.Text = MyLabel.Text.AppendChar(ch);
```

will append the new printed character to a label's text.

**IMPORTANT:** this function only works when your game uses "new-style key handling mode". This mode is selected in the [General Settings](GeneralSettings#backwards-compatibility) with "Use old-style key handling" option in the "Backwards compatibility" group.

*Compatibility:* supported since AGS 3.6.0.

---

### `repeatedly_execute`

```ags
repeatedly_execute()
```

Called every game cycle (normally 40 times per second). Additional
information is [available here](RepExec).

---

### `repeatedly_execute_always`

```ags
repeatedly_execute_always()
```

Called every game cycle, even when a blocking routine (e.g speech or
cutscene) is in progress. You **cannot** call any blocking functions
from this event handler. `repeatedly_execute_always` is called
**before** the game entities (characters, rooms, etc) get updated.
Additional information is [available here](RepExec).

---

### `late_repeatedly_execute_always`

```ags
late_repeatedly_execute_always()
```

Called every game cycle, even when a blocking routine (e.g. speech or
cutscene) is in progress. You **cannot** call any blocking functions
from this event handler. `late_repeatedly_execute_always` is called
**after** the game entities (characters, rooms, etc) have been
updated, but before the game screen is redrawn.

---

### `unhandled_event`

```ags
unhandled_event(LocationType what, CursorMode mode)
```

Called when an interaction event occurs for an object, but no corresponding event handler has been set. This is typically used to display a default "I can't do that" type response in-order to avoid having to add unique messages for all in-game interactions. The values of WHAT and MODE tell you what the player did. The possible values for WHAT are defined by [LocationType](StandardEnums#locationtype) enum, and MODE is one of the cursor modes.

---

### `on_call`

```ags
on_call (int value)
```

This callback can only be declared on Room Script.

It's called after the function `CallRoomScript(value)` is executed,
but not immediately. You can read an arbitrary integer value passed
from `CallRoomScript`. Additional information is [available here](Globalfunctions_General#callroomscript).
