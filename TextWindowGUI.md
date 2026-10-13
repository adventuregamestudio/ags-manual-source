## `TextWindowGUI` functions and properties

This struct extends the GUI struct, so it has all functions and properties from [`GUI`](GUI), in addition to the properties below.

*Compatibility:* TextWindowGUI is supported by AGS 3.5.0 and later versions.

---

### `TextWindowGUI.LeftGraphic`

```ags
import readonly attribute int TextWindowGUI.LeftGraphic
```

Gets the sprite number used for this TextWindow's left border.


Example:

```ags
TextWindowGUI *tw = gMyTextGui.AsTextWindow;
if (tw != null)
{
  Display("The left border uses the sprite %d.", tw.LeftGraphic);
}
```

will display the sprite number used for the left border of the text window GUI gMyTextGui.

*Compatibility:* supported by **AGS 3.6.3** and later versions.

---

### `TextWindowGUI.TopLeftGraphic`

```ags
import readonly attribute int TextWindowGUI.TopLeftGraphic
```

Gets the sprite number used for this TextWindow's top-left corner.


Example:

```ags
TextWindowGUI *tw = gMyTextGui.AsTextWindow;
if (tw != null)
{
  Display("The top left corner uses the sprite %d.", tw.TopLeftGraphic);
}
```

will display the sprite number used for the top left corner of the text window GUI gMyTextGui.

*Compatibility:* supported by **AGS 3.6.3** and later versions.

---

### `TextWindowGUI.TopGraphic`

```ags
import readonly attribute int TextWindowGUI.TopGraphic
```

Gets the sprite number used for this TextWindow's top border.


Example:

```ags
TextWindowGUI *tw = gMyTextGui.AsTextWindow;
if (tw != null)
{
  Display("The top border uses the sprite %d.", tw.TopGraphic);
}
```

will display the sprite number used for the top border of the text window GUI gMyTextGui.

*Compatibility:* supported by **AGS 3.6.3** and later versions.

---

### `TextWindowGUI.TopRightGraphic`

```ags
import readonly attribute int TextWindowGUI.TopRightGraphic
```

Gets the sprite number used for this TextWindow's top-right corner.


Example:

```ags
TextWindowGUI *tw = gMyTextGui.AsTextWindow;
if (tw != null)
{
  Display("The top right corner uses the sprite %d.", tw.TopRightGraphic);
}
```

will display the sprite number used for the top right corner of the text window GUI gMyTextGui.

*Compatibility:* supported by **AGS 3.6.3** and later versions.

---

### `TextWindowGUI.RightGraphic`

```ags
import readonly attribute int TextWindowGUI.RightGraphic
```

Gets the sprite number used for this TextWindow's right border.


Example:

```ags
TextWindowGUI *tw = gMyTextGui.AsTextWindow;
if (tw != null)
{
  Display("The right border uses the sprite %d.", tw.RightGraphic);
}
```

will display the sprite number used for the right border of the text window GUI gMyTextGui.

*Compatibility:* supported by **AGS 3.6.3** and later versions.

---

### `TextWindowGUI.BottomRightGraphic`

```ags
import readonly attribute int TextWindowGUI.BottomRightGraphic
```

Gets the sprite number used for this TextWindow's bottom-right corner.


Example:

```ags
TextWindowGUI *tw = gMyTextGui.AsTextWindow;
if (tw != null)
{
  Display("The bottom right corner uses the sprite %d.", tw.BottomRightGraphic);
}
```

will display the sprite number used for the bottom right corner of the text window GUI gMyTextGui.

*Compatibility:* supported by **AGS 3.6.3** and later versions.

---

### `TextWindowGUI.BottomGraphic`

```ags
import readonly attribute int TextWindowGUI.BottomGraphic
```

Gets the sprite number used for this TextWindow's bottom border.


Example:

```ags
TextWindowGUI *tw = gMyTextGui.AsTextWindow;
if (tw != null)
{
  Display("The bottom border uses the sprite %d.", tw.BottomGraphic);
}
```

will display the sprite number used for the bottom border of the text window GUI gMyTextGui.

*Compatibility:* supported by **AGS 3.6.3** and later versions.

---

### `TextWindowGUI.BottomLeftGraphic`

```ags
import readonly attribute int TextWindowGUI.BottomLeftGraphic
```

Gets the sprite number used for this TextWindow's bottom-left corner.


Example:

```ags
TextWindowGUI *tw = gMyTextGui.AsTextWindow;
if (tw != null)
{
  Display("The bottom left corner uses the sprite %d.", tw.BottomLeftGraphic);
}
```

will display the sprite number used for the bottom left corner of the text window GUI gMyTextGui.

*Compatibility:* supported by **AGS 3.6.3** and later versions.

---

### `TextWindowGUI.TextColor`

```ags
int TextWindowGUI.TextColor
```

Gets/sets the text color to be used when rendering text on this TextWindowGUI.

Example:

```ags
// a custom MySay function with character-specific text color;
// (assuming that gTextGui is assigned as a speech GUI)
void MySay(this Character*, const string message) {
    gTextGui.AsTextWindow.TextColor = this.SpeechColor;
    this.Say(message);
}
```

above function adjusts gTextGui's text color before calling a standard [`Character.Say`](Character#charactersay) function. It assumes that gTextGui was assigned as a default textwindow for the character speech. Such custom function may then be used as:

```ags
cEgo.MySay("My text has my colors!");
```

---

### `TextWindowGUI.TextPadding`

```ags
int TextWindowGUI.TextPadding
```

Gets/sets the amount of padding, in pixels, surrounding the text in the TextWindow.

Example:

```ags
TextWindowGUI *tw = gMyTextGui.AsTextWindow;
if (tw != null)
{
  tw.TextPadding = 6;
}
```

will keep the text 6 pixels away from the borders of the text window GUI gMyTextGui.
