## `Touch` functions and properties

### `Touch.TouchPointerCount`

```ags
readonly static int Touch.TouchPointerCount
```

Number of touch pointers registered by the game engine.

Example:

```ags
function repeatedly_execute()
{
  int fingers = 0;
  for (int i = 0; i < Touch.TouchPointerCount; i++)
  {
    if (Touch.TouchPointers[i].IsDown)
    {
      fingers++;
    }
  }
  if (fingers == 2 && !gInventory.Visible)
  {
    gInventory.Visible = true;
  }
}
```

will open the inventory when the player touches the screen with two fingers at once.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Touch.TouchPointers`

```ags
readonly static TouchPointer* Touch.TouchPointers[]
```

Accesses a touch pointer state by its identifier.

Example:

```ags
function repeatedly_execute()
{
  int fingers = 0;
  for (int i = 0; i < Touch.TouchPointerCount; i++)
  {
    if (Touch.TouchPointers[i].IsDown)
    {
      fingers++;
    }
  }
  if (fingers == 2 && !gInventory.Visible)
  {
    gInventory.Visible = true;
  }
}
```

will open the inventory when the player touches the screen with two fingers at once.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [`TouchPointer`](TouchPointer)