## Global functions (Wait)

`Wait*` functions block the execution of a current script for certain duration, and optionally let player skip the waiting with a key or mouse clicks. After wait is over, the script continues as usual. `Wait` works almost everywhere, except for [`(late_)repeatedly_execute_always`](Globalfunctions_Event#repeatedly_execute_always) functions.

**Before AGS 3.6.0**, Wait* functions would return '0' if the waiting ended with a timeout, or '1' if it was skipped by a player.

**Since AGS 3.6.0**, Wait* functions do return a more sophisticated **Skip Reason** on why they were skipped:

- `0` - ended by timeout or `SkipWait()`;
- `!= 0` - an integer value which combines [InputType](StandardEnums#inputtype), [KeyMod](Keycodes#key-modifiers), and a key or button code corresponding to that input type; that describes what input device and which button on that device were used to skip this "wait".

If you only need to find out whether WaitX ended with a timeout or a player's input, all you need is to compare it with `0`:

```ags
if (WaitMouseKey(100) == 0) {
    Display("Time out!");
}

if (WaitMouse(100) != 0) {
    Display("Skipped by a mouse click.");
}
```

If you need to determine exact details of how the "wait" was skipped, you have to split returned value into `InputType`, key mod, and a button code using [bitwise operations](ScriptKeywords#operators):

```ags
int result = WaitMouseKey(100);
InputType type = result & eInputAny;
int keymod = result & eKeyModMask; // extract key mod flags
int keycode = result & eKeyCodeMask; // extract key or button code
```

For example, this is how you may Wait until player presses either Space key or Left Mouse button:

```ags
while (true) {
    int result = WaitMouseKey(-1);
    InputType type = result & eInputAny;
    int keycode = result & eKeyCodeMask;
    if ((type == eInputMouse && keycode == eMouseLeft) ||
        (type == eInputKeyboard && keycode == eKeySpace)) {
        break; // break the waiting loop
    }
}
```

and this is how the key combinations may be tested (Ctrl + S in this case):

```ags
while (true) {
    int result = WaitKey(-1);
    int keycode = result & eKeyCodeMask;
    int keymod = result & eKeyModMask;
    if (keymod == eKeyModCtrl && keycode == eKeyS) {
        break; // break the waiting loop
    }
}
```

---

### `SkipWait`

```ags
void SkipWait (optional int resultValue)
```

Cancels current Wait function, regardless of its type, if one was active at the moment.

Optionally lets provide a "result value" that will be returned by the Wait function, or by [Game.BlockingWaitSkipped](Game#gameblockingwaitskipped) property. Because of how Wait's return code is defined, this has to be a positive value not greater than 16777215.

*Compatibility:* Supported by **AGS 3.6.0** and later versions. A "resultValue" argument is supported since **AGS 3.6.3**.

*See also:* [`WaitKey`](Globalfunctions_Wait#waitkey),
[`WaitMouse`](Globalfunctions_Wait#waitmouse),
[`WaitMouseKey`](Globalfunctions_Wait#waitmousekey),
[`Game.BlockingWaitSkipped`](Game#gameblockingwaitskipped)

---

### `Wait`

```ags
void Wait (int time)
```

Pauses the script and lets the game continue for TIME loops. There are
normally 40 loops/second (unless you change it with SetGameSpeed), so
using a value of 80 will wait 2 seconds. Note that no other scripts can
run while the Wait function is in the background.

Example:

```ags
cEgo.Walk(120, 140, eBlock, eWalkableAreas);
Wait(80);
cEgo.FaceLocation(1000,100);
```

will move the character EGO to 120,140, wait until he gets there then
wait for 2 seconds (80 game cycles) and then face right.

*See also:* [`WaitKey`](Globalfunctions_Wait#waitkey),
[`WaitMouse`](Globalfunctions_Wait#waitmouse),
[`WaitMouseKey`](Globalfunctions_Wait#waitmousekey),
[`Game.BlockingWaitSkipped`](Game#gameblockingwaitskipped)

---

### `WaitInput`

```ags
int WaitInput(InputType types, int timeout = -1)
```

Pauses the script and lets the game continue until EITHER:

- `timeout` loops have elapsed, or
- the player presses a key or button on one of the devices listed in `types`

The `types` may contain any combined values from the [`InputType` enum](StandardEnums#inputtype).

If `timeout` is -1, it will Wait forever, until a key is pressed.

Returns the **Skip Reason** of why it was skipped (see top of this page for more details):

- `0` - ended by timeout;
- `!= 0` - ended by player input;

Example:

```ags
WaitInput(eInputKeyboard + eInputMouse, 200);
```

will pause the script and wait until 5 seconds have passed (if game is 40 frames per second) or the player
presses a key or clicks the mouse.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`Wait`](Globalfunctions_Wait#wait), 
[`WaitKey`](Globalfunctions_Wait#waitkey), 
[`WaitMouse`](Globalfunctions_Wait#waitmouse),
[`WaitMouseKey`](Globalfunctions_Wait#waitmousekey),
[`Game.BlockingWaitSkipped`](Game#gameblockingwaitskipped),
[`SkipWait`](Globalfunctions_Wait#skipwait)

---

### `WaitKey`

```ags
int WaitKey (int timeout = -1)
```

Pauses the script and lets the game continue until EITHER:

- `timeout` loops have elapsed, or
- the player presses a key

If `timeout` is -1, it will Wait forever, until a key is pressed.

Returns the **Skip Reason** of why it was skipped (see top of this page for more details):

- `0` - ended by timeout;
- `!= 0` - ended by key keycode;

Example:

```ags
WaitKey(200);
```

will pause the script and wait until 5 seconds have passed or the player
presses a key.

*Compatibility:* infinite timeout and complex skip reason are only supported by **AGS 3.6.0** and later versions.

*See also:* [`Wait`](Globalfunctions_Wait#wait),
[`WaitMouse`](Globalfunctions_Wait#waitmouse),
[`WaitMouseKey`](Globalfunctions_Wait#waitmousekey),
[`Game.BlockingWaitSkipped`](Game#gameblockingwaitskipped)

---

### `WaitMouseKey`

```ags
int WaitMouseKey (int timeout = -1)
```

Pauses the script and lets the game continue until EITHER:

- `timeout` loops have elapsed, or
- the player presses a key, or
- the player clicks a mouse button

If `timeout` is -1, it will Wait forever, until either a key is pressed or the mouse is clicked.

Returns the **Skip Reason** of why it was skipped (see top of this page for more details):

- `0` - ended by timeout;
- `!= 0` - ended by keycode or mouse click;

Example:

```ags
WaitMouseKey(200);
```

will pause the script and wait until 5 seconds have passed or the player
presses a key or clicks the mouse.

*Compatibility:* infinite timeout and complex skip reason are only supported by **AGS 3.6.0** and later versions.

*See also:* [`Wait`](Globalfunctions_Wait#wait), 
[`WaitKey`](Globalfunctions_Wait#waitkey),
[`WaitMouse`](Globalfunctions_Wait#waitmouse),
[`Game.BlockingWaitSkipped`](Game#gameblockingwaitskipped)

---

### `WaitMouse`

```ags
int WaitMouseKey (int timeout = -1)
```

Pauses the script and lets the game continue until EITHER:

- `timeout` loops have elapsed, or
- the player clicks a mouse button

If `timeout` is -1, it will Wait forever, until the mouse is clicked.

Returns the **Skip Reason** of why it was skipped (see top of this page for more details):

- `0` - if the time elapsed, or 
- `!= 0` - ended by mouse click;

Example:

```ags
WaitMouse(200);
```

will pause the script and wait until 5 seconds have passed (if game is 40 frames per second) or the player
presses clicks the mouse.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`Wait`](Globalfunctions_Wait#wait), 
[`WaitKey`](Globalfunctions_Wait#waitkey), 
[`WaitMouseKey`](Globalfunctions_Wait#waitmousekey),
[`Game.BlockingWaitSkipped`](Game#gameblockingwaitskipped),
[`SkipWait`](Globalfunctions_Wait#skipwait)
