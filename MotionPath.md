## `MotionPath` functions and properties

MotionPath struct represents a *process* of moving along a path of points in a 2D space. Besides the path of points itself, it provides properties and functions for going forward and backward along that path, reading current progress and position in space, or changing the progress directly.

MotionPath is not necessarily connected to a certain real entity, and does not move anything on its own. It represents a state of moving of *some* object, which may be one of the built-in AGS object types (such as Character or room Object), or your own custom object type, or even an imaginary object. There are two general uses of this struct:

1. If you have a moving Character or room Object, you may retrieve their MotionPath and either read or override their movement through it.
2. If you have a custom object, or a standard object that does not have a moving feature (such as GUI, for example), you may create MotionPath and use it as a reference to know where your custom object has to be located every given moment.

MotionPath is based on a array (sequence) of points, connected with straight lines. There's a start point and end point, and any number of points between (so 2 points at least). A section between each two points is called a **Stage** and stages have zero-based index. The section between point 0 and point 1 is stage 0, the section between point 1 and 2 is stage 1, and so on. There's always one extra stage, which begins with the end point and leads nowhere; it's there for technical reasons. But this means that the total number of stages is equal to the number of points and not number of point pairs, although that's bit counterintuitive.

The **Progress** of the current stage is a floating-point value that may be between 0.0 and 1.0, including 0, but excluding 1. When the progress reaches 1 the MotionPath advances to the next stage. If the progress was reset back beyond 0.0, then the MotionPath backs to the previous stage. The current position at any given time is calculated from the progress value by taking a vector between first and second point of a current stage, and multiplying by the progress.

To summarize: **Stage** is a pair of points, and **Progress** is a fraction of a completed path between them.

At any moment MotionPath reports **Position** which corresponds to the current stage and progress. This position property may be used to tell where the moving object is located.

Each stage has **Speed** values associated with it (SpeedX and SpeedY properties), they tell how fast a object should be moving along X and Y axes when making a step in this stage. These speed values are given in pixels. The movement speed determines the number of steps required to pass a stage.

MotionPath has a **Direction** property, which defines the order it walks the path points, and hence - the sequence of stages. eForwards direction means going from the point 0 to the point N-1, while eBackwards direction means going from the point N-1 to point 0. Depending on RepeatStyle, direction may change during motion.
Note that the Stage index is always counted from the first pair of points up. If the Direction is eBackwards then the path Stages are passed in the reverse direction. However, the Progress always means a fraction of advance in the *current direction*.

**RepeatStyle** property defines what happens when you reach the end of the path. The motion may stop, or reset, or change direction depending on this value. See a [corresponding section](MotionPath#motionpathrepeatstyle) below for a more detailed explanation.

MotionPath's progress may be controlled manually, even if it's not created by you but retrieved from the game object. This is done by calling **StepForward()** or **StepBackward()** functions respectively. StepForward advances the current progress one step forth and StepBackward reverts one step back. The actual amount of progress added per each step depends on the distance between stage points and speed values. You may alternate StepForward() or StepBackward() at will, not forced to only step either forth or back all the time.

Note that StepForward and StepBackward act *relatively* to the Direction property. StepForward always advances in current direction, and StepBackward goes in opposite direction. Thus, if the Direction is set to eBackwards, then StepForward() will actually make a step backwards, and StepBackward() double-reverses the movement resulting in a step forwards.

*Compatibility:* The MotionPath struct is supported by **AGS 4.0.0** and later versions.

*See also:*
[Character.MotionPath](Character#charactermotionpath),
[Object.MotionPath](Object#objectmotionpath),
[Pathfinder](Pathfinder),
[MaskPathfinder](MaskPathfinder),
[Room.PathFinder](Room#roompathfinder)

### `MotionPath.Create`

```ags
static MotionPath* MotionPath.Create(Point* path[], float speedx, optional float speedy, optional RepeatStyle, optional Direction)
```

Creates a new MotionPath from the array of Points and x/y speed values. Each stage of this path will use same x/y speeds, but may have different velocities, depending on the direction vector between two points.

Example:

```ags
Point* path[] = Room.PathFinder.FindPath(20, 30, 200, 160);
if (path != null)
{
  motion = MotionPath.Create(path, 2.0, 2.0, eRepeatAlternate);
}
```

will find a path between two points in the room and create a MotionPath from it, moving by 2 pixels per step along both axes and going back and forth between the ends of the path. Here `motion` is a global MotionPath* variable; see [`StepForward`](MotionPath#motionpathstepforward) for how to move something along it.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `MotionPath.Create2`

```ags
static MotionPath* MotionPath.Create2(Point* path[], float speedx[], float speedy[], optional RepeatStyle, optional Direction)
```

Creates a new MotionPath from the array of Points and arrays of x/y speed values per each stage.

Example:

```ags
Point* path[] = Room.PathFinder.FindPath(20, 30, 200, 160);
if (path != null)
{
  float speedx[] = new float[path.Length];
  float speedy[] = new float[path.Length];
  for (int i = 0; i < path.Length; i++)
  {
    speedx[i] = 1.0 + IntToFloat(i);
    speedy[i] = 1.0 + IntToFloat(i);
  }
  motion = MotionPath.Create2(path, speedx, speedy);
}
```

will find a path between two points in the room and create a MotionPath from it, where every next stage is moved along faster than the previous one. Each stage has its own x/y speed value, given in the speed arrays. Here `motion` is a global MotionPath* variable; see [`StepForward`](MotionPath#motionpathstepforward) for how to move something along it.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `MotionPath.GetPath`

```ags
Point*[] MotionPath.GetPath()
```

Gets the copy of an underlying path as an array of Points.

Example:

```ags
MotionPath* motion = cEgo.MotionPath;
if (motion != null)
{
  Point* path[] = motion.GetPath();
  Display("The path has %d points.", path.Length);
}
```

will get a copy of the path the character cEgo is currently moving along, and display the number of points in it.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `MotionPath.StepBack`

```ags
MotionPath.StepBack()
```

Moves the current progress one step back. If the progress goes past 0, then the MotionPath returns to the previous Stage in the sequence, according to the current Direction:
* eForwards - goes to Stage - 1.
* eBackwards - goes to Stage + 1.

In that case the progress wraps to some value close but not equal to 1.0 (actual value will depend on the step's size).

If at the same time the current stage was the first stage of this direction, then the result depends on the RepeatStyle property:
* eOnce - Stage remains at the current index, and Progress remains 0.
* eOnceReset - Stage remains at the current index, and Progress remains 0.
* eOnceAndBack - if current is a starting direction, then Stage remains at the current index (and Progress remains 0); if current is a second direction, then Stage remains at the current index but Direction changes to the opposite (and progress wraps to a value close to 1.0).
* eRepeat - Stage wraps to opposite end of the path (and progress wraps to a value close to 1.0).
* eRepeatAlternate - Stage remains at the current index but Direction changes to the opposite (and progress wraps to a value close to 1.0).

Example:

```ags
function room_RepExec()
{
  if (motion == null)
  {
    return;
  }
  if (IsKeyPressed(eKeyRightArrow))
  {
    motion.StepForward();
  }
  else if (IsKeyPressed(eKeyLeftArrow))
  {
    motion.StepBack();
  }
  cart.X = motion.PositionX;
  cart.Y = motion.PositionY;
}
```

will let the player move the overlay cart back and forth along a track with the arrow keys. Here `motion` and `cart` are global variables: a MotionPath created with MotionPath.Create, and a room overlay.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `MotionPath.StepForward`

```ags
MotionPath.StepForward()
```

Moves the current progress one step forward. If the progress goes past 1.0, then the MotionPath advances to the next Stagein the sequence, according to the current Direction:
* eForwards - goes to Stage + 1.
* eBackwards - goes to Stage - 1.

In that case the progress wraps to some value close or equal to 0.0 (actual value will depend on the step's size).

If at the same time the current stage was the last stage of this direction, then the result depends on the RepeatStyle property:

* eOnce - MotionPath is marked as "completed".
* eOnceReset - MotionPath is marked as "completed". Stage gets reset to the first index (depends on starting direction).
* eOnceAndBack - if current is a starting direction, then Stage remains at the current index but Direction changes to the opposite (and progress wraps to a value close to 0.0); if current is a second direction, then MotionPath is marked as "completed".
* eRepeat - Stage wraps to opposite end of the path (and progress wraps to a value close to 0.0).
* eRepeatAlternate - Stage remains at the current index but Direction changes to the opposite (and progress wraps to a value close to 0.0).

Example:

```ags
MotionPath* motion;
Overlay* cart;

function room_AfterFadeIn()
{
  Point* path[] = Room.PathFinder.FindPath(20, 30, 200, 160);
  if (path != null)
  {
    motion = MotionPath.Create(path, 2.0);
    cart = Overlay.CreateRoomGraphical(motion.PositionX, motion.PositionY, 300);
    cart.GraphicAnchorX = 0.5; // keep the bottom center of the sprite on the path,
    cart.GraphicAnchorY = 1.0; // the same way as characters are placed
  }
}

function room_RepExec()
{
  if (motion != null && !motion.IsCompleted)
  {
    motion.StepForward();
    cart.X = motion.PositionX;
    cart.Y = motion.PositionY;
  }
}
```

will move a room overlay showing the sprite 300 along a path found by the room's pathfinder, one step every game cycle, until the end of the path is reached. A MotionPath does not move anything by itself: your script decides what to do with the position it reports.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `MotionPath.Reset`

```ags
MotionPath.Reset(optional int stage, optional float progress)
```

Reset the current path to the certain stage and progress position [0.0, 1.f). If no stage and progress arguments are passed, then it resets to Stage 0, Progress 0.0.

This function may be used to freely set any stage and progress, disregarding current position and movement speeds.

Example:

```ags
function btnRestartRace_OnClick(GUIControl *control, MouseButton button)
{
  motion.Reset();
  cart.X = motion.PositionX;
  cart.Y = motion.PositionY;
}
```

will put the overlay cart back at the start of its track. Here `motion` and `cart` are global variables: a MotionPath created with MotionPath.Create, and a room overlay.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `MotionPath.Valid`

```ags
readonly bool MotionPath.Valid
```

Checks whether this motion path is currently valid. MotionPaths created by you in script should always stay valid, but a MotionPath acquired from a Character or Object stops being valid as soon as that character or object stops moving. In the latter case you need to get a new MotionPath instance from them when they begin moving again.

Example:

```ags
MotionPath* egoPath;

function repeatedly_execute()
{
  if (egoPath == null || !egoPath.Valid)
  {
    egoPath = cEgo.MotionPath; // null when cEgo is not moving
  }
  if (egoPath != null)
  {
    lblStage.Text = String.Format("Stage %d", egoPath.Stage);
  }
}
```

will keep a reference to the character's current motion path, and get a new one whenever the old one becomes invalid, which happens when the character stops moving.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `MotionPath.Direction`

```ags
readonly Direction MotionPath.Direction
```

Gets this motion path's current moving direction (forwards or backwards).

Note that functions StepForward and StepBackward act relatively to this direction property. If MotionPath's direction is set as eBackwards, then calling StepForward will actually make a move backwards, and calling StepBackward will make a move forward.

Example:

```ags
function room_RepExec()
{
  if (motion == null)
  {
    return;
  }
  motion.StepForward();
  birdOverlay.X = motion.PositionX;
  birdOverlay.Y = motion.PositionY;
  if (motion.Direction == eBackwards)
  {
    birdOverlay.Flip = eFlipLeftToRight;
  }
  else
  {
    birdOverlay.Flip = eFlipNone;
  }
}
```

will move the room overlay birdOverlay back and forth along a path, and mirror its image whenever it goes backwards. Here `motion` is a global MotionPath* variable, created with MotionPath.Create using the eRepeatAlternate style.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `MotionPath.RepeatStyle`

```ags
readonly RepeatStyle MotionPath.RepeatStyle
```

Gets this motion path's repeat style.

Following styles are supported:
* eOnce - MotionPath goes in the starting direction until the end of the path and completes.
* eOnceReset - MotionPath goes in the starting direction until the end of the path, resets to beginning and completes.
* eOnceAndBack - MotionPath goes in the starting direction until the end of the path, then goes in the opposite direction until the former beginning and completes.
* eRepeat - MotionPath keeps going in the starting direction perpetually, when reaching an end of the path it resets to beginning and continues.
* eRepeatAlternate - MotionPath keeps going between two ends of the paths perpetually, switching direction every time it reaches either end.

Example:

```ags
MotionPath* motion = oBall.MotionPath;
if (motion != null && motion.RepeatStyle == eRepeat)
{
  Display("The object keeps going along its path forever.");
}
```

will tell if the object oBall repeats its path endlessly.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `MotionPath.WalkWhere`

```ags
readonly WalkWhere MotionPath.WalkWhere
```

Gets if this motion path's was generated by pathfinder respecting walkable areas.

Example:

```ags
MotionPath* motion = cEgo.MotionPath;
if (motion != null && motion.WalkWhere == eWalkableAreas)
{
  Display("The path respects the walkable areas.");
}
```

will tell if the path the character cEgo is walking along was generated by the pathfinder respecting walkable areas.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `MotionPath.IsCompleted`

```ags
readonly bool MotionPath.IsCompleted
```

Gets whether this motion path has reached the end and an object should stop.

Example:

```ags
function room_RepExec()
{
  if (motion == null)
  {
    return;
  }
  motion.StepForward();
  cart.X = motion.PositionX;
  cart.Y = motion.PositionY;
  if (motion.IsCompleted)
  {
    motion = null;
    cart.Remove();
  }
}
```

will move the overlay cart along a path every game cycle, and remove it once it has reached the end. Here `motion` and `cart` are global variables: a MotionPath created with MotionPath.Create, and a room overlay.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `MotionPath.StageCount`

```ags
readonly int MotionPath.StageCount
```

Gets number of path stages. Last stage defines the path's end-point, and there's always at least 2 stages.

Example:

```ags
MotionPath* motion = cEgo.MotionPath;
if (motion != null)
{
  DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
  surface.DrawingColor = Game.GetColorFromRGB(255, 0, 0);
  for (int i = 0; i < motion.StageCount - 1; i++)
  {
    surface.DrawLine(motion.StageX[i], motion.StageY[i], motion.StageX[i + 1], motion.StageY[i + 1]);
  }
  surface.Release();
}
```

will draw the path of the character cEgo onto the room background with red lines, which may help when testing the walkable areas.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `MotionPath.StageX`

```ags
readonly int MotionPath.StageX[]
```

Accesses the X coordinate of the each stage's starting position.

Example:

```ags
MotionPath* motion = cEgo.MotionPath;
if (motion != null)
{
  DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
  surface.DrawingColor = Game.GetColorFromRGB(255, 0, 0);
  for (int i = 0; i < motion.StageCount - 1; i++)
  {
    surface.DrawLine(motion.StageX[i], motion.StageY[i], motion.StageX[i + 1], motion.StageY[i + 1]);
  }
  surface.Release();
}
```

will draw the path of the character cEgo onto the room background with red lines, which may help when testing the walkable areas.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `MotionPath.StageY`

```ags
readonly int MotionPath.StageY[]
```

Accesses the Y coordinate of the each stage's starting position.

Example:

```ags
MotionPath* motion = cEgo.MotionPath;
if (motion != null)
{
  DrawingSurface *surface = Room.GetDrawingSurfaceForBackground();
  surface.DrawingColor = Game.GetColorFromRGB(255, 0, 0);
  for (int i = 0; i < motion.StageCount - 1; i++)
  {
    surface.DrawLine(motion.StageX[i], motion.StageY[i], motion.StageX[i + 1], motion.StageY[i + 1]);
  }
  surface.Release();
}
```

will draw the path of the character cEgo onto the room background with red lines, which may help when testing the walkable areas.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `MotionPath.StageSpeedX`

```ags
readonly float MotionPath.StageSpeedX[]
```

Accesses the horizontal speed magnitude of the each stage.

Example:

```ags
MotionPath* motion = cEgo.MotionPath;
if (motion != null)
{
  Display("The first stage's horizontal speed is %.1f.", motion.StageSpeedX[0]);
}
```

will display the horizontal speed of the first stage of the character's current path.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `MotionPath.StageSpeedY`

```ags
readonly float MotionPath.StageSpeedY[]
```

Accesses the vertical speed magnitude of the each stage.

Example:

```ags
MotionPath* motion = cEgo.MotionPath;
if (motion != null)
{
  Display("The first stage's vertical speed is %.1f.", motion.StageSpeedY[0]);
}
```

will display the vertical speed of the first stage of the character's current path.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `MotionPath.Stage`

```ags
readonly int MotionPath.Stage
```

Gets the current stage index.

Example:

```ags
MotionPath* motion = cEgo.MotionPath;
if (motion != null)
{
  Display("The character is on stage %d of %d.", motion.Stage + 1, motion.StageCount);
}
```

will display which stage of its path the character is currently on.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `MotionPath.Progress`

```ags
readonly float MotionPath.Progress
```

Gets the progress of the current stage in the range [0.0, 1.f).

Example:

```ags
MotionPath* motion = cEgo.MotionPath;
if (motion != null)
{
  Display("The current stage is %d percent complete.", FloatToInt(motion.Progress * 100.0));
}
```

will display how much of the current stage of the path the character has covered, in percents.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `MotionPath.PositionX`

```ags
readonly int MotionPath.PositionX
```

Gets the current would-be X coordinate of an object.

Example:

```ags
function room_RepExec()
{
  if (motion != null)
  {
    motion.StepForward();
    cart.X = motion.PositionX;
    cart.Y = motion.PositionY;
  }
}
```

will move the overlay cart along a path, one step every game cycle. Here `motion` and `cart` are global variables: a MotionPath created with MotionPath.Create, and a room overlay.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `MotionPath.PositionY`

```ags
readonly int MotionPath.PositionY
```

Gets the current would-be Y coordinate of an object.

Example:

```ags
function room_RepExec()
{
  if (motion != null)
  {
    motion.StepForward();
    cart.X = motion.PositionX;
    cart.Y = motion.PositionY;
  }
}
```

will move the overlay cart along a path, one step every game cycle. Here `motion` and `cart` are global variables: a MotionPath created with MotionPath.Create, and a room overlay.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `MotionPath.VelocityX`

```ags
readonly float MotionPath.VelocityX
```

Gets the current X magnitude of an object's velocity.

Example:

```ags
function room_RepExec()
{
  if (motion == null)
  {
    return;
  }
  motion.StepForward();
  fish.X = motion.PositionX;
  fish.Y = motion.PositionY;
  // turn the fish to face the direction it is moving in
  fish.Rotation = Maths.RadiansToDegrees(Maths.ArcTan2(motion.VelocityY, motion.VelocityX));
}
```

will move the room overlay fish along a path, and rotate its image so that it always faces the direction of its movement (the fish sprite itself should face right, and the overlay's GraphicAnchorX and GraphicAnchorY should be set to 0.5, so that the fish is centered on the path). Here `motion` is a global MotionPath* variable, created with MotionPath.Create.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `MotionPath.VelocityY`

```ags
readonly float MotionPath.VelocityY
```

Gets the current Y magnitude of an object's velocity.

Example:

```ags
function room_RepExec()
{
  if (motion == null)
  {
    return;
  }
  motion.StepForward();
  fish.X = motion.PositionX;
  fish.Y = motion.PositionY;
  // turn the fish to face the direction it is moving in
  fish.Rotation = Maths.RadiansToDegrees(Maths.ArcTan2(motion.VelocityY, motion.VelocityX));
}
```

will move the room overlay fish along a path, and rotate its image so that it always faces the direction of its movement (the fish sprite itself should face right, and the overlay's GraphicAnchorX and GraphicAnchorY should be set to 0.5, so that the fish is centered on the path). Here `motion` is a global MotionPath* variable, created with MotionPath.Create.