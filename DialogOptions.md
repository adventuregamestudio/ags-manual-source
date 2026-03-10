## `DialogOptions` functions and properties

DialogOptions struct lets configure standard dialog options look and behavior, and read their state when they are displayed on screen.

Be aware that the majority of the properties here become irrelevant if your game uses [Custom dialog options rendering](CustomDialogOptions) system in script.

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

---

### `DialogOptions.BulletGraphic`

```ags
static attribute int DialogOptions.BulletGraphic
```

Gets/sets the sprite to use as a bullet point before each dialog option (0 for none).

---

### `DialogOptions.Font`

```ags
static attribute int DialogOptions.Font
```

Gets/sets the font to use when displaying dialog options.

For backwards compatibility this property starts initialized with a eUndefinedFont value. While its set to eUndefinedFont, a Game.NormalFont will be used for options instead.

---

### `DialogOptions.GUIX`

```ags
static attribute int DialogOptions.GUIX
```

Gets/sets on-screen X position of dialog options GUI; set to -1 if it should use default placement.
---

### `DialogOptions.GUIY`

```ags
static attribute int DialogOptions.GUIY
```

Gets/sets on-screen Y position of dialog options GUI; set to -1 if it should use default placement.

---

### `DialogOptions.HighlightColor`

```ags
static attribute int DialogOptions.HighlightColor
```

Gets/sets the color used to draw the active (selected) dialog option.

---

### `DialogOptions.ItemGap`

```ags
static attribute int DialogOptions.ItemGap
```

Gets/sets the vertical gap between dialog options (in pixels).

---

### `DialogOptions.ItemNumbering`

```ags
static attribute DialogOptionsNumbering DialogOptions.ItemNumbering
```

Gets/sets whether dialog options have numbers before them, and the numeric keys can be used to select them.

---

### `DialogOptions.MaxGUIWidth`

```ags
static attribute int DialogOptions.MaxGUIWidth
```

Get/sets the maximal width of the auto-resizing GUI on which dialog options are drawn.

---

### `DialogOptions.MinGUIWidth`

```ags
static attribute int DialogOptions.MinGUIWidth
```

Get/sets the minimal width of the auto-resizing GUI on which dialog options are drawn.

---

### `DialogOptions.Overlay`

```ags
static readonly attribute Overlay* DialogOptions.Overlay
```

Gets the Overlay object that represents dialog options on screen. This property will only return a valid Overlay while dialog options are shown, and returns null when they are not shown. You may further access this Overlay's properties and modify them (position, z-order, transparency and so forth).

While it's technically possible, it is not recommended to change overlay's graphic though, because the engine will replace it with a dialog options image whenever they are updated. Removing this Overlay will also not affect the state of the options nor the Dialog, and a new overlay will appear during the next update anyway.

---

### `DialogOptions.PaddingX`

```ags
static attribute int DialogOptions.PaddingX
```

Gets/sets the horizontal offset at which options are drawn on a standard GUI.

---

### `DialogOptions.PaddingY`

```ags
static attribute int DialogOptions.PaddingY
```

Gets/sets the vertical offset at which options are drawn on a standard GUI.

---

### `DialogOptions.ReadColor`

```ags
static attribute int DialogOptions.ReadColor
```

Gets/sets the color used to draw the dialog options that have already been selected once; set to -1 for no distinct color.

---

### `DialogOptions.TemplateGUI`

```ags
static attribute GUI* DialogOptions.TemplateGUI
```

Gets/sets the GUI that will be used to display dialog options; set null to use default options look.

---

### `DialogOptions.TextAlignment`

```ags
static attribute HorizontalAlignment DialogOptions.TextAlignment
```

Gets/sets the horizontal alignment of each dialog option's text.

---

### `DialogOptions.ZOrder`

```ags
static attribute int DialogOptions.ZOrder
```

Gets/sets the z-order of dialog options, relative to GUI and on-screen Overlays.
