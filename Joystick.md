## `Joystick` functions and properties

### `Joystick.JoystickCount`

```ags
static readonly int Joystick.JoystickCount
```

Gets number of connected joysticks.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Joystick.Joysticks`

```ags
static readonly Joystick\* Joystick.Joysticks[]
```

Gets a connected joystick by index or null on invalid index.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Joystick.Name`

```ags
readonly String Joystick.Name
```

Gets joystick name

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Joystick.IsConnected`

```ags
readonly bool Joystick.IsConnected
```

Checks if joystick is really connected

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Joystick.IsGamepad`

```ags
readonly bool Joystick.IsGamepad
```

Checks if joystick is a valid gamepad

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Joystick.IsGamepadButtonDown`

```ags
bool Joystick.IsGamepadButtonDown(eGamepad_Button button)
```

Checks if a gamepad button is pressed, including dpad.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [`eGamepad_Button`](StandardEnums#egamepad_button)

---

### `Joystick.GetGamepadAxis`

```ags
float Joystick.GetGamepadAxis(eGamepad_Axis axis, optional float dead_zone);
```

Gets gamepad axis or trigger, trigger only has positive values.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [`eGamepad_Axis`](StandardEnums#egamepad_axis)

---

### `Joystick.GetAxis`

```ags
float Joystick.GetAxis(int axis, optional float dead_zone)
```

Gets joystick axis or trigger, by index

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Joystick.IsButtonDown`

```ags
bool Joystick.IsButtonDown(int button)
```

Checks if a joystick button is pressed.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Joystick.GetHat`

```ags
eJoystick_Hat Joystick.Joystick.GetHat(int hat)
```

Gets the direction a dpad from the joystick is pointing to.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [`eJoystick_Hat`](StandardEnums#ejoystick_hat)

---

### `Joystick.AxisCount`

```ags
readonly int Joystick.AxisCount
```

Gets axis count from the joystick

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Joystick.ButtonCount`

```ags
readonly int Joystick.ButtonCount
```

Gets button count from the joystick

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Joystick.HatCount`

```ags
readonly int Joystick.HatCount
```

Gets hat count from the joystick

*Compatibility:* Supported by **AGS 4.0.0** and later versions.