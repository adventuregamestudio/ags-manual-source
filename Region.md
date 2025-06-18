## `Region` functions and properties

### `Region.GetAtRoomXY`

*(Formerly known as global function `GetRegionAt`, which is now obsolete)*

```ags
static Region* Region.GetAtRoomXY(int x, int y)
```

Returns the region at ROOM co-ordinates (X,Y). If there is no region
there, or if invalid co-ordinates are specified, the Region\*
representing region 0 will be returned.

Example:

```ags
if (Region.GetAtRoomXY(player.x, player.y) == region[0]) {
    Display("The player is not currently standing on a region.");
}
```

*See also:* [`Region.GetAtScreenXY`](Region#regiongetatscreenxy), [`Character.GetAtRoomXY`](Character#charactergetatroomxy),
[`Hotspot.GetAtRoomXY`](Hotspot#hotspotgetatroomxy),
[`Object.GetAtRoomXY`](Object#objectgetatroomxy), [`GetWalkableAreaAtRoom`](Globalfunctions_Room#getwalkableareaatroom)

---

### `Region.GetAtScreenXY`

```ags
static Region* Region.GetAtScreenXY(int x, int y)
```

Returns the region at SCREEN co-ordinates (X,Y). If there is no region
there, or if invalid co-ordinates are specified, the Region\*
representing region 0 will be returned.

Example:

```ags
Region* r = Region.GetAtScreenXY(mouse.x, mouse.y);
if (r != region[0]) {
    Display("The mouse is over region %d", r.ID);
}
```

will display the message if there is any region under the mouse cursor.

*Compatibility:* Supported by **AGS 3.5.0** and later versions.

*See also:* [`Region.GetAtRoomXY`](Region#regiongetatroomxy), [`Character.GetAtScreenXY`](Character#charactergetatscreenxy), [`Hotspot.GetAtScreenXY`](Hotspot#hotspotgetatscreenxy), [`Object.GetAtScreenXY`](Object#objectgetatscreenxy), [`Game.GetLocationName`](Game#gamegetlocationname), [`GetLocationType`](Globalfunctions_General#getlocationtype)

---

### `Region.GetDrawingSurface`

```ags
static DrawingSurface* Region.GetDrawingSurface()
```

Gets a drawing surface for the current room's 8-bit regions mask, which allows you to directly draw onto that mask. Note that this function is static and relates to all regions at once (not a particular region), because all of them are painted on the same mask.

After calling this method, use the various [DrawingSurface functions](DrawingSurface) to modify the mask. Since this is a 8-bit mask, DrawingColor will match the region's index. Color value 0 will refer to "no region" (eraser), color value 1 to region number 1, and so forth. Don't forget to call `Release()` on the surface when you are finished to update the room's state.

Any changes you make will only last until the player leaves the room, at which point they will be lost. If you need to make long-lasting changes, you can use this method in the Player Enters Room event, which will recreate these changes whenever player returns back.

Example:

```ags
DrawingSurface *surface = Region.GetDrawingSurface();
surface.DrawingColor = 4;
surface.DrawRectangle(50, 50, 100, 100);
surface.Release();
```

will paint a rectangle for the region 4.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`GetDrawingSurfaceForWalkbehind`](Globalfunctions_Room#getdrawingsurfaceforwalkbehind),
[`GetDrawingSurfaceForWalkableArea`](Globalfunctions_Room#getdrawingsurfaceforwalkablearea),
[`Hotspot.GetDrawingSurface`](Hotspot#hotspotgetdrawingsurface)

---

### `Region.GetProperty`

```ags
int  Region.GetProperty(const string property)
```

Gets an integer custom property for this region.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Region.GetTextProperty`

```ags
String Region.GetTextProperty(const string property)
```

Gets a text custom property for this region.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Region.SetProperty`

```ags
bool Region.SetProperty(const string property, int value)
```

Sets an integer custom property for this region.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Region.SetTextProperty`

```ags
bool Region.SetTextProperty(const string property, const string value)
```

Sets a text custom property for this region.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Region.RunInteraction`

*(Formerly known as `RunRegionInteraction`, which is now obsolete)*

```ags
Region.RunInteraction(int event)
```

Runs the event handler as if the EVENT for the region had been
activated.

**NOTE:** Unlike the other RunInteraction commands, this one does not
take a cursor mode. Instead, it uses an event type as follows:

0 While player stands on region<br>
1 Player walks onto region<br>
2 Player walks off region

Example:

```ags
region[4].RunInteraction(1);
```

will run the actions defined in the event handler script for "Player
walks onto region" for region 4.

*See also:*
[`Character.RunInteraction`](Character#characterruninteraction),
[`Hotspot.RunInteraction`](Hotspot#hotspotruninteraction)

---

### `Region.Tint`

*(Formerly known as `SetRegionTint`, which is now obsolete)*

```ags
Region.Tint(int red, int green, int blue, int amount, optional int luminance)
```

Changes the region to have RGB tint (RED, GREEN, BLUE) with AMOUNT
percent saturation.

The red, green and blue values are between 0 and 255, and you supply the
same values that you would use in the editor.

For the meaning of all the parameters, see
[`SetAmbientTint`](Globalfunctions_General#setambienttint).

**NOTE:** The tint will be reset when the player leaves the room, so you
need to use it in Player Enters Room if you want a permanent change.

**NOTE:** This function only works in true-color games.

**NOTE:** To remove the region tint, set the LightLevel property to 0.

Example:

```ags
region[2].Tint(180, 20, 20, 50);
```

will set region 2's RGB tint to (180, 20, 20) with 50`%` opacity.

*Compatibility:* Optional *luminance* parameter is supported only by
**AGS 3.4.0** and later versions.

*See also:* [`Region.LightLevel`](Region#regionlightlevel),
[`SetAmbientTint`](Globalfunctions_General#setambienttint)

---

### `Region.Enabled`

*(Formerly known as `DisableRegion`, which is now obsolete)*<br>
*(Formerly known as `EnableRegion`, which is now obsolete)*

```ags
bool Region.Enabled
```

Enables/disables the specified region. If you set this to false, then
all areas of the screen that were previously part of the region now act
as type 0 (no region). You can turn the region back on later by setting
this to true.

While a region is disabled, it will not be returned by
Region.GetAtRoomXY, and if the character walks onto the region then its
events will not get run.

Example:

```ags
region[3].Enabled = false;
```

will disable region number 3.

*See also:* [`Hotspot.Enabled`](Hotspot#hotspotenabled),
[`RemoveWalkableArea`](Globalfunctions_Room#removewalkablearea),
[`RestoreWalkableArea`](Globalfunctions_Room#restorewalkablearea)

---

### `Region.ID`

```ags
readonly int Region.ID
```

Gets the region number of this region. This allows you to interoperate
with old script using the number-based region functions.

Example:

```ags
Display("Region 3 is number %d.", region[3].ID);
```

displays region 3's number (which will be 3).

*See also:* [`Region.GetAtRoomXY`](Region#regiongetatroomxy)

---

### `Region.LightLevel`

*(Formerly known as `SetAreaLightLevel`, which is now obsolete)*

```ags
int Region.LightLevel
```

Gets/sets the region's light level. This does the same thing as the
Light Level textbox in the editor, but allows you to change it at
run-time.

The light level is from **-100 to 100**. This is different from the
editor, which takes values from 0 to 200. Subtract 100 from the value
you would use in the editor when calling this function. The reason for
this discrepancy is legacy reasons from the DOS editor days.

In 8-bit games you cannot use positive light level for brightening
effect, but you may still use negative values to produce darkening
effect.

To disable region lighting and tinting effects, set LightLevel to 0.

**NOTE:** The light level will be reset to the editor settings when the
player leaves the room, so you need to use it in Player Enters Room if
you want a permanent change.

**NOTE:** Setting a light level will disable any RGB tint set for the
region.

**NOTE:** Region's light level does NOT override individual character
and object light levels.

Example:

```ags
region[2].LightLevel = 100;
```

will set region 2's level light to 100.

*See also:* [`Region.Tint`](Region#regiontint),
[`SetAmbientLightLevel`](Globalfunctions_General#setambientlightlevel),
[`Character.SetLightLevel`](Character#charactersetlightlevel),
[`Object.SetLightLevel`](Object#objectsetlightlevel)

---

### `Region.TintEnabled`

```ags
readonly bool Region.TintEnabled
```

Gets whether the region currently has an RGB tint enabled for it.

Returns *true* if it does, and *false* if it does not. If it does not,
then the LightLevel property reflects the region lighting.

If this property is *false*, then the TintRed, TintGreen, TintBlue,
TintSaturation and TintLuminance properties are invalid.

Example:

```ags
if (region[4].TintEnabled) {
    Display("Region 4 is tinted!!");
}
```

will display a message if region 4 is tinted

*See also:* [`Region.Tint`](Region#regiontint)

---

### `Region.TintBlue`

```ags
readonly int Region.TintBlue
```

Gets the *Blue* setting for the region's current tint.

This property is read-only; to change it, use the
[`Region.Tint`](Region#regiontint) command.

**NOTE:** If the [`Region.TintEnabled`](Region#regiontintenabled)
property is false, then this value is meaningless.

Example:

```ags
if (region[4].TintEnabled) {
    Display("Region 4 is tinted RGB (%d,%d,%d) Saturation %d.",
            region[4].TintRed, region[4].TintGreen,
            region[4].TintBlue, region[4].TintSaturation);
}
```

will display a message with the region's tints.

*See also:* [`Region.Tint`](Region#regiontint),
[`Region.TintEnabled`](Region#regiontintenabled),
[`Region.TintGreen`](Region#regiontintgreen),
[`Region.TintRed`](Region#regiontintred),
[`Region.TintLuminance`](Region#regiontintluminance)

---

### `Region.TintGreen`

```ags
readonly int Region.TintGreen
```

Gets the *Green* setting for the region's current tint.

This property is read-only; to change it, use the
[`Region.Tint`](Region#regiontint) command.

**NOTE:** If the [`Region.TintEnabled`](Region#regiontintenabled)
property is false, then this value is meaningless.

Example:

```ags
if (region[4].TintEnabled) {
    Display("Region 4 is tinted RGB (%d,%d,%d) Saturation %d.",
            region[4].TintRed, region[4].TintGreen,
            region[4].TintBlue, region[4].TintSaturation);
}
```

will display a message with the region's tints.

*See also:* [`Region.Tint`](Region#regiontint),
[`Region.TintEnabled`](Region#regiontintenabled),
[`Region.TintBlue`](Region#regiontintblue),
[`Region.TintRed`](Region#regiontintred),
[`Region.TintSaturation`](Region#regiontintsaturation),
[`Region.TintLuminance`](Region#regiontintluminance)

---

### `Region.TintRed`

```ags
readonly int Region.TintRed
```

Gets the *Red* setting for the region's current tint.

This property is read-only; to change it, use the
[`Region.Tint`](Region#regiontint) command.

**NOTE:** If the [`Region.TintEnabled`](Region#regiontintenabled)
property is false, then this value is meaningless.

Example:

```ags
if (region[4].TintEnabled) {
    Display("Region 4 is tinted RGB (%d,%d,%d) Saturation %d.",
            region[4].TintRed, region[4].TintGreen,
            region[4].TintBlue, region[4].TintSaturation);
}
```

will display a message with the region's tints.

*See also:* [`Region.Tint`](Region#regiontint),
[`Region.TintEnabled`](Region#regiontintenabled),
[`Region.TintBlue`](Region#regiontintblue),
[`Region.TintGreen`](Region#regiontintgreen),
[`Region.TintSaturation`](Region#regiontintsaturation),
[`Region.TintLuminance`](Region#regiontintluminance)

---

### `Region.TintSaturation`

```ags
readonly int Region.TintSaturation
```

Gets the *saturation* setting for the region's current tint.

This property is read-only; to change it, use the
[`Region.Tint`](Region#regiontint) command.

**NOTE:** If the [`Region.TintEnabled`](Region#regiontintenabled)
property is false, then this value is meaningless.

Example:

```ags
if (region[4].TintEnabled) {
    Display("Region 4 is tinted RGB (%d,%d,%d) Saturation %d.",
            region[4].TintRed, region[4].TintGreen,
            region[4].TintBlue, region[4].TintSaturation);
}
```

will display a message with the region's tints.

*See also:* [`Region.Tint`](Region#regiontint),
[`Region.TintEnabled`](Region#regiontintenabled),
[`Region.TintBlue`](Region#regiontintblue),
[`Region.TintGreen`](Region#regiontintgreen),
[`Region.TintRed`](Region#regiontintred),
[`Region.TintLuminance`](Region#regiontintluminance)

---

### `Region.TintLuminance`

```ags
readonly int Region.TintLuminance
```

Gets the *luminance* setting for the region's current tint.

This property is read-only; to change it, use the
[`Region.Tint`](Region#regiontint) command.

**NOTE:** If the [`Region.TintEnabled`](Region#regiontintenabled)
property is false, then this value is meaningless.

*See also:* [`Region.Tint`](Region#regiontint),
[`Region.TintEnabled`](Region#regiontintenabled),
[`Region.TintBlue`](Region#regiontintblue),
[`Region.TintGreen`](Region#regiontintgreen),
[`Region.TintRed`](Region#regiontintred),
[`Region.TintSaturation`](Region#regiontintsaturation)
