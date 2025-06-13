## `Camera` functions and properties

The Camera struct lets you manage room cameras. To put it simply, the cameras allow the player to see inside the room. Each camera captures some rectangular area in a room, and this captured image may then be displayed on screen. Right now AGS only supports drawing what a camera can see within a [`Viewport`](Viewport), but its uses may be extended in the future.

Cameras have a variable size and position within the room. The camera's size (the width and height of the rectangle) determines how much of the room they capture: the larger the size - the larger the visible portion of the room. The maximum camera size is limited by the dimensions of the room background, so it may not be larger than the room. It is also not possible to move the camera beyond the edges of the room background.

By default there is a single camera, known as the "primary camera". This camera may be accessed as [`Game.Camera`](Game#gamecamera). But you may create more cameras if necessary, using [`Camera.Create`](Camera#cameracreate).

Camera is linked to a Viewport using the [`Viewport.Camera`](Viewport#viewportcamera) property. By default, the "primary camera" is linked to the "primary viewport" ([`Screen.Viewport`](Screen#screenviewport)), but that may be changed if necessary. Moreover, a camera may be linked to any number of different Viewports, in which case all of the Viewports will display the same image of the room.

When a camera is linked to a Viewport the camera's contents are stretched to fill Viewport's rectangle, the difference in size between the viewport and the camera determines the level of scaling that is applied:

* If the camera is larger than the viewport the displayed image will be zoomed-out (scaled down)
* If the camera is smaller than the viewport the displayed image will be zoomed-in (scaled up)

Camera's X, Y properties specify the position if its *top-left corner* in the room. Naturally, its bottom-right corner will be at (X + Width, Y + Height), while the center at (X + Width / 2, Y + Height / 2).

By default each camera follows the player character, this is know as "auto-tracking". You may override this by disabling [`Camera.AutoTracking`](Camera#cameraautotracking) property. Also, auto-tracking is disabled whenever you manually set camera's position. In order to make it follow the player character again, set AutoTracking to true.

Moving a camera around the room (also known as "camera scrolling" or "room scrolling") is done simply by changing its X and Y properties, or using [SetAt](Camera#camerasetat) function. The common example is:

```ags
Game.Camera.X = 0;
while (Game.Camera.X < (Room.Width - Game.Camera.Width))
{
    Game.Camera.X += 1;
    Wait(1); // Wait is necessary to let the engine redraw the game
}

```

Above script will scroll the camera from the leftmost to the rightmost room border.

**NOTE:** Although cameras display the room, in their current implementation they are considered global persistent objects and will automatically change rooms with the player's character.

**IMPORTANT:** Cameras are initialized with an automatic sizing mode that snaps the primary Viewport and the camera to the size of the game screen or size of a room background (whichever is *smaller*), each time that a new room is loaded. This is convenient if you want to rely on a standard camera behavior. If you prefer to manage the sizing  yourself this behavior may be disabled using the [`Screen.AutoSizeViewportOnRoomLoad`](Screen#screenautosizeviewportonroomload) property.

*Compatibility:* The Camera struct is supported by **AGS 3.5.0** and later versions.

*See also:* [`Viewport`](Viewport), [`Screen.AutoSizeViewportOnRoomLoad`](Screen#screenautosizeviewportonroomload), [`Game.Camera`](Game#gamecamera), [`Game.Cameras`](Game#gamecameras)

---

### `Camera.Create`

```ags
static Camera* Camera.Create();
```

Creates a new camera and returns a pointer which you may store in a variable (local or global one) and use to operate it. Any camera created like this is also added to the global array [`Game.Cameras`](Game#gamecameras) and may be also accessed by its index.

The new camera will, by default, be located at the room's origin (0, 0) and be sized to fit either the game screen or the room background, whichever is *smaller*. It will also have auto-tracking enabled. You may change any of its properties later.

You may always delete previously created cameras using [`Camera.Delete`](Camera#cameradelete) function.

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

*See also:* [`Camera.Delete`](Camera#cameradelete), [`Game.Camera`](Game#gamecamera), [`Game.Cameras`](Game#gamecameras), [`Viewport.Camera`](Viewport#viewportcamera)

---

### `Camera.Delete`

```ags
void Camera.Delete();
```

Removes an existing camera. This also removes this camera's pointer from [`Game.Cameras`](Game#gamecameras) array.

It's always *safe* to delete a camera. If this camera was linked to a Viewport on screen, then that Viewport will automatically unlink the deleted camera and will not show anything until a new camera is assigned to it.

**IMPORTANT:** In the **Game.Cameras** array cameras are arranged in the order they were created. If you delete one from the middle all of the cameras with a higher index will be shifted towards the beginning of array.

*See also:* [`Camera.Create`](Camera#cameracreate), [`Game.Camera`](Game#gamecamera), [`Game.Cameras`](Game#gamecameras)

---

### `Camera.SetAt`

```ags
void Camera.SetAt(int x, int y);
```

Changes the camera's position in the room and disables automatic tracking of the player character.

**NOTE:** Camera can never cross the room's bounds (and can never be larger than the current room). If you try to move the camera outside of the room, its position will be automatically snapped to the nearest position inside the room.

Example 1:

```ags
// Center the primary camera around the Pirate character's middle point
ViewFrame* curFrame = Game.GetViewFrame(cPirate.View, cPirate.Loop, cPirate.Frame);
Game.Camera.SetAt(cPirate.x, cPirate.y - Game.SpriteHeight[curFrame.Graphic] / 2);
```

Example 2:

```ags
// Scroll the camera across the room
while (Game.Camera.X < (Room.Width - Game.Camera.Width))
{
    Game.Camera.X += 1;
    Wait(1);
}
// Snap back to player character and begin tracking it again
Game.Camera.AutoTracking = true;
```

*See also:* [`Camera.SetSize`](Camera#camerasetsize), [`Camera.AutoTracking`](Camera#cameraautotracking), [`Camera.X`](Camera#camerax), [`Camera.Y`](Camera#cameray), [`Camera.Width`](Camera#camerawidth), [`Camera.Height`](Camera#cameraheight)

---

### `Camera.SetSize`

```ags
void Camera.SetSize(int width, int height);
```

Changes the camera's capture dimensions, in room coordinates.

If the camera's size is larger than the Viewport it is drawn in, then the room will be scaled down (producing a zoomed-out effect). If the camera's size is smaller than the Viewport then the room will be scaled up (producing a zoomed-in effect).

**NOTE:** Camera's size can never exceed current room's size, if you try to set a higher width or height then it will be clamped. If game transits from a large room to a smaller room, any active cameras that exceed the new room's size will be shrunk automatically.

Example 1:

```ags
// Make the camera show only half of the room's size
Game.Camera.SetSize(Room.Width / 2, Room.Height / 2);
```

Example 2:

```ags
// Zoom camera in and then out, keeping the aspect ratio.
int w = Game.Camera.Width;
int h = Game.Camera.Height;
float aspect_ratio = IntToFloat(h) / IntToFloat(w);
while (Game.Camera.Height > Room.Height / 4)
{
    w -= 2;
    h = FloatToInt(IntToFloat(w) * aspect_ratio, eRoundNearest);
    Game.Camera.SetSize(w, h);
    Wait(1);
}
Wait(60);
while (Game.Camera.Height < Room.Height)
{
    w += 2;
    h = FloatToInt(IntToFloat(w) * aspect_ratio, eRoundNearest);
    Game.Camera.SetSize(w, h);
    Wait(1);
}
```

*See also:* [`Camera.SetAt`](Camera#camerasetat), [`Camera.X`](Camera#camerax), [`Camera.Y`](Camera#cameray), [`Camera.Width`](Camera#camerawidth), [`Camera.Height`](Camera#cameraheight), [`Screen.AutoSizeViewportOnRoomLoad`](Screen#screenautosizeviewportonroomload)

---

### `Camera.AutoTracking`

```ags
bool Camera.AutoTracking;
```

Gets/sets whether this camera will automatically follow the player character's position in the room. When disabled, the camera retains its position until you change it using script commands or turn auto-tracking back on.

*See also:* [`Camera.SetAt`](Camera#camerasetat), [`Camera.X`](Camera#camerax), [`Camera.Y`](Camera#cameray)

---

### `Camera.Height`

```ags
int Camera.Height;
```

Gets/sets the camera's capture height in room coordinates.

**NOTE:** Camera's size can never exceed current room's size, if you try to set a higher width or height then it will be clamped. If game transits from a large room to a smaller room, any active cameras that exceed the new room's size will be shrunk automatically.

*See also:* [`Camera.SetAt`](Camera#camerasetat), [`Camera.SetSize`](Camera#camerasetsize), [`Camera.X`](Camera#camerax), [`Camera.Y`](Camera#cameray), [`Camera.Width`](Camera#camerawidth), [`Screen.AutoSizeViewportOnRoomLoad`](Screen#screenautosizeviewportonroomload)

---

### `Camera.Width`

```ags
int Camera.Width;
```

Gets/sets the camera's capture width in room coordinates.

**NOTE:** Camera's size can never exceed current room's size, if you try to set a higher width or height then it will be clamped. If game transits from a large room to a smaller room, any active cameras that exceed the new room's size will be shrunk automatically.

*See also:* [`Camera.SetAt`](Camera#camerasetat), [`Camera.SetSize`](Camera#camerasetsize), [`Camera.X`](Camera#camerax), [`Camera.Y`](Camera#cameray), [`Camera.Height`](Camera#cameraheight), [`Screen.AutoSizeViewportOnRoomLoad`](Screen#screenautosizeviewportonroomload)

---

### `Camera.X`

```ags
int Camera.X;
```

Gets/sets the X position of this camera in the room. Setting a value will also disable auto-tracking.

**NOTE:** Camera can never cross the room's bounds (and can never be larger than the current room). If you try to move the camera outside of the room, its position will be automatically snapped to the nearest position inside the room.

Example:

```ags
// Scroll the camera across the room horizontally
while (Game.Camera.X < (Room.Width - Game.Camera.Width))
{
    Game.Camera.X += 1;
    Wait(1);
}
```

*See also:* [`Camera.SetAt`](Camera#camerasetat), [`Camera.SetSize`](Camera#camerasetsize), [`Camera.AutoTracking`](Camera#cameraautotracking), [`Camera.Y`](Camera#cameray), [`Camera.Width`](Camera#camerawidth), [`Camera.Height`](Camera#cameraheight)

---

### `Camera.Y`

```ags
int Camera.Y;
```

Gets/sets the Y position of this camera in the room. Setting a value will also disable auto-tracking.

**NOTE:** Camera can never cross the room's bounds (and can never be larger than the current room). If you try to move the camera outside of the room, its position will be automatically snapped to the nearest position inside the room.

Example:

```ags
// Scroll the camera across the room vertically
while (Game.Camera.Y < (Room.Height - Game.Camera.Height))
{
    Game.Camera.Y += 1;
    Wait(1);
}
```

*See also:* [`Camera.SetAt`](Camera#camerasetat), [`Camera.SetSize`](Camera#camerasetsize), [`Camera.AutoTracking`](Camera#cameraautotracking), [`Camera.X`](Camera#camerax), [`Camera.Width`](Camera#camerawidth), [`Camera.Height`](Camera#cameraheight)

---

### `Camera.Rotation`

```ags
float Camera.Rotation;
```

Gets/sets the camera rotation in degrees.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Camera.Shader`

```ags
ShaderInstance* Camera.Shader
```

Gets/sets the shader of this camera.

Example:

```ags
ShaderProgram wave = ShaderProgram.CreateFromFile("$DATA$/Shaders/wave.glsl");
Game.Camera.Shader = wave.Default; // assign the default instance for the shader
```

*Compatibility:* Supported by **AGS 4.0.0** and later versions.