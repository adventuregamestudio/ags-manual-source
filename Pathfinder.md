## `Pathfinder` functions and properties

Pathfinder struct is the base type for all pathfinders created by the engine. Any other pathfinder is a descendant of this type, and inherits all its functions.

Pathfinder suggests that there's a 2D space where each (x,y) point is either passable or unpassable. Its functions are used to test certain coordinates, and find a path from one point to another.

You cannot create an instance of the parent Pathfinder type directly. Rooms provide their own pathfinder with [Room.PathFinder](Room#roompathfinder) property. You may create custom pathfinders of specific child types, such as [MaskPathfinder](MaskPathfinder).

*Compatibility:* The Pathfinder struct is supported by **AGS 4.0.0** and later versions.

*See also:*
[MaskPathfinder](MaskPathfinder),
[Room.PathFinder](Room#roompathfinder)

### `Pathfinder.FindPath`

```ags
Point*[] Pathfinder.FindPath(int srcx, int srcy, int dstx, int dsty)
```

Tries to find a move path from source to destination, returns an array of navigation points. If no suitable path was found, or source position equals destination, then returns null.

Example:

```ags
Point* path[] = Room.PathFinder.FindPath(100, 100, 200, 200);
if (path != null)
{
    for (int i = 0; i < path.Length - 1; i++)
    {
        oObject.SetPosition(path[i].x, path[i].y);
        oObject.Move(path[i + 1].x, path[i + 1].y, 5, eNoBlock, eAnywhere);
        while (oObject.Moving) Wait(1);
    }
}
```

This will find a path between two points in room and move a room object along that path. Of course normally you would just use [Object.Move](Object#objectmove) alone, since it does pathfinding itself. But above script may be used if you need more control over what object does along the way. Also, a similar script may be used to move kind of entities that do not have a "Move" function, such as Overlays or GUI, for example.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Pathfinder.Trace`

```ags
Point* Pathfinder.Trace(int srcx, int srcy, int dstx, int dsty)
```

Traces a straight path from source to destination, returns the furthermost point reached.

Trace may be used when you need to know how far an object may move in the certain direction. Its result may also be used to find the nearest "wall" in that direction: because its return value is the last passable point before the wall, then the next coordinate along the same line will be non-passable.

Example:

```ags
Point* dest = Room.PathFinder.Trace(player.x, player.y, player.x + directionX, player.y + directionY);
player.x = dest.x;
player.y = dest.y;
```

This will find the farthemost point from player character along the given vector, and "teleport" character there.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Pathfinder.IsWalkableAt`

```ags
bool Pathfinder.IsWalkableAt(int x, int y)
```

Gets whether given coordinates can be walked on.

Example:

```ags
function on_mouse_click(MouseButton button, int mx, int my)
{
  if (button == eMouseLeft)
  {
    Point* p = Screen.ScreenToRoomPoint(mx, my);
    if (mouse.Mode == eModeWalkto && !Room.PathFinder.IsWalkableAt(p.x, p.y))
    {
      player.Say("I can't get there.");
    }
    else
    {
      Room.ProcessClick(mx, my, mouse.Mode);
    }
  }
}
```

will make the player comment when they click with the Walk cursor on a spot that cannot be walked on, instead of walking to the nearest possible place; all the other clicks are processed as usual. The pathfinder works in room coordinates, so the position of the click is converted from screen coordinates first.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:*
[`WalkableArea.GetAtRoomXY`](WalkableArea#walkableareagetatroomxy)

---

### `Pathfinder.NearestWalkablePoint`

```ags
Point* Pathfinder.NearestWalkablePoint(int x, int y)
```

Finds the nearest walkable position, close to the given coordinates. Returns null if no walkable position is found.

Example:

```ags
Point* p = Room.PathFinder.NearestWalkablePoint(oBall.X, oBall.Y);
if (p != null)
{
  cDog.x = p.x;
  cDog.y = p.y;
}
```

will place the character cDog on the walkable spot nearest to the ball, even if the ball itself lies outside of the walkable areas.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:*
[`Character.PlaceOnWalkableArea`](Character#characterplaceonwalkablearea)
[`Room.NearestWalkableArea`](Room#roomnearestwalkablearea)
