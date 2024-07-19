## `TextWindowGUI` functions and properties

This struct extends the GUI struct, so it has all functions and properties from [`GUI`](GUI), in addition to the properties below.

*Compatibility:* TextWindowGUI is supported by AGS 3.5.0 and later versions.

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
