## `DrawingSurface` functions and properties

The DrawingSurface family of functions allow you to directly draw onto
dynamic sprites and room backgrounds in the game. You get a drawing
surface by calling
[`DynamicSprite.GetDrawingSurface`](DynamicSprite#dynamicspritegetdrawingsurface)
or
[`Room.GetDrawingSurfaceForBackground`](Room#roomgetdrawingsurfaceforbackground),
and you can then use the following methods to draw onto the surface.

**IMPORTANT:** You **MUST** call the
[`Release`](DrawingSurface#drawingsurfacerelease) method when you have
finished drawing onto the surface. This allows AGS to update its cached
copies of the image and upload it to video memory if appropriate.


---

### `DrawingSurface.BlendImage`

```ags
DrawingSurface.BlendImage(int x, int y, int spriteSlot, BlendMode mode, optional int transparency, optional int width, optional int height, optional int part_x, optional int part_y, optional int part_width, optional int part_height)
```

Blends a sprite onto the surface with its top-left corner at (x,y).

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:*
[`DrawingSurface.BlendSurface`](DrawingSurface#drawingsurfaceblendsurface),
[`BlendMode`](StandardEnums#blendmode)

---

### `DrawingSurface.BlendMode`

```ags
BlendMode DrawingSurface.BlendMode
```

Gets/sets the current BlendMode that will be used for drawing onto this surface.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [`BlendMode`](StandardEnums#blendmode)

---

### `DrawingSurface.BlendSurface`

```ags
DrawingSurface.BlendSurface(DrawingSurface *surfaceToDraw, BlendMode mode, optional int transparency, optional int x, optional int y, optional int width, optional int height, optional int part_x, optional int part_y, optional int part_width, optional int part_height)
```

Blends the specified surface onto this surface.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:*
[`DrawingSurface.BlendImage`](DrawingSurface#drawingsurfaceblendimage),
[`BlendMode`](StandardEnums#blendmode)

---

### `DrawingSurface.Clear`

*(Formerly known as `RawClearScreen`, which is now obsolete)*

```ags
DrawingSurface.Clear(optional int color)
```

Clears the surface to the specified COLOR (this is a number you can
find in the Colors pane of the editor). The current contents of the
surface will be lost.

If you do not supply the COLOR parameter, or use COLOR_TRANSPARENT,
the surface will be cleared to be fully transparent.

Example:

```ags
DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
surface.Clear(14);
surface.DrawingColor = 13;
surface.DrawCircle(160,100,50);
surface.Release();
```

clears the room background to be fully yellow, then draws a pink circle
in the middle of it.

*See also:*
[`DrawingSurface.DrawingColor`](DrawingSurface#drawingsurfacedrawingcolor)

---

### `DrawingSurface.ColorDepth`

```ags
readonly int DrawingSurface.ColorDepth
```

Gets the colour depth of this surface.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `DrawingSurface.CreateCopy`

*(Formerly known as `RawSaveScreen`, which is now obsolete)*

```ags
DrawingSurface* DrawingSurface.CreateCopy()
```

Makes a backup copy of the current surface, in order that it can be
restored later. This could be useful to back up a background scene
before writing over it, or to save a certain state of your drawing to
restore later.

Unlike the obsolete RawSaveScreen command in previous versions of AGS,
backup surfaces created with this command are not lost when the player
changes room or restores a game. However, surfaces containing a copy of
room backgrounds can be **very large**, using up a large amount of
memory and can increase the save game sizes significantly. Therefore, it
is **strongly recommended** that you Release any backup copy surfaces as
soon as you are done with them.

Example:

```ags
DrawingSurface *backup = surface.CreateCopy();
DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
surface.DrawTriangle(0,0,160,100,0,200);
surface.Release();
Wait(80);
surface = Room.GetDrawingSurfaceForBackground();
surface.DrawSurface(backup);
surface.Release();
backup.Release();
```

will save a copy of the room background, draw a triangle onto it, wait
for a while and then restore the original background.

*See also:*
[`DrawingSurface.DrawSurface`](DrawingSurface#drawingsurfacedrawsurface)

---

### `DrawingSurface.DrawCircle`

*(Formerly known as `RawDrawCircle`, which is now obsolete)*

```ags
DrawingSurface.DrawCircle(int x, int y, int radius)
```

Draws a filled circle of radius RADIUS with its center at (X,Y) in the
current drawing color.

Example:

```ags
DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
surface.DrawingColor = 14;
surface.DrawCircle(160,100,50);
surface.Release();
```

will draw a circle in the center of the screen, of 50 pixels radius.

*See also:*
[`DrawingSurface.DrawLine`](DrawingSurface#drawingsurfacedrawline),
[`DrawingSurface.DrawingColor`](DrawingSurface#drawingsurfacedrawingcolor)

---

### `DrawingSurface.DrawImage`

*(Formerly known as `RawDrawImage`, which is now obsolete)*<br>
*(Formerly known as `RawDrawImageResized`, which is now obsolete)*<br>
*(Formerly known as `RawDrawImageTransparent`, which is now obsolete)*

```ags
DrawingSurface.DrawImage(int x, int y, int slot, optional int transparency,
                         optional int width, optional int height,
                         optional int cut_x, optional  int cut_y, 
                         optional int cut_width, optional int cut_height)
```

Draws image SLOT from the sprite manager onto the surface at location
(X,Y).

Optionally, you can also specify the transparency of the image. This is
a number from 0-100; using a *transparency* of 50 will draw the image
semi-transparent; using 0 means it will not be transparent.

You can also resize the image as you draw it. In order to do this,
simply specify a *width* and *height* that you wish to resize the image
to when it is drawn.

As of **AGS 3.6.0**, you can also cut the original image in a specific
rectangle using it's x,y position and width and height.

**NOTE:** This command only works if the image to be drawn is the same
color depth as the surface that you are drawing onto.

**NOTE:** Transparency does not work in 256-color games, or with
256-color sprites.

**NOTE:** The X and Y co-ordinates given are ROOM co-ordinates, not
SCREEN co-ordinates. This means that in a scrolling room you can draw
outside the current visible area.

Example:

```ags
DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
surface.DrawImage(100, 100, oDoor.Graphic, 40);
surface.Release();
```

will draw the *oDoor* object's graphic onto the room background at (100,
100), at `40%` transparency.

*See also:*
[`DrawingSurface.DrawLine`](DrawingSurface#drawingsurfacedrawline),
[`DrawingSurface.DrawString`](DrawingSurface#drawingsurfacedrawstring),
[`DrawingSurface.DrawSurface`](DrawingSurface#drawingsurfacedrawsurface),
[`Room.ColorDepth`](Room#roomcolordepth)

---

### `DrawingSurface.DrawLine`

*(Formerly known as `RawDrawLine`, which is now obsolete)*

```ags
DrawingSurface.DrawLine(int from_x, int from_y, int to_x, int to_y,
                        optional int thickness)
```

Draws a line from (FROM_X, FROM_Y) to (TO_X, TO_Y) in the surface's
current drawing color.

The *thickness* parameter allows you to specify how thick the line is,
the default being 1 pixel.

**NOTE:** The X and Y co-ordinates given are ROOM co-ordinates, not
SCREEN co-ordinates. This means that in a scrolling room you can draw
outside the current visible area.

Example:

```ags
DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
surface.DrawingColor = 14;
surface.DrawLine(0, 0, 160, 100);
surface.Release();
```

will draw a line from the left top of the screen (0,0) to the middle of
the screen (160,100);

*See also:*
[`DrawingSurface.DrawCircle`](DrawingSurface#drawingsurfacedrawcircle),
[`DrawingSurface.DrawRectangle`](DrawingSurface#drawingsurfacedrawrectangle),
[`DrawingSurface.DrawTriangle`](DrawingSurface#drawingsurfacedrawtriangle),
[`DrawingSurface.DrawingColor`](DrawingSurface#drawingsurfacedrawingcolor)

---

### `DrawingSurface.DrawMessageWrapped`

*(Formerly known as `RawPrintMessageWrapped`, which is now obsolete)*

```ags
DrawingSurface.DrawMessageWrapped(int x, int y, int width,
                                  FontType font, int message_number)
```

Draws the room message MESSAGE_NUMBER onto the surface at (x,y), using
the specified FONT.

WIDTH is the width of the virtual textbox enclosing the text, and is the
point that the text will wrap at. This command is designed for writing a
long message to the screen with it wrapping normally like a standard
label would do.

The text will be printed using the current drawing color.

Example:

```ags
DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
surface.DrawingColor = 14;
surface.DrawMessageWrapped(80, 40, 160, Game.NormalFont, 10);
surface.Release();
```

will display message 10 in the center of the screen, starting from Y =
40.

*See also:*
[`DrawingSurface.DrawString`](DrawingSurface#drawingsurfacedrawstring),
[`DrawingSurface.DrawingColor`](DrawingSurface#drawingsurfacedrawingcolor),
[`DrawingSurface.DrawStringWrapped`](DrawingSurface#drawingsurfacedrawstringwrapped)

---

### `DrawingSurface.DrawPixel`

```ags
DrawingSurface.DrawPixel(int x, int y)
```

Draws a single pixel onto the surface at (X,Y) in the current color.

**NOTE:** This command is not fast enough to use repeatedly to build up
an image. Only use it for single pixel adjustments.

Example:

```ags
DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
surface.DrawingColor = 14;
surface.DrawPixel(50, 50);
surface.Release();
```

draws a yellow pixel in the top left of the room background

*See also:*
[`DrawingSurface.DrawingColor`](DrawingSurface#drawingsurfacedrawingcolor),
[`DrawingSurface.DrawLine`](DrawingSurface#drawingsurfacedrawline),
[`DrawingSurface.GetPixel`](DrawingSurface#drawingsurfacegetpixel),
[`DrawingSurface.SetPixel`](DrawingSurface#drawingsurfacesetpixel)

---

### `DrawingSurface.DrawRectangle`

*(Formerly known as `RawDrawRectangle`, which is now obsolete)*

```ags
DrawingSurface.DrawRectangle(int x1, int y1, int x2, int y2)
```

Draws a filled rectangle in the current color with its top-left corner
at (x1,y1) and its bottom right corner at (x2, y2)

**NOTE:** The X and Y co-ordinates given are ROOM co-ordinates, not
SCREEN co-ordinates. This means that in a scrolling room you can draw
outside the current visible area.

Example:

```ags
DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
surface.DrawingColor = 14;
surface.DrawRectangle(0, 0, 160, 100);
surface.Release();
```

will draw a rectangle over the top left hand quarter of the screen.

*See also:*
[`DrawingSurface.DrawImage`](DrawingSurface#drawingsurfacedrawimage),
[`DrawingSurface.DrawLine`](DrawingSurface#drawingsurfacedrawline)

---

### `DrawingSurface.DrawString`

*(Formerly known as `RawPrint`, which is now obsolete)*

```ags
DrawingSurface.DrawString(int x, int y, FontType font, string text, ...)
```

Draws the *text* onto the surface at (x, y), using the supplied font
number. The text will be drawn in the current drawing color.

You can insert the value of variables into the message. For more
information, see the [string formatting](StringFormats)
section.

Example:

```ags
DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
surface.DrawingColor = 14;
surface.DrawString(0, 100, Game.NormalFont, "Text written into the background!");
surface.Release();
```

will write some text onto the middle-left of the room background

*See also:* [`GetTextWidth`](Globalfunctions_General#gettextwidth),
[`DrawingSurface.DrawStringWrapped`](DrawingSurface#drawingsurfacedrawstringwrapped),
[`DrawingSurface.DrawingColor`](DrawingSurface#drawingsurfacedrawingcolor)

---

### `DrawingSurface.DrawStringWrapped`

```ags
DrawingSurface.DrawStringWrapped(int x, int y, int width,
                                 FontType font, Alignment,
                                 const string text, ...)
```

Draws the *text* onto the surface at (x,y), using the specified FONT.

*width* is the width of the virtual textbox enclosing the text, and is
the point that the text will wrap at. You can use the *alignment*
parameter to determine how the text is horizontally aligned.

The text will be printed using the current drawing color.

As of **AGS 3.6.1**, you can insert the value of variables into the message. 
For more information, see the [string formatting](StringFormats) section.

Example:

```ags
DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
surface.DrawingColor = 14;
surface.DrawStringWrapped(80, 40, 160, Game.NormalFont, eAlignCentre, "Hello, my name is Bob.");
surface.Release();
```

will display the text in the center of the screen, starting from Y = 40.

*Compatibility:* Supported by **AGS 3.0.1** and later versions.

*See also:*
[`DrawingSurface.DrawString`](DrawingSurface#drawingsurfacedrawstring),
[`DrawingSurface.DrawingColor`](DrawingSurface#drawingsurfacedrawingcolor),
[`DrawingSurface.DrawMessageWrapped`](DrawingSurface#drawingsurfacedrawmessagewrapped)

---

### `DrawingSurface.DrawSurface`

*(Formerly known as `RawDrawFrameTransparent`, which is now obsolete)*<br>
*(Formerly known as `RawRestoreScreen`, which is now obsolete)*

```ags
DrawingSurface.DrawSurface(DrawingSurface* source, optional int transparency,
                           optional int x, optional int y, 
                           optional int width, optional int height,
                           optional int cut_x, optional  int cut_y, 
                           optional int cut_width, optional int cut_height)
```

Draws the specified surface on top of this surface, optionally using
*transparency* percent transparency. As of **AGS 3.6.0**, it also supports additional
parameters to draw a piece of the surface onto a specific area in the destination surface.

This allows you to perform day-to-night fading and other special
effects.

**NOTE:** You cannot use the *transparency* parameter with 256-color
surfaces.

**NOTE:** This command can be a bit on the slow side, so don't call it
from repeatedly_execute.

**TIP:** If you want to gradually fade in a second background, create a
copy of the original surface and then restore it after each iteration,
otherwise the backgrounds will converge too quickly.

Example:

```ags
DrawingSurface *mainBackground = Room.GetDrawingSurfaceForBackground(0);
DrawingSurface *nightBackground = Room.GetDrawingSurfaceForBackground(1);
mainBackground.DrawSurface(nightBackground, 50);
mainBackground.Release();
nightBackground.Release();
```

this will draw background frame 1 onto frame 0 at 50`%` opacity.

*See also:*
[`DrawingSurface.DrawImage`](DrawingSurface#drawingsurfacedrawimage),
[`SetAmbientTint`](Globalfunctions_General#setambienttint)

---

### `DrawingSurface.DrawTriangle`

*(Formerly known as `RawDrawTriangle`, which is now obsolete)*

```ags
DrawingSurface.DrawTriangle(int x1, int y1, int x2, int y2, int x3, int y3)
```

Draws a filled triangle in the current color with corners at the points
(x1,y1), (x2,y2) and (x3,y3).

Example:

```ags
DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
surface.DrawingColor = 14;
surface.DrawTriangle(0,0,160,100,0,200);
surface.Release();
```

will draw a triangle with corners at the points (0,0),(160,100),(0,200).

*See also:*
[`DrawingSurface.DrawImage`](DrawingSurface#drawingsurfacedrawimage),
[`DrawingSurface.DrawLine`](DrawingSurface#drawingsurfacedrawline),
[`DrawingSurface.DrawRectangle`](DrawingSurface#drawingsurfacedrawrectangle)

---

### `DrawingSurface.Release`

```ags
DrawingSurface.Release()
```

Tells AGS that you have finished drawing onto this surface, and that AGS
can now upload the changed image into video memory.

After calling this method, you can no longer use the DrawingSurface
instance. To do any further drawing, you need to get the surface again.

Example:

```ags
DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
surface.DrawingColor = 14;
surface.DrawLine(0, 0, 50, 50);
surface.Release();
```

draws a yellow diagonal line across the top-left of the current room
background, then releases the image.

*See also:*
[`DynamicSprite.GetDrawingSurface`](DynamicSprite#dynamicspritegetdrawingsurface),
[`Room.GetDrawingSurfaceForBackground`](Room#roomgetdrawingsurfaceforbackground)

---

### `DrawingSurface.DrawingColor`

*(Formerly known as `RawSetColor`, which is now obsolete)*

```ags
int DrawingSurface.DrawingColor
```

Gets/sets the current drawing color on this surface. Set this before
using commands like [`DrawLine`](DrawingSurface#drawingsurfacedrawline), which
use this color for their drawing.

You can set this either to an AGS Color Number (as you'd get from the
Colors pane in the editor) or to the special constant
COLOR_TRANSPARENT, which allows you to draw transparent areas onto the
surface.

Example:

```ags
DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
surface.DrawingColor = 14;
surface.DrawLine(0, 0, 160, 100);
surface.DrawingColor = Game.GetColorFromRGB(255, 255, 255);
surface.DrawLine(0, 199, 160, 100);
surface.Release();
```

will draw a yellow line from the left top of the screen (0,0) to the
middle of the screen (160,100), and a white line from the bottom left to
the middle.

*See also:*
[`DrawingSurface.DrawCircle`](DrawingSurface#drawingsurfacedrawcircle),
[`DrawingSurface.DrawLine`](DrawingSurface#drawingsurfacedrawline),
[`DrawingSurface.DrawRectangle`](DrawingSurface#drawingsurfacedrawrectangle),
[`Game.GetColorFromRGB`](Game#gamegetcolorfromrgb)

---

### `DrawingSurface.GetPixel`

```ags
int DrawingSurface.GetPixel(int x, int y)
```

Returns the AGS Color Number of the pixel at (X,Y) on the surface.

**NOTE:** This command is relatively slow. Don't use it to try and
process an entire image.

Example:

```ags
DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
Display("The color of the middle pixel is %d.", surface.GetPixel(160, 100));
surface.Release();
```

displays the pixel color of the center pixel on the screen.

*Compatibility:* Supported by **AGS 3.0.1** and later versions.
Since **AGS 4.0.0** 32-bit colors are in the format `0xAARRGGBB`.

*See also:*
[`DrawingSurface.DrawingColor`](DrawingSurface#drawingsurfacedrawingcolor),
[`DrawingSurface.DrawPixel`](DrawingSurface#drawingsurfacedrawpixel),
[`DrawingSurface.SetPixel`](DrawingSurface#drawingsurfacesetpixel)

---

### `DrawingSurface.SetPixel`

```ags
DrawingSurface.SetPixel(int x, int y, int color)
```

Changes the colour of a single pixel on the surface. This operation ignores drawing settings, and simply sets the color value.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:*
[`DrawingSurface.DrawingColor`](DrawingSurface#drawingsurfacedrawingcolor),
[`DrawingSurface.DrawPixel`](DrawingSurface#drawingsurfacedrawpixel),
[`DrawingSurface.GetPixel`](DrawingSurface#drawingsurfacegetpixel)

---

### `DrawingSurface.Height`

```ags
readonly int DrawingSurface.Height
```

Gets the height of the surface.

Example:

```ags
DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
Display("The background is %d x %d!", surface.Width, surface.Height);
surface.Release();
```

displays the size of the surface to the player

*See also:*
[`DrawingSurface.Width`](DrawingSurface#drawingsurfacewidth)

---

### `DrawingSurface.Width`

```ags
readonly int DrawingSurface.Width
```

Gets the width of the surface.

Example:

```ags
DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
Display("The background is %d x %d!", surface.Width, surface.Height);
surface.Release();
```

displays the size of the surface to the player

*See also:* [`DrawingSurface.Height`](DrawingSurface#drawingsurfaceheight)

