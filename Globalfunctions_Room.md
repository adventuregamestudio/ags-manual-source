## Global functions (room actions)

### `AreThingsOverlapping`

```ags
AreThingsOverlapping(int thing1, int thing2)
```

Checks whether two characters or objects are overlapping each other on
screen. This simply carries out a quick rectangular check on the two
things to decide - so if they have large transparent regions around the
edges, it may seem to be overlapping too soon.

THING1 and THING2 can either be a CHARID, or can be an object number
PLUS 1000. So for example, passing EGO as THING1, and 1004 as THING2,
will compare the character EGO with Object 4 in the current room.

Returns 0 if they are not overlapping, or the overlapping amount if they
are. This amount is an arbitrary scale, but 1 means they are just about
touching, with higher numbers representing an increasing degree of overlap.

Calling this function with both the parameters as objects is the same as
calling Object.IsCollidingWithObject.

Example:

```ags
if (AreThingsOverlapping(1002, EGO)) {
    // code here
}
```

will run the code if object 2 is overlapping EGO. This could be useful
if object 2 was a bullet, for instance.

*See also:*
[`Character.IsCollidingWithChar`](Character#characteriscollidingwithchar),
[`Object.IsCollidingWithObject`](Object#objectiscollidingwithobject)

---

### `DisableGroundLevelAreas`

```ags
DisableGroundLevelAreas(int disableTints)
```

Disables all ground-level events. This means that all Region events, the
Player Stands On Hotspot event, and the room edges become disabled.

This command is useful in conjunction with the `character[].z` variable,
if you want the player to be able to temporarily fly or levitate, for
example. It allows you to stop the character from triggering Player
Stands On events while they are in the air.

This command is also useful during some cutscenes, if you don't want the
player to trigger events as they walk around the room while in the
cutscene.

The DISABLETINTS parameter specifies whether the visual effects of the
regions (i.e. light levels and tints) are also disabled. If you pass this
as 0, then just the events will be turned off.

Example:

```ags
DisableGroundLevelAreas(0);
```

will disable all ground-level events, but leave light levels working

*See also:* [`Hotspot.Enabled`](Hotspot#hotspotenabled),
[`Region.Enabled`](Region#regionenabled),
[`EnableGroundLevelAreas`](Globalfunctions_Room#enablegroundlevelareas)

---

### `EnableGroundLevelAreas`

```ags
EnableGroundLevelAreas()
```

Re-enables all ground-level events. This is used to reverse the effects
of using the DisableGroundLevelAreas command, and will return things to
normal.

Example:

```ags
EnableGroundLevelAreas();
```

will re-enable all ground-level events.

*See also:* [`Hotspot.Enabled`](Hotspot#hotspotenabled),
[`Region.Enabled`](Region#regionenabled),
[`DisableGroundLevelAreas`](Globalfunctions_Room#disablegroundlevelareas)

---

### `GetBackgroundFrame`

```ags
GetBackgroundFrame()
```

Returns the number of the current background being displayed. In a room
without animating backgrounds, this will always return 0. Otherwise, the
current frame number is returned from 0 to 4.

Example:

```ags
if (GetBackgroundFrame()==4)
    oMoon.Visible = true;
```

will turn on object oMoon if the background frame of the room is frame 4.

*See also:* [`SetBackgroundFrame`](Globalfunctions_Room#setbackgroundframe)

---

### `GetDrawingSurfaceForWalkableArea`

```ags
DrawingSurface* GetDrawingSurfaceForWalkableArea()
```

Gets a drawing surface for the current room's 8-bit walkable mask, which allows you to directly draw onto that mask.

After calling this method, use the various [DrawingSurface functions](DrawingSurface) to modify the mask. Since this is a 8-bit mask, DrawingColor will match the walkable area's index. Color value 0 will refer to "no area" (erase pathways), color value 1 to area number 1, and so forth. Don't forget to call `Release()` on the surface when you are finished to update the room's state.

Any changes you make will only last until the player leaves the room, at which point they will be lost. If you need to make long-lasting changes, you can use this method in the Player Enters Room event, which will recreate these changes whenever player returns back.

Example:

```ags
DrawingSurface *surface = GetDrawingSurfaceForWalkableArea();
surface.DrawingColor = 4;
surface.DrawRectangle(50, 50, 100, 100);
surface.Release();
```

will paint a rectangle for the walkable area 4.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`GetDrawingSurfaceForWalkbehind`](Globalfunctions_Room#getdrawingsurfaceforwalkbehind),
[`Hotspot.GetDrawingSurface`](Hotspot#hotspotgetdrawingsurface),
[`Region.GetDrawingSurface`](Region#regiongetdrawingsurface)

---

### `GetDrawingSurfaceForWalkbehind`

```ags
DrawingSurface* GetDrawingSurfaceForWalkbehind()
```

Gets a drawing surface for the current room's 8-bit walk-behind mask, which allows you to directly draw onto that mask.

After calling this method, use the various [DrawingSurface functions](DrawingSurface) to modify the mask. Since this is a 8-bit mask, DrawingColor will match the walk-behind's index. Color value 0 will refer to "no area" (erase walk-behinds), color value 1 to walk-behind number 1, and so forth. Don't forget to call `Release()` on the surface when you are finished to update the room's state.

Any changes you make will only last until the player leaves the room, at which point they will be lost. If you need to make long-lasting changes, you can use this method in the Player Enters Room event, which will recreate these changes whenever player returns back.

Example:

```ags
DrawingSurface *surface = GetDrawingSurfaceForWalkbehind();
surface.DrawingColor = 4;
surface.DrawRectangle(50, 50, 100, 100);
surface.Release();
```

will paint a rectangle for the walk-behind 4.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`GetDrawingSurfaceForWalkableArea`](Globalfunctions_Room#getdrawingsurfaceforwalkablearea),
[`Hotspot.GetDrawingSurface`](Hotspot#hotspotgetdrawingsurface),
[`Region.GetDrawingSurface`](Region#regiongetdrawingsurface)

---

### `GetScalingAt`

```ags
GetScalingAt (int x, int y)
```

Returns the room area scaling at room co-ordinates (X,Y).

The value returned is from 1 to 200, with 100 being the normal un-scaled
setting.

Example:

```ags
if (GetScalingAt(player.x, player.y) == 100)
    Display ("The player is currently at normal size.");
```

*See also:* [`GetWalkableAreaAt`](Globalfunctions_Room#getwalkableareaat),
[`SetAreaScaling`](Globalfunctions_Room#setareascaling)

---

### `GetViewportX`

**This function is obsolete since AGS 3.5.0. Use [`Game`](Game#gamecamera).[`Camera.X`](Camera#camerax) instead.**

```ags
GetViewportX()
```

Returns the X-position of the main camera's rectangle in a room. This
allows you to find out what part of the room the player is looking at.

---

### `GetViewportY`

**This function is obsolete since AGS 3.5.0. Use [`Game`](Game#gamecamera).[`Camera.Y`](Camera#cameray) instead.**

```ags
GetViewportY()
```

Returns the Y-position of the main camera's rectangle in a room. This
allows you to find out what part of the room the player is looking at.

---

### `GetWalkableAreaAt`

**This function is obsolete since AGS 3.5.0. Use equivalent [`GetWalkableAreaAtScreen`](Globalfunctions_Room#getwalkableareaatscreen) instead.**

```ags
int GetWalkableAreaAt(int x, int y)
```

Returns the ID of the walkable area at *screen* co-ordinates (X, Y). If there is no walkable area there, or if invalid co-ordinates are specified, returns 0.

---

### `GetWalkableAreaAtRoom`

```ags
int GetWalkableAreaAtRoom(int x, int y)
```

Returns the ID of the walkable area at *room* co-ordinates (X, Y). If there is no walkable area there, or if invalid co-ordinates are specified, returns 0.

This function may be useful when you need to find a walkable area relative to some other point in room, a room object or character.

Example:

```ags
if (GetWalkableAreaAtRoom(player.x, player.y) > 0)
    Display ("Player is standing on the walkable area.");
```

*Compatibility:* Supported by **AGS 3.5.0** and later versions.

*See also:* [`GetWalkableAreaAtScreen`](Globalfunctions_Room#getwalkableareaatscreen), [`Hotspot.GetAtRoomXY`](Hotspot#hotspotgetatroomxy),
[`Region.GetAtRoomXY`](Region#regiongetatroomxy),
[`GetScalingAt`](Globalfunctions_Room#getscalingat)

---

### `GetWalkableAreaAtScreen`

```ags
int GetWalkableAreaAtScreen(int x, int y)
```

Returns the ID of the walkable area at *screen* co-ordinates (X, Y). If there is no walkable area there, or if invalid co-ordinates are specified, returns 0.

There has to be an active room viewport at the specified screen location for this function to succeed, otherwise it also returns 0.

This function is convenient when you need to find a walkable area relative to some screen point, such as mouse cursor's position.

Example:

```ags
if (GetWalkableAreaAtScreen(mouse.x, mouse.y) == 0)
    Display ("You can't walk there.");
```

*Compatibility:* Supported by **AGS 3.5.0** and later versions.

*See also:* [`GetWalkableAreaAtRoom`](Globalfunctions_Room#getwalkableareaatroom), [`Hotspot.GetAtScreenXY`](Hotspot#hotspotgetatscreenxy),
[`Region.GetAtScreenXY`](Region#regiongetatscreenxy),
[`GetScalingAt`](Globalfunctions_Room#getscalingat)

---

### `GetWalkBehindBase`

```ags
int GetWalkBehindBase(int area)
```

Returns the baseline of the requested walk-behind area. If return value is 0 - that means that the walk-behind is disabled.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:* [`SetWalkBehindBase`(Globalfunctions_Room#setwalkbehindbase)], [`Object.Baseline`](Object#objectbaseline)

---

### `HasPlayerBeenInRoom`

```ags
HasPlayerBeenInRoom(int room_number)
```

Checks whether the player has ever been in ROOM_NUMBER (i.e. has the
'First Time Player Enters Room' event there ever been run). Returns 1 if
they have, and 0 if they haven't.

You can use this function to determine whether the player has been to a
particular location previously. If you reset the room with ResetRoom,
then this command will return 0 until they enter the room again.

This command will always return 1 if you ask it about the current room;
and it will always return 0 if you ask it about a non-state saving room
(i.e. rooms numbered > 300).

Example:

```ags
if (HasPlayerBeenInRoom(14)) {
    Display("The player has been to room 14 before.");
}
```

will display a message if the player has been to room 14.

*See also:* [`ResetRoom`](Globalfunctions_Room#resetroom)

---

### `ReleaseViewport`

**This function is obsolete since AGS 3.5.0. Use [`Game`](Game#gamecamera).[`Camera.AutoTracking`](Camera#cameraautotracking) instead.**

```ags
ReleaseViewport()
```

Releases the lock on the main camera, allowing it to automatically
scroll around following the player character as normal.

---

### `RemoveWalkableArea`

```ags
RemoveWalkableArea (int areanum)
```

Removes the walkable areas in color AREANUM from the current room. You
can put the area back with RestoreWalkableArea.

NOTE: When the player leaves the screen, all the walkable areas are
reset. Therefore, if you want an area to remain off when they leave the
screen, you will need to set a flag, then run the RemoveWalkableArea
command in the "Player enters room" event when they return.

Example:

```ags
RemoveWalkableArea(5);
```

will make the walking area 5 unwalkable.

*See also:* [`RestoreWalkableArea`](Globalfunctions_Room#restorewalkablearea)

---

### `ResetRoom`

```ags
ResetRoom (int room_number)
```

Discards all the data that the engine has in memory about when the
player last visited ROOM_NUMBER, and resets it as if they'd never been
there. The next time the player goes to that room, all the objects and
scripts will be in their initial state (as set up in the editor), and
not how they were when the player left the room. The "First time enters
room" event will be run when they enter this room again.

This function is useful if you want to have a "View intro" option to
allow the player to watch an intro again - this function can reset all
the objects in the intro rooms to their starting positions.

NOTE: You cannot reset the current room (i.e. the room that the player is
in).

Example:

```ags
ResetRoom(0);
```

will reset the intro room so it can be played again if the player wants
to.

*See also:* [`HasPlayerBeenInRoom`](Globalfunctions_Room#hasplayerbeeninroom)

---

### `RestoreWalkableArea`

```ags
RestoreWalkableArea (int areanum)
```

Makes the area AREANUM walkable again.

Example:

```ags
RestoreWalkableArea(4);
```

will make the walking area 4 walkable again.

*See also:* [`RemoveWalkableArea`](Globalfunctions_Room#removewalkablearea)

---

### `SetAreaScaling`

```ags
SetAreaScaling(int area, int min, int max)
```

Changes walkable area number AREA's scaling.

There are two ways to use this command:<br>
1. Pass the same value for MIN and MAX. This will give the walkable area
fixed scaling (same as setting it in the editor with "Use continuous
scaling" un-ticked).<br>
2. Pass different values for MIN and MAX. In this case, continuous
scaling is enabled for the walkable area, and will go from MIN at the
top to MAX at the bottom.

MIN and MAX have ranges from 5 to 200, the same as in the editor. Pass
100 for both values to revert to the normal zoom level (100`%`) for that
area.

Example:

```ags
SetAreaScaling(5, 120, 170);
```

will set walkable area 5 to use continuous scaling from 120 to 170
percent.

*See also:* [`GetScalingAt`](Globalfunctions_Room#getscalingat),
[`GetWalkableAreaAt`](Globalfunctions_Room#getwalkableareaat)

---

### `SetBackgroundFrame`

```ags
SetBackgroundFrame (int frame)
```

Locks the background to frame number FRAME of an animating-background
screen. (Values for FRAME are from 0 to 4). This allows you to use the
animating backgrounds feature for another purpose - you can have two
frames of the background, one for example with a spaceship crashed on
it. Then, once the right event has happened, call SetBackgroundFrame in
the Player Enters Room event to set the background before the screen
fades in.

Pass the *frame* as -1 to return to the default behavior of
automatically cycling through all the background frames.

The frame lock is released when the game changes rooms.

Example:

```ags
SetBackgroundFrame(4);
```

will change the current room's background frame to 4.

*See also:* [`GetBackgroundFrame`](Globalfunctions_Room#getbackgroundframe)

---

### `SetViewport`

**This function is obsolete since AGS 3.5.0. Use [`Game`](Game#gamecamera).[`Camera.SetAt`](Camera#camerasetat) instead.**

```ags
SetViewport(int x, int y)
```

Locks the main camera to having the top-left hand corner at (X,Y) in
a scrolling room. This allows you to manually pan across a scrolling
room or to have the camera follow a non-player character.

The lock is released when you either call [`ReleaseViewport`](Globalfunctions_Room#releaseviewport) or the player
changes rooms.

---

### `SetWalkBehindBase`

```ags
SetWalkBehindBase (int area, int baseline)
```

Changes the walk-behind AREA to have new BASELINE. This effectively
allows you to turn walk-behinds on and off, although you can do other
tricks with it as well. BASELINE is from 1 to the height of the room
(normally 200) and moves the line which you set originally in the
editor.

Passing BASELINE as 0 disables the walk-behind area, so that the player
will always walk in front of it.

Basically, if the character's feet are below BASELINE, he will be drawn
in front of it, otherwise he will be drawn behind it.

Example:

```ags
SetWalkBehindBase (3,0);
```

will disable the walkbehind area number 3.

*See also:* [`GetWalkBehindBase`(Globalfunctions_Room#getwalkbehindbase)], [`Object.Baseline`](Object#objectbaseline)
