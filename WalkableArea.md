## `WalkableArea` functions and properties

### `WalkableArea.GetAtScreenXY`

```ags
static WalkableArea* WalkableArea.GetAtScreenXY(int x, int y)
```

Gets the walkable area at the specified position on the screen.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `WalkableArea.GetAtRoomXY`

```ags
static WalkableArea* WalkableArea.GetAtRoomXY(int x, int y)
```

Returns the walkable area at the specified position within this room.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `WalkableArea.DrawingSurface`

```ags
static WalkableArea.DrawingSurface* GetDrawingSurface()
```

Gets the drawing surface for the 8-bit walkable mask

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `WalkableArea.SetScaling`

```ags
void WalkableArea.SetScaling(int min, int max)
```

Changes this walkable area's scaling level.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `WalkableArea.GetProperty`

```ags
int WalkableArea.GetProperty(const string property)
```

Gets an integer custom property for this area.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `WalkableArea.GetTextProperty`

```ags
String WalkableArea.GetTextProperty(const string property)
```

Gets a text custom property for this area.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `WalkableArea.SetProperty`

```ags
bool WalkableArea.SetProperty(const string property, int value)
```

Sets an integer custom property for this area.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `WalkableArea.SetTextProperty`

```ags
bool WalkableArea.SetTextProperty(const string property, const string value)
```

Sets a text custom property for this area.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `WalkableArea.ID`

```ags
readonly int WalkableArea.ID
```

Gets the ID number for this area.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `WalkableArea.Enabled`

```ags
bool WalkableArea.Enabled
```

Gets/sets whether this walkable area is enabled.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `WalkableArea.ScalingMin`

```ags
readonly int WalkableArea.ScalingMin
```

Gets this walkable area's minimal scaling level.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `WalkableArea.ScalingMax`

```ags
readonly int WalkableArea.ScalingMax
```

Gets this walkable area's maximal scaling level.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `WalkableArea.FaceDirectionRatio`

```ags
static float WalkableArea.FaceDirectionRatio
```

Gets/sets the optional y/x ratio of character's facing directions, determining directional loop selection for each Character while on this area.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---
