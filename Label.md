## `Label` functions and properties

Label is a subclass of [`GUIControl`](GUIControl) and therefore inherits all GUIControl's functions and properties in addition to its own, which are listed below.

**Compatibility notes:**

Prior to **AGS 3.6.0** Labels added 1 pixel to the font's linespacing property (see [`GetFontLineSpacing`](Globalfunctions_General#getfontlinespacing)) when drawing multiple text lines. Starting with 3.6.0 they use strictly font's linespacing value.

---

### `Label.Font`

*(Formerly known as `SetLabelFont`, which is now obsolete)*

```ags
FontType Label.Font;
```

Gets/sets the font used to display the label's text. This is useful if
you have a standard SCI font for your English version, but want to
change to a TTF font for foreign language versions.

Example:

```ags
if (IsTranslationAvailable()) {
    lblStatus.Font = eFontForeign;
}
```

will change label 'lblStatus' to use font "Foreign" if a game
translation is in use.

*See also:* [`IsTranslationAvailable`](Globalfunctions_General#istranslationavailable),
[`Label.Text`](Label#labeltext),
[`TextBox.Font`](TextBox#textboxfont)

---

### `Label.Text`

*(Formerly known as `SetLabelText`, which is now obsolete)*<br>
*(Formerly known as `Label.GetText`, which is now obsolete)*<br>
*(Formerly known as `Label.SetText`, which is now obsolete)*

```ags
String Label.Text;
```

Gets/sets the text displayed in the specified label.

Examples:

```ags
lblGameChapter.Text = "Chapter 2";
```
will display "Chapter 2" text on label 'lblGameChapter'.

```ags
lblHotspotName.Text = Game.GetLocationName(mouse.x, mouse.y);
```
will display the name of the location the cursor is over on label 'lblHotspotName'

```ags
lblScore.Text = String.Format("Game score: %d out of %d", cur_score, total_score);
```
will display a [formatted string](StringFormats) with game score variable values on label 'lblScore'.

Besides a regular text, or text formatted with String.Format, you can add some special values called "tokens" which will be replaced and automatically updated during the game. Multiple tokens may be present on a single label. The following tokens are currently supported, and will be substituted by actual values at runtime:

Token | Description
--- | ---
`@GAMENAME@` | The game's name, from General Settings or Game.Name property
`@OVERHOTSPOT@` | The name of the location (hotspot, object, etc) which the cursor is over

Example:

```ags
lblStatus.Text = "Your cursor is over @OVERHOTSPOT@.";
```

*See also:* [`Button.NormalGraphic`](Button#buttonnormalgraphic),
[`Button.Text`](Button#buttontext),
[`Label.TextColor`](Label#labeltextcolor),
[`Label.Font`](Label#labelfont)

---

### `Label.TextAlignment`

```ags
Alignment Label.TextAlignment;
```

Gets/sets how the text is aligned relative to the label's rectangle.

*See also:* [Standard Enumerated Types](StandardEnums), [`Label.Font`](Label#labelfont),
[`Label.Text`](Label#labeltext)

*Compatibility:* Supported by **AGS 3.5.0** and later versions.
This property used to have the type HorizontalAlignment prior to **AGS 3.6.2**.

---

### `Label.TextColor`

*(Formerly known as `SetLabelColor`, which is now obsolete)*

```ags
int Label.TextColor;
```

Gets/sets the text color used to display the label's text.

Example:

```ags
lblStatus.TextColor = 14;
```

will change label 'lblStatus' to have yellow text.

*See also:* [`Label.Font`](Label#labelfont),
[`Label.Text`](Label#labeltext)

