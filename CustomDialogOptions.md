## Custom dialog options rendering

By default, AGS comes with two types of dialog options -- displaying
them using the size and position of an existing GUI, or creating a text
window to display the options in.

As of AGS 3.1 and above, if neither of these methods suit you (for example,
because you want to use picture-based dialog options, or you want to add
scroll arrows), you can now implement the dialog options display
yourself in the script.

**NOTE:** This topic involves some advanced scripting. If you're just
starting out with AGS, please just use one of the built-in dialog option
styles for now, and come back to this later when you're comfortable with
scripting.

To write your custom dialog options code, you need to add a number of even handlers in your script, taken from the list below. *Two of them must be present* (`dialog_options_get_dimensions` and `dialog_options_render`), but others are optional, so use only those that match your game needs. These function would work somewhat similar to the [common event handlers](Globalfunctions_Event), except they are only run whenever the dialog options are on screen.

All of these functions accept [`DialogOptionsRenderingInfo`](DialogOptionsRenderingInfo) object as a first argument. This object may be used to get and set various properties, including accessing current Dialog object.

**NOTE:** These functions don't have to go in the global script; you can put them
in any script you like. However, beware that if the functions are
present in more than one script they could interfere with each other and
cause problems.

**COMPATIBILITY NOTE:** The older versions of AGS (pre-3.4.0) were working with somewhat different set of functions. There was no `dialog_options_repexec` and `dialog_options_key_press`, but you had to add `dialog_options_get_active` function instead in which you'd set an active dialog option (or none) on each game update, for example, depending on the cursor position. Mouse clicks were processed by the engine, and if an active option was set in `dialog_options_get_active` then it was run automatically.
There is a compatibility switch in the General Settings called "Use old-style dialog options rendering API" that toggles between the old and new behavior. If you have imported older project and want to keep old dialog option scripts, then make sure it's enabled. If you wish to update your game to the new behavior, then turn it off.

**IMPORTANT:** All of the Custom Dialog functions run on the
non-blocking thread. That means that you should not make any blocking
calls, such as Character.Say, Wait or Display within them, as they may
not behave correctly.

---

### `dialog_options_get_dimensions`

```ags
dialog_options_get_dimensions(DialogOptionsRenderingInfo *info)
```

This is the first function that will be called before dialog options are shown. It is meant to find out which part of the screen you will be drawing onto. Do that by setting the X, Y, Width and Height properties of the `info` struct. Only when the width and height are greater than 0 will the custom dialog system be activated.

**NOTE:** you may also use this function to set other things up, not only `DialogOptionsRenderingInfo`.

---

### `dialog_options_render`

```ags
dialog_options_render(DialogOptionsRenderingInfo *info)
```

This function is called right before the dialog options are shown or updated on screen. The `info` struct will supply a [`DrawingSurface`](DrawingSurface) which may be accessed using `info.Surface` property, and which you may draw upon.

---

### `dialog_options_mouse_click`

```ags
dialog_options_mouse_click(DialogOptionsRenderingInfo *info, MouseButton button, int mx, int my)
```

This function is called if the player clicks the mouse anywhere on dialog options. It corresponds to the regular [`on_mouse_click`](Globalfunctions_Event#on_mouse_click). You might want to use this to process clicks on dialog options, and also on some custom elements like scroll arrows, for example.

The `mx` and `my` parameters contain mouse's position at the time of click. They are more reliable than using global variables `mouse.x` and `mouse.y`, because this function may be called with a small delay when the mouse have already moved away a bit.
These parameters contain position in *screen coordinates*, as sometimes it may be necessary to know if player clicked outside of the dialog options bounds. Subtract [DialogOptionsRenderInfo.X](DialogOptionsRenderingInfo#dialogoptionsrenderinginfox) and Y property values from these to tell the relative position.

---

### `dialog_options_key_press`

```ags
dialog_options_key_press(DialogOptionsRenderingInfo *info, eKeyCode keycode, int mod)
```

This function is called if the player presses any key while custom dialog options are shown on screen. It corresponds to the regular [`on_key_press`](Globalfunctions_Event#on_key_press), and you can use this to implement key-controlled selection of dialog option, for example.

**Compatibility:** the optional `mod` argument is supported since **AGS 3.6.0**. Please refer to the [`on_key_press`](Globalfunctions_Event#on_key_press) description for details.

---

### `dialog_options_text_input`

```ags
dialog_options_text_input(DialogOptionsRenderingInfo *info, int ch)
```

This function is called if the player's key presses form a printable character. It corresponds to the regular [`on_text_input`](Globalfunctions_Event#on_text_input), and you can use this to handle a custom text input in your dialog options.

**Compatibility:** Supported by AGS 3.6.0 and above. `dialog_options_text_input` will only work if you have "Use old-style key handling" option disabled in the "General Settings".

---

### `dialog_options_repexec`

```ags
dialog_options_repexec(DialogOptionsRenderingInfo *info)
```

This function works similar to a general [`repeatedly_execute`](Globalfunctions_Event#repeatedly_execute) function, but is called only while the dialog options are shown on the screen. You may use this to update the dialog options state, for example, for determining which option the mouse is currently hovering over, or scripting timed actions.

---

### `dialog_options_close`

```ags
dialog_options_close(DialogOptionsRenderingInfo *info)
```

This function will be called right before the dialog options are removed from the screen. This is a place to perform a "cleanup" in case you need to dispose temporary resources (like dynamic sprites), hide GUIs, stop sounds, and so forth.

**NOTE:** this is not the same as the end of the current dialog. Dialog options may be shown and hidden multiple times during the dialog, every time the game lets player to choose a dialog option.

**Compatibility:** Supported by AGS 3.6.0 and above.

---

### `dialog_options_get_active`

```ags
dialog_options_get_active(DialogOptionsRenderingInfo *info)
```

*Compatibility:* `dialog_options_get_active` is a deprecated function that works only if you have "Use old-style dialog options rendering API" enabled in General Settings. In a modern script use `dialog_options_repexec` instead.

This function is called as the player moves the mouse around the screen. This needs to work out which option the mouse is currently hovering over, so that AGS knows which option to process if the player clicks the mouse button. Use it to set `info.ActiveOptionID` property, or do any other changes.

---

### Example A. Classic mouse controls

```ags
int dlg_opt_color = 14;
int dlg_opt_acolor = 13;
int dlg_opt_ncolor = 4;

void dialog_options_get_dimensions(DialogOptionsRenderingInfo *info)
{
    // Create a 200x200 dialog options area at (50,100)
    info.X = 50;
    info.Y = 100;
    info.Width = 200;
    info.Height = 200;
    // Enable alpha channel for the drawing surface
    info.HasAlphaChannel = true;
    // Put the text parser at the bottom (if enabled)
    info.ParserTextBoxX = 10;
    info.ParserTextBoxY = 160;
    info.ParserTextBoxWidth = 180;
}

void dialog_options_render(DialogOptionsRenderingInfo *info)
{
    info.Surface.Clear(dlg_opt_color);
    int ypos = 0;
    // Render all the options that are enabled
    for (int i = 1; i <= info.DialogToRender.OptionCount; i++)
    {
        if (info.DialogToRender.GetOptionState(i) == eOptionOn)
        {
            if (info.ActiveOptionID == i)
                info.Surface.DrawingColor = dlg_opt_acolor;
            else
                info.Surface.DrawingColor = dlg_opt_ncolor;

            info.Surface.DrawStringWrapped(5, ypos, info.Width - 10, eFontNormal, eAlignLeft,
                                           info.DialogToRender.GetOptionText(i));
            ypos += GetTextHeight(info.DialogToRender.GetOptionText(i), eFontNormal, info.Width - 10);
        }
    }
}

void dialog_options_repexec(DialogOptionsRenderingInfo *info)
{
    info.ActiveOptionID = 0;
    if (mouse.y < info.Y || mouse.y >= info.Y + info.Height ||
        mouse.x < info.X || mouse.x >= info.X + info.Width)
    {
        return; // return if the mouse is outside UI bounds
    }

    int ypos = 0;
    // Find the option that corresponds to where the mouse cursor is
    for (int i = 1; i <= info.DialogToRender.OptionCount; i++)
    {
        if (info.DialogToRender.GetOptionState(i) == eOptionOn)
        {
            ypos += GetTextHeight(info.DialogToRender.GetOptionText(i), eFontNormal, info.Width - 10);
            if ((mouse.y - info.Y) < ypos)
            {
                info.ActiveOptionID = i;
                return;
            }
        }
    }
}

void dialog_options_mouse_click(DialogOptionsRenderingInfo *info, MouseButton button)
{
    if (info.ActiveOptionID > 0)
        info.RunActiveOption();
}
```

The examples above are slightly naive; in reality you would probably
want to track the Y position of each option in a variable to save having
to continually scan through all the options.

---

### Example B. Keyboard controls

```ags
int dlg_opt_color = 14;
int dlg_opt_acolor = 13;
int dlg_opt_ncolor = 4;

void dialog_options_get_dimensions(DialogOptionsRenderingInfo *info)
{
    // Create a 200x200 dialog options area at (50,100)
    info.X = 50;
    info.Y = 100;
    info.Width = 200;
    info.Height = 200;
    info.ActiveOptionID = 1; // set to first option
}

void dialog_options_render(DialogOptionsRenderingInfo *info)
{
    info.Surface.Clear(dlg_opt_color);
    int ypos = 0;
    // Render all the options that are enabled
    for (int i = 1; i <= info.DialogToRender.OptionCount; i++)
    {
        if (info.DialogToRender.GetOptionState(i) == eOptionOn)
        {
            if (info.ActiveOptionID == i)
                info.Surface.DrawingColor = dlg_opt_acolor;
            else
                info.Surface.DrawingColor = dlg_opt_ncolor;

            info.Surface.DrawStringWrapped(5, ypos, info.Width - 10, eFontNormal, eAlignLeft,
                                           info.DialogToRender.GetOptionText(i));
            ypos += GetTextHeight(info.DialogToRender.GetOptionText(i), eFontNormal, info.Width - 10);
        }
    }
}

void dialog_options_key_press(DialogOptionsRenderingInfo *info, eKeyCode keycode, int mod)
{
    if (keycode == eKeyUpArrow)
    {
        // check all options upwards until found an active one
        for (int next_opt = info.ActiveOptionID - 1; next_opt >= 1; next_opt--)
        {
            if (info.DialogToRender.GetOptionState(next_opt) == eOptionOn)
            {
                info.ActiveOptionID = next_opt;
                break;
            }
        }
    }
    else if (keycode == eKeyDownArrow)
    {
        // check all options downwards until found an active one
        for (int next_opt = info.ActiveOptionID + 1; next_opt <= info.DialogToRender.OptionCount; next_opt++)
        {
            if (info.DialogToRender.GetOptionState(next_opt) == eOptionOn)
            {
                info.ActiveOptionID = next_opt;
                break;
            }
        }
    }
    else if (keycode == eKeyReturn || keycode == eKeySpace)
    {
        info.RunActiveOption();
    }
}
```
