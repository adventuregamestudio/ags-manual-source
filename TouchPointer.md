## `TouchPointer` functions and properties

### `TouchPointer.ID`

```ags
readonly int TouchPointer.ID
```

Gets this touch pointer's identifier.

Example:

```ags
int touchStartX[10];
bool touchWasDown[10];

function repeatedly_execute()
{
  for (int i = 0; i < Touch.TouchPointerCount; i++)
  {
    TouchPointer* tp = Touch.TouchPointers[i];
    if (tp.ID < 10)
    {
      if (tp.IsDown && !touchWasDown[tp.ID])
      {
        touchStartX[tp.ID] = tp.X; // this finger has just touched the screen
      }
      touchWasDown[tp.ID] = tp.IsDown;
    }
  }
}
```

will remember where each finger first touched the screen, using the pointer's ID to keep the data of different fingers apart. Comparing a finger's start position with its current one lets you detect swipes.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `TouchPointer.IsDown`

```ags
readonly bool TouchPointer.IsDown
```

Gets whether this touch pointer is currently down (in touching state).

Example:

```ags
function repeatedly_execute()
{
  if (Touch.TouchPointerCount == 0)
  {
    return;
  }
  TouchPointer* tp = Touch.TouchPointers[0];
  gFingerMarker.Visible = tp.IsDown;
  if (tp.IsDown)
  {
    gFingerMarker.SetPosition(tp.X - gFingerMarker.Width / 2, tp.Y - gFingerMarker.Height / 2);
  }
}
```

will show the GUI gFingerMarker centered under the first finger while it touches the screen, and hide it when the finger is lifted.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `TouchPointer.X`

```ags
readonly int TouchPointer.X
```

Gets this touch pointer's X screen coordinate.

Example:

```ags
function repeatedly_execute()
{
  if (Touch.TouchPointerCount == 0)
  {
    return;
  }
  TouchPointer* tp = Touch.TouchPointers[0];
  gFingerMarker.Visible = tp.IsDown;
  if (tp.IsDown)
  {
    gFingerMarker.SetPosition(tp.X - gFingerMarker.Width / 2, tp.Y - gFingerMarker.Height / 2);
  }
}
```

will show the GUI gFingerMarker centered under the first finger while it touches the screen, and hide it when the finger is lifted.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `TouchPointer.Y`

```ags
readonly int TouchPointer.Y
```

Gets this touch pointer's Y screen coordinate.

Example:

```ags
function repeatedly_execute()
{
  if (Touch.TouchPointerCount == 0)
  {
    return;
  }
  TouchPointer* tp = Touch.TouchPointers[0];
  gFingerMarker.Visible = tp.IsDown;
  if (tp.IsDown)
  {
    gFingerMarker.SetPosition(tp.X - gFingerMarker.Width / 2, tp.Y - gFingerMarker.Height / 2);
  }
}
```

will show the GUI gFingerMarker centered under the first finger while it touches the screen, and hide it when the finger is lifted.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.