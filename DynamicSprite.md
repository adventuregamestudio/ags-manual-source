## `DynamicSprite` functions and properties

DynamicSprite lets you create a new sprite dynamically at runtime,
either a new blank sprite, a copy of the existing one, or load one from a file.
DynamicSprites may be modified in various ways, and drawn upon. The latter is done by
retrieving a [`DrawingSurface`](DrawingSurface) with [`GetDrawingSurface()`](DynamicSprite#dynamicspritegetdrawingsurface) function.

Each new DynamicSprite is assigned an arbitrary ID, which may be read with [`Graphic`](DynamicSprite#dynamicspritegraphic) property.
This ID lets you assign DynamicSprite to any property that normally accepts a sprite number, or pass into any function that
accepts a sprite number as a parameter.

**IMPORTANT:** DynamicSprite exist so long as it is assigned to at least one `DynamicSprite*` variable in your script.
As soon as there's no variable that has a reference to it, the sprite is disposed.
This also means that if you only have a local `DynamicSprite*` variable, then the sprite will be disposed
when the function ends. If you want to keep the sprite, then you should store it in a global variable for as long as you need it.

You may also call [`Delete`](DynamicSprite#dynamicspritedelete) function anytime to explicitly remove the sprite's image from memory.

**IMPORTANT:** Assigning DynamicSprite's ID to another object (such as `Button.NormalGraphic`, for example)
does not count as a reference. If DynamicSprite is deleted while its ID is assigned to a object's property,
that object's property will retain the invalid sprite's number, but the object will be displayed using placeholder sprite 0 for safety reasons.


### `DynamicSprite.Create`

```ags
static DynamicSprite* DynamicSprite.Create(int width, int height,
                                           optional bool hasAlphaChannel)
```

Creates a new blank dynamic sprite of the specified size. It will
initially be fully transparent, and can optionally have an alpha
channel. This command is useful if you just want to create a new sprite
and then use the DrawingSurface commands to draw onto it.

If the game color depth is lower than 32-bit, then the
*hasAlphaChannel* parameter will be ignored.

Use the [`Graphic`](DynamicSprite#dynamicspritegraphic) property of the
DynamicSprite to interface with other commands and to use the new sprite
in the game.

Example:

```ags
// Declare a variable in a global scope (outside of any function)
DynamicSprite* globalSprite;

// Then in a function...
DynamicSprite* sprite = DynamicSprite.Create(50, 30);
DrawingSurface *surface = sprite.GetDrawingSurface();
surface.DrawingColor = 14;
surface.DrawPixel(25, 15);
surface.Release();
oMyObject.Graphic = sprite.Graphic;
globalSprite = sprite; // keep the sprite in a global variable
```

creates a 50x30 sprite, draws a white dot in the middle, then assigns to a global variable to keep the sprite, and sets this sprite's ID to an Object.

*See also:* [`DynamicSprite.Delete`](DynamicSprite#dynamicspritedelete),
[`DynamicSprite.Graphic`](DynamicSprite#dynamicspritegraphic),
[`DynamicSprite.GetDrawingSurface`](DynamicSprite#dynamicspritegetdrawingsurface)

---

### `DynamicSprite.CreateFromBackground`

```ags
static DynamicSprite* DynamicSprite.CreateFromBackground(
    optional int frame, optional int x, optional int y,
    optional int width, optional int height)
```

Creates a new dynamic sprite containing a copy of the specified room
background.

The most basic use of this function is to supply no parameters, in which
case the sprite will contain an exact copy of the current room
background.

If you want, you can supply the *frame* only, in which case you will get
a complete copy of that background frame number from the current room.

Optionally, you can specify a portion of the background to grab. You
must either supply all or none of the x, y, width and height parameters;
if you do supply them, this allows you to just get a small portion of
the background image into the new sprite. All co-ordinates are room co-ordinates.

Use the [`Graphic`](DynamicSprite#dynamicspritegraphic) property of the
DynamicSprite to interface with other commands and to use the new sprite
in the game.

Example:

```ags
DynamicSprite* sprite = DynamicSprite.CreateFromBackground(GetBackgroundFrame(), 130, 70, 60, 60);
DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
surface.DrawImage(0, 0, sprite.Graphic);
surface.Release();
sprite.Delete();
```

creates a copy of the center 60x60 area of the current room's background, and draws it
onto the top left corner of the background image. Then disposes the temporary sprite.

*See also:* [`DynamicSprite.Delete`](DynamicSprite#dynamicspritedelete)

---

### `DynamicSprite.CreateFromDrawingSurface`

```ags
static DynamicSprite* DynamicSprite.CreateFromDrawingSurface(
    DrawingSurface* surface, int x, int y,
    int width, int height)
```

Creates a new dynamic sprite containing a copy of the specified portion
of the drawing surface. This allows you to easily create new sprites
from portions of other sprites.

**NOTE:** The *x*, *y*, *width* and *height* parameters respect the
DrawingSurface's
[`UseHighResCoordinates`](DrawingSurface#drawingsurfaceusehighrescoordinates)
setting, so make sure that the type of co-ordinates that you are using
match up with what the drawing surface expects.

Use the [`Graphic`](DynamicSprite#dynamicspritegraphic) property of the
DynamicSprite to interface with other commands and to use the new sprite
in the game.

Example:

```ags
// Declare a variable in a global scope (outside of any function)
DynamicSprite* globalSprite;

// Then in a function...
DynamicSprite* sprite = DynamicSprite.CreateFromExistingSprite(oMyObject.Graphic);
DrawingSurface *surface = sprite.GetDrawingSurface();
DynamicSprite *newSprite = DynamicSprite.CreateFromDrawingSurface(surface, 0, 0, 10, 10);
surface.Release();
sprite.Delete();
oMyObject.Graphic = newSprite.Graphic;
globalSprite = newSprite; // keep the sprite in a global variable
```

changes object's image to be just the top-left corner of what it previously was.

*Compatibility:* Supported by **AGS 3.0.2** and later versions.

*See also:* [`DynamicSprite.Delete`](DynamicSprite#dynamicspritedelete)

---

### `DynamicSprite.CreateFromExistingSprite`

```ags
static DynamicSprite* DynamicSprite.CreateFromExistingSprite(
    int slot, optional bool preserveAlphaChannel)
```

Creates a new dynamic sprite containing a copy of the specified sprite
*slot*.

Returns the DynamicSprite instance representing the new sprite. This
function is useful as it effectively allows you to apply transformations
such as resizing to any sprite in the game.

Use the [`Graphic`](DynamicSprite#dynamicspritegraphic) property of the
DynamicSprite to interface with other commands and to use the new sprite
in the game.

*preserveAlphaChannel* determines whether the sprite's alpha channel
will also be copied across. It is false by default for backwards
compatibility reasons, and is useful because it allows you to strip the
alpha channel in order to do whole image transparency. This parameter
has no effect with sprites that do not have an alpha channel.

Use the [`Graphic`](DynamicSprite#dynamicspritegraphic) property of the
DynamicSprite to interface with other commands and to use the new sprite
in the game.

Example:

```ags
// Declare a variable in a global scope (outside of any function)
DynamicSprite* globalSprite;

DynamicSprite* sprite = DynamicSprite.CreateFromExistingSprite(myObject.Graphic);
DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
surface.DrawingColor = 14;
surface.DrawLine(0, 0, surface.Width, surface.Height);
surface.Release();
oMyObject.Graphic = newSprite.Graphic;
globalSprite = newSprite; // keep the sprite in a global variable
```

creates a copy of object's current sprite, draws a line across, and assigns back to the same object.

*See also:* [`DynamicSprite.Delete`](DynamicSprite#dynamicspritedelete),
[`DynamicSprite.Resize`](DynamicSprite#dynamicspriteresize)

---

### `DynamicSprite.CreateFromFile`

*(Formerly known as `LoadImageFile`, which is now obsolete)*

```ags
static DynamicSprite* DynamicSprite.CreateFromFile(string filename)
```

Loads an external image FILENAME into memory as a sprite.

Returns the DynamicSprite instance representing the sprite, or *null* if
the image could not be loaded (file not found or unsupported format).

Only BMP and PCX files can be loaded with this command.

Use the [`Graphic`](DynamicSprite#dynamicspritegraphic) property of the
DynamicSprite to interface with other commands and to use the new sprite
in the game.

**NOTE:** Since AGS 3.4.1 you can use location tokens in filename, like
with [`File.Open`](File#fileopen) and similar commands.

Use the [`Graphic`](DynamicSprite#dynamicspritegraphic) property of the
DynamicSprite to interface with other commands and to use the new sprite
in the game.

Example:

```ags
DynamicSprite* sprite = DynamicSprite.CreateFromFile("CustomAvatar.bmp");
if (sprite != null) {
    DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
    surface.DrawImage(100, 80, sprite.Graphic);
    surface.Release();
    sprite.Delete();
}
```

will load the file "CustomAvatar.bmp" and if successful draw the image
near the middle of the background.

Once the image is finished with, Delete should be called on it.

*See also:* [`DynamicSprite.CreateFromExistingSprite`](DynamicSprite#dynamicspritecreatefromexistingsprite),
[`DynamicSprite.CreateFromScreenshot`](DynamicSprite#dynamicspritecreatefromscreenshot),
[`DynamicSprite.Delete`](DynamicSprite#dynamicspritedelete)

---

### `DynamicSprite.CreateFromSaveGame`

*(Formerly known as `LoadSaveSlotScreenshot`, which is now obsolete)*

```ags
static DynamicSprite* DynamicSprite.CreateFromSaveGame(
    int saveSlot, int width, int height)
```

Loads the screenshot for save game SAVESLOT into memory, resizing it to
WIDTH x HEIGHT.

Returns the DynamicSprite instance of the image if successful, or
returns *null* if the screenshot could not be loaded (perhaps the save
game didn't include one).

In order for this to work, the "Save screenshots in save games" option
must be ticked in the main Game Settings pane.

Use the [`Graphic`](DynamicSprite#dynamicspritegraphic) property of the
DynamicSprite to interface with other commands and to use the new sprite
in the game.

Example:

```ags
// at top of script, outside event functions
DynamicSprite *buttonSprite;

// inside an event function
buttonSprite = DynamicSprite.CreateFromSaveGame(1, 50, 50);
if (buttonSprite != null) {
    btnScrnshot.NormalGraphic = buttonSprite.Graphic;
}
```

will load the screenshot for save game 1 and resize it to 50x50. It then
places it onto the btnScrnshot GUI button.

Once the GUI is disposed of, Delete should be called on the sprite.

*See also:* [`DynamicSprite.Delete`](DynamicSprite#dynamicspritedelete),
[`Game.GetSaveSlotDescription`](Game#gamegetsaveslotdescription),
[`DynamicSprite.CreateFromExistingSprite`](DynamicSprite#dynamicspritecreatefromexistingsprite)
[`DynamicSprite.CreateFromFile`](DynamicSprite#dynamicspritecreatefromfile),
[`DynamicSprite.CreateFromScreenShot`](DynamicSprite#dynamicspritecreatefromscreenshot)

---

### `DynamicSprite.CreateFromScreenShot`

```ags
static DynamicSprite* DynamicSprite.CreateFromScreenShot(
    optional int width, optional int height, optional RenderLayer layer)
```

Creates a new DynamicSprite instance with a copy of the current screen
in it, resized to WIDTH x HEIGHT. If you do not supply the width or
height, then a full screen sized sprite will be created.

You can optionally pass a layer parameter that is a flag parameter
that can take any bitwise combination of RenderLayer type. You can
use this parameter to do a screenshot without include GUIs or mouse
cursor.

This command can be useful if you're creating a save game screenshots
GUI, in order to display the current game position as well as the saved
slot positions.

**NOTE:** This command can be slow when using the Direct3D graphics
driver.

Use the [`Graphic`](DynamicSprite#dynamicspritegraphic) property of the
DynamicSprite to interface with other commands and to use the new sprite
in the game.

Example:

```ags
DynamicSprite* ds = DynamicSprite.CreateFromScreenShot(50, 50);
DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
surface.DrawImage(100, 100, ds.Graphic);
surface.Release();
ds.Delete();
```

takes a screen shot, and draws it onto the background scene at
(100,100).

*Compatibility:* Optional `layer` parameter is supported only by **AGS 3.6.2** and later versions.

*See also:* [`RenderLayer`](StandardEnums#renderlayer),
[`DynamicSprite.Delete`](DynamicSprite#dynamicspritedelete),
[`Game.GetSaveSlotDescription`](Game#gamegetsaveslotdescription),
[`DynamicSprite.CreateFromExistingSprite`](DynamicSprite#dynamicspritecreatefromexistingsprite)
[`DynamicSprite.CreateFromFile`](DynamicSprite#dynamicspritecreatefromfile),
[`DynamicSprite.CreateFromSaveGame`](DynamicSprite#dynamicspritecreatefromsavegame)

---

### `DynamicSprite.ChangeCanvasSize`

```ags
DynamicSprite.ChangeCanvasSize(int width, int height, int x, int y);
```

Changes the sprite size to *width* x *height*, placing the current image
at offset (x, y) within the new canvas. Unlike the
[`Resize`](DynamicSprite#dynamicspriteresize) command, the current image is
kept at its original size.

This function allows you to enlarge the sprite background in order to
draw more onto it than its current boundaries allow. It is effectively
the opposite of [`Crop`](DynamicSprite#dynamicspritecrop). The additional
surface area will be transparent.

The width and height are specified in pixels.

Example:

```ags
DynamicSprite* sprite = DynamicSprite.CreateFromExistingSprite(10);
sprite.ChangeCanvasSize(sprite.Width + 10, sprite.Height, 5, 0);
DrawingSurface *surface = sprite.GetDrawingSurface();
surface.DrawingColor = 14;
surface.DrawLine(0, 0, 5, surface.Height);
surface.Release();
sprite.Delete();
```

creates a dynamic sprite as a copy of sprite 10, enlarges it by 5 pixels
to the left and right, and draws a line in the new area to the left.

*See also:* [`DynamicSprite.Crop`](DynamicSprite#dynamicspritecrop),
[`DynamicSprite.Resize`](DynamicSprite#dynamicspriteresize),
[`DynamicSprite.Height`](DynamicSprite#dynamicspriteheight),
[`DynamicSprite.Width`](DynamicSprite#dynamicspritewidth)

---

### `DynamicSprite.CopyTransparencyMask`

```ags
DynamicSprite.CopyTransparencyMask(int fromSpriteSlot)
```

Copies the transparency mask from the specified sprite slot onto the
dynamic sprite. The dynamic sprite's transparency and/or alpha channel
will be replaced with the one from the other sprite.

This command is designed for special effects. It is fairly slow since it
involves inspecting each pixel of the image, so it's not recommended
that you use it often.

The source sprite must be the same size and color depth as the dynamic
sprite.

**NOTE:** This command makes all pixels that are transparent in the
source sprite also transparent in the dynamic sprite. It does not make
opaque pixels from the source sprite into opaque pixels on the dynamic
sprite (because it wouldn't know what color to make them).

If the source image has an alpha channel, then the dynamic sprite will
have an alpha channel created as a copy of the one from the source
sprite.

Example:

```ags
DynamicSprite* sprite = DynamicSprite.CreateFromExistingSprite(10);
sprite.CopyTransparencyMask(11);
object[0].Graphic = sprite.Graphic;
Wait(80);
sprite.Delete();
```

creates a dynamic sprite as a copy of sprite 10, changes its
transparency mask to use that of sprite 11, and displays it on object 0.

*See also:*
[`DynamicSprite.CreateFromExistingSprite`](DynamicSprite#dynamicspritecreatefromexistingsprite)

---

### `DynamicSprite.Crop`

```ags
DynamicSprite.Crop(int x, int y, int width, int height);
```

Crops the sprite down to *width* x *height*, starting from (x,y) in the
image. The width and height are specified in pixels.

This allows you to trim the edges off a sprite, and perform related
tasks. Only the area with its top-left corner as (x,y) and of WIDTH x
HEIGHT in size will remain.

Example:

```ags
DynamicSprite* sprite = DynamicSprite.CreateFromExistingSprite(10);
sprite.Crop(10, 10, sprite.Width - 10, sprite.Height - 10);
DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
surface.DrawImage(100, 100, sprite.Graphic);
surface.Release();
sprite.Delete();
```

will create a dynamic sprite as a copy of sprite 10, cut off the left and top 10
pixels, and then draw it onto the room background at (100,100).

*See also:*
[`DynamicSprite.ChangeCanvasSize`](DynamicSprite#dynamicspritechangecanvassize),
[`DynamicSprite.Flip`](DynamicSprite#dynamicspriteflip),
[`DynamicSprite.Height`](DynamicSprite#dynamicspriteheight),
[`DynamicSprite.Width`](DynamicSprite#dynamicspritewidth)

---

### `DynamicSprite.Delete`

*(Formerly known as `DeleteSprite`, which is now obsolete)*

```ags
DynamicSprite.Delete();
```

Deletes the specified dynamic sprite from memory. Use this when you are
no longer displaying the sprite and it can be safely disposed of.

You do not normally need to delete sprites, since the AGS Sprite Cache
manages loading and deleting sprites automatically.

However, when an extra sprite has been loaded into the game (for
example, with the CreateFromFile or CreateFromScreenShot commands) then
AGS does not delete it automatically, and you must call this command
instead.

Example:

```ags
DynamicSprite* sprite = DynamicSprite.CreateFromFile("CustomAvatar.bmp");
object[1].Graphic = sprite.Graphic;
Wait(200);
object[1].Graphic = 22;
sprite.Delete();
```

will load the file "CustomAvatar.bmp", change Object 1 to display this
graphic, wait 5 seconds, then change object 1 back to its old sprite 22
and free the new image.

*See also:*
[`DynamicSprite.CreateFromScreenShot`](DynamicSprite#dynamicspritecreatefromscreenshot),
[`DynamicSprite.Graphic`](DynamicSprite#dynamicspritegraphic)

---

### `DynamicSprite.Flip`

```ags
DynamicSprite.Flip(eFlipDirection);
```

Flips the dynamic sprite according to the parameter:

*eFlipLeftToRight* flips the image from left to right<br>
*eFlipUpsideDown* flips the image from top to bottom<br>
*eFlipBoth* flips the image from top to bottom and left to right

Example:

```ags
DynamicSprite* sprite = DynamicSprite.CreateFromFile("CustomAvatar.bmp");
sprite.Flip(eFlipUpsideDown);
DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
surface.DrawImage(100, 100, sprite.Graphic);
surface.Release();
sprite.Delete();
```

will load the CustomAvatar.bmp image, flip it upside down, and then draw
it onto the room background at (100,100).

*See also:* [`DynamicSprite.Crop`](DynamicSprite#dynamicspritecrop),
[`DynamicSprite.Resize`](DynamicSprite#dynamicspriteresize),
[`DynamicSprite.Rotate`](DynamicSprite#dynamicspriterotate)

---

### `DynamicSprite.GetDrawingSurface`

```ags
DrawingSurface* DynamicSprite.GetDrawingSurface();
```

Gets the drawing surface for this dynamic sprite, which allows you to
modify the sprite by drawing onto it in various ways.

After calling this method, use the various
[DrawingSurface functions](DrawingSurface) to modify the sprite, then
call Release on the surface when you are finished.

Example:

```ags
DynamicSprite *sprite = DynamicSprite.CreateFromExistingSprite(object[0].Graphic);
DrawingSurface *surface = sprite.GetDrawingSurface();
surface.DrawingColor = 13;
surface.DrawLine(0, 0, 20, 20);
surface.Release();
object[0].Graphic = sprite.Graphic;
Wait(40);
sprite.Delete();
```

this creates a dynamic sprite as a copy of Object 0's existing sprite,
draws a pink diagonal line across it, sets this new sprite onto the
object for 1 second and then removes it.

*See also:*
[`DynamicSprite.CreateFromExistingSprite`](DynamicSprite#dynamicspritecreatefromexistingsprite),
[`DrawingSurface.DrawLine`](DrawingSurface#drawingsurfacedrawline),
[`DrawingSurface.Release`](DrawingSurface#drawingsurfacerelease)

---

### `DynamicSprite.Resize`

```ags
DynamicSprite.Resize(int width, int height);
```

Resizes an existing dynamic sprite to WIDTH x HEIGHT pixels.

**NOTE:** Resizing is a relatively slow operation, so do not attempt to
resize sprites every game loop; only do it when necessary.

Example:

```ags
DynamicSprite* sprite = DynamicSprite.CreateFromExistingSprite(10);
sprite.Resize(sprite.Width * 2, sprite.Height * 2);
DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
surface.DrawImage(100, 100, sprite.Graphic);
surface.Release();
sprite.Delete();
```

will create a dynamic sprite as a copy of sprite 10, stretch it to double its original
size, and then draw it onto the room background at (100,100).

*See also:*
[`DynamicSprite.ChangeCanvasSize`](DynamicSprite#dynamicspritechangecanvassize),
[`DynamicSprite.Crop`](DynamicSprite#dynamicspritecrop),
[`DynamicSprite.Flip`](DynamicSprite#dynamicspriteflip),
[`DynamicSprite.Rotate`](DynamicSprite#dynamicspriterotate),
[`DynamicSprite.Height`](DynamicSprite#dynamicspriteheight),
[`DynamicSprite.Width`](DynamicSprite#dynamicspritewidth)

---

### `DynamicSprite.Rotate`

```ags
DynamicSprite.Rotate(int angle, optional int width, optional int height)
```

Rotates the dynamic sprite by the specified *angle*. The angle is in
degrees, and must lie between 1 and 359. The image will be rotated
clockwise by the specified angle. For a counter-clockwise rotation - convert the angle to a clockwise one using formula: "angle = ((-counter_angle) + 360)".

Optionally, you can specify the width and height of the rotated image.
By default, AGS will automatically calculate the new size required to
hold the rotated image, but you can override this by passing the
parameters in. Note that specifying a width/height does not stretch the image, it just
allows you to set the final sprite dimensions. The rotated image will be centered in the sprite.

**NOTE:** Sprite may have to be resized in order to accomodate rotated image. This is not because the image is scaled, but because after rotation original image's corners may be positioned "outside" of the initial rectangle. Because sprite is resized, and image is centered in it, it may look like image is moving towards right-bottom. To counter that effect when you draw this sprite somewhere, you have change the sprite's position, subtracting a difference of half its width and height, for example:

    int x = (Game.SpriteWidth[original_sprnum] - sprite.Width) / 2;
    int y = (Game.SpriteHeight[original_sprnum] - sprite.Height]) / 2;
    surface.DrawImage(x, y, sprite.Graphic);

**NOTE:** Rotating is a "lossy" operation. This means that each time you rotate a sprite it will loose a bit of image quality (except when rotated by 90, 180 and 270 degrees). High-resolution sprites will loose quality slower in general, but will nevertheless. If you have to rotate a sprite multiple times, the solution is to keep the original sprite and rotate its new copy to a new angle each time, instead of rotating same sprite over and over again.

**NOTE:** Rotating is a relatively slow operation, try keeping a number of sprites rotated each game loop low, and do it only when necessary.

Example:

```ags
DynamicSprite* sprite = DynamicSprite.CreateFromExistingSprite(10);
sprite.Rotate(90);
DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
surface.DrawImage(100, 100, sprite.Graphic);
surface.Release();
sprite.Delete();
```

will create a dynamic sprite as a copy of sprite 10, rotate it 90 degrees clockwise,
draw the result onto the screen, and then delete the image.

*See also:* [`DynamicSprite.Flip`](DynamicSprite#dynamicspriteflip),
[`DynamicSprite.Resize`](DynamicSprite#dynamicspriteresize),
[`DynamicSprite.Height`](DynamicSprite#dynamicspriteheight),
[`DynamicSprite.Width`](DynamicSprite#dynamicspritewidth)

---

### `DynamicSprite.SaveToFile`

```ags
DynamicSprite.SaveToFile(string filename)
```

Saves the dynamic sprite to the specified file.

The filename you supply must have a .PCX or .BMP extension; they are the
only two file types that the engine supports.

Returns 1 if the sprite was saved successfully, or 0 if it failed.

**NOTE:** Since AGS 3.4.1 you can (and should!) use location tokens in filename, like
with [`File.Open`](File#fileopen) and similar commands.

Example:

```ags
DynamicSprite* sprite = DynamicSprite.CreateFromFile("CustomAvatar.bmp");
sprite.Rotate(90);
sprite.SaveToFile("$SAVEGAMEDIR$/RotatedAvatar.bmp");
sprite.Delete();
```

will load the CustomAvatar.bmp image, rotate it 90 degrees clockwise,
then save the result back to the disk to the game saves folder.

*Compatibility:* location tokens in path are supported since AGS 3.4.1.

*See also:*
[`DynamicSprite.CreateFromFile`](DynamicSprite#dynamicspritecreatefromfile),
[`SaveScreenShot`](Globalfunctions_General#savescreenshot)

---

### `DynamicSprite.Tint`

```ags
DynamicSprite.Tint(int red, int green, int blue, int saturation, int luminance)
```

Tints the dynamic sprite to (RED, GREEN, BLUE) with SATURATION percent
saturation. For the meaning of all the parameters, see
[`SetAmbientTint`](Globalfunctions_General#setambienttint).

The tint set by this function is permanent for the dynamic sprite --
after the tint has been set, it is not possible to remove it. If you
call Tint again with different parameters, it will apply the new tint to
the already tinted sprite from the first call.

**NOTE:** This function only works with hi-color sprites.

Example:

```ags
DynamicSprite* sprite = DynamicSprite.CreateFromExistingSprite(object[0].Graphic);
sprite.Tint(255, 0, 0, 100, 100);
DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
surface.DrawImage(100, 80, sprite.Graphic);
surface.Release();
sprite.Delete();
```

creates a copy of object 0's sprite, tints it red, and draws it onto the
room background.

*See also:* [`DynamicSprite.Flip`](DynamicSprite#dynamicspriteflip),
[`DynamicSprite.Height`](DynamicSprite#dynamicspriteheight),
[`DynamicSprite.Width`](DynamicSprite#dynamicspritewidth),
[`SetAmbientTint`](Globalfunctions_General#setambienttint)

---

### `DynamicSprite.ColorDepth`

```ags
readonly int DynamicSprite.ColorDepth;
```

Gets the color depth of this dynamic sprite. This can be 8, 16 or 32
and is not necessarily the same as the game color depth (though this
usually will be the case).

Example:

```ags
DynamicSprite* sprite = DynamicSprite.CreateFromFile("CustomAvatar.bmp");
if (sprite != null) {
    Display("The image is %d x %d pixels, at %d-bit depth.", sprite.Width, sprite.Height, sprite.ColorDepth);
    sprite.Delete();
}
```

displays the color depth of the CustomAvatar.bmp image.

*See also:* [`DynamicSprite.Height`](DynamicSprite#dynamicspriteheight),
[`DynamicSprite.Width`](DynamicSprite#dynamicspritewidth)

---

### `DynamicSprite.Graphic`

```ags
readonly int DynamicSprite.Graphic;
```

Gets the sprite slot number in which this dynamic sprite is stored. This
value can then be passed to other functions and properties, such as
[`Button.NormalGraphic`](Button#buttonnormalgraphic).

Example:


```ags
// at top of script, outside event functions
DynamicSprite *buttonSprite;

// inside an event function
buttonSprite = DynamicSprite.CreateFromScreenShot(80, 50);
if (buttonSprite != null) {
    btnScrnshot.NormalGraphic = buttonSprite.Graphic;
}
```

places a screen grab of the current game session onto btnScrnshot.

Once the GUI is disposed of, Delete should be called on the sprite.

*See also:*
[`DynamicSprite.CreateFromScreenShot`](DynamicSprite#dynamicspritecreatefromscreenshot),
[`DynamicSprite.Delete`](DynamicSprite#dynamicspritedelete)

---

### `DynamicSprite.Height`

```ags
readonly int DynamicSprite.Height;
```

Gets the height of this dynamic sprite. The height is always returned in
pixels.

Example:

```ags
DynamicSprite* sprite = DynamicSprite.CreateFromFile("CustomAvatar.bmp");
if (sprite != null) {
    Display("The image is %d x %d pixels.", sprite.Width, sprite.Height);
    sprite.Delete();
}
```

displays the size of the CustomAvatar.bmp image.

*See also:* [`DynamicSprite.Resize`](DynamicSprite#dynamicspriteresize),
[`DynamicSprite.Width`](DynamicSprite#dynamicspritewidth)

---

### `DynamicSprite.Width`

```ags
readonly int DynamicSprite.Width;
```

Gets the width of this dynamic sprite. The width is always returned in
pixels.

Example:

```ags
DynamicSprite* sprite = DynamicSprite.CreateFromFile("CustomAvatar.bmp");
if (sprite != null) {
    Display("The image is %d x %d pixels.", sprite.Width, sprite.Height);
    sprite.Delete();
}
```

displays the size of the CustomAvatar.bmp image.

*See also:* [`DynamicSprite.Height`](DynamicSprite#dynamicspriteheight),
[`DynamicSprite.Resize`](DynamicSprite#dynamicspriteresize)
