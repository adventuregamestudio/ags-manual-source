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

### `WalkableArea.GetByScriptName`

```ags
static WalkableArea* WalkableArea.GetByScriptName(string scriptName)
```

Returns a pointer to the WalkableArea with the specified script name, or null if it does not exist.

Normally you do not need to use this, as there will be a automatically created global script variable for each WalkableArea which got a script name.
Where GetByScriptName() function may come useful is situation in which you a) do not know exact name, b) had to store walkable area's reference in a string for some reason. Good examples of this are saving walkable area's name in a [custom property](CustomProperties), or a [file](File), then reading it back.

Example:

```ags
void OpenDoor(Object *obj) {
    obj.SetView(VDOORANIMATION);
    obj.Animate(1, 4, eOnce, eBlock);
    String exitName = obj.GetTextProperty("ExitArea");
    WalkableArea* exitArea = WalkableArea.GetByScriptName(exitName);
    if (exitArea != null) {
        exitArea.Enabled = true;
    }
}
```

Animates a "door" object, retrieves "exit" area related to that door, and enables it.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [`WalkableArea.ScriptName`](WalkableArea#walkableareascriptname)

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

### `WalkableArea.ScriptName`

```ags
readonly String WalkableArea.ScriptName
```

Gets the script name of the walkable area, which serves as a unique identifier, as set in the AGS Editor.

This may be useful if you have a pointer to some walkable area stored in your variable, and want to know what it actually is. Normally you don't need a script name, as you have an automatic global variable for each walkable area in the game, but sometimes you may want to save its script name as a text either to display it somewhere for testing purposes, or keep as a reference. You may later use [WalkableArea.GetByScriptName](WalkableArea#walkableareagetbyscriptname) function to retrieve the walkable area by the previously saved script name.

Example:

```
function TestAreaAt(int x, int y)
{
	WalkableArea* area = WalkableArea.GetAtRoomXY(x, y);
    System.Log(eLogDebug, "There's walkable area %s at position %d,%d", area.ScriptName, x, y);
}
```

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [`WalkableArea.GetByScriptName`](WalkableArea#walkableareagetbyscriptname)

---

### `WalkableArea.FaceDirectionRatio`

```ags
static float WalkableArea.FaceDirectionRatio
```

Gets/sets the optional y/x ratio of character's facing directions, determining directional loop selection for each Character while on this area.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.
