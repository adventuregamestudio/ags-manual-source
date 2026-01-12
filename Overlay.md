## `Overlay` functions and properties

The overlays are simple sprite placeholders, they are created in script, and display images on screen. They cannot be interacted with by player, nor interact with other game objects. This makes them ideal for creating temporary visual effects. But they also may be used to script a custom object from ground up; that may involve some advanced scripting though.

Overlays may be created as *screen overlays* and *room overlays*. This defines the visual layer they are positioned in, and which other game objects they are visually sorted with.

---

### `Overlay.CreateGraphical`

*(Formerly known as `CreateGraphicOverlay`, which is now obsolete)*

```ags
static Overlay* Overlay.CreateGraphical(int x, int y, int slot, optional bool transparent, optional bool clone)
```

Creates a *screen overlay* using an image from SLOT sprite. The image is placed at (X,Y) in the screen coordinates.

The screen overlays are positioned in the screen graphic layer, and are visually sorted among other screen overlays and GUI using overlay's [ZOrder](Overlay#overlayzorder) property.

If *transparent* is true then the overlay will be drawn in the same way
as characters/objects, if it is false then a black rectangle will be
painted behind the sprite. Default value is "true".
**NOTE:** for "historical reasons", *transparent* parameter works only in 8-bit games, and does not work in 16-bit and 32-bit games at all, so it's currently useless there.

If *clone* is true then the overlay will make a copy of the SLOT image, otherwise it will create a shared reference, similar to how room objects or characters do. Default value is "false". In practice this only makes a difference if you are creating an overlay from the [DynamicSprite](DynamicSprite): if you make a copy then the original dynamic sprite is safe to discard; also any changes to dynamic sprite will be applied to overlay only if it uses shared reference (no copy). If you create alot of overlays, sharing (non copying) may improve game's perfomance and decrease the memory requirements. For the same perfomance reasons regular sprites are never copied, regardless of this parameter's value (because they cannot be deleted or edited at runtime).

**NOTE:** if you are using a [DynamicSprite](DynamicSprite) when creating an overlay, and *not* copying the image, then you have to make sure the sprite is not deleted so long as overlay stays on screen, or overlay's image will reset to sprite 0.

**NOTE:** if the Overlay variable goes out of scope, the overlay will be removed. Hence, if you want the overlay to last on-screen outside of the
script function where it was created, the `Overlay*` variable declaration needs to be at the top of the script and outside any script
functions.

**NOTE:** if the player goes to a different room all active overlays are removed automatically.

Example:

```ags
Overlay* myOverlay = Overlay.CreateGraphical(100, 100, 300);
Wait(40);
myOverlay.Remove();
```

will create an overlay using sprite number 300, at the coordinates 100,100. It will display for 1 second, then remove it.

*See also:* [`Overlay.CreateRoomGraphical`](Overlay#overlaycreateroomgraphical),
[`Overlay.CreateTextual`](Overlay#overlaycreatetextual),
[`Overlay.CreateRoomTextual`](Overlay#overlaycreateroomtextual),
[`Overlay.Remove`](Overlay#overlayremove),
[`Overlay.Graphic`](Overlay#overlaygraphic)

---

### `Overlay.CreateRoomGraphical`

```ags
static Overlay* Overlay.CreateRoomGraphical(int x, int y, int slot, optional bool transparent, optional bool clone)
```

Creates a *room overlay* using an image from SLOT sprite. The image is placed at (X,Y) in the screen coordinates.

The room overlays are positioned in the room graphic layer, and are visually sorted among other room overlays, characters, room objects and walk-behinds using overlay's [ZOrder](Overlay#overlayzorder) property, similar to how other objects in rooms use `Baseline` property.

All the parameters and use are identical to [`Overlay.CreateGraphical`](Overlay#overlaycreategraphical) function, please refer to its description for more details.

**NOTE:** as overlays are a simplistic kind of object that does not have any automated behavior, it won't interact with anything inside the room, and won't change its z-order according to its Y coordinate automatically, unlike characters and room objects.
If you'd like room overlay to change its "baseline" along with its vertical position, you will have to do something like this in global or room's "repeatedly execute" script function:

```ags
over.ZOrder = over.Y + over.Height;
```

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`Overlay.CreateGraphical`](Overlay#overlaycreategraphical),
[`Overlay.CreateTextual`](Overlay#overlaycreatetextual),
[`Overlay.CreateRoomTextual`](Overlay#overlaycreateroomtextual),
[`Overlay.Remove`](Overlay#overlayremove),
[`Overlay.Graphic`](Overlay#overlaygraphic)

---

### `Overlay.CreateRoomTextual`

```ags
static Overlay* Overlay.CreateRoomTextual(int x, int y, int width,
                                          FontType font, int color, string text)
```

Creates a *room overlay* containing the text you pass at the position specified. This overlay looks identical to the way speech text is
displayed in conversations, except that with this command the text stays on the screen until either you remove it with the Remove command, or the
player goes to a different room, in which case it is automatically removed.

The room overlays are positioned in the room graphic layer, and are visually sorted among other room overlays, characters, room objects and walk-behinds using overlay's [ZOrder](Overlay#overlayzorder) property, similar to how other objects in rooms use `Baseline` property.

All the parameters and use are identical to [`Overlay.CreateTextual`](Overlay#overlaycreatetextual) function, please refer to its description for more details.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`Overlay.CreateGraphical`](Overlay#overlaycreategraphical),
[`Overlay.CreateRoomGraphical`](Overlay#overlaycreateroomgraphical),
[`Overlay.CreateTextual`](Overlay#overlaycreatetextual),
[`Overlay.Remove`](Overlay#overlayremove)

---

### `Overlay.CreateTextual`

*(Formerly known as `CreateTextOverlay`, which is now obsolete)*

```ags
static Overlay* Overlay.CreateTextual(int x, int y, int width,
                                      FontType font, int color, string text)
```

Creates a *screen overlay* containing the text you pass at the position
specified. A screen overlay looks identical to the way speech text is
displayed in conversations, except that with this command the text stays
on the screen until either you remove it with the Remove command, or the
player goes to a different room, in which case it is automatically
removed.

The screen overlays are positioned in the screen graphic layer, and are visually sorted among other screen overlays and GUI using overlay's [ZOrder](Overlay#overlayzorder) property.

The X and Y parameters specify the upper-left corner of where the text
will be written. WIDTH is the width, in pixels, of the text area. FONT
is the font number from the editor to use (0 is the normal font, 1 is
the speech font). COLOR is the text color. Finally, TEXT is obviously the text that gets displayed.

If a *negative* value is passed as either X or Y coordinate, then the text will be instead centered on screen along the respective axis.

The function returns the Overlay, which you use later to reposition and
remove the overlay.

You can insert the value of variables into the message. For more
information, see the [string formatting](StringFormats)
section.

**NOTE:** if the Overlay variable goes out of scope, the overlay will be
removed. Hence, if you want the overlay to last on-screen outside of the
script function where it was created, the `Overlay*` variable
declaration needs to be at the top of the script and outside any script
functions.

**NOTE:** if the player goes to a different room all active overlays are removed automatically.

Example:

```ags
Overlay* myOverlay = Overlay.CreateTextual(50, 80, 120, Game.SpeechFont, 15, "This is a text overlay");
Wait(40);
myOverlay.Remove();
```

will display a 120 pixels text area with its upper left corner at
coordinates 50,80 containing the string "This is a text overlay" using
the speech font and white color. It will be displayed for 1 second, then
removed.

As it's been said, textual Overlays are displayed in the way similar to the current speech style ([`Speech.Style`](Speech#speechstyle)). This means that if speech style is "eSpeechLucasarts" or "eSpeechSierra", then overlays only have a text, but no background. If speech style is "eSpeechSierraWithBackground", then overlays have a text and a background box.

Following [game variables](Gamevariables) affect the textual overlay's look:
* `game.speech_text_gui` - defines a GUI to use as a background (in "eSpeechSierraWithBackground" style only);
* `game.text_align` - defines text alignment (eAlignLeft, eAlignCentre, eAlignRight).

*See also:*
[`Overlay.CreateGraphical`](Overlay#overlaycreategraphical),
[`Overlay.CreateRoomGraphical`](Overlay#overlaycreateroomgraphical),
[`Overlay.CreateRoomTextual`](Overlay#overlaycreateroomtextual),
[`Overlay.X`](Overlay#overlayx), [`Overlay.Y`](Overlay#overlayy),
[`Overlay.Remove`](Overlay#overlayremove)

---

### `Overlay.Remove`

*(Formerly known as `RemoveOverlay`, which is now obsolete)*

```ags
Overlay.Remove()
```

Removes the specified overlay from the screen. Use this when you are
done using the overlay.

Example:

```ags
Overlay* myOverlay = Overlay.CreateTextual(50,80,120,2,15,"This is a text overlay");
Wait(200);
myOverlay.Remove();
```

will create a text overlay , wait for 200 game cycles (about 5 seconds)
and then remove the overlay from the screen.

*See also:* [`Overlay.CreateTextual`](Overlay#overlaycreatetextual)

---

### `Overlay.SetPosition`

```ags
Overlay.SetPosition(int x, int y, int width = 0, int height = 0)
```

Sets this overlay's position, and optionally its size.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:* [`Overlay.X`](Overlay#overlayx), [`Overlay.Y`](Overlay#overlayy),
[`Overlay.Width`](Overlay#overlaywidth),
[`Overlay.Height`](Overlay#overlayheight),
[`Overlay.SetSize`](Overlay#overlaysetsize)

---

### `Overlay.SetSize`

```ags
Overlay.SetSize(int width, int height)
```

Changes the size of the overlay. Works with both graphical and textual overlays.

When the size is set *will stretch or shrink* the overlay's image, both horizontally and vertically.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:* [`Overlay.X`](Overlay#overlayx), [`Overlay.Y`](Overlay#overlayy),
[`Overlay.Width`](Overlay#overlaywidth),
[`Overlay.Height`](Overlay#overlayheight),
[`Overlay.SetPosition`](Overlay#overlaysetposition)

---

### `Overlay.SetText`

*(Formerly known as `SetTextOverlay`, which is now obsolete)*

```ags
Overlay.SetText(int width, FontType font, int color, string text, ...)
```

Replaces the specified overlay with a new one, at the same co-ordinates
but with the new specified text, width, font and color.

You can insert the value of variables into the message. For more
information, see the [string formatting](StringFormats)
section.

Example:

```ags
Overlay* myOverlay = Overlay.CreateTextual(50,80,120,Game.SpeechFont,15,"This is a text overlay");
Wait(200);
myOverlay.SetText(120,Game.SpeechFont,15,"This is another text overlay");
```

will create a text overlay , wait for 200 game cycles (about 5 seconds)
and then replace the overlay with another one.

*See also:* [`Overlay.CreateTextual`](Overlay#overlaycreatetextual),
[`Overlay.Remove`](Overlay#overlayremove)

---

### `Overlay.Graphic`

```ags
int Overlay.Graphic;
```

Gets/sets the sprite slot number that the overlay is currently using. Textual overlays always return -1, as their image is generated and stored internally, and does not have a formal "number". Setting `Graphic` of a textual overlay will effectively change them to a graphical overlay.

**NOTE:** unlike [`Overlay.CreateGraphical`](Overlay#overlaycreategraphical), where you may choose whether to make an image's copy or a shared reference, setting `Graphic` will always make a shared reference to the sprite. If this is a [DynamicSprite](DynamicSprite), you have to make sure it is not deleted so long as overlay stays on screen, or overlay's image will reset to sprite 0.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`Overlay.CreateGraphical`](Overlay#overlaycreategraphical),
[`Overlay.CreateRoomGraphical`](Overlay#overlaycreateroomgraphical),
[`Overlay.X`](Overlay#overlayx), [`Overlay.Y`](Overlay#overlayy),
[`Overlay.Width`](Overlay#overlaywidth),
[`Overlay.Height`](Overlay#overlayheight)

---

### `Overlay.GraphicHeight`

```ags
readonly int Overlay.GraphicHeight;
```

Gets the original height of the overlay's image. This property may be used to know the unscaled image's size (as [`Height`](Overlay#overlayheight) property scales it), which is useful if the original sprite was disposed, or for textual overlays that generate its own internal image.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`Overlay.X`](Overlay#overlayx), [`Overlay.Y`](Overlay#overlayy), [`Overlay.GraphicWidth`](Overlay#overlaygraphicwidth), [`Overlay.Height`](Overlay#overlayheight),
[`Overlay.Width`](Overlay#overlaywidth)

---

### `Overlay.GraphicWidth`

```ags
readonly int Overlay.GraphicWidth;
```

Gets the original width of the overlay's image. This property may be used to know the unscaled image's size (as [`Width`](Overlay#overlaywidth) property scales it), which is useful if the original sprite was disposed, or for textual overlays that generate its own internal image.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`Overlay.X`](Overlay#overlayx), [`Overlay.Y`](Overlay#overlayy), [`Overlay.GraphicHeight`](Overlay#overlaygraphicheight),
[`Overlay.Width`](Overlay#overlaywidth),
[`Overlay.Height`](Overlay#overlayheight)

---

### `Overlay.Height`

```ags
int Overlay.Height;
```

Gets/sets the height of this overlay. Changing this property *will stretch or shrink* the overlay's image vertically. Works with both graphical and textual overlays.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`Overlay.X`](Overlay#overlayx), [`Overlay.Y`](Overlay#overlayy), [`Overlay.Width`](Overlay#overlaywidth)

---

### `Overlay.InRoom`

```ags
readonly bool Overlay.InRoom;
```

Tells whether current overlay is a room (returns "true") or screen (if "false") overlay.

*See also:* [`Overlay.CreateGraphical`](Overlay#overlaycreategraphical),
[`Overlay.CreateTextual`](Overlay#overlaycreatetextual),
[`Overlay.CreateRoomGraphical`](Overlay#overlaycreateroomgraphical),
[`Overlay.CreateRoomTextual`](Overlay#overlaycreateroomtextual)

---

### `Overlay.Text`

```ags
String Overlay.Text;
```

Gets/sets the textual description of this overlay.

Overlay's Text property stores an arbitrary value, similar to the Character.RealName or Object.Name, that has purely informative purpose and does not affect the overlay itself in any way. If Overlay was created using Overlay.CreateTextual(), or modified using Overlay.SetText(), then the Text property will return a text used when generating the overlay's image. If it was created using Overlay.CreateGraphical(), then it will return empty string. But you also may assign a custom value to this property for your own purposes.

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

*See also:* [`Overlay.CreateTextual`](Overlay#overlaycreatetextual),
[`Overlay.CreateRoomTextual`](Overlay#overlaycreateroomtextual)

---

### `Overlay.Transparency`

```ags
int Overlay.Transparency;
```

Gets/sets the transparency of this overlay.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`Overlay.Visible`](Overlay#overlayvisible)
[`Object.Transparency`](Object#objecttransparency)
[`Character.Transparency`](Character#charactertransparency),
[`GUI.Transparency`](GUI#guitransparency)

---

### `Overlay.Valid`

*(Formerly known as `IsOverlayValid`, which is now obsolete)*

```ags
readonly bool Overlay.Valid;
```

Returns whether the overlay is displaying on screen, or has already been removed. An overlay may be removed either by a script command, or by the engine when overlay's time have run out. When this happens, Overlay object remains in script, but overlay itself is gone. Overlay.Valid property allows to check its actual state. This is useful for overlays which are not controlled exclusively by your script, such as speech texts.

Example:

```ags
Overlay* myOverlay = Overlay.CreateTextual(50,80,120,2,15,"This is a text overlay");
Display("Overlay valid before: %d", myOverlay.Valid);
myOverlay.Remove();
Display("Overlay valid after: %d", myOverlay.Valid);
```

creates an overlay, and prints out the Valid property (which will be 1).
Then, removes the overlay and prints Valid again (which will now be 0).

*See also:* [`Overlay.CreateTextual`](Overlay#overlaycreatetextual),
[`Overlay.Remove`](Overlay#overlayremove)

---

### `Overlay.Visible`

```ags
readonly bool Overlay.Visible;
```

Gets/sets whether the overlay is visible. This is *true* by default, but you can set it to *false* in order to temporarily hide the Overlay.

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

*See also:* [`Overlay.Transparency`](Overlay#overlaytransparency)

---

### `Overlay.Width`

```ags
int Overlay.Width;
```

Get/sets the width of this overlay. Changing this property *will stretch or shrink* the overlay's image horizontally. Works with both graphical and textual overlays.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`Overlay.X`](Overlay#overlayx), [`Overlay.Y`](Overlay#overlayy), [`Overlay.Height`](Overlay#overlayheight)

---

### `Overlay.X`

*(Formerly known as `MoveOverlay`, which is now obsolete)*

```ags
int Overlay.X;
```

Gets/sets the X co-ordinate of the overlay (i.e. the left hand side of
the overlay).

This allows you to dynamically move overlays around the screen.

Example:

```ags
Overlay* testOverlay = Overlay.CreateTextual(50,80,120,2,15,"This is a text overlay");
while (testOverlay.X < 100) {
    testOverlay.X++;
    Wait(1);
}
testOverlay.Remove();
```

creates a text overlay, then gradually slides it across the screen.

*See also:* [`Overlay.CreateTextual`](Overlay#overlaycreatetextual),
[`Overlay.Y`](Overlay#overlayy),
[`Overlay.Remove`](Overlay#overlayremove)

---

### `Overlay.Y`

*(Formerly known as `MoveOverlay`, which is now obsolete)*

```ags
int Overlay.Y;
```

Gets/sets the Y co-ordinate of the overlay (i.e. the top edge of the
overlay).

This allows you to dynamically move overlays around the screen.

Example:

```ags
Overlay* testOverlay = Overlay.CreateTextual(50,50,120,2,15,"This is a text overlay");
while (testOverlay.Y < 100) {
    testOverlay.Y++;
    Wait(1);
}
testOverlay.Remove();
```

creates a text overlay, then gradually slides it down the screen.

*See also:* [`Overlay.CreateTextual`](Overlay#overlaycreatetextual),
[`Overlay.X`](Overlay#overlayx),
[`Overlay.Remove`](Overlay#overlayremove)

---

### `Overlay.ZOrder`

```ags
int Overlay.ZOrder;
```

Gets/sets the Overlay's drawing z-order. This allows you to dynamically change the ordering of Overlays on the screen.

Z-order setting is an arbitrary integer number that can be positive or negative. It tells how the overlapping objects should be sorted.
Those with lower z-order are drawn at the back, and those with higher z-order are drawn at the front.

The *screen* Overlays are sorted among themselves and GUI, and thus their ZOrder is relative to other screen Overlays z-orders and GUIs z-order values.

The *room* Overlays are sorted among themselves and other in-room elements, such as: room objects, characters and walk-behinds. Their ZOrder is relative to Character's and Object's Baseline setting (or Y coordinate, if Baseline is 0).

**IMPORTANT:** If two or more Overlays (or other objects) have equal ZOrder, their draw order is **undefined**, and should not be relied upon. This is because of how sprite sorting is done in the engine.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`GUI.ZOrder`](GUI#guizorder),
[`Character.Baseline`](Character#characterbaseline),
[`Object.Baseline`](Object#objectbaseline)
