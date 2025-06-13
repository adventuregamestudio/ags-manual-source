## `Button` functions and properties

Button is a subclass of [`GUIControl`](GUIControl) and therefore inherits all GUIControl's functions and properties in addition to its own, which are listed below.

---

### `Button.Animate`

*(Formerly known as `AnimateButton`, which is now obsolete)*

```ags
Button.Animate(int view, int loop, int delay, optional RepeatStyle, 
               optional BlockingStyle, optional Direction, 
               optional int frame, optional int volume)
```

Animates a GUI button by playing the specified view loop on it. This
could be useful for Sierra-style death animations and other effects.

LOOP from VIEW will be played on the button. The DELAY specifies the
speed of the animation - larger numbers are slower. This has the same
values you use with the [`Character.Animate`](Character#characteranimate) and [`Object.Animate`](Object#objectanimate) commands.

The *RepeatStyle* parameter sets whether the animation will continuously repeat the cycling through the frames. This can be `eOnce`, in which case the animation will start from the first frame of LOOP, and go through each frame in turn until the last frame, where it will stop. If `RepeatStyle` is `eRepeat`, then when the last frame is reached, it will go back to the first frame and start over again with the animation.

For *blocking* you can pass either `eBlock` (in which case the function will wait for the animation to finish before returning), or `eNoBlock` (in which case the animation will start to play, but your script will continue). The default is `eBlock`.

*Direction* specifies which way the animation plays. You can either pass `eForwards` (the default) or `eBackwards`.

*Frame* lets you specify the starting frame, which should be one of the chosen loop's frame.
Note that for compatibility reasons if direction is eBackwards the animation actually begins with the previous frame. If you pass frame 0 (the default) then it will begin with the last frame in the loop.

*Volume* lets you specify the relative volume in percents (0-100) of the frame-linked sounds for the duration of this animation. It's 100 by default (which means - unchanged).

You can abort an animation at any time by setting the button's
[NormalGraphic property](Button#buttonnormalgraphic), or starting a new animation on the same button.

**NOTE:** This command destroys the button's normal, pushed and
mouseover images. If you want to return the button to normal usage after
playing an animation, you will have to set the Graphic properties to
restore the images.

**NOTE:** This command does not support flipped view frames. Any frames
marked as "Flipped" will in fact be drawn normally when on a button.

Example:

```ags
btnDeathAnim.Animate(6, 2, 4, eRepeat);
```

will animate the 'btnDeathAnim' button using loop 2 of view 6, with a
delay of 4 cycles per frame, and repeat the animation continually.

Compatibility: Parameters *BlockingStyle*, *Direction*, *frame* and *volume* are supported only by AGS 3.6.0 and later versions.

*See also:* [`Button.Animating`](Button#buttonanimating),
[`Button.Frame`](Button#buttonframe),
[`Button.Loop`](Button#buttonloop),
[`Button.View`](Button#buttonview),
[`Button.NormalGraphic`](Button#buttonnormalgraphic),  
[`Character.Animate`](Character#characteranimate),
[`Object.Animate`](Object#objectanimate),

---

### `Button.Click`

```ags
Button.Click()
```

Forces Button's OnClick event. If there is a script function bound to
that event it will be run, otherwise nothing happens.

*Compatibility:* Supported by **AGS 3.4.0** and later versions.

*See also:* [`GUI.Click`](GUI#guiclick),
[`GUI.ProcessClick`](GUI#guiprocessclick)

---

### `Button.Animating`

```ags
readonly bool Button.Animating
```

Returns true if the specified button is currently animating, or false
otherwise.

This property is read-only. To change button's animation, use the
[`Animate`](Button#buttonanimate) command.

Example:

```ags
btnDeathAnim.Animate(6, 2, 4, eRepeat);
while (btnDeathAnim.Animating) Wait(1);
```

will animate button and wait until the animation finishes.

*Compatibility:* Supported by **AGS 3.4.1** and later versions.

*See also:* [`Button.Animate`](Button#buttonanimate),
[`Button.Frame`](Button#buttonframe),
[`Button.Loop`](Button#buttonloop),
[`Button.View`](Button#buttonview),
[`Button.Graphic`](Button#buttongraphic)

---

### `Button.Frame`

```ags
readonly int Button.Frame
```

Gets the frame number that the animated button is currently set to. If
the button is not currently animated, this will be 0 (in which case the
Graphic property will hold its sprite number).

This property is read-only. To change button's animation, use the
[`Animate`](Button#buttonanimate) command.

*Compatibility:* Supported by **AGS 3.4.1** and later versions.

*See also:* [`Button.Animating`](Button#buttonanimating),
[`Button.Loop`](Button#buttonloop),
[`Button.View`](Button#buttonview),
[`Button.Graphic`](Button#buttongraphic)

---

### `Button.Loop`

```ags
readonly int Button.Loop
```

Gets the loop that the animated button is currently set to. If the
button is not currently animated, this will be 0 (in which case the
Graphic property will hold its sprite number).

This property is read-only. To change button's animation, use the
[`Animate`](Button#buttonanimate) command.

*Compatibility:* Supported by **AGS 3.4.1** and later versions.

*See also:* [`Button.Animate`](Button#buttonanimate),
[`Button.Frame`](Button#buttonframe),
[`Button.View`](Button#buttonview),
[`Button.Graphic`](Button#buttongraphic)

---

### `Button.View`

```ags
readonly int Button.View
```

Gets the view that the animated button is currently set to. If the
button is not currently animated, this will be 0 (in which case the
Graphic property will hold its sprite number).

This property is read-only. To change button's animation, use the
[`Animate`](Button#buttonanimate) command.

*Compatibility:* Supported by **AGS 3.4.1** and later versions.

*See also:* [`Button.Animate`](Button#buttonanimate),
[`Button.Frame`](Button#buttonframe),
[`Button.Loop`](Button#buttonloop),
[`Button.Graphic`](Button#buttongraphic)

---

### `Button.ClipImage`

```ags
bool Button.ClipImage;
```

Gets/sets whether the button clips its image to the button boundaries.

For example, if the button is sized 30x30, but its Graphic is a 50x50
image, then this property controls whether the image is allowed to spill
over the edge of the button.

The default is false, i.e. the image is not clipped.

Setting this to true can be useful in that it ensures that the button's
image is not larger than the button's clickable area, which can cause
confusion when it happens.

Example:

```ags
btnOK.ClipImage = true;
```

sets the *btnOK* button so that its image will be restrained to the
button's clickable area.

*See also:* [`Button.Graphic`](Button#buttongraphic)

---

### `Button.Font`

```ags
FontType Button.Font
```

Gets/sets the font used by the button to display text.

The font number must correspond to one of the fonts from the Fonts pane
in the AGS Editor.

Example:

```ags
btnOK.Font = eFontMain;
```

will change the *btnOK* button to use Font "Main".

*See also:* [`Label.Font`](Label#labelfont),
[`TextBox.Font`](TextBox#textboxfont)

---

### `Button.Graphic`

*(Formerly part of `GetButtonPic`, which is now obsolete)*

```ags
readonly int Button.Graphic;
```

Gets the current image on a GUI button. If a value less than 1 is
returned, then no image is currently displayed on the button.

This property is read-only; in order to set the image, you must use one
of the [`NormalGraphic`](Button#buttonnormalgraphic),
[`MouseOverGraphic`](Button#buttonmouseovergraphic) or
[`PushedGraphic`](Button#buttonpushedgraphic) properties.

Example:

```ags
Display("The button is currently using sprite %d.", btnPlay.Graphic);
```

will display btnPlay's current sprite number.

*See also:* [`Button.ClipImage`](Button#buttonclipimage),
[`Button.MouseOverGraphic`](Button#buttonmouseovergraphic),
[`Button.NormalGraphic`](Button#buttonnormalgraphic),
[`Button.PushedGraphic`](Button#buttonpushedgraphic)

---

### `Button.GraphicFlip`

```ags
eFlipDirection Button.GraphicFlip;
```

Gets/sets the flip direction of the button's graphic.

Example:

```ags
btnPlay.GraphicFlip = eFlipLeftToRight;
```

will mirror btnPlay's sprite horizontally.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [`eFlipDirection`](StandardEnums#eflipdirection)

---

### `Button.MouseOverGraphic`

*(Formerly part of `GetButtonPic`, which is now obsolete)*<br>
*(Formerly part of `SetButtonPic`, which is now obsolete)*

```ags
int Button.MouseOverGraphic;
```

Gets/sets the button's mouse-over sprite. This can be -1, which
indicates that the button does not have a mouse-over graphic.

Example:

```ags
Display("The button's mouse-over image is sprite %d.", btnPlay.MouseOverGraphic);
```

will display btnPlay's mouse-over sprite number.

*See also:* [`Button.Graphic`](Button#buttongraphic),
[`Button.NormalGraphic`](Button#buttonnormalgraphic),
[`Button.PushedGraphic`](Button#buttonpushedgraphic)

---

### `Button.NormalGraphic`

*(Formerly part of `GetButtonPic`, which is now obsolete)*<br>
*(Formerly part of `SetButtonPic`, which is now obsolete)*

```ags
int Button.NormalGraphic;
```

Gets/sets the button's normal sprite (i.e. the graphic used when the
button is not pushed and the mouse is not over it).

Note that setting this to a different sprite will change the button's
size to match the size of the new sprite.

Example:

```ags
Display("The button's normal image is sprite %d.", btnPlay.NormalGraphic);
```

will display btnPlay's normal sprite number.

*See also:* [`Button.ClipImage`](Button#buttonclipimage)
[`Button.Graphic`](Button#buttongraphic),
[`Button.MouseOverGraphic`](Button#buttonmouseovergraphic),
[`Button.PushedGraphic`](Button#buttonpushedgraphic),
[`Button.TextColor`](Button#buttontextcolor)

---

### `Button.PushedGraphic`

*(Formerly part of `GetButtonPic`, which is now obsolete)*<br>
*(Formerly part of `SetButtonPic`, which is now obsolete)*

```ags
int Button.PushedGraphic;
```

Gets/sets the button's pushed sprite (i.e. the graphic used when the
button is pushed in by the user). This can be -1, which indicates that
the button does not have a pushed image.

Example:

```ags
Display("The button's pushed image is sprite %d.", btnPlay.PushedGraphic);
```

will display btnPlay's pushed sprite number.

*See also:* [`Button.Graphic`](Button#buttongraphic),
[`Button.MouseOverGraphic`](Button#buttonmouseovergraphic),
[`Button.NormalGraphic`](Button#buttonnormalgraphic)

---

### `Button.Text`

*(Formerly known as `SetButtonText`, which is now obsolete)*<br>
*(Formerly known as `Button.GetText`, which is now obsolete)*<br>
*(Formerly known as `Button.SetText`, which is now obsolete)*

```ags
String Button.Text;
```

Gets/sets the text displayed on the button.

Example:

```ags
btnController.Text = "Enable jibble";
```

Will change button btnController to read "Enable jibble".

Besides a regular text, there are few special text values, called "tokens". If you assign any of these to the Button, it will update its looks automatically during the game. Currently supported are 3 tokens, that make button display selected inventory item's graphic (that is - inventory item assigned as [`player.ActiveInventory`](Character#characteractiveinventory)). They differ in a way item's graphic is aligned:

Token | Description
--- | ---
`(INV)` | Stretch to fit the button
`(INVNS)` | Draw centered at actual size
`(INVSHR)` | Stretch if too big, draw in actual size if not

Please note that any of these work only if you assign some normal graphic to the button, in which case the inventory item is drawn on top of it.

Example:

```ags
btnSelectedInv.Text = "(INVNS)";
```

Will make button btnSelectedInv display active inventory item's graphic, or just its normal graphic when no inventory item is selected.

*See also:* [`Button.NormalGraphic`](Button#buttonnormalgraphic),
[`Label.Text`](Label#labeltext)

---

### `Button.TextAlignment`

```ags
Alignment Button.TextAlignment;
```

Gets/sets how the text is aligned relative to the button's rectangle.

If the button is displaying an image rather than text, then this property has no effect.

*See also:* [Standard Enumerated Types](StandardEnums), [`Button.NormalGraphic`](Button#buttonnormalgraphic)

*Compatibility:* Supported by **AGS 3.5.0** and later versions.

---

### `Button.TextColor`

```ags
int Button.TextColor;
```

Gets/sets the text color used to display the button's text.

If the button is displaying an image rather than text, then this
property has no effect.

Example:

```ags
btnRestart.TextColor = 15;
```

will change button 'btnRestart' to have white text.

*See also:* [`Button.NormalGraphic`](Button#buttonnormalgraphic)

---

### `Button.TextPaddingHorizontal`

```ags
int Button.TextPaddingHorizontal;
```

Gets/sets amount of padding, restricting the text from left and right.
This property helps to position the text within the button.

Padding here is the space in pixels between the text and the edges of the button.

Example:

```ags
btnSettings.TextPaddingHorizontal = 5;
```

will ensure there is 5 pixels of space on both sides of the text within the button.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:* [`Button.TextPaddingVertical`](Button#buttontextpaddingvertical)

---

### `Button.TextPaddingVertical`

```ags
int Button.TextPaddingVertical;
```

Gets/sets amount of padding, restricting the text from top and bottom.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:* [`Button.TextPaddingHorizontal`](Button#buttontextpaddinghorizontal)

---

### `Button.WrapText`

```ags
bool Button.WrapText;
```

Gets/sets whether the button will wrap its text.

**NOTE:** If the text contains linebreaks (e.g., `\n`), set this property to `true` to honor those breaks.

Example:

```ags
btnRiddle.Text = "Click me for a hint\nWhat has keys but can't open locks?";
btnRiddle.WrapText = true;
```

Here the button btnRiddle has a text that requires wrap set to true to display correctly.
We can imagine clicking it gives a piano sound.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.