## `Object` functions and properties

### `Object.Animate`

*(Formerly known as `AnimateObject`, which is now obsolete)*<br>
*(Formerly known as `AnimateObjectEx`, which is now obsolete)*

```ags
Object.Animate(int loop, int delay, optional RepeatStyle,
               optional BlockingStyle, optional Direction, optional int frame, optional int volume)
```

Starts the object animating, using loop number LOOP of its current view, and optionally starting at FRAME.
The overall speed of the animation is set with DELAY, where 0 is the
fastest, and increasing numbers mean slower. The delay for each frame is
worked out as DELAY + FRAME SPD, so the individual frame speeds are
relative to this overall speed.

The *RepeatStyle* parameter sets whether the animation will continuously
repeat the cycling through the frames. This can be *eOnce* (or zero), in
which case the animation will start from the first frame of LOOP, and go
through each frame in turn until the last frame, where it will stop. If
RepeatStyle is *eRepeat* (or 1), then when the last frame is reached, it
will go back to the first frame and start over again with the animation.
If RepeatStyle is 2 then it will do the animation once, but then return
the graphic to the first frame and stop (whereas repeat=0 will leave the
graphic on the last frame).

For *blocking* you can pass either eBlock (in which case the function
will wait for the animation to finish before returning), or eNoBlock (in
which case the animation will start to play, but your script will
continue). The default is eBlock.

*Direction* specifies which way the animation plays. You can either pass
eForwards (the default) or eBackwards.

*Frame* lets you specify the starting frame, which should be one of the chosen loop's frame.<br>
Note that for compatibility reasons if direction is eBackwards the animation actually begins with the *previous frame*. If you pass frame 0 (the default) then it will begin with the last frame in the loop.

*Volume* lets you specify the relative volume in percents (0-100) of the frame-linked sounds for the duration of this animation. It's 100 by default (which means - unchanged).

You need to use SetView at some stage before this command, in order to
set up the object's current view.

Example:

```ags
object[0].Animate(2, 5);
object[1].Animate(1, 3, eOnce, eBlock, eBackwards);
```

will animate object 0 using loop 2 of its current view, at speed 5, and
play the animation once only. This happens in the background. Then,
object 1 will animate backwards using loop 1 of its current view, at
speed 3. The function won't return until the animation is finished.

*Compatibility:* Optional *frame* parameter is supported only by **AGS 3.5.0** and later versions.<br>
Optional *volume* parameter is supported only by **AGS 3.6.0** and later versions.

*See also:* [`Button.Animate`](Button#buttonanimate),
[`Character.Animate`](Character#characteranimate),
[`Object.Animating`](Object#objectanimating),
[`Object.SetView`](Object#objectsetview),
[`Object.StopAnimating`](Object#objectstopanimating)

---

### `Object.BlendMode`

```ags
BlendMode Object.BlendMode
```

Gets/sets the blending mode for this object.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [`BlendMode`](StandardEnums#blendmode)

---

### `Object.GetAtRoomXY`

```ags
static Object* Object.GetAtRoomXY(int x, int y)
```

Checks if there is a room object at ROOM co-ordinates (X,Y). Returns
the object if there is, or *null* if there is not.

**NOTE:** When looking up for an object under coordinates, GetAtRoomXY is affected by the game setting ["Pixel-perfect click detection"](GeneralSettings#visual). It's possible to change this behavior in script by changing OPT_PIXELPERFECT option (see [SetGameOption](Globalfunctions_General#setgameoption)).

Example:

```ags
if (Object.GetAtRoomXY(oBullet.x, oBullet.y) == oWall) {
    Display("A bullet hits the wall.");
}
```

will display the message if the object oBullet is over the object oWall.

*Compatibility:* Supported by **AGS 3.5.0** and later versions.

*See also:* [`Object.GetAtScreenXY`](Object#objectgetatscreenxy),
[`Character.GetAtRoomXY`](Character#charactergetatroomxy),
[`Hotspot.GetAtRoomXY`](Hotspot#hotspotgetatroomxy),
[`Region.GetAtRoomXY`](Region#regiongetatroomxy),
[`Game.GetLocationName`](Game#gamegetlocationname), [`GetLocationType`](Globalfunctions_General#getlocationtype)

---

### `Object.GetAtScreenXY`

*(Formerly known as global function `GetObjectAt`, which is now obsolete)*

```ags
static Object* Object.GetAtScreenXY(int x, int y)
```

Checks if there is a room object at SCREEN co-ordinates (X,Y). Returns
the object if there is, or *null* if there is not.

**NOTE:** When looking up for an object under coordinates, GetAtScreenXY is affected by the game setting ["Pixel-perfect click detection"](GeneralSettings#visual). It's possible to change this behavior in script by changing OPT_PIXELPERFECT option (see [SetGameOption](Globalfunctions_General#setgameoption)).

Example:

```ags
if (Object.GetAtScreenXY(mouse.x, mouse.y) == oRock) {
    Display("Clicked on the rock.");
}
```

will display the message if there is the object oRock under the mouse cursor.

*See also:* [`Object.GetAtRoomXY`](Object#objectgetatroomxy), [`Character.GetAtScreenXY`](Character#charactergetatscreenxy), [`Hotspot.GetAtScreenXY`](Hotspot#hotspotgetatscreenxy), [`Region.GetAtScreenXY`](Region#regiongetatscreenxy), [`Game.GetLocationName`](Game#gamegetlocationname), [`GetLocationType`](Globalfunctions_General#getlocationtype)

---

### `Object.GetByName`

```ags
static Object* Object.GetByName(string scriptName)
```

Returns a pointer to the Object with the specified script name, or null if it does not exist.

Normally you do not need to use this, as there will be a automatically created global script variable for each Object which got a script name.
Where GetByName() function may come useful is situation in which you a) do not know exact name, b) had to store object's reference in a string for some reason. Good examples of this are saving object's name in a [custom property](CustomProperties), or a [file](File), then reading it back.

Example:

```ags
void on_event(EventType event, int data) {
  if(event == eEventEnterRoomBeforeFadein) {

    String hauntedObjectName = player.GetTextProperty("haunted-object");

    Object* hauntedObject = Object.GetByName(hauntedObjectName);
    if (hauntedObject != null) {
      // Do something special if the room has an object the player haunts
      hauntedObject.Transparency = 50;
      hauntedObject.Clickable = false;
    }
  }
}
```

In this example, the player has a custom property that stores the script name of room objects it's haunted by,
and if a room has said object, that object becomes haunted - in the example it just becomes "ghostly": non-clickable and transparent.

*Compatibility:* Supported by **AGS 3.6.1** and later versions.

*See also:* [`Object.ScriptName`](Object#objectscriptname), [`Object.GetAtRoomXY`](Object#objectgetatroomxy), [`Object.GetAtScreenXY`](Object#objectgetatscreenxy)

---

### `Object.GetProperty`

*(Formerly known as `GetObjectProperty`, which is now obsolete)*

```ags
Object.GetProperty(string property)
```

Returns the custom property setting of the PROPERTY for the specified
object.

This command works with Number properties (it returns the number), and
with Boolean properties (returns 1 if the box was checked, 0 if not).

Use the equivalent GetTextProperty function to get a text property.

Example:

```ags
if (object[0].GetProperty("Value") > 200)
    Display("Object 0's value is over 200!");
```

will print the message if object 0 has its "Value" property set to more
than 200.

*See also:* [`Object.GetTextProperty`](Object#objectgettextproperty)

---

### `Object.GetTextProperty`

*(Formerly known as `GetObjectPropertyText`, which is now obsolete)*<br>
*(Formerly known as `Object.GetPropertyText`, which is now obsolete)*

```ags
String Object.GetTextProperty(string property)
```

Returns the custom property setting of the PROPERTY for the specified
object.

This command works with Text properties only. The property's text will
be returned from this function.

Use the equivalent GetProperty function to get a non-text property.

Example:

```ags
String description = object[0].GetTextProperty("Description");
Display("Object 0's description: %s", description);
```

will retrieve Object 0's "description" property then display it.

*See also:* [`Object.GetProperty`](Object#objectgetproperty)

---

### `Object.SetProperty`

```ags
bool Object.SetProperty(const string property, int value)
```

Sets the new *value* for the custom *property* for the specified room
object. Returns TRUE if such property exists and FALSE on failure.

This command works with Number properties (it sets the numeric value),
and with Boolean properties (sets FALSE is value is equal to 0, or TRUE
otherwise).

Use the equivalent SetTextProperty function to set new text property
value.

Example:

```ags
oTable.SetProperty("ItemCapacity", 5);
```

will change Table's "ItemCapacity" custom property to 5.

*Compatibility:* Supported by **AGS 3.4.0** and later versions.

*See also:* [`Object.SetTextProperty`](Object#objectsettextproperty)

---

### `Object.SetTextProperty`

```ags
void Object.SetTextProperty(const string property, const string value)
```

Sets the new *value* text for the custom *property* for the specified
room object.

This command works with Text properties only. The property's text will
be changed to new value.

Use the equivalent SetProperty function to set a non-text property.

Example:

```ags
oTable.SetTextProperty("Description", "A dull furniture");
```

will change table's "description" property.

*Compatibility:* Supported by **AGS 3.4.0** and later versions.

*See also:* [`Object.SetProperty`](Object#objectsetproperty)

---

### `Object.IsCollidingWithObject`

*(Formerly known as `AreObjectsColliding`, which is now obsolete)*

```ags
bool Object.IsCollidingWithObject(Object* obj2)
```

Checks if the specified object and OBJ2 are touching each other. Returns
*true* if they are, and *false* if they are not.

**NOTE:** This function only performs a rectangular check, even when
pixel-perfect click detection is turned on.

Example:

```ags
if (object[2].IsCollidingWithObject(object[3]))
{
    Display("object 2 and 3 are colliding!");
}
```

will display the message if the objects 2 and 3 are colliding.

*See also:* [`AreThingsOverlapping`](Globalfunctions_Room#arethingsoverlapping)

---

### `Object.IsInteractionAvailable`

```ags
Object.IsInteractionAvailable(CursorMode)
```

Checks whether there is an event handler defined for activating the room
object in cursor mode MODE.

This function is very similar to RunInteraction, except that rather than
run the event handler script function, it simply returns *true* if
something would have happened, or *false* if unhandled_event would have
been run.

Example:

```ags
if (oDoor.IsInteractionAvailable(eModeInteract) == 0)
    Display("interacting with this door would not do anything.");
```

*Compatibility:* Supported by **AGS 3.4.0** and later versions.

*See also:* [`IsInteractionAvailable`](Globalfunctions_General#isinteractionavailable),
[`Object.RunInteraction`](Object#objectruninteraction)

---

### `Object.Move`

*(Formerly known as `MoveObject`, which is now obsolete)*<br>
*(Formerly known as `MoveObjectDirect`, which is now obsolete)*

```ags
Object.Move(int x, int y, int speed, optional BlockingStyle,
            optional WalkWhere);
```

Starts the object moving from its current location to (X,Y). It will
move at speed SPEED, which uses the same scale as the character Walk
Speed values in the AGS Editor.

Speed value may be either positive or negative.
Positive values mean a speed of N pixels per game frame, while negative values mean a speed of 1/N pixels per game frame (that is - N game frames spent for a single pixel).

If *BlockingStyle* is eNoBlock (the default), then control returns to
the script immediately, and the object will move in the background.

If *BlockingStyle* is eBlock then this command will wait for the object
to finish moving before your script resumes.

If *WalkWhere* is eWalkableAreas (the default), then the object will
attempt to get as close a possible to (X,Y) by using the room's walkable
areas.

If *WalkWhere* is eAnywhere, then the object will simply walk directly
from its current location to (X,Y), ignoring the room walkable areas.

Example:

```ags
object[2].Move(125, 40, 4, eBlock);
```

will move object 2 to 125,40 and return control to the player when the
object gets there.

*See also:* [`Object.Moving`](Object#objectmoving),
[`Character.Walk`](Character#characterwalk),
[`Object.StopMoving`](Object#objectstopmoving)

---

### `Object.RemoveTint`

*(Formerly known as `RemoveObjectTint`, which is now obsolete)*

```ags
Object.RemoveTint()
```

Undoes the effects of calling Tint, and returns the object to using the
room's ambient tint.

Example:

```ags
object[1].Tint(0, 250, 0, 30, 100);
Wait(40);
object[1].RemoveTint();
```

will tint object 1 green for a second, then turn it back to normal.

*See also:* [`Object.Tint`](Object#objecttint)

---

### `Object.RunInteraction`

*(Formerly known as `RunObjectInteraction`, which is now obsolete)*

```ags
Object.RunInteraction(CursorMode)
```

Runs the event handler as if the player had clicked the mouse on the
object in the current room, using the specified cursor mode.

Example:

```ags
object[3].RunInteraction(eModeInteract);
```

will execute the code defined in object 3's "Interact with object" event
handler.

*See also:* [`Room.ProcessClick`](Room#roomprocessclick),
[`Object.IsInteractionAvailable`](Object#objectisinteractionavailable),
[`Character.RunInteraction`](Character#characterruninteraction),
[`Hotspot.RunInteraction`](Hotspot#hotspotruninteraction)

---

### `Object.SetLightLevel`

```ags
void Object.SetLightLevel(int light_level)
```

Sets individual light level for this room object.

The light level is from **-100 to 100**.

In 8-bit games you cannot use positive light level for brightening
effect, but you may still use negative values to produce darkening
effect.

To disable object lighting and tinting effects, call SetLightLevel with
parameter *light_level* 0.

**NOTE:** Setting a light level will disable any RGB tint set for the
object.

**NOTE:** Object's individual light level OVERRIDES both ambient light
level and local region light level.

Example:

```ags
oLamp.LightLevel = 100;
```

This will give the lamp maximal individual brightness.

*Compatibility:* Supported by **AGS 3.4.0** and later versions.

*See also:* [`Object.Tint`](Object#objecttint),
[`SetAmbientLightLevel`](Globalfunctions_General#setambientlightlevel),
[`Character.SetLightLevel`](Character#charactersetlightlevel),
[`Region.LightLevel`](Region#regionlightlevel)

---

### `Object.SetPosition`

*(Formerly known as `SetObjectPosition`, which is now obsolete)*

```ags
Object.SetPosition(int x, int y)
```

Changes the object's position to (X,Y). These co-ordinates specify the
lower-left hand corner of the object.

This command is equivalent to setting the object.X and object.Y
separately, but provides a more convenient way of doing so.

**NOTE:** This command cannot be used while the object is moving.

Example:

```ags
object[2].SetPosition(50, 100);
```

will change object's 2 position to 50,100.

*See also:* [`Object.X`](Object#objectx),
[`Object.Y`](Object#objecty)

---

### `Object.SetView`

*(Formerly known as `SetObjectFrame`, which is now obsolete)*<br>
*(Formerly known as `SetObjectView`, which is now obsolete)*

```ags
Object.SetView(int view, optional int loop, optional int frame)
```

Sets the object's view to VIEW, and changes the object's graphic to
FRAME of LOOP in VIEW. If you do not supply the loop or frame:
* since **AGS 3.6.0**: the loop and/or frame are reset to 0;
* in earlier versions they would be left at values they had before SetView was called.

You must use this command before calling Animate, so that AGS knows
which view to animate the object with.

**NOTE:** prior to **AGS 3.6.1** calling this function would play any sound linked to the given frame. This behavior is disabled in **3.6.1**, because it's causing a duplicate sound playback when SetView is followed by Animate.

Example:

```ags
object[3].SetView(14);
object[1].SetView(5, 2, 1);
```

will change object 3's view to view number 14 while resetting to loop 0 and frame 0 of that view, and change object 1 to
view 5, loop 2, frame 1.

*See also:* [`Object.Animate`](Object#objectanimate)
[`Object.Frame`](Object#objectframe)
[`Object.Loop`](Object#objectloop)
[`Object.View`](Object#objectview)

---

### `Object.StopAnimating`

```ags
Object.StopAnimating()
```

Stops the object from animating. It will remain on its current frame
until you change it or start a new animation.

Example:

```ags
if (object[2].Animating) {
    object[2].StopAnimating();
}
```

will stop object 2 animating if it currently is doing so.

*See also:* [`Object.Animate`](Object#objectanimate),
[`Object.Animating`](Object#objectanimating)

---

### `Object.StopMoving`

*(Formerly known as `StopObjectMoving`, which is now obsolete)*

```ags
Object.StopMoving()
```

Stops the object from moving. It will remain in its current position
until any further commands are issued.

Example:

```ags
if (object[2].Moving) {
    object[2].StopMoving();
}
```

will stop object 2 moving if it currently is doing so.

*See also:* [`Object.Moving`](Object#objectmoving),
[`Object.Move`](Object#objectmove),
[`Character.StopMoving`](Character#characterstopmoving)

---

### `Object.Tint`

*(Formerly known as `SetObjectTint`, which is now obsolete)*

```ags
Object.Tint(int red, int green, int blue,
            int saturation, int luminance)
```

Tints the object on the screen to (RED, GREEN, BLUE) with SATURATION
percent saturation.

This function applies a tint to a specific object. For the meaning of
all the parameters, see [`SetAmbientTint`](Globalfunctions_General#setambienttint).

The tint set by this function overrides any ambient tint set for the
room. For this reason, passing the SATURATION as 0 to this function does
not turn it off - rather, it ensures that no tint is applied to the
object (even if an ambient tint is set).

To remove the tint set by this function and return to using the ambient
tint for this object, call [`RemoveTint`](Object#objectremovetint).

**NOTE:** This function only works in true-color games and with true-color
sprites.

Example:

```ags
object[1].Tint(0, 250, 0, 30, 100);
```

will tint object 1 green.

*See also:* [`Object.RemoveTint`](Object#objectremovetint),
[`SetAmbientTint`](Globalfunctions_General#setambienttint)

---

### `Object.Animating`

*(Formerly known as `IsObjectAnimating`, which is now obsolete)*

```ags
readonly bool Object.Animating
```

Returns 1 if the specified object is currently animating.<br>
Returns 0 if the object has finished its animation.

This property is read-only. To change object animation, use the
[`Animate`](Object#objectanimate) command.

Example:

```ags
object[2].Animate(5, 0);
while (object[2].Animating) Wait(1);
```

will animate object 2 and wait until the animation finishes.

In reality, you would simply use the Blocking parameter of Animate so
you wouldn't need to do this.

*See also:* [`Object.Animate`](Object#objectanimate),
[`Object.Moving`](Object#objectmoving),
[`Object.StopAnimating`](Object#objectstopanimating),
[`Object.X`](Object#objectx), [`Object.Y`](Object#objecty)

---

### `Object.AnimationVolume`

```ags
int Object.AnimationVolume
```

Gets/sets the object's animation sound volume, which is a *relative* volume (0-100) of frame-linked sounds that play during object's animations.

**NOTE:** all the volume properties in Object act as relative factors: these are `AnimationVolume`, "volume" parameter of [`Animate`](Object#objectanimate) function.
The final frame sound's volume will be equal to `sound volume * AnimationVolume % * Animate's volume param %` when `Animate` is called.

*Compatibility:* Supported by **AGS 3.6.1** and later versions.

*SeeAlso:* [`Object.Animate`](Object#objectanimate),
[`Object.SetView`](Object#objectsetview),
[`Object.View`](Object#objectview)

---

### `Object.Baseline`

*(Formerly known as `GetObjectBaseline`, which is now obsolete)*<br>
*(Formerly known as `SetObjectBaseline`, which is now obsolete)*

```ags
int Object.Baseline
```

Gets/sets the object's baseline. This allows you to set a specific
base line for the object which works as its z-order value when sorting characters, room objects and walk-behinds on screen.

The baseline's value can be from 1 to the height of the room.
Setting it to 0 (or lower) will make object go back to using its current Y coordinate as the baseline (that's the default).

If you want to get the effective object's baseline, and Baseline's property value is 0, then the baseline is the object's Y co-ordinate.

**IMPORTANT:** If two or more Objects (or Characters, and so on) have equal Baseline, their draw order is **undefined**, and should not be relied upon. This is because of how sprite sorting is done in the engine.

Example 1:

```ags
oDoor.Baseline = 100;
```

will change oDoor object's baseline to a line positioned at y coordinate
100.

Example 2:

```ags
int zorder = oDoor.Baseline;
if (zorder == 0)
    zorder = oDoor.y;
```

will find out the object's effective z-order in the room.

*See also:* [`Character.Baseline`](Character#characterbaseline),
[`Object.Y`](Object#objecty),
[`SetWalkBehindBase`](Globalfunctions_Room#setwalkbehindbase)

---

### `Object.BlockingHeight`

```ags
int Object.BlockingHeight
```

Gets/sets the object's blocking height.

The blocking height determines how large of a blocking rectangle the
object exerts to stop characters walking through it. If this is set to 0
(the default), then the blocking rectangle is automatically calculated
to be the object's width, and 5 pixels high.

You can manually change the setting by entering a blocking height in
pixels, which is the size of walkable area that the object effectively
removes by being there.

**NOTE:** This property has no effect unless the
[`Solid`](Object#objectsolid) property is set to *true*.

Example:

```ags
oRock.BlockingHeight = 20;
```

will make the Rock object block 20 pixels high (10 above and 10 below
its baseline)

*See also:* [`Object.BlockingWidth`](Object#objectblockingwidth),
[`Object.Solid`](Object#objectsolid)

---

### `Object.BlockingWidth`

```ags
int Character.BlockingWidth
```

Gets/sets the object's blocking width.

The blocking width determines how large of a blocking rectangle the
object exerts to stop characters walking through it. If this is set to 0
(the default), then the blocking rectangle is automatically calculated
to be the object's width, and 5 pixels high.

You can manually change the setting by entering a blocking width in
pixels, which is the size of walkable area that the object effectively
removes by being there.

**NOTE:** This property has no effect unless the
[`Solid`](Object#objectsolid) property is set to *true*.

Example:

```ags
oRock.BlockingWidth = 50;
```

will make the Rock object block 50 pixels wide (25 pixels to the left of
its center, and 25 to the right)

*See also:* [`Object.BlockingHeight`](Object#objectblockingheight),
[`Object.Solid`](Object#objectsolid)

---

### `Object.Clickable`

*(Formerly known as `SetObjectClickable`, which is now obsolete)*

```ags
bool Object.Clickable
```

Gets/sets whether the object is recognized as something which the player
can interact with.

If this is set to 1, then the player can look at, speak to, and so on
the object. If it is set to 0, then the object will not respond to
clicks and the mouse will activate whatever is behind the object. This
is useful if you are using the object for visual effects and don't want
it to be clicked on by the player.

Example:

```ags
object[2].Clickable = 0;
```

will make object 2 ignore clicks from the player.

*See also:*
[`Object.Visible`](Object#objectvisible),
[`Character.Clickable`](Character#characterclickable),
[`Hotspot.Enabled`](Hotspot#hotspotenabled),
[`Region.Enabled`](Region#regionenabled),
[`RemoveWalkableArea`](Globalfunctions_Room#removewalkablearea),
[`RestoreWalkableArea`](Globalfunctions_Room#restorewalkablearea)

---

### `Object.Enabled`

```ags
bool Object.Enabled
```

Gets/sets whether the object will be drawn and updated during the game update.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Object.Frame`

```ags
readonly int Object.Frame
```

Gets the frame number that the object is currently set to. If the object
is not currently assigned a view, this will be 0 (in which case the
Graphic property will hold its sprite number).

This property is read-only. To change the frame, use the animation
functions.

Example:

```ags
Display("Object oDoor's frame is currently %d.", oDoor.Frame);
```

will display the oDoor object's current frame number

*SeeAlso:* [`Object.Graphic`](Object#objectgraphic),
[`Object.Loop`](Object#objectloop),
[`Object.View`](Object#objectview)

---

### `Object.Graphic`

*(Formerly known as `GetObjectGraphic`, which is now obsolete)*<br>
*(Formerly known as `SetObjectGraphic`, which is now obsolete)*

```ags
int Object.Graphic
```

Gets/sets the sprite slot number that the object is currently displayed
as. You can get the slot number from the Sprite Manager. If the object
is currently animating (from an Animate command) and you change the
Graphic, then the animation will be stopped.

Example:

```ags
object[2].Graphic = 100;
```

will change the object 2's image to the image stored in the sprite
manager's slot 100.

*See also:* [`Object.SetView`](Object#objectsetview)

---

### `Object.GraphicRotation`

```ags
float Object.GraphicRotation
```

Gets/sets the object's sprite rotation in degrees.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Object.HasExplicitLight`

```ags
readonly bool Object.HasExplicitTint
```

Returns *true* if the object has a light set explicitly with the
[`Object.SetLightLevel`](Object#objectsetlightlevel) command.

Returns *false* if the object has no explicit light level, but it may
still be lighted by
[`SetAmbientLightLevel`](Globalfunctions_General#setambientlightlevel) or a region
light.

*Compatibility:* Supported by **AGS 3.4.1** and later versions.

*SeeAlso:* [`Object.SetLightLevel`](Object#objectsetlightlevel)

---

### `Object.HasExplicitTint`

```ags
readonly bool Object.HasExplicitTint
```

Returns *true* if the object has a tint set explicitly with the
[`Object.Tint`](Object#objecttint) command.

Returns *false* if the object has no explicit tint, but it may still be
tinted by [`SetAmbientTint`](Globalfunctions_General#setambienttint) or a region tint.

*Compatibility:* Supported by **AGS 3.4.1** and later versions.

*SeeAlso:* [`Object.Tint`](Object#objecttint),
[`Object.RemoveTint`](Object#objectremovetint)

---

### `Object.ID`

```ags
readonly int Object.ID
```

Gets the object's ID number. This is the object's number from the
editor, and is useful if you need to interoperate with legacy code that
uses the object's number rather than name.

Example:

```ags
MoveObject(oRock.ID, 100, 50, 5);
```

uses the obsolete MoveObject function to move the Rock object to (100,
50) at speed 5.

---

### `Object.ManualScaling`

*(Formerly known as `Object.IgnoreScaling`, which is now obsolete)*

```ags
bool Object.ManualScaling
```

Gets/sets whether the object uses manually specified scaling instead of 
using the walkable area scaling. This is equivalent, **though opposite**, 
to the "UseRoomAreaScaling" property in the Objects pane of the editor.

If it is set to false, then the object will be stretched or shrunk as
appropriate on walkable areas.

If this is set to true, the object scaling will instead be set by modifying
it's Scaling property.

Example:

```ags
oDoor.ManualScaling = true;
```

will tell the Door object not to be scaled on walkable areas.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`Object.Scaling`](Object#objectscaling)

---

### `Object.IgnoreWalkbehinds`

**This property is obsolete since AGS 3.5.0 and not recommended for use at all.**

*(Formerly known as `SetObjectIgnoreWalkbehinds`, which is now obsolete)*

```ags
bool Object.IgnoreWalkbehinds
```

Sets whether the object is affected by walkbehind areas. Setting this to
*false* (the default setting) means that the object will be placed
behind walk-behind areas according to the relevant baselines.

If this is set to *true*, then the object will never be placed behind a
walk-behind area. This is useful if for example you want an object to be
a picture on a wall, and the wall can be walked behind - but you also
want it to act correctly in relation to characters, so changing its
baseline wouldn't work.

**IMPORTANT:** This property is a "dirty hack" and not recommended for use at all. It breaks the logic of drawing order for room elements, and only works as intended if your game is run using Software graphics driver. We strongly suggest to design your rooms without it and rely on [`Baseline`](Object#objectbaseline) property instead.

---

### `Object.LightLevel`

```ags
readonly int Object.LightLevel
```

If the object has an individual light set explicitly with the
[`Object.SetLightLevel`](Object#objectsetlightlevel) command, this
property returns the light level value. Otherwise it returns 0.

**NOTE:** without individual light level set, Object.LightLevel returns
0 even if the object is affected by the ambient or region's light.

*Compatibility:* Supported by **AGS 3.4.1** and later versions.

*SeeAlso:* [`Object.SetLightLevel`](Object#objectsetlightlevel),
[`SetAmbientLightLevel`](Globalfunctions_General#setambientlightlevel),

---

### `Object.Loop`

```ags
readonly int Object.Loop
```

Gets the loop that the object is currently set to. If the object is not
currently assigned a view, this will be 0 (in which case the Graphic
property will hold its sprite number).

This property is read-only. To change the loop, use the animation
functions.

Example:

```ags
Display("Object oDoor's loop is currently %d.", oDoor.Loop);
```

will display the oDoor object's current loop number

*SeeAlso:* [`Object.Frame`](Object#objectframe),
[`Object.Graphic`](Object#objectgraphic),
[`Object.View`](Object#objectview)

---

### `Object.MotionPath`

```ags
readonly MotionPath\* Object.MotionPath
```

Gets this object's current MotionPath, or null if it's not moving.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Object.MovePath`

```ags
Object.MovePath(Point\*[], int speed, optional BlockingStyle, optional RepeatStyle, optional Direction)
```

Moves the object along the path, ignoring walkable areas.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Object.Moving`

*(Formerly known as `IsObjectMoving`, which is now obsolete)*

```ags
readonly bool Object.Moving
```

Returns 1 if the object is currently moving, or 0 if not.

This property is read-only; to change the object's movement, use the
[`Move`](Object#objectmove) and
[`StopMoving`](Object#objectstopmoving) commands.

Example:

```ags
object[2].Move(125,40,3);
while (object[2].Moving) Wait(1);
```

will move object 2 to 125,40 and return control to the player when the
object gets there.

*See also:* [`Object.Animating`](Object#objectanimating),
[`Object.StopMoving`](Object#objectstopmoving)

---

### `Object.Name`

*(Formerly known as `GetObjectName`, which is now obsolete)*<br>
*(Formerly known as `Object.GetName`, which is now obsolete)*

```ags
String Object.Name;
```

Gets/sets the name of the object.

**NOTE:** This property may be changed only in AGS versions 3.6.0 or higher. It is read-only in earlier versions.

Example:

```ags
Display("Object 0's name is %s.", object[0].Name);
```

will retrieve and then display object 0's name.

*See also:* [`Game.GetLocationName`](Game#gamegetlocationname)

---

### `Object.ScriptName`

```ags
readonly String Object.ScriptName;
```

Gets the script name of the Object, which serves as a unique identifier, as set in the AGS Editor.

This may be useful if you have a pointer to some object stored in your variable, and want to know what it actually is. Normally you don't need a script name, as you have an automatic global variable for each object in the game, but sometimes you may want to save its script name as a text either to display it somewhere for testing purposes, or keep as a reference. You may later use [Object.GetByName](Object#objectgetbyname) function to retrieve the object by the previously saved script name.

Example:

```ags
void Touch(this Object*)
{
  System.Log(eLogInfo, "You touched %s.", this.ScriptName);
  // Do things when object is touched
}
```

In this example all objects gain a new method `Touch`, that can be called using `oName.Touch()`, that will log the script name of the touched object.

*Compatibility:* Supported by **AGS 3.6.1** and later versions.

*See also:* [`Object.GetByName`](Object#objectgetbyname),  [`Object.Name`](Object#objectname)

---

### `Object.Scaling`

```ags
int Object.Scaling
```

Gets/sets the object's current scaling level, in percents. The permitted value range is 1 to 32767. An object that has the 
regular size has this property set to 100.

You can only set the value of this property if [`Object.ManualScaling`](Object#objectmanualscaling) is enabled
for the object; otherwise, the scaling is determined automatically based 
on the walkable area that the object is on.

This property returns the actual current scaling at any given moment, whether it was set by you or acquired from the walkable area.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`Object.ManualScaling`](Object#objectmanualscaling)

---

### `Object.Shader`

```ags
ShaderInstance\* Object.Shader
```

Gets/sets the shader of this object.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Object.Solid`

```ags
bool Object.Solid
```

Gets/sets whether the object can be walked through by characters.

If this is set to true, then the object is solid and will block the path
of solid characters. If this is set to false, then the object can be
walked through by characters.

**NOTE:** solid objects only block characters, they don't block other
objects from moving through them.

Example:

```ags
oSmallrock.Solid = true;
```

will mean that the Smallrock object blocks the path of characters.

*See also:* [`Object.BlockingHeight`](Object#objectblockingheight),
[`Object.BlockingWidth`](Object#objectblockingwidth)

---

### `Object.TintBlue`

```ags
readonly int Object.TintBlue
```

Gets the *Blue* setting for the object's current tint.

This property is read-only; to change it, use the
[`Object.Tint`](Object#objecttint) command.

**NOTE:** If the
[`Object.HasExplicitTint`](Object#objecthasexplicittint) property is
false, then this value is meaningless.

*Compatibility:* Supported by **AGS 3.4.1** and later versions.

*See also:* [`Object.Tint`](Object#objecttint),
[`Object.HasExplicitTint`](Object#objecthasexplicittint),
[`Object.TintGreen`](Object#objecttintgreen),
[`Object.TintRed`](Object#objecttintred),
[`Object.TintLuminance`](Object#objecttintluminance)

---

### `Object.TintGreen`

```ags
readonly int Object.TintGreen
```

Gets the *Green* setting for the object's current tint.

This property is read-only; to change it, use the
[`Object.Tint`](Object#objecttint) command.

**NOTE:** If the
[`Object.HasExplicitTint`](Object#objecthasexplicittint) property is
false, then this value is meaningless.

*Compatibility:* Supported by **AGS 3.4.1** and later versions.

*See also:* [`Object.Tint`](Object#objecttint),
[`Object.TintBlue`](Object#objecttintblue),
[`Object.TintRed`](Object#objecttintred),
[`Object.TintSaturation`](Object#objecttintsaturation),
[`Object.TintLuminance`](Object#objecttintluminance)

---

### `Object.TintRed`

```ags
readonly int Object.TintRed
```

Gets the *Red* setting for the object's current tint.

This property is read-only; to change it, use the
[`Object.Tint`](Object#objecttint) command.

**NOTE:** If the
[`Object.HasExplicitTint`](Object#objecthasexplicittint) property is
false, then this value is meaningless.

*Compatibility:* Supported by **AGS 3.4.1** and later versions.

*See also:* [`Object.Tint`](Object#objecttint),
[`Object.TintBlue`](Object#objecttintblue),
[`Object.TintGreen`](Object#objecttintgreen),
[`Object.TintSaturation`](Object#objecttintsaturation),
[`Object.TintLuminance`](Object#objecttintluminance)

---

### `Object.TintSaturation`

```ags
readonly int Object.TintSaturation
```

Gets the *saturation* setting for the object's current tint.

This property is read-only; to change it, use the
[`Object.Tint`](Object#objecttint) command.

**NOTE:** If the
[`Object.HasExplicitTint`](Object#objecthasexplicittint) property is
false, then this value is meaningless.

*Compatibility:* Supported by **AGS 3.4.1** and later versions.

*See also:* [`Object.Tint`](Object#objecttint),
[`Object.TintBlue`](Object#objecttintblue),
[`Object.TintGreen`](Object#objecttintgreen),
[`Object.TintRed`](Object#objecttintred),
[`Object.TintLuminance`](Object#objecttintluminance)

---

### `Object.TintLuminance`

```ags
readonly int Object.TintLuminance
```

Gets the *luminance* setting for the object's current tint.

This property is read-only; to change it, use the
[`Object.Tint`](Object#objecttint) command.

**NOTE:** If the
[`Object.HasExplicitTint`](Object#objecthasexplicittint) property is
false, then this value is meaningless.

*Compatibility:* Supported by **AGS 3.4.1** and later versions.

*See also:* [`Object.Tint`](Object#objecttint),
[`Object.TintBlue`](Object#objecttintblue),
[`Object.TintGreen`](Object#objecttintgreen),
[`Object.TintRed`](Object#objecttintred),
[`Object.TintSaturation`](Object#objecttintsaturation)

---

### `Object.Transparency`

*(Formerly known as `SetObjectTransparency`, which is now obsolete)*

```ags
int Object.Transparency
```

Gets/sets the object's transparency level.

If this is set to 100, it means that the object is totally invisible,
and lower values represent varying levels of transparency. Set this to 0
to stop the object being transparent.

**NOTE:** Transparency only works in 32-bit color games.

**NOTE:** When using the Software driver, a large transparent object
can significantly slow down AGS.

Some rounding is done internally when the transparency is stored --
therefore, if you get the transparency after setting it, the value you
get back might be one out. Therefore, using a loop with
`object[0].Transparency++;` is not recommended as it will probably end
too quickly.

In order to fade an object in/out, the best approach is shown in the
example below:

Example:

```ags
int trans = object[0].Transparency;
while (trans < 100) {
    trans++;
    object[0].Transparency = trans;
    Wait(1);
}
```

will gradually fade out the object from its current transparency level
to being fully invisible.

*See also:* [`Character.Transparency`](Character#charactertransparency),
[`GUI.Transparency`](GUI#guitransparency)

---

### `Object.UseRegionTint`

```ags
bool Object.UseRegionTint
```

Gets/sets whether the object uses region tinting.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Object.View`

```ags
readonly int Object.View
```

Gets the view that the object is currently set to. This is either the
view number, or 0 if the object is not currently assigned a view (in
which case the Graphic property will hold its sprite number instead).

This property is read-only. To change the view, use the SetView
function. To remove the view, set the Graphic property to a sprite slot.

Example:

```ags
Display("Object oDoor's view is currently view %d.", oDoor.View);
```

will display the oDoor object's current view number

*SeeAlso:* [`Object.SetView`](Object#objectsetview),
[`Object.Graphic`](Object#objectgraphic),
[`Object.Loop`](Object#objectloop),
[`Object.Frame`](Object#objectframe)

---

### `Object.Visible`

*(Formerly known as `IsObjectOn`, which is now obsolete)*<br>
*(Formerly known as `ObjectOff`, which is now obsolete)*<br>
*(Formerly known as `ObjectOn`, which is now obsolete)*

```ags
bool Object.Visible
```

Gets/sets the visible state of the object. If this is 1 (true), the
object is switched on and visible in the room. If you set this to 0
(false), the object disappears and no longer appears in the room.

Example:

```ags
object[5].Visible = false;
```

will make object number 5 in the current room disappear.

*See also:*
[`Object.Clickable`](Object#objectclickable),

---

### `Object.X`

*(Formerly known as `GetObjectX`, which is now obsolete)*

```ags
int Object.X
```

Gets/sets the X co-ordinate of the object.

**NOTE:** This property cannot be changed while the object is moving.

Example:

```ags
Display("Object 1's X co-ordinate is %d.", object[1].X);
```

will display the X co-ordinate of object 1.

*See also:* [`Object.Y`](Object#objecty),
[`Object.Animating`](Object#objectanimating),
[`Object.Visible`](Object#objectvisible),
[`Object.SetPosition`](Object#objectsetposition)

---

### `Object.Y`

*(Formerly known as `GetObjectY`, which is now obsolete)*

```ags
int Object.Y
```

Gets/sets the Y co-ordinate of the object, which is the bottom of the
object's image.

**NOTE:** This property cannot be changed while the object is moving.

**NOTE:** If you try to use this co-ordinate with Object.GetAtScreenXY,
you will find that the object does not get picked up. The object's
sprite is drawn from the Y co-ordinate at (Object.Y - Height) to
(Object.Y - 1).

Example:

```ags
Display("Object 1's Y co-ordinate is %d.", object[1].Y);
```

will display the Y co-ordinate of object 1.

*See also:* [`Object.Animating`](Object#objectanimating),
[`Object.Baseline`](Object#objectbaseline),
[`Object.X`](Object#objectx),
[`Object.SetPosition`](Object#objectsetposition)
