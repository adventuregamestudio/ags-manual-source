## `Hotspot` functions and properties

### `Hotspot.GetAtRoomXY`

```ags
static Hotspot* Hotspot.GetAtRoomXY(int x, int y)
```

Returns the hotspot at ROOM co-ordinates (X,Y). If there is no hotspot
there, or if invalid co-ordinates are specified, the Hotspot\*
representing hotspot 0 will be returned.

Example:

```ags
if (Hotspot.GetAtRoomXY(player.x, player.y) == hPressurePlate) {
    Display("As soon as adventurer stands on the pressure plate the door opens.");
}
```

will display the message if the player character is over the hPressurePlate hotspot.

*Compatibility:* Supported by **AGS 3.5.0** and later versions.

*See also:* [`Hotspot.GetAtScreenXY`](Hotspot#hotspotgetatscreenxy),
[`Character.GetAtRoomXY`](Character#charactergetatroomxy),
[`Object.GetAtRoomXY`](Object#objectgetatroomxy),
[`Region.GetAtRoomXY`](Region#regiongetatroomxy),
[`Game.GetLocationName`](Game#gamegetlocationname), [`GetLocationType`](Globalfunctions_General#getlocationtype)

---

### `Hotspot.GetAtScreenXY`

*(Formerly known as global function `GetHotspotAt`, which is now
obsolete)*

```ags
static Hotspot* Hotspot.GetAtScreenXY(int x, int y)
```

Returns the hotspot at SCREEN co-ordinates (X,Y). If there is no hotspot
there, or if invalid co-ordinates are specified, the Hotspot\*
representing hotspot 0 will be returned.

Example:

```ags
if (Hotspot.GetAtScreenXY(mouse.x, mouse.y) == hDoor)
    Display("Mouse on the door");
else if (Hotspot.GetAtScreenXY(mouse.x, mouse.y) != hotspot[0])
    Display("Mouse is on something (but not the door)!");
else
    Display("Mouse not on a hotspot");
```

will display a message depending on what the mouse is on.

*See also:* [`Hotspot.GetAtRoomXY`](Hotspot#hotspotgetatroomxy), [`Character.GetAtScreenXY`](Character#charactergetatscreenxy), [`Object.GetAtScreenXY`](Object#objectgetatscreenxy), [`Region.GetAtScreenXY`](Region#regiongetatscreenxy), [`Game.GetLocationName`](Game#gamegetlocationname), [`GetLocationType`](Globalfunctions_General#getlocationtype)

---

### `Hotspot.GetByName`

```ags
static Hotspot* Hotspot.GetByName(string scriptName)
```

Returns a pointer to the Hotspot with the specified script name, or null if it does not exist.

Normally you do not need to use this, as there will be a automatically created global script variable for each Hotspot which got a script name.
Where GetByName() function may come useful is situation in which you a) do not know exact name, b) had to store object's reference in a string for some reason. Good examples of this are saving object's name in a [custom property](CustomProperties), or a [file](File), then reading it back.

Example:

```ags
void OpenDoor(Object *obj) {
    obj.SetView(VDOORANIMATION);
    obj.Animate(1, 4, eOnce, eBlock);
    String exitName = obj.GetTextProperty("LinkedExit");
    Hotspot* exitHotspot = Hotspot.GetByName(exitName);
    if (exitHotspot != null) {
        exitHotspot.Enabled = true;
    }
}
```

Animates a "door" object, retrieves "exit" hotspot related to that door, and enables it.

*Compatibility:* Supported by **AGS 3.6.1** and later versions.

*See also:* [`Hotspot.ScriptName`](Hotspot#hotspotscriptname)

---

### `Hotspot.GetDrawingSurface`

```ags
static DrawingSurface* Hotspot.GetDrawingSurface()
```

Gets a drawing surface for the current room's 8-bit hotspot mask, which allows you to directly draw onto that mask. Note that this function is static and relates to all hotspots at once (not a particular hotspot), because all of them are painted on the same mask.

After calling this method, use the various [DrawingSurface functions](DrawingSurface) to modify the mask. Since this is a 8-bit mask, DrawingColor will match the hotspot's index. Color value 0 will refer to "no hotspot" (eraser), color value 1 to hotspot number 1, and so forth. Don't forget to call `Release()` on the surface when you are finished to update the room's state.

Any changes you make will only last until the player leaves the room, at which point they will be lost. If you need to make long-lasting changes, you can use this method in the Player Enters Room event, which will recreate these changes whenever player returns back.

Example:

```ags
DrawingSurface *surface = Hotspot.GetDrawingSurface();
surface.DrawingColor = 4;
surface.DrawRectangle(50, 50, 100, 100);
surface.Release();
```

will paint a rectangle for the hotspot 4.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`GetDrawingSurfaceForWalkbehind`](Globalfunctions_Room#getdrawingsurfaceforwalkbehind),
[`GetDrawingSurfaceForWalkableArea`](Globalfunctions_Room#getdrawingsurfaceforwalkablearea),
[`Region.GetDrawingSurface`](Region#regiongetdrawingsurface)

---

### `Hotspot.GetProperty`

*(Formerly known as `GetHotspotProperty`, which is now obsolete)*

```ags
Hotspot.GetProperty(string property)
```

Returns the custom property setting of the PROPERTY for the hotspot.

This command works with Number properties (it returns the number), and
with Boolean properties (returns 1 if the box was checked, 0 if not).

Use the equivalent GetTextProperty function to get a text property.

Example:

```ags
if (hDoorway.GetProperty("Value") > 200)
    Display("Hotspot %s's value is over 200!", hDoorway.ScriptName);
```

will print the message if hotspot hDoorway has its "Value" property set to more
than 200.

*See also:* [`Hotspot.GetTextProperty`](Hotspot#hotspotgettextproperty)

---

### `Hotspot.GetTextProperty`

*(Formerly known as `GetHotspotPropertyText`, which is now obsolete)*<br>
*(Formerly known as `Hotspot.GetPropertyText`, which is now obsolete)*

```ags
String Hotspot.GetTextProperty(string property)
```

Returns the custom property setting of the PROPERTY for the hotspot.

This command works with Text properties only. The property's text will
be returned from this function.

Use the equivalent GetProperty function to get a non-text property.

Example:

```ags
String description = hDoorway.GetTextProperty("Description");
Display("Hotspot %s's description: %s", hDoorway.ScriptName, description);
```

will retrieve hotspot hDoorway's "description" property and display it.

*See also:* [`Hotspot.GetProperty`](Hotspot#hotspotgetproperty)

---

### `Hotspot.SetProperty`

```ags
bool Hotspot.SetProperty(const string property, int value)
```

Sets the new *value* for the custom *property* for the specified
hotspot. Returns TRUE if such property exists and FALSE on failure.

This command works with Number properties (it sets the numeric value),
and with Boolean properties (sets FALSE is value is equal to 0, or TRUE
otherwise).

Use the equivalent SetTextProperty function to set new text property
value.

Example:

```ags
hDoor.SetProperty("LockDifficulty", 5);
```

will change Door hotspot's "LockDifficulty" custom property to 5.

*Compatibility:* Supported by **AGS 3.4.0** and later versions.

*See also:* [`Hotspot.SetTextProperty`](Hotspot#hotspotsettextproperty)

---

### `Hotspot.SetTextProperty`

```ags
bool Hotspot.SetTextProperty(const string property, const string value)
```

Sets the new *value* text for the custom *property* for the specified
hotspot. Returns TRUE if such property exists and FALSE on failure.

This command works with Text properties only. The property's text will
be changed to new value.

Use the equivalent SetProperty function to set a non-text property.

Example:

```ags
hDoor.SetTextProperty("Description", "The sturdy door");
```

will change Door's "description" property.

*Compatibility:* Supported by **AGS 3.4.0** and later versions.

*See also:* [`Hotspot.SetProperty`](Hotspot#hotspotsetproperty)

---

### `Hotspot.IsInteractionAvailable`

```ags
Hotspot.IsInteractionAvailable(CursorMode)
```

Checks whether there is an event handler defined for activating the
hotspot in cursor mode MODE.

This function is very similar to RunInteraction, except that rather than
run the event handler script function, it simply returns *true* if
something would have happened, or *false* if unhandled_event would have
been run.

Example:

```ags
if (hTable.IsInteractionAvailable(eModeLookat) == 0)
    Display("looking on this table would not do anything.");
```

*Compatibility:* Supported by **AGS 3.4.0** and later versions.

*See also:* [`IsInteractionAvailable`](Globalfunctions_General#isinteractionavailable),
[`Hotspot.RunInteraction`](Hotspot#hotspotruninteraction)

---

### `Hotspot.RunInteraction`

*(Formerly known as `RunHotspotInteraction`, which is now obsolete)*

```ags
Hotspot.RunInteraction(CursorMode)
```

Processes the event handler as if the player had clicked the mouse on
the hotspot using the specified cursor mode.

Example:

```ags
hDoor.RunInteraction(eModeLookat);
```

will run the code defined in the "LOOK AT HOTSPOT" event for hotspot
hDoor.

*See also:* [`Room.ProcessClick`](Room#roomprocessclick),
[`Hotspot.IsInteractionAvailable`](Hotspot#hotspotisinteractionavailable),
[`Character.RunInteraction`](Character#characterruninteraction),
[`Object.RunInteraction`](Object#objectruninteraction)

---

### `Hotspot.Enabled`

*(Formerly known as `DisableHotspot`, which is now obsolete)*<br>
*(Formerly known as `EnableHotspot`, which is now obsolete)*

```ags
bool Hotspot.Enabled
```

Enables/disables the specified hotspot. If you set this to false, then
all areas of the screen that were previously made up of the hotspot now
act as type 0 (no hotspot). You can turn the hotspot back on later by
setting this back to true.

This setting is persisted in-game; that is, it will not be reset when
the player re-enters the room.

The default value of this property is always *true*.

Example:

```ags
hBrownTree.Enabled = false;
```

will disable the hBrownTree hotspot.

*See also:* [`Region.Enabled`](Region#regionenabled),
[`RemoveWalkableArea`](Globalfunctions_Room#removewalkablearea),
[`RestoreWalkableArea`](Globalfunctions_Room#restorewalkablearea)

---

### `Hotspot.ID`

```ags
readonly int Hotspot.ID
```

Gets the hotspot number of this hotspot. This allows you to interoperate
with old script using the number-based hotspot functions.

Example:

```ags
Display("Hotspot hDoor is hotspot number %d.", hDoor.ID);
Display("Hotspot 3 is number %d.", hotspot[3].ID);
```

displays hDoor's hotspot number, and then displays hotspot 3's number
(which will be 3).

*See also:* [`Hotspot.GetAtScreenXY`](Hotspot#hotspotgetatscreenxy)

---

### `Hotspot.Name`

*(Formerly known as `GetHotspotName`, which is now obsolete)*<br>
*(Formerly known as `Hotspot.GetName`, which is now obsolete)*

```ags
String Hotspot.Name;
```

Gets/sets the name of the hotspot. This is a human-readable name (description), not a script name.

**NOTE:** This property may be changed only in AGS versions 3.6.0 or higher. It is read-only in earlier versions.

Example:

```ags
Display("Hotspot %s's name is %s.", hDoorway.ScriptName, hDoorway.Name);
```

will retrieve and then display hotspot 3's name.

*See also:* [`Game.GetLocationName`](Game#gamegetlocationname)

---

### `Hotspot.ScriptName`

```ags
readonly String Hotspot.ScriptName
```

Gets the script name of the hotspot, which serves as a unique identifier, as set in the AGS Editor.

This may be useful if you have a pointer to some hotspot stored in your variable, and want to know what it actually is. Normally you don't need a script name, as you have an automatic global variable for each hotspot in the game, but sometimes you may want to save its script name as a text either to display it somewhere for testing purposes, or keep as a reference. You may later use [Hotspot.GetByName](Hotspot#hotspotgetbyname) function to retrieve the hotspot by the previously saved script name.

Example:

```
void InteractHotspot(Hotspot *h) {
    System.Log(eLogInfo, "Interacted with hotspot %s", h.ScriptName);
    h.RunInteraction(eModeInteract);
}
```

*Compatibility:* Supported by **AGS 3.6.1** and later versions.

*See also:* [`Hotspot.GetByName`](Hotspot#hotspotgetbyname)

---

### `Hotspot.WalkToX`

*(Formerly known as `GetHotspotPointX`, which is now obsolete)*

```ags
readonly int Hotspot.WalkToX
```

Gets the X room co-ordinate of the hotspot's walk-to point. If the
hotspot does not have a walk-to point, returns -1.

Example:

```ags
player.Walk(hTable.WalkToX, hTable.WalkToY, eBlock, eWalkableAreas);
```

will move the character to hotspot hTable's walk-to point.

*See also:* [`Hotspot.WalkToY`](Hotspot#hotspotwalktoy),
[`MoveCharacterToHotspot`](Globalfunctions_General#movecharactertohotspot)

---

### `Hotspot.WalkToY`

*(Formerly known as `GetHotspotPointY`, which is now obsolete)*

```ags
readonly int Hotspot.WalkToY
```

Gets the Y room co-ordinate of the hotspot's walk-to point. If the
hotspot does not have a walk-to point, returns -1.

Example:

```ags
player.Walk(hTable.WalkToX, hTable.WalkToY, eBlock, eWalkableAreas);
```

will move the character to hotspot hTable's walk-to point.

*See also:* [`Hotspot.WalkToX`](Hotspot#hotspotwalktox),
[`MoveCharacterToHotspot`](Globalfunctions_General#movecharactertohotspot)
