## `Joystick` functions and properties

### `Joystick.JoystickCount`

```ags
static readonly int Joystick.JoystickCount
```

Gets number of connected joysticks.

Example:

```ags
Joystick* gamepad;

function game_start()
{
  for (int i = 0; i < Joystick.JoystickCount; i++)
  {
    if (Joystick.Joysticks[i].IsGamepad)
    {
      gamepad = Joystick.Joysticks[i];
      break;
    }
  }
}
```

will go through all the connected joysticks and remember the first one that is a gamepad, so that the rest of the script can use it.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Joystick.Joysticks`

```ags
static readonly Joystick* Joystick.Joysticks[]
```

Gets a connected joystick by index or null on invalid index.

Example:

```ags
Joystick* gamepad;

function game_start()
{
  for (int i = 0; i < Joystick.JoystickCount; i++)
  {
    if (Joystick.Joysticks[i].IsGamepad)
    {
      gamepad = Joystick.Joysticks[i];
      break;
    }
  }
}
```

will go through all the connected joysticks and remember the first one that is a gamepad, so that the rest of the script can use it.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Joystick.Name`

```ags
readonly String Joystick.Name
```

Gets joystick name

Example:

```ags
if (gamepad != null)
{
  lblController.Text = gamepad.Name;
}
else
{
  lblController.Text = "No gamepad connected";
}
```

will show the name of the gamepad in the label lblController on a controls menu. Here `gamepad` is a global Joystick* variable, found as shown in the [`JoystickCount`](Joystick#joystickjoystickcount) example.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Joystick.IsConnected`

```ags
readonly bool Joystick.IsConnected
```

Checks if joystick is really connected

Example:

```ags
function repeatedly_execute_always()
{
  if (gamepad != null && !gamepad.IsConnected && !gGamepadLost.Visible)
  {
    gGamepadLost.Visible = true;
  }
}
```

will show the GUI gGamepadLost, which may ask the player to reconnect their controller, as soon as the gamepad gets disconnected. Here `gamepad` is a global Joystick* variable, found as shown in the [`JoystickCount`](Joystick#joystickjoystickcount) example.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Joystick.IsGamepad`

```ags
readonly bool Joystick.IsGamepad
```

Checks if joystick is a valid gamepad

Example:

```ags
Joystick* gamepad;

function game_start()
{
  for (int i = 0; i < Joystick.JoystickCount; i++)
  {
    if (Joystick.Joysticks[i].IsGamepad)
    {
      gamepad = Joystick.Joysticks[i];
      break;
    }
  }
}
```

will go through all the connected joysticks and remember the first one that is a gamepad. Only gamepads can be used with IsGamepadButtonDown and GetGamepadAxis.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Joystick.IsGamepadButtonDown`

```ags
bool Joystick.IsGamepadButtonDown(eGamepad_Button button)
```

Checks if a gamepad button is pressed, including dpad.

Example:

```ags
bool startWasDown;

function repeatedly_execute_always()
{
  if (gamepad == null)
  {
    return;
  }
  bool startDown = gamepad.IsGamepadButtonDown(eGamepad_ButtonStart);
  if (startDown && !startWasDown)
  {
    gMenu.Visible = !gMenu.Visible;
  }
  startWasDown = startDown;
}
```

will show or hide the GUI gMenu each time the player presses the Start button. The previous state of the button is remembered, so that holding the button down toggles the menu only once. Here `gamepad` is a global Joystick* variable, found as shown in the [`JoystickCount`](Joystick#joystickjoystickcount) example.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [`eGamepad_Button`](StandardEnums#egamepad_button)

---

### `Joystick.GetGamepadAxis`

```ags
float Joystick.GetGamepadAxis(eGamepad_Axis axis, optional float dead_zone);
```

Gets gamepad axis or trigger, trigger only has positive values.

Example:

```ags
function repeatedly_execute_always()
{
  if (gamepad == null)
  {
    return;
  }
  float x = gamepad.GetGamepadAxis(eGamepad_AxisLeftX, 0.2);
  float y = gamepad.GetGamepadAxis(eGamepad_AxisLeftY, 0.2);
  Mouse.SetPosition(mouse.x + FloatToInt(x * 5.0, eRoundNearest), mouse.y + FloatToInt(y * 5.0, eRoundNearest));
}
```

will let the player move the mouse cursor with the gamepad's left stick, ignoring the small stick movements within the dead zone of 0.2. The values are rounded to the nearest whole pixel, so that the cursor moves at the same speed in both directions. Here `gamepad` is a global Joystick* variable, found as shown in the [`JoystickCount`](Joystick#joystickjoystickcount) example.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [`eGamepad_Axis`](StandardEnums#egamepad_axis)

---

### `Joystick.GetAxis`

```ags
float Joystick.GetAxis(int axis, optional float dead_zone)
```

Gets joystick axis or trigger, by index

Example:

```ags
function repeatedly_execute_always()
{
  if (joy == null || joy.AxisCount < 2)
  {
    return;
  }
  float x = joy.GetAxis(0, 0.2);
  float y = joy.GetAxis(1, 0.2);
  Mouse.SetPosition(mouse.x + FloatToInt(x * 5.0, eRoundNearest), mouse.y + FloatToInt(y * 5.0, eRoundNearest));
}
```

will let the player move the mouse cursor with the first two axes of the joystick, ignoring the small movements within the dead zone of 0.2. Here `joy` is a global Joystick* variable, taken from [`Joystick.Joysticks`](Joystick#joystickjoysticks).

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Joystick.IsButtonDown`

```ags
bool Joystick.IsButtonDown(int button)
```

Checks if a joystick button is pressed.

Example:

```ags
bool fireWasDown;

function repeatedly_execute()
{
  if (joy == null)
  {
    return;
  }
  bool fireDown = joy.IsButtonDown(0);
  if (fireDown && !fireWasDown)
  {
    Mouse.Click(eMouseLeft);
  }
  fireWasDown = fireDown;
}
```

will make the first button of the joystick work like a left mouse click. The previous state of the button is remembered, so that holding the button down clicks only once. Here `joy` is a global Joystick* variable, taken from [`Joystick.Joysticks`](Joystick#joystickjoysticks).

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Joystick.GetHat`

```ags
eJoystick_Hat Joystick.Joystick.GetHat(int hat)
```

Gets the direction a dpad from the joystick is pointing to.

Example:

```ags
function repeatedly_execute()
{
  if (joy == null || joy.HatCount == 0)
  {
    return;
  }
  eJoystick_Hat hat = joy.GetHat(0);
  if (hat > 0 && (hat & eJoystick_HatLeft) != 0)
  {
    Game.Camera.X -= 4;
  }
  else if (hat > 0 && (hat & eJoystick_HatRight) != 0)
  {
    Game.Camera.X += 4;
  }
}
```

will scroll the room view while the first hat of the joystick is pushed left or right. The hat values are bit flags, so testing them with `&` also catches the diagonal directions. Here `joy` is a global Joystick* variable, taken from [`Joystick.Joysticks`](Joystick#joystickjoysticks).

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [`eJoystick_Hat`](StandardEnums#ejoystick_hat)

---

### `Joystick.AxisCount`

```ags
readonly int Joystick.AxisCount
```

Gets axis count from the joystick

Example:

```ags
lblController.Text = String.Format("%s: %d axes, %d buttons, %d hats",
                                   joy.Name, joy.AxisCount, joy.ButtonCount, joy.HatCount);
```

will describe the joystick in the label lblController, for example on a controls menu where the player assigns the joystick's buttons. Here `joy` is a global Joystick* variable, taken from [`Joystick.Joysticks`](Joystick#joystickjoysticks).

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Joystick.ButtonCount`

```ags
readonly int Joystick.ButtonCount
```

Gets button count from the joystick

Example:

```ags
lblController.Text = String.Format("%s: %d axes, %d buttons, %d hats",
                                   joy.Name, joy.AxisCount, joy.ButtonCount, joy.HatCount);
```

will describe the joystick in the label lblController, for example on a controls menu where the player assigns the joystick's buttons. Here `joy` is a global Joystick* variable, taken from [`Joystick.Joysticks`](Joystick#joystickjoysticks).

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Joystick.HatCount`

```ags
readonly int Joystick.HatCount
```

Gets hat count from the joystick

Example:

```ags
lblController.Text = String.Format("%s: %d axes, %d buttons, %d hats",
                                   joy.Name, joy.AxisCount, joy.ButtonCount, joy.HatCount);
```

will describe the joystick in the label lblController, for example on a controls menu where the player assigns the joystick's buttons. Here `joy` is a global Joystick* variable, taken from [`Joystick.Joysticks`](Joystick#joystickjoysticks).

*Compatibility:* Supported by **AGS 4.0.0** and later versions.