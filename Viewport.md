## `Viewport` functions and properties

Viewport struct lets you manage screen viewports. Viewports are used to display contents of a [Camera](Camera). You may think that the viewport is a "TV screen" that shows the image that the camera have captured in the room. Viewport is defined by a rectangle on the game screen, in screen coordinates, and a [Camera link](Viewport#viewportcamera).

By default there is a single viewport called "primary viewport". This viewport may be accessed as [`Screen.Viewport`](Screen#screenviewport). But you may create more using [`Viewport.Create`](Viewport#viewportcreate).

Viewport has a link to camera through a [`Viewport.Camera`](Viewport#viewportcamera) property. Same camera may be linked to multiple viewports at once. If no camera is linked then a viewport is not displayed at all. Viewports are allowed to go partially or fully offscreen, which may be useful to create transition effects, for example. Viewports may overlap, in which case their order of display is defined by [`ZOrder`](Viewport#viewportzorder) property.

By default viewport covers whole game screen, but you may change that anytime using Width and Height properties, and also [SetPosition](Viewport#viewportsetposition) function. Camera's contents are stretched to fill viewport's rectangle, and because of that the difference between viewport's and camera's sizes create zoom effect (scaling):

* If the camera is larger than the viewport the displayed image will be zoomed-out (scaled down)
* If the camera is smaller than the viewport the displayed image will be zoomed-in (scaled up)

Viewport's X, Y properties specify the position if its top-left corner on the game screen. You may use these to move the viewport around the screen.

**IMPORTANT:** The game starts in automatic viewport mode that snaps primary viewport and camera to the size of the game screen or size of a room background, whatever is *smaller*, each time new room is loaded. This is convenient in case you want to rely on a common behavior. If you prefer to customize viewports yourself standard behavior may be disabled using [`Screen.AutoSizeViewportOnRoomLoad`](Screen#screenautosizeviewportonroomload) property.

*Compatibility:* Viewport struct is supported by **AGS 3.5.0** and later versions.

*See also:* [`Camera`](Camera), [`Screen.AutoSizeViewportOnRoomLoad`](Screen#screenautosizeviewportonroomload), [`Screen.Viewport`](Screen#screenviewport), [`Screen.Viewports`](Screen#screenviewports)

---

### `Viewport.Create`

```ags
static Viewport* Viewport.Create()
```

Creates a new viewport and returns a pointer which you may store in a variable (local or global one) and use to operate it. Any viewport created like this may be also accessed by [`Screen.Viewports`](Screen#screenviewports) by its index.

The new viewport will cover whole game screen by default and does not have any camera linked initially. You may assign or reassign the camera and change any of its properties later.

You may always delete previously created viewport using [`Viewport.Delete`](Viewport#viewportdelete) function.

Example:

```ags
// Create a new viewport and a new camera, and link them together
Viewport* myView = Viewport.Create();
Camera* myCam = Camera.Create();
myView.Camera = myCam;
// Set the viewport to half the screen size, centered and camera to half the room's
myView.SetPosition(Screen.Width / 4, Screen.Height / 4, Screen.Width / 2, Screen.Height / 2);
// Make sure that the new viewport is on top of the primary one
myView.ZOrder = Screen.Viewport.ZOrder + 1;
// Set the camera to half of the room size, placed at the right-bottom of the room
myCam.SetSize(Room.Width / 2, Room.Height / 2);
myCam.SetAt(Room.Width - myCam.Width, Room.Height - myCam.Height);
// Wait a little (or play a cutscene, and so on)
Wait(100);
// Delete temporary camera and viewport
myCam.Delete();
myView.Delete();
```

*See also:* [`Viewport.Camera`](Viewport#viewportcamera), [`Viewport.Delete`](Viewport#viewportdelete), [`Screen.Viewport`](Screen#screenviewport), [`Screen.Viewports`](Screen#screenviewports)

---

### `Viewport.Delete`

```ags
void Viewport.Delete();
```

Removes an existing viewport. This also removes this viewport's pointer from [`Screen.Viewports`](Screen#screenviewports) array.

It's always safe to delete a viewport, even if it's the last viewport remaining: in such case the room will simply be not displayed on screen. You may create another viewport later and make the room show up again.

**IMPORTANT:** in **Screen.Viewports** array viewports are arranged in the order they were created. When you delete one in the middle all the following viewports will be shifted towards beginning of array.

*See also:* [`Viewport.Create`](Viewport#viewportcreate), [`Screen.Viewport`](Screen#screenviewport), [`Screen.Viewports`](Screen#screenviewports)

---

### `Viewport.GetAtScreenXY`

```ags
static Viewport* Viewport.GetAtScreenXY(int x, int y)
```

Finds if there's any viewport at the specified screen coordinates and returns the topmost one. Hidden viewports will not be tested, but those without camera will still be. If no viewport was found under provided coordinates then this function returns null.

Example:

```ags
Viewport* view = Viewport.GetAtScreenXY(mouse.x, mouse.y);
if (view != null)
{
    view.Visible = false;
    Wait(1);
    view.Visible = true;
}
```

will find a viewport under the mouse cursor, and if there's one then make it "blink".

---

### `Viewport.RoomToScreenPoint`

```ags
Point* Viewport.RoomToScreenPoint(int roomx, int roomy, bool clipViewport = true);
```

Returns the point on screen corresponding to the given room coordinates if seen through this viewport. Resulting Point struct will contain screen x and y coordinates.

Function does the conversion from room to screen's coordinate system, correctly taking into account viewport's position on screen, camera's position in the room and its transformation (scaling etc).

If **clipViewport** is true then this function will only return a point if given room coordinates are currently seen inside viewport and return null otherwise. If **clipViewport** is false then viewport's bounds will be ignored.

Example:

```ags
Point* pt = myViewport.RoomToScreenPoint(player.x, player.y);
if (pt != null) {
    Display("Player character is displayed at (%d, %d) on screen.", pt.x, pt.y);
} else {
    Display("Player character is currently offscreen.");
}
```

*See also:* [`Viewport.ScreenToRoomPoint`](Viewport#viewportscreentoroompoint), [`Screen.RoomToScreenPoint`](Screen#screenroomtoscreenpoint), [`Screen.ScreenToRoomPoint`](Screen#screenscreentoroompoint)

---

### `Viewport.SetPosition`

```ags
void Viewport.SetPosition(int x, int y, int width, int height);
```

Changes viewport's position on the screen. Offscreen positons are valid, if a viewport is entirely offscreen it will simply not be drawn.

Example 1:

```ags
// Set the primary viewport to half the screen size, aligned in the center.
Screen.Viewport.SetPosition(Screen.Width / 4, Screen.Height / 4, Screen.Width / 2, Screen.Height / 2);
```

Example 2:

```ags
// Shrink the viewport towards the center of the screen,
// and then enlarge it back, while keeping the aspect ratio.
int w = Screen.Viewport.Width;
int h = Screen.Viewport.Height;
float aspect_ratio = IntToFloat(h) / IntToFloat(w);
while (h > Screen.Height / 4)
{
    w -= 2;
    h = FloatToInt(IntToFloat(w) * aspect_ratio, eRoundNearest);
    Screen.Viewport.SetPosition((Screen.Width - w) / 2, (Screen.Height - h) / 2, w, h);
    Wait(1);
}
Wait(60);
while (h < Screen.Height)
{
    w += 2;
    h = FloatToInt(IntToFloat(w) * aspect_ratio, eRoundNearest);
    Screen.Viewport.SetPosition((Screen.Width - w) / 2, (Screen.Height - h) / 2, w, h);
    Wait(1);
}
```

*See also:* [`Viewport.X`](Viewport#viewportx), [`Viewport.Y`](Viewport#viewporty), [`Viewport.Width`](Viewport#viewportwidth), [`Viewport.Height`](Viewport#viewportheight), [`Viewport.ZOrder`](Viewport#viewportzorder), [`Screen.AutoSizeViewportOnRoomLoad`](Screen#screenautosizeviewportonroomload)

---

### `Viewport.ScreenToRoomPoint`

```ags
Point* Viewport.ScreenToRoomPoint(int scrx, int scry, optional bool clipViewport);
```

Returns the point in room corresponding to the given screen coordinates if seen through this viewport. Resulting Point struct will contain room x and y coordinates.

Function does the conversion from screen to room's coordinate system, correctly taking into account viewport's location on screen, camera's position in the room and its transformation (scaling etc).

If **clipViewport** is false then viewport's bounds will be ignored and this function returns valid point all the time. If it is true then this function will only return a point if given screen coordinates are inside viewport's rectangle and return null otherwise. The default is false.

Example:

```ags
Point* pt = myViewport.ScreenToRoomPoint(mouse.x, mouse.y, true);
if (pt != null) {
    Display("Mouse cursor is over room location: (%d, %d).", pt.x, pt.y);
} else {
    Display("Mouse cursor is currently outside the viewport.");
}
```

*See also:* [`Viewport.RoomToScreenPoint`](Viewport#viewportscreentoroompoint), [`Screen.RoomToScreenPoint`](Screen#screenroomtoscreenpoint), [`Screen.ScreenToRoomPoint`](Screen#screenscreentoroompoint)

---

### `Viewport.Camera`

```ags
Camera *Viewport.Camera;
```

Gets/sets the camera to be displayed in this viewport. Changing cameras is safe anytime, and the looks inside viewport will be changed during next drawing frame.

Setting Viewport.Camera to null will make an empty viewport, which does not display anything.

Example:

```ags
Camera* myCam = Camera.Create();
myCam.SetSize(Room.Width / 2, Room.Height / 2);
Screen.Viewport.Camera = myCam;
```

Will create a new camera, half of the room's size, and assign it to the primary viewport.

*See also:* [`Camera.Create`](Camera#cameracreate), [`Game.Camera`](Game#gamecamera), [`Game.Cameras`](Game#gamecameras)

---

### `Viewport.Height`

```ags
int Viewport.Height;
```

Gets/sets the viewport's height in screen coordinates.

*See also:* [`Viewport.SetPosition`](Viewport#viewportsetposition), [`Viewport.X`](Viewport#viewportx), [`Viewport.Y`](Viewport#viewporty), [`Viewport.Width`](Viewport#viewportwidth), [`Viewport.ZOrder`](Viewport#viewportzorder), [`Screen.AutoSizeViewportOnRoomLoad`](Screen#screenautosizeviewportonroomload)

---

### `Viewport.Shader`

```ags
ShaderInstance\* Viewport.Shader
```

- [ ] Gets/sets the shader of this viewport.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Viewport.Visible`

```ags
bool Viewport.Visible;
```

Gets/sets whether the viewport is enabled and drawn on screen.

---

### `Viewport.Width`

```ags
int Viewport.Width;
```

Gets/sets the viewport's width in screen coordinates.

*See also:* [`Viewport.SetPosition`](Viewport#viewportsetposition), [`Viewport.X`](Viewport#viewportx), [`Viewport.Y`](Viewport#viewporty), [`Viewport.Height`](Viewport#viewportheight), [`Viewport.ZOrder`](Viewport#viewportzorder), [`Screen.AutoSizeViewportOnRoomLoad`](Screen#screenautosizeviewportonroomload)

---

### `Viewport.X`

```ags
int Viewport.X;
```

Gets/sets the X position on the screen where this viewport is located.

Example:

```ags
while (Screen.Viewport.X < Screen.Width)
{
    Screen.Viewport.X += 1;
    Wait(1);
}
```

will pan viewport right until it's completely offscreen.

*See also:* [`Viewport.SetPosition`](Viewport#viewportsetposition), [`Viewport.Y`](Viewport#viewporty), [`Viewport.Width`](Viewport#viewportwidth), [`Viewport.Height`](Viewport#viewportheight), [`Viewport.ZOrder`](Viewport#viewportzorder), [`Screen.AutoSizeViewportOnRoomLoad`](Screen#screenautosizeviewportonroomload)

---

### `Viewport.Y`

```ags
int Viewport.Y;
```

Gets/sets the Y position on the screen where this viewport is located.

Example:

```ags
while (Screen.Viewport.Y < Screen.Height)
{
    Screen.Viewport.Y += 1;
    Wait(1);
}
```

will pan viewport down until it's completely offscreen.

*See also:* [`Viewport.SetPosition`](Viewport#viewportsetposition), [`Viewport.X`](Viewport#viewportx), [`Viewport.Width`](Viewport#viewportwidth), [`Viewport.Height`](Viewport#viewportheight), [`Viewport.ZOrder`](Viewport#viewportzorder), [`Screen.AutoSizeViewportOnRoomLoad`](Screen#screenautosizeviewportonroomload)

---

### `Viewport.ZOrder`

```ags
int Viewport.ZOrder;
```

Gets/sets the viewport's z-order relative to other viewports. This will be taken into account if multiple viewports overlap to define which should be displayed above and which below.

The z-order is an arbitrary number and only have meaning in comparison with other viewport's setting. The lower value puts viewport to the back and higher value to the top.

**IMPORTANT:** If two or more Viewports have equal ZOrder, their draw order is **undefined**, and should not be relied upon. This is because of how draw sort is done in the engine.

Example:

```ags
Viewport* myView = Viewport.Create();
myView.ZOrder = Screen.Viewport.ZOrder + 1;
```

will create a new viewport and place is on top of the primary one.

*See also:* [`Viewport.SetPosition`](Viewport#viewportsetposition), [`Viewport.X`](Viewport#viewportx), [`Viewport.Y`](Viewport#viewporty), [`Viewport.Width`](Viewport#viewportwidth), [`Viewport.Height`](Viewport#viewportheight)
