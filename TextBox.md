## `TextBox` functions and properties

TextBox is a subclass of [`GUIControl`](GUIControl) and therefore inherits all GUIControl's functions and properties in addition to its own, which are listed below.

---

### `TextBox.Font`

*(Formerly known as `SetTextBoxFont`, which is now obsolete)*

```ags
FontType TextBox.Font
```

Gets/sets the font used by the specified text box. This might be useful
if you need a player input text box to use a different font with foreign
language translations, for example.

Example:

```ags
txtUserInput.Font = eFontNormal;
```

will change the *txtUserInput* text box to use Font "Normal".

*See also:* [`Label.Font`](Label#labelfont),
[`TextBox.Text`](TextBox#textboxtext)

---

### `TextBox.ShowBorder`

```ags
bool TextBox.ShowBorder
```

Gets/sets whether the text box's border is shown.

Border is drawn using color from TextColor property.

*Compatibility:* Supported by **AGS 3.5.0** and later versions.

---

### `TextBox.Text`

*(Formerly known as `GetTextBoxText`, which is now obsolete)*<br>
*(Formerly known as `SetTextBoxText`, which is now obsolete)*<br>
*(Formerly known as `TextBox.GetText`, which is now obsolete)*<br>
*(Formerly known as `TextBox.SetText`, which is now obsolete)*

```ags
String TextBox.Text;
```

Gets/sets the text box contents. This might be useful to reset the text
box to blank after the user has typed something in, or to fill in a
default value.

Example:

```ags
txtUserInput.Text = "";
```

will clear the txtUserInput text box.

*See also:* [`TextBox.Font`](TextBox#textboxfont),
[`String.CompareTo`](String#stringcompareto),
[`Label.Text`](Label#labeltext)

---

### `TextBox.TextAlignment`

```ags
Alignment TextBox.TextAlignment;
```

Gets/sets how the text is aligned relative to the textbox's rectangle.

*See also:* [Alignment enum](StandardEnums#alignment)

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

---

### `TextBox.TextColor`

```ags
int TextBox.TextColor;
```

Gets/sets the text color used to draw the text box. This affects both
the text in the text box, and also the text box's border.

Example:

```ags
txtInput.TextColor = 14;
```

will change text box 'txtInput' to have yellow text.

*Compatibility:* Supported by **AGS 3.1.0** and later versions.

*See also:* [`TextBox.ShowBorder`](TextBox#textboxshowborder), [`TextBox.Text`](TextBox#textboxtext)

