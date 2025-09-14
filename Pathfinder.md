## `Pathfinder` functions and properties

### `Pathfinder.FindPath`

```ags
Point*[] Pathfinder.FindPath(int srcx, int srcy, int dstx, int dsty)
```

Tries to find a move path from source to destination, returns an array of navigation points.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Pathfinder.Trace`

```ags
Point* Pathfinder.Trace(int srcx, int srcy, int dstx, int dsty)
```

Traces a straight path from source to destination, returns the furthermost point reached.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Pathfinder.IsWalkableAt`

```ags
bool Pathfinder.IsWalkableAt(int x, int y)
```

Gets whether given coordinates can be walked on.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Pathfinder.NearestWalkablePoint`

```ags
Point* Pathfinder.NearestWalkablePoint(int x, int y)
```

Finds the nearest walkable position, close to the given coordinates. Returns null if no walkable position is found.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.