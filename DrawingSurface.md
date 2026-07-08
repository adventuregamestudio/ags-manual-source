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

**NOTE:** Transparency parameter does not work in 256-color games, or with
256-color sprites.

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
[`DrawingSurface.DrawSurface`](DrawingSurface#drawingsurfacedrawsurface)

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
The pixel thickness respects the
[`UseHighResCoordinates`](DrawingSurface#drawingsurfaceusehighrescoordinates)
property.

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
[`DrawingSurface.UseHighResCoordinates`](DrawingSurface#drawingsurfaceusehighrescoordinates)

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

This function draws text as a single line, treating any linebreak characters as regular characters. If you need to draw a multi-line text, use [`DrawStringWrapped`](DrawingSurface#drawingsurfacedrawstringwrapped) instead.

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

You may use linebreak characters - `[` or `\n`, - for breaking the line of text at arbitrary position.

As of **AGS 3.6.1**, you can insert the value of variables into the text. 
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

**NOTE:** In high-color games, the first 32 color numbers have a
special meaning due to an AGS feature which maintains compatibility with
8-bit games. Therefore, if you draw onto the surface using a blue color
number 0-31 you will get a different number when you GetPixel -- and in
fact the color drawn may not be what you expect. To get around this,
add 1 Red or Green component to adjust the color number out of this
range.

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

*See also:*
[`DrawingSurface.DrawingColor`](DrawingSurface#drawingsurfacedrawingcolor),
[`DrawingSurface.DrawPixel`](DrawingSurface#drawingsurfacedrawpixel),
[`DrawingSurface.UseHighResCoordinates`](DrawingSurface#drawingsurfaceusehighrescoordinates)

---

### `DrawingSurface.GetPixelsCopy`

```ags
char[] DrawingSurface.GetPixelsCopy(optional int x, optional int y, optional int width, optional int height);
```

Returns a dynamic array of bytes, containing a copy of this surface's pixel data taken from the specified region. If no region parameters are provided, then the whole surface data is copied.
The pixels are arranged in the horizontal order starting from the top-left corner of the image.

The pixel data format depends on the surface's color depth:

  - 8-bit: each byte corresponds to a pixel and contains a palette index. The total size of the array = number of pixels in the copied region.
  - 16-bit: each sequence of 2 bytes correspond to a pixel. The color is stored in R5G6B5 format, which means 5 bits for Red, 6 bits for Green and 5 bits for Blue color component. In order to set or get individual components you should be using [bitwise operators](ScriptKeywords#operators), although it is easier to do if you use [`DrawingSurface.GetPixelsCopy16`](DrawingSurface#drawingsurfacegetpixelscopy16) function instead.
  - 32-bit: each sequence of 4 bytes correspond to a pixel. The color is stored in A8R8G8B8 format, which means that first byte is Alpha, second is Red, third is Green and fourth is Blue. For 32-bit surfaces you should also consider using [`DrawingSurface.GetPixelsCopy32`](DrawingSurface#drawingsurfacegetpixelscopy32) function instead.

Accessing specific pixel is done by converting image coordinates using this formula:

```ags
int index = y * (width * bpp) + x * bpp;
```

Where `bpp` is bytes per pixel, or surface's ColorDepth divided by 8 (1 for 8-bit, 2 for 16-bit and 4 for 32-bit surface).

Because copying whole pixel data is a *relatively* slow operation, this function is worth using when you need to perform a large number of reads or changes over image's pixels. When you only need to read or set a few pixels, it's much better to use [`GetPixel`](DrawingSurface#drawingsurfacegetpixel) and [`DrawPixel`](DrawingSurface#drawingsurfacedrawpixel) instead.

Example:

```ags
DrawingSurface* ds = GetDrawingSurfaceForWalkableArea();
char[] pixels = ds.GetPixelsCopy();
ds.Release();
int x, y;
for (int i = 0; i < pixels.Length; i++)
{
    if (pixels[i] == 10)
	{
	    x = i % ds.Width;
		y = i / ds.Width;
		break;
	}
}
```

will scan the walkable mask pixels until finding any pixel with color index 10 (belongs to walkable area 10), then save its coordinates in variables `x` and `y` for the further use.

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

*See also:* [`DrawingSurface.GetPixelsCopy16`](DrawingSurface#drawingsurfacegetpixelscopy16),
[`DrawingSurface.GetPixelsCopy32`](DrawingSurface#drawingsurfacegetpixelscopy32),
[`DrawingSurface.SetPixels`](DrawingSurface#drawingsurfacesetpixels),
['DrawingSurface.ColorDepth'](DrawingSurface#drawingsurfacecolordepth),
[`DrawingSurface.Height`](DrawingSurface#drawingsurfaceheight),
[`DrawingSurface.Width`](DrawingSurface#drawingsurfacewidth)

---

### `DrawingSurface.GetPixelsCopy16`

```ags
short[] DrawingSurface.GetPixelsCopy16(optional int x, optional int y, optional int width, optional int height);
```

Returns a dynamic array of 16-bit integers (aka `short`), containing a copy of this surface's pixel data taken from the specified region. If no region parameters are provided, then the whole surface data is copied.
The pixels are arranged in the horizontal order starting from the top-left corner of the image.

This function is meant to be used with 16-bit drawing surfaces. Using it with surfaces of any other formats (8 or 32-bit) will not cause any errors, but may not be convenient in practice.

With 16-bit surfaces the returned array contains direct pixel values (each element is a pixel). The color is stored in R5G6B5 format, which means 5 bits for Red, 6 bits for Green and 5 bits for Blue color component. In order to set or get individual components you should be using [bitwise operators](ScriptKeywords#operators), as demonstrated below:

```ags
char red = (pixel >> 11) & 0x1f;
char green = (pixel >> 5) & 0x3f;
char blue = (pixel) & 0x1f;
```

Because copying whole pixel data is a *relatively* slow operation, this function is worth using when you need to perform a large number of reads or changes over image's pixels at once. When you only need to read or set a few pixels, it's much better to use [`GetPixel`](DrawingSurface#drawingsurfacegetpixel) and [`DrawPixel`](DrawingSurface#drawingsurfacedrawpixel) instead.

Example:

```ags
short[] pixels = ds.GetPixelsCopy16();
short pixel = pixels[10];
char blue = pixel & 0x1f;
```

will get the 10th pixel of this surface (counting from the top-left), and retrieve its Blue color component.

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

*See also:* [`DrawingSurface.GetPixelsCopy`](DrawingSurface#drawingsurfacegetpixelscopy),
[`DrawingSurface.GetPixelsCopy32`](DrawingSurface#drawingsurfacegetpixelscopy32),
[`DrawingSurface.SetPixels16`](DrawingSurface#drawingsurfacesetpixels16),
['DrawingSurface.ColorDepth'](DrawingSurface#drawingsurfacecolordepth),
[`DrawingSurface.Height`](DrawingSurface#drawingsurfaceheight),
[`DrawingSurface.Width`](DrawingSurface#drawingsurfacewidth)

---

### `DrawingSurface.GetPixelsCopy32`

```ags
int[] DrawingSurface.GetPixelsCopy32(optional int x, optional int y, optional int width, optional int height);
```

Returns a dynamic array of integers, containing a copy of this surface's pixel data taken from the specified region. If no region parameters are provided, then the whole surface data is copied.
The pixels are arranged in the horizontal order starting from the top-left corner of the image.

This function is meant to be used with 32-bit drawing surfaces. Using it with surfaces of any other formats (8 or 16-bit) will not cause any errors, but may not be convenient in practice.

With 32-bit surfaces the returned array contains direct pixel values (each element is a pixel). The color is stored in A8R8G8B8 format, which means 8 bits for Alpha, 8 bits for Red, 8 bits for Green and 8 bits for Blue color component. In order to set or get individual components you should be using [bitwise operators](ScriptKeywords#operators), as demonstrated below:

```ags
char alpha = (pixel >> 24) & 0xff;
char red = (pixel >> 16) & 0xff;
char green = (pixel >> 8) & 0xff;
char blue = (pixel) & 0xff;
```

Because copying whole pixel data is a *relatively* slow operation, this function is worth using when you need to perform a large number of reads or changes over image's pixels. When you only need to read or set a few pixels, it's much better to use [`GetPixel`](DrawingSurface#drawingsurfacegetpixel) and [`DrawPixel`](DrawingSurface#drawingsurfacedrawpixel) instead.

Example:

```ags
short[] pixels = ds.GetPixelsCopy32();
int pixel = pixels[10];
char red = (pixel >> 16) & 0xff;
```

will get the 10th pixel of this surface (counting from the top-left), and retrieve its Red color component.

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

*See also:* [`DrawingSurface.GetPixelsCopy`](DrawingSurface#drawingsurfacegetpixelscopy),
[`DrawingSurface.GetPixelsCopy16`](DrawingSurface#drawingsurfacegetpixelscopy16),
[`DrawingSurface.SetPixels32`](DrawingSurface#drawingsurfacesetpixels32),
['DrawingSurface.ColorDepth'](DrawingSurface#drawingsurfacecolordepth),
[`DrawingSurface.Height`](DrawingSurface#drawingsurfaceheight),
[`DrawingSurface.Width`](DrawingSurface#drawingsurfacewidth)

---

### `DrawingSurface.SetPixels`

```ags
void DrawingSurface.SetPixels(char pixels[], optional int x, optional int y, optional int width, optional int height);
```

Copies the pixel data from the provided dynamic array of bytes onto the specified region of the drawing surface. Pixels are copied until reaching either the end of array or the region's boundaries. If no region parameters are provided, then will try to fill the whole surface, so long as the array has enough data.

The pixel data in array is supposed to match the DrawingSurface's color depth:

  - 8-bit: each byte corresponds to a pixel and contains a palette index. The total size of the array = number of pixels in the copied region.
  - 16-bit: each sequence of 2 bytes correspond to a pixel. The color is stored in R5G6B5 format, which means 5 bits for Red, 6 bits for Green and 5 bits for Blue color component. It is easier if you use [`DrawingSurface.SetPixels16`](DrawingSurface#drawingsurfacesetpixels16) function instead.
  - 32-bit: each sequence of 4 bytes correspond to a pixel. The color is stored in A8R8G8B8 format, which means that first byte is Alpha, second is Red, third is Green and fourth is Blue. For 32-bit surfaces you should also consider using [`DrawingSurface.SetPixels32`](DrawingSurface#drawingsurfacesetpixels32) function instead.
  
If the data format does not match, there will be no error reported, but the looks of the image may become broken.

Because copying whole pixel data is a *relatively* slow operation, this function is worth using when you need to perform a large number of changes over image's pixels at once. When you only need to set a few pixels, it's much better to use [`DrawPixel`](DrawingSurface#drawingsurfacedrawpixel) instead.

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

*See also:* [`DrawingSurface.GetPixelsCopy`](DrawingSurface#drawingsurfacegetpixelscopy),
[`DrawingSurface.SetPixels16`](DrawingSurface#drawingsurfacesetpixels16),
[`DrawingSurface.SetPixels32`](DrawingSurface#drawingsurfacesetpixels32),
['DrawingSurface.ColorDepth'](DrawingSurface#drawingsurfacecolordepth),
[`DrawingSurface.Height`](DrawingSurface#drawingsurfaceheight),
[`DrawingSurface.Width`](DrawingSurface#drawingsurfacewidth)

---

### `DrawingSurface.SetPixels16`

```ags
void DrawingSurface.SetPixels16(short pixels[], optional int x, optional int y, optional int width, optional int height);
```

Copies the pixel data from the provided dynamic array of 16-bit integers (aka `short`) onto the specified region of the drawing surface. Pixels are copied until reaching either the end of array or the region's boundaries. If no region parameters are provided, then will try to fill the whole surface, so long as the array has enough data.

This function is meant to be used with 16-bit drawing surfaces. Using it with surfaces of any other formats (8 or 32-bit) will not cause any errors, but may not be convenient in practice.

With 16-bit surfaces the array contains direct pixel values (each element is a pixel). The color is stored in R5G6B5 format, which means 5 bits for Red, 6 bits for Green and 5 bits for Blue color component. In order to set individual components you should be using [bitwise operators](ScriptKeywords#operators), as demonstrated below:

```ags
int pixel =
    ((blue >> 3) & 0x1f) |
    ((green >> 2) & 0x3f) << 5 |;
    ((red >> 3) & 0x1f) << 11;
```
  
If the data format does not match, there will be no error reported, but the looks of the image may become broken.

Because copying whole pixel data is a *relatively* slow operation, this function is worth using when you need to perform a large number of changes over image's pixels at once. When you only need to set a few pixels, it's much better to use [`DrawPixel`](DrawingSurface#drawingsurfacedrawpixel) instead.

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

*See also:* [`DrawingSurface.GetPixelsCopy16`](DrawingSurface#drawingsurfacegetpixelscopy16),
[`DrawingSurface.SetPixels`](DrawingSurface#drawingsurfacesetpixels),
[`DrawingSurface.SetPixels32`](DrawingSurface#drawingsurfacesetpixels32),
['DrawingSurface.ColorDepth'](DrawingSurface#drawingsurfacecolordepth),
[`DrawingSurface.Height`](DrawingSurface#drawingsurfaceheight),
[`DrawingSurface.Width`](DrawingSurface#drawingsurfacewidth)

---

### `DrawingSurface.SetPixels32`

```ags
void DrawingSurface.SetPixels32(int pixels[], optional int x, optional int y, optional int width, optional int height);
```

Copies the pixel data from the provided dynamic array of integers onto the specified region of the drawing surface. Pixels are copied until reaching either the end of array or the region's boundaries. If no region parameters are provided, then will try to fill the whole surface, so long as the array has enough data.

This function is meant to be used with 32-bit drawing surfaces. Using it with surfaces of any other formats (8 or 16-bit) will not cause any errors, but may not be convenient in practice.

With 32-bit surfaces the array contains direct pixel values (each element is a pixel). The color is stored in A8R8G8B8 format, which means 8 bits for Alpha, 8 bits for Red, 8 bits for Green and 8 bits for Blue color component. In order to set or get individual components you should be using [bitwise operators](ScriptKeywords#operators), as demonstrated below:

```ags
int pixel = 
	(alpha & 0xff) << 24 |
    (red & 0xff) << 16 |
	(green & 0xff) << 8 |
	(blue & 0xff);
```

If the data format does not match, there will be no error reported, but the looks of the image may become broken.

Example:

```ags
DrawingSurface* ds = Room.GetDrawingSurfaceForBackground();
int pixels[] = ds.GetPixelsCopy32(100, 100, 50, 50);
for (int i = 0; i < pixels.Length; i++)
{
    pixels[i] = pixels[i] & 0x00FF0000;
}
ds.SetPixels32(pixels, 150, 100, 50);
ds.Release();
```

will take a copy of the room background's pixels from the region starting at (100,100), 50 pixels wide and 50 pixels high, convert every pixel color to a hue of red, and apply the result back.

Because copying whole pixel data is a *relatively* slow operation, this function is worth using when you need to perform a large number of changes over image's pixels at once. When you only need to set a few pixels, it's much better to use [`DrawPixel`](DrawingSurface#drawingsurfacedrawpixel) instead.

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

*See also:* [`DrawingSurface.GetPixelsCopy32`](DrawingSurface#drawingsurfacegetpixelscopy32),
[`DrawingSurface.SetPixels`](DrawingSurface#drawingsurfacesetpixels),
[`DrawingSurface.SetPixels16`](DrawingSurface#drawingsurfacesetpixels16),
['DrawingSurface.ColorDepth'](DrawingSurface#drawingsurfacecolordepth),
[`DrawingSurface.Height`](DrawingSurface#drawingsurfaceheight),
[`DrawingSurface.Width`](DrawingSurface#drawingsurfacewidth)

---

### `DrawingSurface.ColorDepth`

```ags
readonly int DrawingSurface.ColorDepth
```

Gets the color depth of the surface, in bits per pixel (8, 16, or 32).

*See also:* [`DrawingSurface.Height`](DrawingSurface#drawingsurfaceheight)
[`DrawingSurface.Width`](DrawingSurface#drawingsurfacewidth)

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

*See also:* [`DrawingSurface.ColorDepth`](DrawingSurface#drawingsurfacecolordepth)
[`DrawingSurface.Width`](DrawingSurface#drawingsurfacewidth)

---

### `DrawingSurface.UseHighResCoordinates`

**This property is obsolete since AGS 3.5.0 and not recommended for use at all.**

```ags
bool DrawingSurface.UseHighResCoordinates
```

Gets/sets whether you want to use high-resolution co-ordinates with this
surface.

By default, this property will be set such that drawing surface
co-ordinates use the same co-ordinate system as the rest of the game, as
per the "Use low-res co-ordinates in script" game setting. However, if
your game is 640x400 or higher you can customize whether this drawing
surface uses native co-ordinates or the low-res 320x200 co-ordinates by
changing this property.

Setting this property affects **ALL** other commands performed on this
drawing surface, including the [`Width`](DrawingSurface#drawingsurfacewidth)
and [`Height`](DrawingSurface#drawingsurfaceheight) properties.

**IMPORTANT:** This property is a remnant of the old and since deprecated feature in AGS which allowed to treat all coordinates in high-resolution games as if they were for low-resolution. For example: have 640x400 game but use 320x200 measurements in script, which would make each drawing operation to be performed x2 thicker on screen.<br>
Since AGS 3.5.0 this property is ignored unless you have backwards-compatible "Allow relative asset resolutions" option enabled in General Settings.

---

### `DrawingSurface.Valid`

```ags
readonly bool DrawingSurface.Valid
```

Gets whether the drawing surface is valid to be used, and is actually linked to a existing image.

The drawing surface may become invalid if it was acquired from a dynamic sprite and that sprite got deleted, or if it was acquired from a room background but the room was changed. Any operation on an invalid DrawingSurface will be ignored.
In general, it is not recommended to keep DrawingSurface objects for a prolonged period of time, nor store them in a global variable. But if you do so for some reasons, and there's a chance that the underlying image is disposed, then this property may help to double check if it's safe to still use the surface for drawing.

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

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

*See also:* [`DrawingSurface.ColorDepth`](DrawingSurface#drawingsurfacecolordepth),
[`DrawingSurface.Height`](DrawingSurface#drawingsurfaceheight)

