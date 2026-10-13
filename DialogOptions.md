## `DialogOptions` functions and properties

DialogOptions struct lets configure standard dialog options look and behavior, and read their state when they are displayed on screen.

Be aware that the majority of the properties here become irrelevant if your game uses [Custom dialog options rendering](CustomDialogOptions) system in script.

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

---

### `DialogOptions.BulletGraphic`

```ags
static int DialogOptions.BulletGraphic
```

Gets/sets the sprite to use as a bullet point before each dialog option (0 for none).

Example:

```ags
DialogOptions.BulletGraphic = 120;
```

will draw the sprite 120 as a bullet point before each dialog option.

---

### `DialogOptions.DrawMode`

```ags
static DialogOptionsDrawMode DialogOptions.DrawMode
```

Gets/sets which draw mode to use when displaying dialog options, standard or custom rendering. This property can have following values:

 - *eDialogOptDraw_Auto* - the draw mode will be chosen automatically, depending on whether [`dialog_options_get_dimensions`](CustomDialogOptions#dialog_options_get_dimensions) function is present in game script. This mode is set by default.
 - *eDialogOptDraw_Standard* - standard draw mode will be used: dialog options will be drawn either as a plain rectangle, or use GUI if one is assigned (see [`DialogOptions.TemplateGUI`](DialogOptions#dialogoptionstemplategui)).
 - *eDialogOptDraw_Custom* - custom rendering will be used, but only if necessary functions are present in game script.

DrawMode is initialized as "Auto" by default. This means that the engine will search the game scripts for the presence of custom rendering functions, and choose a drawing mode depending on whether they exist or not. Normally you do not need to change this value in your games at all. The original purpose this property was added for is to be able to test different draw modes within the same game without disabling a custom render script.

Example:

```ags
DialogOptions.DrawMode = eDialogOptDraw_Standard;
```

will make the game use the standard dialog options look, even if the game script contains the custom rendering functions.

*See also:* [Custom dialog options rendering](CustomDialogOptions)

---

### `DialogOptions.Font`

```ags
static int DialogOptions.Font
```

Gets/sets the font to use when displaying dialog options.

For backwards compatibility this property starts initialized with a eUndefinedFont value. While its set to eUndefinedFont, a Game.NormalFont will be used for options instead.

Example:

```ags
DialogOptions.Font = Game.SpeechFont;
```

will draw the dialog options using the speech font.

---

### `DialogOptions.GUIX`

```ags
static int DialogOptions.GUIX
```

Gets/sets on-screen X position of dialog options GUI; set to -1 if it should use default placement.

Example:

```ags
DialogOptions.GUIX = 20;
DialogOptions.GUIY = 120;
```

will place the dialog options GUI at the position 20,120 on screen.

---

### `DialogOptions.GUIY`

```ags
static int DialogOptions.GUIY
```

Gets/sets on-screen Y position of dialog options GUI; set to -1 if it should use default placement.

Example:

```ags
DialogOptions.GUIX = 20;
DialogOptions.GUIY = 120;
```

will place the dialog options GUI at the position 20,120 on screen.

---

### `DialogOptions.HasCustomRender`

```ags
static readonly bool DialogOptions.HasCustomRender
```

Tells if the current dialog options use [custom rendering in script](CustomDialogOptions).

**NOTE:** This value reflects a presence of certain script functions. The custom rendering cannot be switched on or off other than by either adding or removing these functions in script.

Example:

```ags
// this script module changes the look of the standard dialog options,
// which has no effect if the game draws the options itself
if (!DialogOptions.HasCustomRender)
{
  DialogOptions.HighlightColor = Game.GetColorFromRGB(255, 255, 0);
  DialogOptions.ReadColor = Game.GetColorFromRGB(128, 128, 128);
}
```

will adjust the colors of the standard dialog options, but only if the game does not draw the options with its own script functions.

---

### `DialogOptions.HighlightColor`

```ags
static int DialogOptions.HighlightColor
```

Gets/sets the color used to draw the active (selected) dialog option.

Example:

```ags
DialogOptions.HighlightColor = Game.GetColorFromRGB(255, 255, 0);
```

will draw the active (selected) dialog option in yellow.

---

### `DialogOptions.ItemCount`

```ags
static readonly int DialogOptions.ItemCount
```

Gets the number of displayed dialog options. Returns 0 if dialog options are not currently displayed.

This property only returns valid value if you are using standard dialog option looks. It cannot be used in case of [custom options rendering](CustomDialogOptions).

Example:

```ags
function repeatedly_execute_always()
{
  if (DialogOptions.ItemCount > 0)
  {
    lblStatus.Text = String.Format("%d options to choose from", DialogOptions.ItemCount);
  }
  else
  {
    lblStatus.Text = "";
  }
}
```

will update the label lblStatus with the number of options while the standard dialog options are displayed, and clear it when they are not.

*See also:* [`DialogOptions.ItemOptionID`](DialogOptions#dialogoptionsitemoptionid)

---

### `DialogOptions.ItemGap`

```ags
static int DialogOptions.ItemGap
```

Gets/sets the vertical gap between dialog options (in pixels).

Example:

```ags
DialogOptions.ItemGap = 4;
```

will leave a vertical gap of 4 pixels between the dialog options.

---

### `DialogOptions.ItemHeight`

```ags
static readonly int DialogOptions.ItemHeight[]
```

Gets the height of a displayed option at certain index. The valid index range goes from 0 to (DialogOptions.ItemCount - 1). This is essentially a height of option's text, which may be wrapped into more than one line if it's long enough.

This property only returns valid value if you are using standard dialog option looks. It cannot be used in case of [custom options rendering](CustomDialogOptions).

Example:

```ags
function repeatedly_execute_always()
{
  lblOption.Text = "";
  Overlay* ovr = DialogOptions.Overlay;
  if (ovr == null)
  {
    return; // dialog options are not shown
  }
  for (int i = 0; i < DialogOptions.ItemCount; i++)
  {
    int x = ovr.X + DialogOptions.ItemX[i];
    int y = ovr.Y + DialogOptions.ItemY[i];
    if (mouse.x >= x && mouse.x < x + DialogOptions.ItemWidth[i] &&
        mouse.y >= y && mouse.y < y + DialogOptions.ItemHeight[i])
    {
      lblOption.Text = Dialog.CurrentDialog.GetOptionText(DialogOptions.ItemOptionID[i]);
    }
  }
}
```

will find which of the standard dialog options is under the mouse cursor, and show its text in the label lblOption. The item positions are relative to the dialog options overlay, so the overlay's position is added to them.

*See also:* [`DialogOptions.ItemCount`](DialogOptions#dialogoptionsitemcount),
[`DialogOptions.ItemWidth](DialogOptions#dialogoptionsitemwidth),
[`DialogOptions.ItemX`](DialogOptions#dialogoptionsitemx),
[`DialogOptions.ItemY`](DialogOptions#dialogoptionsitemy)

---

### `DialogOptions.ItemNumbering`

```ags
static DialogOptionsNumbering DialogOptions.ItemNumbering
```

Gets/sets whether dialog options have numbers before them, and the numeric keys can be used to select them.

Example:

```ags
DialogOptions.ItemNumbering = eDialogOptNumbers_Display;
```

will show a number before each dialog option and let the player select the options with the number keys.

---

### `DialogOptions.ItemOptionID`

```ags
static readonly int DialogOptions.ItemOptionID[]
```

Gets the dialog option ID represented by the displayed item at certain index. The valid index range goes from 0 to (DialogOptions.ItemCount - 1).

The returned option ID may be used to read or change option's properties, using functions like [`Dialog.GetOptionState`](Dialog#dialoggetoptionstate), [`Dialog.GetOptionText`](Dialog#dialoggetoptiontext) and similar.

This property only returns valid value if you are using standard dialog option looks. It cannot be used in case of [custom options rendering](CustomDialogOptions).

Example:

```ags
function repeatedly_execute_always()
{
  lblOption.Text = "";
  Overlay* ovr = DialogOptions.Overlay;
  if (ovr == null)
  {
    return; // dialog options are not shown
  }
  for (int i = 0; i < DialogOptions.ItemCount; i++)
  {
    int x = ovr.X + DialogOptions.ItemX[i];
    int y = ovr.Y + DialogOptions.ItemY[i];
    if (mouse.x >= x && mouse.x < x + DialogOptions.ItemWidth[i] &&
        mouse.y >= y && mouse.y < y + DialogOptions.ItemHeight[i])
    {
      lblOption.Text = Dialog.CurrentDialog.GetOptionText(DialogOptions.ItemOptionID[i]);
    }
  }
}
```

will find which of the standard dialog options is under the mouse cursor, and show its text in the label lblOption. The displayed item index is not the same as the option's ID, so ItemOptionID is used to get the option's text from the dialog.

*See also:* [`DialogOptions.ItemCount`](DialogOptions#dialogoptionsitemcount)

---

### `DialogOptions.ItemWidth`

```ags
static readonly int DialogOptions.ItemWidth[]
```

Gets the width of a displayed option at certain index. The valid index range goes from 0 to (DialogOptions.ItemCount - 1). This is essentially a width of option's text.

This property only returns valid value if you are using standard dialog option looks. It cannot be used in case of [custom options rendering](CustomDialogOptions).

Example:

```ags
function repeatedly_execute_always()
{
  lblOption.Text = "";
  Overlay* ovr = DialogOptions.Overlay;
  if (ovr == null)
  {
    return; // dialog options are not shown
  }
  for (int i = 0; i < DialogOptions.ItemCount; i++)
  {
    int x = ovr.X + DialogOptions.ItemX[i];
    int y = ovr.Y + DialogOptions.ItemY[i];
    if (mouse.x >= x && mouse.x < x + DialogOptions.ItemWidth[i] &&
        mouse.y >= y && mouse.y < y + DialogOptions.ItemHeight[i])
    {
      lblOption.Text = Dialog.CurrentDialog.GetOptionText(DialogOptions.ItemOptionID[i]);
    }
  }
}
```

will find which of the standard dialog options is under the mouse cursor, and show its text in the label lblOption. The item positions are relative to the dialog options overlay, so the overlay's position is added to them.

*See also:* [`DialogOptions.ItemCount`](DialogOptions#dialogoptionsitemcount),
[`DialogOptions.ItemHeight](DialogOptions#dialogoptionsitemheight),
[`DialogOptions.ItemX`](DialogOptions#dialogoptionsitemx),
[`DialogOptions.ItemY`](DialogOptions#dialogoptionsitemy)

---

### `DialogOptions.ItemX`

```ags
static readonly int DialogOptions.ItemX[]
```

Gets the x coordinate of a displayed option at certain index. The valid index range goes from 0 to (DialogOptions.ItemCount - 1). The x coordinate is relative to the dialog options overlay position.

This property only returns valid value if you are using standard dialog option looks. It cannot be used in case of [custom options rendering](CustomDialogOptions).

Example:

```ags
function repeatedly_execute_always()
{
  lblOption.Text = "";
  Overlay* ovr = DialogOptions.Overlay;
  if (ovr == null)
  {
    return; // dialog options are not shown
  }
  for (int i = 0; i < DialogOptions.ItemCount; i++)
  {
    int x = ovr.X + DialogOptions.ItemX[i];
    int y = ovr.Y + DialogOptions.ItemY[i];
    if (mouse.x >= x && mouse.x < x + DialogOptions.ItemWidth[i] &&
        mouse.y >= y && mouse.y < y + DialogOptions.ItemHeight[i])
    {
      lblOption.Text = Dialog.CurrentDialog.GetOptionText(DialogOptions.ItemOptionID[i]);
    }
  }
}
```

will find which of the standard dialog options is under the mouse cursor, and show its text in the label lblOption. The item positions are relative to the dialog options overlay, so the overlay's position is added to them.

*See also:* [`DialogOptions.ItemCount`](DialogOptions#dialogoptionsitemcount),
[`DialogOptions.ItemWidth](DialogOptions#dialogoptionsitemwidth),
[`DialogOptions.ItemHeight](DialogOptions#dialogoptionsitemheight),
[`DialogOptions.ItemY`](DialogOptions#dialogoptionsitemy)

---

### `DialogOptions.ItemY`

```ags
static readonly int DialogOptions.ItemY[]
```

Gets the y coordinate of a displayed option at certain index. The valid index range goes from 0 to (DialogOptions.ItemCount - 1). The y coordinate is relative to the dialog options overlay position.

This property only returns valid value if you are using standard dialog option looks. It cannot be used in case of [custom options rendering](CustomDialogOptions).

Example:

```ags
function repeatedly_execute_always()
{
  lblOption.Text = "";
  Overlay* ovr = DialogOptions.Overlay;
  if (ovr == null)
  {
    return; // dialog options are not shown
  }
  for (int i = 0; i < DialogOptions.ItemCount; i++)
  {
    int x = ovr.X + DialogOptions.ItemX[i];
    int y = ovr.Y + DialogOptions.ItemY[i];
    if (mouse.x >= x && mouse.x < x + DialogOptions.ItemWidth[i] &&
        mouse.y >= y && mouse.y < y + DialogOptions.ItemHeight[i])
    {
      lblOption.Text = Dialog.CurrentDialog.GetOptionText(DialogOptions.ItemOptionID[i]);
    }
  }
}
```

will find which of the standard dialog options is under the mouse cursor, and show its text in the label lblOption. The item positions are relative to the dialog options overlay, so the overlay's position is added to them.

*See also:* [`DialogOptions.ItemCount`](DialogOptions#dialogoptionsitemcount),
[`DialogOptions.ItemWidth](DialogOptions#dialogoptionsitemwidth),
[`DialogOptions.ItemHeight](DialogOptions#dialogoptionsitemheight),
[`DialogOptions.ItemX`](DialogOptions#dialogoptionsitemx)

---

### `DialogOptions.MaxGUIWidth`

```ags
static int DialogOptions.MaxGUIWidth
```

Get/sets the maximal width of the auto-resizing GUI on which dialog options are drawn.

Example:

```ags
DialogOptions.MaxGUIWidth = 240;
```

will not let the auto-resizing dialog options GUI grow wider than 240 pixels.

---

### `DialogOptions.MinGUIWidth`

```ags
static int DialogOptions.MinGUIWidth
```

Get/sets the minimal width of the auto-resizing GUI on which dialog options are drawn.

Example:

```ags
DialogOptions.MinGUIWidth = 120;
```

will not let the auto-resizing dialog options GUI become narrower than 120 pixels.

---

### `DialogOptions.Overlay`

```ags
static readonly Overlay* DialogOptions.Overlay
```

Gets the Overlay object that represents dialog options on screen. This property will only return a valid Overlay while dialog options are shown, and returns null when they are not shown. You may further access this Overlay's properties and modify them (position, z-order, transparency and so forth).

While it's technically possible, it is not recommended to change overlay's graphic though, because the engine will replace it with a dialog options image whenever they are updated. Removing this Overlay will also not affect the state of the options nor the Dialog, and a new overlay will appear during the next update anyway.

Example:

```ags
function repeatedly_execute_always()
{
  if (DialogOptions.Overlay != null)
  {
    DialogOptions.Overlay.Transparency = 30;
  }
}
```

will make the dialog options slightly translucent whenever they are displayed.

---

### `DialogOptions.PaddingX`

```ags
static int DialogOptions.PaddingX
```

Gets/sets the horizontal offset at which options are drawn on a standard GUI.

Example:

```ags
DialogOptions.PaddingX = 6;
DialogOptions.PaddingY = 4;
```

will draw the dialog options 6 pixels from the left and 4 pixels from the top of the standard GUI.

---

### `DialogOptions.PaddingY`

```ags
static int DialogOptions.PaddingY
```

Gets/sets the vertical offset at which options are drawn on a standard GUI.

Example:

```ags
DialogOptions.PaddingX = 6;
DialogOptions.PaddingY = 4;
```

will draw the dialog options 6 pixels from the left and 4 pixels from the top of the standard GUI.

---

### `DialogOptions.ReadColor`

```ags
static int DialogOptions.ReadColor
```

Gets/sets the color used to draw the dialog options that have already been selected once; set to -1 for no distinct color.

Example:

```ags
DialogOptions.ReadColor = Game.GetColorFromRGB(128, 128, 128);
```

will draw the dialog options that the player has already selected once in gray.

---

### `DialogOptions.TemplateGUI`

```ags
static GUI* DialogOptions.TemplateGUI
```

Gets/sets the GUI that will be used to display dialog options; set null to use default options look.

Example:

```ags
DialogOptions.TemplateGUI = gDialogOptions;
```

will display the dialog options on the GUI gDialogOptions, instead of the default plain rectangle.

---

### `DialogOptions.TextAlignment`

```ags
static HorizontalAlignment DialogOptions.TextAlignment
```

Gets/sets the horizontal alignment of each dialog option's text.

Example:

```ags
DialogOptions.TextAlignment = eAlignCenter;
```

will center the text of each dialog option.

---

### `DialogOptions.ZOrder`

```ags
static int DialogOptions.ZOrder
```

Gets/sets the z-order of dialog options, relative to GUI and on-screen Overlays.

Example:

```ags
DialogOptions.ZOrder = 100;
```

will draw the dialog options in front of GUIs and on-screen overlays that have a lower z-order.
