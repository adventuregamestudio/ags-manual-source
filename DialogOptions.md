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

---

### `DialogOptions.Font`

```ags
static int DialogOptions.Font
```

Gets/sets the font to use when displaying dialog options.

For backwards compatibility this property starts initialized with a eUndefinedFont value. While its set to eUndefinedFont, a Game.NormalFont will be used for options instead.

---

### `DialogOptions.GUIX`

```ags
static int DialogOptions.GUIX
```

Gets/sets on-screen X position of dialog options GUI; set to -1 if it should use default placement.
---

### `DialogOptions.GUIY`

```ags
static int DialogOptions.GUIY
```

Gets/sets on-screen Y position of dialog options GUI; set to -1 if it should use default placement.

---

### `DialogOptions.HasCustomRender`

```ags
static readonly bool DialogOptions.HasCustomRender
```

Tells if the current dialog options use [custom rendering in script](CustomDialogOptions).

**NOTE:** This value reflects a presence of certain script functions. The custom rendering cannot be switched on or off other than by either adding or removing these functions in script.

---

### `DialogOptions.HighlightColor`

```ags
static int DialogOptions.HighlightColor
```

Gets/sets the color used to draw the active (selected) dialog option.

---

### `DialogOptions.ItemCount`

```ags
static readonly int DialogOptions.ItemCount
```

Gets the number of displayed dialog options. Returns 0 if dialog options are not currently displayed.

This property only returns valid value if you are using standard dialog option looks. It cannot be used in case of [custom options rendering](CustomDialogOptions).

*See also:* [`DialogOptions.ItemOptionID`](DialogOptions#dialogoptionsitemoptionid)

---

### `DialogOptions.ItemGap`

```ags
static int DialogOptions.ItemGap
```

Gets/sets the vertical gap between dialog options (in pixels).

---

### `DialogOptions.ItemHeight`

```ags
static readonly int DialogOptions.ItemHeight[]
```

Gets the height of a displayed option at certain index. The valid index range goes from 0 to (DialogOptions.ItemCount - 1). This is essentially a height of option's text, which may be wrapped into more than one line if it's long enough.

This property only returns valid value if you are using standard dialog option looks. It cannot be used in case of [custom options rendering](CustomDialogOptions).

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

---

### `DialogOptions.ItemOptionID`

```ags
static readonly int DialogOptions.ItemOptionID[]
```

Gets the dialog option ID represented by the displayed item at certain index. The valid index range goes from 0 to (DialogOptions.ItemCount - 1).

The returned option ID may be used to read or change option's properties, using functions like [`Dialog.GetOptionState`](Dialog#dialoggetoptionstate), [`Dialog.GetOptionText`](Dialog#dialoggetoptiontext) and similar.

This property only returns valid value if you are using standard dialog option looks. It cannot be used in case of [custom options rendering](CustomDialogOptions).

*See also:* [`DialogOptions.ItemCount`](DialogOptions#dialogoptionsitemcount)

---

### `DialogOptions.ItemWidth`

```ags
static readonly int DialogOptions.ItemWidth[]
```

Gets the width of a displayed option at certain index. The valid index range goes from 0 to (DialogOptions.ItemCount - 1). This is essentially a width of option's text.

This property only returns valid value if you are using standard dialog option looks. It cannot be used in case of [custom options rendering](CustomDialogOptions).

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

---

### `DialogOptions.MinGUIWidth`

```ags
static int DialogOptions.MinGUIWidth
```

Get/sets the minimal width of the auto-resizing GUI on which dialog options are drawn.

---

### `DialogOptions.Overlay`

```ags
static readonly Overlay* DialogOptions.Overlay
```

Gets the Overlay object that represents dialog options on screen. This property will only return a valid Overlay while dialog options are shown, and returns null when they are not shown. You may further access this Overlay's properties and modify them (position, z-order, transparency and so forth).

While it's technically possible, it is not recommended to change overlay's graphic though, because the engine will replace it with a dialog options image whenever they are updated. Removing this Overlay will also not affect the state of the options nor the Dialog, and a new overlay will appear during the next update anyway.

---

### `DialogOptions.PaddingX`

```ags
static int DialogOptions.PaddingX
```

Gets/sets the horizontal offset at which options are drawn on a standard GUI.

---

### `DialogOptions.PaddingY`

```ags
static int DialogOptions.PaddingY
```

Gets/sets the vertical offset at which options are drawn on a standard GUI.

---

### `DialogOptions.ReadColor`

```ags
static int DialogOptions.ReadColor
```

Gets/sets the color used to draw the dialog options that have already been selected once; set to -1 for no distinct color.

---

### `DialogOptions.TemplateGUI`

```ags
static GUI* DialogOptions.TemplateGUI
```

Gets/sets the GUI that will be used to display dialog options; set null to use default options look.

---

### `DialogOptions.TextAlignment`

```ags
static HorizontalAlignment DialogOptions.TextAlignment
```

Gets/sets the horizontal alignment of each dialog option's text.

---

### `DialogOptions.ZOrder`

```ags
static int DialogOptions.ZOrder
```

Gets/sets the z-order of dialog options, relative to GUI and on-screen Overlays.
