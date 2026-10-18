## `WalkableArea` functions and properties

### `WalkableArea.GetAtScreenXY`

```ags
static WalkableArea* WalkableArea.GetAtScreenXY(int x, int y)
```

Gets the walkable area at the specified position on the screen.

Example:

```ags
WalkableArea *area = WalkableArea.GetAtScreenXY(mouse.x, mouse.y);
if (area != null)
{
  Display("The mouse is over walkable area %d.", area.ID);
}
```

will tell which walkable area, if any, is under the mouse cursor.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `WalkableArea.GetAtRoomXY`

```ags
static WalkableArea* WalkableArea.GetAtRoomXY(int x, int y)
```

Returns the walkable area at the specified position within this room.

Example:

```ags
WalkableArea *area = WalkableArea.GetAtRoomXY(player.x, player.y);
if (area != null)
{
  Display("The player stands on walkable area %d.", area.ID);
}
```

will tell which walkable area, if any, the player character is standing on.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `WalkableArea.GetDrawingSurface`

```ags
static DrawingSurface* GetDrawingSurface()
```

Gets the drawing surface for the 8-bit walkable mask

Example:

```ags
DrawingSurface *surface = WalkableArea.GetDrawingSurface();
surface.DrawingColor = 1;
surface.DrawRectangle(10, 10, 50, 50);
surface.Release();
```

will add a rectangle of the walkable area 1 to the walkable mask, for example to make a newly built bridge walkable. In this 8-bit mask the drawing color is the number of the walkable area.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `WalkableArea.SetScaling`

```ags
void WalkableArea.SetScaling(int min, int max)
```

Changes this walkable area's scaling level.

Example:

```ags
Room.WalkableAreas[1].SetScaling(50, 100);
```

will make the walkable area 1 scale characters from 50% to 100% of their normal size.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `WalkableArea.GetProperty`

```ags
int WalkableArea.GetProperty(const string property)
```

Gets an integer custom property for this area.

Example:

```ags
WalkableArea* area = WalkableArea.GetAtRoomXY(player.x, player.y);
if (area != null && area.GetProperty("Slippery") == 1)
{
  player.Say("Careful, it's slippery here!");
}
```

will make the player comment if they stand on a walkable area that has its boolean custom property "Slippery" checked.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `WalkableArea.GetTextProperty`

```ags
String WalkableArea.GetTextProperty(const string property)
```

Gets a text custom property for this area.

Example:

```ags
WalkableArea* area = WalkableArea.GetAtRoomXY(player.x, player.y);
if (area != null)
{
  AudioClip* step = AudioClip.GetByName(area.GetTextProperty("StepSound"));
  if (step != null)
  {
    step.Play();
  }
}
```

will play the footstep sound whose script name (such as "aStepGravel") is stored in the custom text property "StepSound" of the walkable area under the player. You could call this from the function connected to the OnFrameEvent of the player character, on the frames of the walking animation where the feet touch the ground.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `WalkableArea.SetProperty`

```ags
bool WalkableArea.SetProperty(const string property, int value)
```

Sets an integer custom property for this area.

Example:

```ags
Room.WalkableAreas[1].SetProperty("Slippery", 0); // the ice has melted
```

will uncheck the boolean custom property "Slippery" of the walkable area 1.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `WalkableArea.SetTextProperty`

```ags
bool WalkableArea.SetTextProperty(const string property, const string value)
```

Sets a text custom property for this area.

Example:

```ags
Room.WalkableAreas[1].SetTextProperty("StepSound", "aStepMud"); // it has been raining
```

will change which footstep sound the code from the [`GetTextProperty`](WalkableArea#walkableareagettextproperty) example plays on the walkable area 1.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `WalkableArea.ID`

```ags
readonly int WalkableArea.ID
```

Gets the ID number for this area.

Example:

```ags
WalkableArea *area = WalkableArea.GetAtRoomXY(player.x, player.y);
if (area != null)
{
  Display("The player stands on walkable area %d.", area.ID);
}
```

will display the ID number of the walkable area the player character is standing on.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `WalkableArea.Enabled`

```ags
bool WalkableArea.Enabled
```

Gets/sets whether this walkable area is enabled.

Example:

```ags
Room.WalkableAreas[2].Enabled = false;
```

will disable the walkable area 2, so that characters can no longer walk on it.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `WalkableArea.ScalingMin`

```ags
readonly int WalkableArea.ScalingMin
```

Gets this walkable area's minimal scaling level.

Example:

```ags
WalkableArea *area = Room.WalkableAreas[1];
Display("The area scales characters from %d to %d percent.", area.ScalingMin, area.ScalingMax);
```

will display the minimal and maximal scaling levels of the walkable area 1.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `WalkableArea.ScalingMax`

```ags
readonly int WalkableArea.ScalingMax
```

Gets this walkable area's maximal scaling level.

Example:

```ags
WalkableArea *area = Room.WalkableAreas[1];
Display("The area scales characters from %d to %d percent.", area.ScalingMin, area.ScalingMax);
```

will display the minimal and maximal scaling levels of the walkable area 1.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `WalkableArea.FaceDirectionRatio`

```ags
static float WalkableArea.FaceDirectionRatio
```

Gets/sets the optional y/x ratio of character's facing directions, determining directional loop selection for each Character while on this area.

Example:

```ags
Room.WalkableAreas[1].FaceDirectionRatio = 1.5;
```

will set the y/x ratio that characters use to select their directional loops while standing on the walkable area 1 to 1.5.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---
