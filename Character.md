## `Character` functions and properties

### `Character.AddInventory`

*(Formerly known as global function `AddInventory`, which is now
obsolete)*<br>
*(Formerly known as global function `AddInventoryToCharacter`, which is
now obsolete)*

```ags
Character.AddInventory(InventoryItem *item, optional int addAtIndex)
```

Adds one quantity of the specified item to the character's inventory. This ensures that
the item gets added to the character's inventory list, and that any
on-screen inventory display gets updated if appropriate.

The first parameter is the inventory item's Script Name from the
editor (for example, *iPoster*).

By default, the new item is added to the end of the character's
inventory list. However, you can insert it in a particular position in
the list by supplying the second parameter. The new item is inserted
*before* the current item at *addAtIndex*. Indexes are numbered from 0,
so to add the item at the start of the list, pass 0 as the second
parameter.

Calling the function multiple times will increase the InventoryQuantity for the item.

Example:

```ags
cEgo.AddInventory(iKey);
```

will give inventory item iKey to character EGO.

*See also:* [`Character.HasInventory`](Character#characterhasinventory),
[`Character.LoseInventory`](Character#characterloseinventory),
[`UpdateInventory`](Globalfunctions_General#updateinventory),
[`Character.InventoryQuantity`](Character#characterinventoryquantity)

---

### `Character.AddWaypoint`

*(Formerly known as `MoveCharacterPath`, which is now obsolete)*

```ags
Character.AddWaypoint(int x, int y)
```

Tells the character to move to (X,Y) directly, after it has finished its
current move. This function allows you to queue up a series of moves for
the character to make, if you want them to take a preset path around the
screen. Note that any moves made with this command ignore walkable
areas.

This is useful for situations when you might want a townsperson to
wander onto the screen from one side, take a preset route around it and
leave again.

Example:

```ags
cSomeguy.Walk(160, 100);
cSomeguy.AddWaypoint(50, 150);
cSomeguy.AddWaypoint(50, 50);
```

tells character SOMEGUY to first of all walk to the center of the screen
normally (obeying walkable areas), then move to the bottom left corner
and then top left corner afterwards.

*See also:* [`Character.Move`](Character#charactermove)
[`Character.Walk`](Character#characterwalk)

---

### `Character.Animate`

*(Formerly known as `AnimateCharacter`, which is now obsolete)*<br>
*(Formerly known as `AnimateCharacterEx`, which is now obsolete)*

```ags
Character.Animate(int loop, int delay, optional RepeatStyle,
                  optional BlockingStyle, optional Direction, optional int frame, optional int volume)
```

Starts the character animating, using loop number LOOP of his current
view and optionally starting at FRAME. The overall speed of the animation is set with DELAY, where 0 is
the fastest, and increasing numbers mean slower. The delay for each
frame is worked out as DELAY + FRAME SPD, so the individual frame speeds
are relative to this overall speed.

Before using this command, you should use
[`LockView`](Character#characterlockview) in order to select the view you
want to animate with and prevent any automatic animations (e.g. walking
or idle animations) from playing.

The *RepeatStyle* parameter sets whether the animation will continuously
repeat the cycling through the frames. This can be *eOnce* (or zero), in
which case the animation will start from the first frame of LOOP, and go
through each frame in turn until the last frame, where it will stop. If
RepeatStyle is *eRepeat* (or 1), then when the last frame is reached, it
will go back to the first frame and start over again with the animation.

For *blocking* you can pass either eBlock (in which case the function
will wait for the animation to finish before returning), or eNoBlock (in
which case the animation will start to play, but your script will
continue). The default is eBlock.

*Direction* specifies which way the animation plays. You can either pass
eForwards (the default) or eBackwards.

*Frame* lets you specify the starting frame, which should be one of the chosen loop's frame.<br>
Note that for compatibility reasons if direction is eBackwards the animation actually begins with the *previous frame*. If you pass frame 0 (the default) then it will begin with the last frame in the loop.

*Volume* lets you specify the *relative* volume in percents (0-100) of the frame-linked sounds for the duration of this animation. It's 100 by default (which means - unchanged).
**NOTE:** the *volume* parameter from `Character.Animate` does not replace `Character.AnimationVolume` property, but acts as a relative factor to it and character's scale if [`ScaleVolume`](Character#characterscalevolume) is also set. In other words the final frame sound's volume will be equal to `sound volume * AnimationVolume % * scaled volume % * volume param %`.

Example:

```ags
cEgo.LockView(5);
cEgo.Animate(3, 1, 0, eBlock, eBackwards);
cEgo.UnlockView();
```

will animate the character once using loop number 3 of view 5 backwards,
and wait until the animation finishes before returning.

*Compatibility:* Optional *frame* parameter is supported only by **AGS 3.5.0** and later versions.<br>
Optional *volume* parameter is supported only by **AGS 3.6.0** and later versions.

*See also:* [`Button.Animate`](Button#buttonanimate),
[`Object.Animate`](Object#objectanimate)

---

### `Character.ChangeRoom`

*(Formerly known as `NewRoom`, which is now obsolete)*<br>
*(Formerly known as `NewRoomEx`, which is now obsolete)*<br>
*(Formerly known as `NewRoomNPC`, which is now obsolete)*

```ags
Character.ChangeRoom(int room_number, optional int x, optional int y, optional CharacterDirection direction)
```

Changes the room that the character is in.

If you call this on the player character, then the game will move into
the new room with them.

**IMPORTANT:** For *player* character this command does not change the room immediately;
instead, it will schedule the room change, and perform it only once your script
function has finished (This is to avoid problems with unloading the
script while it is still running). This means that you should not use
any other commands which rely on the new room (object positionings, and
so on) after this command within the same function.

If you call this on a *non-player* character, then they are instantly
transported to the new room number.

Optionally, you can include an X and Y co-ordinate (you must include
either both or neither). If you do so, then the character will also be
moved to the specified co-ordinates in the new room. If you do not supply these parameters, then the character's latest X and Y position will be carried over.

Optionally, you can also include direction parameter, that determines
which direction this character will be facing after room change.

Example:

```ags
player.ChangeRoom(4, 100, 50, eDirectionRight);
```

will move the player character to room 4 and also place him at
coordinates 100,50. This will also mean that the game moves into room 4.

*Compatibility:* Optional *direction* parameter is supported only by
**AGS 3.4.0** and later versions.

*See also:*
[`Character.ChangeRoomAutoPosition`](Character#characterchangeroomautoposition)

---

### `Character.ChangeRoomAutoPosition`

```ags
Character.ChangeRoomAutoPosition(int room_number, optional int newPosition)
```

Changes the room that the character is in, and positions him along one
of the room edges.

This command simulates the behavior of the old "Go to room" interaction
command from AGS 2.72 and previous versions. If *newPosition* is not
specified or is 0, the character will be placed on the opposite side of
the new room, if he is within 10 pixels of a room edge in the current
room.

Alternatively, you can specify the position where he will get placed in
the new room. *newPosition* can be 1000 for the left edge, 2000 for the
right edge, 3000 for the bottom edge and 4000 for the top edge. Then,
add on the offset within that edge where you want to place the
character, in normal room co-ordinates.

**IMPORTANT:** This command does not change the room immediately;
instead, it will perform the actual room change once your script
function has finished (This is to avoid problems with unloading the
script while it is still running). This means that you should not use
any other commands which rely on the new room (object positionings, and
so on) after this command within the same function.

**NOTE:** This command can only be used with the player character.

Example:

```ags
player.ChangeRoomAutoPosition(4, 2100);
```

will move the player character to room 4 and place him half way down the
right hand side of the screen. This will also mean that the game moves
into room 4.

*See also:* [`Character.ChangeRoom`](Character#characterchangeroom)

---

### `Character.ChangeView`

*(Formerly known as `ChangeCharacterView`, which is now obsolete)*

```ags
Character.ChangeView(int view)
```

Changes the normal view number of the character to *view*. This is
useful if, for example, you want the character to change the clothes
they are wearing, and so permanently alter their view number.

**NOTE:** This command is **not** intended to change the view
temporarily to perform an animation. If you want to do that, use the
LockView command instead. This ChangeView command permanently changes
the character's normal walking view.

Example:

```ags
cEgo.ChangeView(5);
```

will make the EGO character use view number 5 as his walking view.

*See also:* [`Character.LockView`](Character#characterlockview),
[`Character.NormalView`](Character#characternormalview)

---

### `Character.FaceCharacter`

*(Formerly known as global function `FaceCharacter`, which is now
obsolete)*

```ags
Character.FaceCharacter(Character* toFace, optional BlockingStyle)
```

Turns the graphic of the character so that it looks like he is facing
character TOFACE. This involves changing the current loop to the
appropriate loop number, and setting the frame number to 0 (standing).

If the character has Turning enabled (i.e. the "Characters turn to face
direction" game option is turned on, and the character does not have the
"Do not turn before walking" option checked), then the character will
turn on the spot in order to face the new direction. In this case, the
BlockingStyle parameter determines whether the script waits for the
character to finish turning (eBlock, the default) or whether the script
continues immediately and the character finishes turning later on
(eNoBlock).

If the character does not have Turning enabled, he will immediately turn
to face the new direction and the BlockingStyle parameter has no effect.
In this case, the screen will not be refreshed straight away -- if you
want to see the character facing his new direction immediately, call
Wait(1);

Example:

```ags
cEgo.FaceCharacter(cMan);
```

will make the character EGO face the character MAN

*See also:*
[`Character.FaceDirection`](Character#characterfacedirection),
[`Character.FaceLocation`](Character#characterfacelocation),
[`Character.FaceObject`](Character#characterfaceobject),
[`Character.Walk`](Character#characterwalk)

---

### `Character.FaceDirection`

```ags
Character.FaceDirection(CharacterDirection direction, BlockingStyle=eBlock)
```

Turns the graphic of the character so that it looks like he is facing
direction *direction*. This involves changing the current loop to the
appropriate loop number, and setting the frame number to 0 (standing).

If the character has Turning enabled (i.e. the "Characters turn to face
direction" game option is turned on, and the character does not have the
"Do not turn before walking" option checked), then the character will
turn on the spot in order to face the new direction. In this case, the
BlockingStyle parameter determines whether the script waits for the
character to finish turning (eBlock, the default) or whether the script
continues immediately and the character finishes turning later on
(eNoBlock).

If the character does not have Turning enabled, he will immediately turn
to face the new direction and the BlockingStyle parameter has no effect.
In this case, the screen will not be refreshed straight away -- if you
want to see the character facing his new direction immediately, call
Wait(1);

Example:

```ags
cEgo.FaceDirection(eDirectionUpRight);
```

will make the character EGO face up-right.

*Compatibility:* Supported by **AGS 3.4.0** and later versions.

*See also:*
[`Character.FaceCharacter`](Character#characterfacecharacter),
[`Character.FaceLocation`](Character#characterfacelocation),
[`Character.FaceObject`](Character#characterfaceobject),
[`Character.Walk`](Character#characterwalk)

---

### `Character.FaceLocation`

*(Formerly known as global function `FaceLocation`, which is now
obsolete)*

```ags
Character.FaceLocation(int x, int y, optional BlockingStyle)
```

Similar to the FaceCharacter function, except that this faces the
character to room co-ordinates (X,Y). This allows him to face not only
other characters, but also hotspots or anything else as well (you can
get co-ordinates by watching the co-ordinates displayed in the Room
Settings mode as you move the mouse over the room background).

If the character has Turning enabled (i.e. the "Characters turn to face
direction" game option is turned on, and the character does not have the
"Do not turn before walking" option checked), then the character will
turn on the spot in order to face the new direction. In this case, the
BlockingStyle parameter determines whether the script waits for the
character to finish turning (eBlock, the default) or whether the script
continues immediately and the character finishes turning later on
(eNoBlock).

If the character does not have Turning enabled, he will immediately turn
to face the new direction and the BlockingStyle parameter has no effect.
In this case, the screen will not be refreshed straight away -- if you
want to see the character facing his new direction immediately, call
Wait(1);

Example:

```ags
cEgo.FaceLocation(cEgo.x + 50, cEgo.y);
```

will make the character face to the east.

*See also:*
[`Character.FaceCharacter`](Character#characterfacecharacter),
[`Character.FaceDirection`](Character#characterfacedirection),
[`Character.FaceObject`](Character#characterfaceobject),
[`Character.Walk`](Character#characterwalk)

---

### `Character.FaceObject`

```ags
Character.FaceObject(Object* object, optional BlockingStyle)
```

Similar to the FaceCharacter function, except that this faces the
character to object OBJECT in the current room.

If the character has Turning enabled (i.e. the "Characters turn to face
direction" game option is turned on, and the character does not have the
"Do not turn before walking" option checked), then the character will
turn on the spot in order to face the new direction. In this case, the
BlockingStyle parameter determines whether the script waits for the
character to finish turning (eBlock, the default) or whether the script
continues immediately and the character finishes turning later on
(eNoBlock).

If the character does not have Turning enabled, he will immediately turn
to face the new direction and the BlockingStyle parameter has no effect.
In this case, the screen will not be refreshed straight away -- if you
want to see the character facing his new direction immediately, call
Wait(1);

Example:

```ags
player.FaceObject(oTable);
```

will make the player character face object oTable.

*See also:*
[`Character.FaceCharacter`](Character#characterfacecharacter),
[`Character.FaceDirection`](Character#characterfacedirection),
[`Character.FaceLocation`](Character#characterfacelocation),
[`Character.Walk`](Character#characterwalk)

---

### `Character.FollowCharacter`

*(Formerly known as global function `FollowCharacter`, which is now
obsolete)*<br>
*(Formerly known as global function `FollowCharacterEx`, which is now
obsolete)*

```ags
Character.FollowCharacter(Character* chartofollow, optional int dist,
                          optional int eagerness)
```

Tells the character to follow CHARTOFOLLOW around, wherever he goes. You
could use this command to have a group of main characters who go around
together, or for example when the hero has rescued someone from the bad
guy, they can follow the hero home.

Pass CHARTOFOLLOW as *null* to stop the character following.

There are a couple of extra optional parameters:

DIST sets how far away from CHARTOFOLLOW that CHARID will stand. If DIST
is 1, they will try to stand very close; if DIST is for example 20, they
will stand about 20 pixels away.

EAGERNESS sets on average how long the character will stand around
before checking if he needs to move again. Setting this to 0 means that
he will always be on the move until he reaches CHARTOFOLLOW; setting
this to 99 means that he will pause and think for a while on route.
Values in between specify different lengths of idle time.

The default values are DIST=10 and EAGERNESS=97.

As a special case, setting DIST=0 and EAGERNESS=0 makes CHARID behave as
if it is chasing CHARTOFOLLOW - it will try and get there as quickly as
possible. Setting EAGERNESS=0 also tells the character not to stop when
they reach CHARTOFOLLOW, but instead to randomly wander around the
character - useful perhaps for a very energetic dog or something.

There is also another special use for this command. You can pass the
special value FOLLOW_EXACTLY as the DIST parameter rather than passing
a number. If you do this, then CHARID will always remain at exactly the
same X and Y co-ordinates as CHARTOFOLLOW. This might be useful for
effects such as a temporary halo over the character and so forth.

If you use FOLLOW_EXACTLY, then EAGERNESS has another meaning. If you
pass 0, CHARID will be drawn in front of CHARTOFOLLOW; if you pass 1, it
will be drawn behind.

When the character being followed changes room, the character following
will change to the same room and position after a number of frames defined
on `game.following_room_timer`.

Example:

```ags
cMan.FollowCharacter(cEgo, 5, 80);
```

will make character MAN follow character EGO standing about 5 pixels
near him and waiting for a while before he makes his move.

*See also:*
[`game.following_room_timer`](Gamevariables),
[`Character.Following`](Character#characterfollowing)

---

### `Character.GetAtRoomXY`

```ags
static Character* Character.GetAtRoomXY(int x, int y)
```

Checks if there is a character at ROOM co-ordinates (X,Y). Returns the character if there is, or null if there is not.

**NOTE:** Any characters with the "Clickable" property set to false will not be seen by this function.

**NOTE:** When looking up for an object under coordinates, GetAtRoomXY is affected by the game setting ["Pixel-perfect click detection"](GeneralSettings#visual). It's possible to change this behavior in script by changing OPT_PIXELPERFECT option (see [SetGameOption](Globalfunctions_General#setgameoption)).

Example:

```ags
Character* target = Character.GetAtRoomXY(oBullet.x, oBullet.y);
if (target != null) {
    Display("Character %s is hit by a bullet", target.Name);
}
```

will display the message if the room object oBullet is over any character.

*Compatibility:* Supported by **AGS 3.5.0** and later versions.

*See also:* [`Character.GetAtScreenXY`](Character#charactergetatscreenxy),
[`Hotspot.GetAtRoomXY`](Hotspot#hotspotgetatroomxy),
[`Object.GetAtRoomXY`](Object#objectgetatroomxy),
[`Region.GetAtRoomXY`](Region#regiongetatroomxy),
[`Game.GetLocationName`](Game#gamegetlocationname)

---

### `Character.GetAtScreenXY`

*(Formerly known as global function `GetCharacterAt`, which is now
obsolete)*

```ags
static Character* Character.GetAtScreenXY(int x, int y)
```

Checks if there is a character at SCREEN co-ordinates (X,Y). Returns the
character if there is, or null if there is not. See the description of
GetLocationName for more on screen co-ordinates.

**NOTE:** Any characters with the "Clickable" property set to false will not
be seen by this function.

**NOTE:** When looking up for an object under coordinates, GetAtScreenXY is affected by the game setting ["Pixel-perfect click detection"](GeneralSettings#visual). It's possible to change this behavior in script by changing OPT_PIXELPERFECT option (see [SetGameOption](Globalfunctions_General#setgameoption)).

Example:

```ags
if (Character.GetAtScreenXY(mouse.x, mouse.y) == cEgo) {
    Display("The mouse is over the main character");
}
```

will display the message if the mouse cursor is over the EGO character

*See also:* [`Character.GetAtRoomXY`](Character#charactergetatroomxy),
[`Hotspot.GetAtScreenXY`](Hotspot#hotspotgetatscreenxy),
[`Object.GetAtScreenXY`](Object#objectgetatscreenxy),
[`Region.GetAtScreenXY`](Region#regiongetatscreenxy),
[`Game.GetLocationName`](Game#gamegetlocationname)

### `Character.GetByName`

```ags
static Character* Character.GetByName(string scriptName)
```

Returns a pointer to the Character with the specified script name, or null if it does not exist.

Normally you do not need to use this, as there will be a automatically created global script variable for each Character which got a script name.
Where GetByName() function may come useful is situation in which you a) do not know exact name, b) had to store object's reference in a string for some reason. Good examples of this are saving object's name in a [custom property](CustomProperties), or a [file](File), then reading it back.

Example:

```ags
String companionName = player.GetTextProperty("Companion");
Character *companion = Character.GetByName(companionName);
```

Retrieves the player's companion name from its custom property, and gets a pointer to an actual Character using that name.

*Compatibility:* Supported by **AGS 3.6.1** and later versions.

*See also:* [`Character.ScriptName`](Character#characterscriptname)

---

### `Character.GetProperty`

*(Formerly known as `GetCharacterProperty`, which is now obsolete)*

```ags
Character.GetProperty(string property)
```

Returns the custom property setting of the PROPERTY for the specified
character.

This command works with Number properties (it returns the number), and
with Boolean properties (returns 1 if the box was checked, 0 if not).

Use the equivalent GetTextProperty function to get a text property.

Example:

```ags
if (cEgo.GetProperty("Value") > 200)
    Display("EGO's value is over 200!");
```

will print the message if EGO has its "Value" property set to more than
200.

*See also:*
[`Character.GetTextProperty`](Character#charactergettextproperty)

---

### `Character.GetTextProperty`

*(Formerly known as `GetCharacterPropertyText`, which is now obsolete)*<br>
*(Formerly known as `Character.GetPropertyText`, which is now obsolete)*

```ags
String Character.GetTextProperty(string property)
```

Returns the custom property setting of the PROPERTY for the specified
character.

This command works with Text properties only. The property's text will
be returned from this function.

Use the equivalent GetProperty function to get a non-text property.

Example:

```ags
String description = cEgo.GetTextProperty("Description");
Display("EGO's description: %s", description);
```

will retrieve EGO's "description" property and display it.

*See also:* [`Character.GetProperty`](Character#charactergetproperty)

---

### `Character.SetProperty`

```ags
bool Character.SetProperty(const string property, int value)
```

Sets the new *value* for the custom *property* for the specified
character. Returns TRUE if such property exists and FALSE on failure.

This command works with Number properties (it sets the numeric value),
and with Boolean properties (sets FALSE is value is equal to 0, or TRUE
otherwise).

Use the equivalent SetTextProperty function to set new text property
value.

Example:

```ags
cEgo.SetProperty("XPLevel", 10);
```

will change EGO character's "XPLevel" custom property to 10.

*Compatibility:* Supported by **AGS 3.4.0** and later versions.

*See also:*
[`Character.SetTextProperty`](Character#charactersettextproperty)

---

### `Character.SetTextProperty`

```ags
bool Character.SetTextProperty(const string property, const string value)
```

Sets the new *value* text for the custom *property* for the specified
character. Returns TRUE if such property exists and FALSE on failure.

This command works with Text properties only. The property's text will
be changed to new value.

Use the equivalent SetProperty function to set a non-text property.

Example:

```ags
cEgo.SetTextProperty("Description", "I am handsome!");
```

will change EGO's "description" property.

*Compatibility:* Supported by **AGS 3.4.0** and later versions.

*See also:* [`Character.SetProperty`](Character#charactersetproperty)

---

### `Character.HasExplicitLight`

```ags
readonly bool Character.HasExplicitTint
```

Returns *true* if the character has a light set explicitly with the
[`Character.SetLightLevel`](Character#charactersetlightlevel) command.

Returns *false* if the character has no explicit light level, but it may
still be lighted by
[`SetAmbientLightLevel`](Globalfunctions_General#setambientlightlevel) or a region
light.

*Compatibility:* Supported by **AGS 3.4.1** and later versions.

*See also:* [`Character.SetLightLevel`](Character#charactersetlightlevel)

---

### `Character.HasInventory`

```ags
bool Character.HasInventory(InventoryItem *item)
```

Checks whether the character currently has the specified inventory item.
Returns *true* if they do, or *false* if they don't.

The parameter is the inventory item's Script Name from the editor (for
example, *iPoster*).

Example:

```ags
if (player.HasInventory(iKey))
{
    Display("The player has the key!!");
}
```

will display a message if the player has the key.

*Compatibility:* Supported by **AGS 3.1.0** and later versions.

*See also:* [`Character.AddInventory`](Character#characteraddinventory),
[`Character.InventoryQuantity`](Character#characterinventoryquantity),
[`Character.LoseInventory`](Character#characterloseinventory)

---

### `Character.IsCollidingWithChar`

*(Formerly known as `AreCharactersColliding`, which is now obsolete)*

```ags
Character.IsCollidingWithChar(Character* otherChar)
```

Checks if the character is touching OTHERCHAR. This function just checks
the baseline of both characters, so if one is standing a fair distance
behind the other, it will not be marked as colliding.

Returns 1 if the characters feet are touching, 0 otherwise.

Example:

```ags
if (cEgo.IsCollidingWithChar(cMan) == 1)
{
    // Colliding code goes here.
}
```

will execute the colliding code only if the characters EGO and MAN are
colliding.

*See also:*
[`Character.IsCollidingWithObject`](Character#characteriscollidingwithobject),
[`Object.IsCollidingWithObject`](Object#objectiscollidingwithobject),
[`AreThingsOverlapping`](Globalfunctions_Room#arethingsoverlapping)

---

### `Character.IsCollidingWithObject`

*(Formerly known as `AreCharObjColliding`, which is now obsolete)*

```ags
Character.IsCollidingWithObject(Object* obj)
```

Checks whether the character's feet (i.e. the bottom third of the
character) are touching OBJ. This can be used to determine if the
character is standing on the object.

Returns 1 if they are, and 0 if they are not.

Example:

```ags
if (cEgo.IsCollidingWithObject(oPressurePlate) == 1) {
    // colliding code here
}
```

will execute the colliding code only if the character cEgo and the object oPressurePlate are colliding.

*See also:*
[`Character.IsCollidingWithChar`](Character#characteriscollidingwithchar),
[`Object.IsCollidingWithObject`](Object#objectiscollidingwithobject),
[`AreThingsOverlapping`](Globalfunctions_Room#arethingsoverlapping)

---

### `Character.LightLevel`

```ags
readonly int Character.LightLevel
```

If the character has an individual light set explicitly with the
[`Character.SetLightLevel`](Character#charactersetlightlevel) command,
this property returns the light level value. Otherwise it returns 0.

**NOTE:** without individual light level set, Character.LightLevel
returns 0 even if the character is affected by the ambient or region's
light.

*Compatibility:* Supported by **AGS 3.4.1** and later versions.

*See also:* [`Character.SetLightLevel`](Character#charactersetlightlevel),
[`SetAmbientLightLevel`](Globalfunctions_General#setambientlightlevel)

---

### `Character.LockView`

*(Formerly known as `SetCharacterView`, which is now obsolete)*

```ags
Character.LockView(int view, optional StopMovementStyle)
```

Sets the character's view to VIEW. This can be used to perform
animations with characters, for example bending down to pick something
up, which don't use the default view.

*StopMovementStyle* determines what to do if character was moving when
this function is called. You can pass either eStopMoving (in which case
the walking character will stop), or eKeepMoving (in which case the
character will keep moving). The default is eStopMoving.

**NOTE:** This function locks the character's view to the specified
view, so that it can only be changed by other script commands (i.e. it
won't automatically be changed by AGS on walkable areas, screen changes,
etc). When you are done with the animation, call UnlockView to allow AGS
to take control back.

Example:

```ags
cEgo.LockView(12);
cEgo.Animate(0, 0, eOnce, eBlock, eForwards);
cEgo.UnlockView();
```

will change the character's EGO view to view 12, perform an animation
using loop 0, wait until the animation finishes and then return the
character to his normal view.

*Compatibility:* Optional *StopMovementStyle* parameter is supported
only by **AGS 3.4.1** and later versions.

*See also:* [`Character.Animate`](Character#characteranimate),
[`Character.ChangeView`](Character#characterchangeview),
[`Character.SpeechView`](Character#characterspeechview),
[`Character.LockViewAligned`](Character#characterlockviewaligned),
[`Character.LockViewOffset`](Character#characterlockviewoffset)
[`Character.UnlockView`](Character#characterunlockview),

---

### `Character.LockViewAligned`

*(Formerly known as `SetCharacterViewEx`, which is now obsolete)*

```ags
Character.LockViewAligned(int view, int loop, Alignment, optional StopMovementStyle)
```

Sets the character's view to VIEW, and sets the character's current
frame to the first frame in LOOP of VIEW.

The main purpose of this command is that it can align the new frame to
the previous one. This is particularly useful if you want to go from the
character's normal walking view to a specific animation - since
characters have the central point as their 'axis', if you have a wider
animation then it can be difficult to stop yourself getting a jumping
effect when the animation starts.

*Alignment* can have one of the following values:

align | description
--- | ---
eAlignLeft | Moves the new frame so that the left hand side is at exactly the same X co-ordinate as the old one was.
eAlignCentre | Leaves the frames centered in the middle.<br>This is the default and using this is equivalent to just calling LockView.
eAlignRight | Moves the new frame so that the right hand side is at exactly the same X co-ordinate as the old one was.

Note that this only aligns the first frame of the animation, so to get
the full benefit all your frames in the animation loop should be the
same width. All following frames will be shifted by the same amount,
until UnlockView is called.

*StopMovementStyle* determines what to do if character was moving when
this function is called. You can pass either eStopMoving (in which case
the walking character will stop), or eKeepMoving (in which case the
character will keep moving). The default is eStopMoving.

**NOTE:** This function locks the character's view to the specified
view, so that it can only be changed by other script commands (i.e. it
won't automatically be changed by the program on regions, screen
changes, etc). When you are done with the animation, call UnlockView to
allow the program to take control back.

Example:

```ags
cEgo.LockViewAligned(12, 1, eAlignLeft);
cEgo.Animate(1, 5, eOnce, eBlock, eForwards);
cEgo.UnlockView();
```

will change the character's EGO view to view 12, perform an animation
using loop 1, wait until the animation finishes and then return the
character to his normal view.

*Compatibility:* Optional *StopMovementStyle* parameter is supported
only by **AGS 3.4.1** and later versions.

*See also:* [`Character.LockView`](Character#characterlockview),
[`Character.LockViewOffset`](Character#characterlockviewoffset),
[`Character.UnlockView`](Character#characterunlockview)

---

### `Character.LockViewFrame`

*(Formerly known as `SetCharacterFrame`, which is now obsolete)*

```ags
Character.LockViewFrame(int view, int loop, int frame, optional StopMovementStyle)
```

Sets the character's graphic to frame FRAME of loop LOOP of view number
VIEW. This is useful if you don't want an animation, but just want to
change the character to display a specific frame.

The frame will be locked to the one you specify until you call
UnlockView.

*StopMovementStyle* determines what to do if character was moving when
this function is called. You can pass either eStopMoving (in which case
the walking character will stop), or eKeepMoving (in which case the
character will keep moving). The default is eStopMoving.

Example:

```ags
cEgo.LockViewFrame(AGHAST, 2, 4);
Wait(40);
cEgo.UnlockView();
```

will change EGO to have frame 4 of loop 2 in the AGHAST view, wait for a
second, then return him to normal.

*Compatibility:* Optional *StopMovementStyle* parameter is supported
only by **AGS 3.4.1** and later versions.

*See also:* [`Character.Animate`](Character#characteranimate),
[`Character.LockView`](Character#characterlockview),
[`Character.UnlockView`](Character#characterunlockview)

---

### `Character.LockViewOffset`

*(Formerly known as `SetCharacterViewOffset`, which is now obsolete)*

```ags
Character.LockViewOffset(int view, int xOffset, int yOffset, optional StopMovementStyle)
```

Sets the character's view to VIEW, in the same way as LockView does.
However, it also adds a specified offset to all the character's frames
until UnlockView is called.

The XOFFSET and YOFFSET parameters specify **in actual game resolution
units** how much to move the character's sprite. Positive values for X
move right, for Y move down; negative values do the opposite.

This command is designed to allow you to cope with those niggly
situations where animations don't quite line up with the standing frame,
assuming all the frames of the animation are the same size. Note that
LockViewAligned is easier to use if your frames will align at the left
or right hand side.

*StopMovementStyle* determines what to do if character was moving when
this function is called. You can pass either eStopMoving (in which case
the walking character will stop), or eKeepMoving (in which case the
character will keep moving). The default is eStopMoving.

**NOTE:** You should only use this command for minor adjustments, since
the offsets do not affect the clickable area of the character, what
walkable area he is in, and so forth. You should limit the use of this
command to in-game cutscenes where the player has no control.

**NOTE:** This function locks the character's view to the specified
view, so that it can only be changed by other script commands (i.e. it
won't automatically be changed by AGS on walkable areas, screen changes,
etc). When you are done with the animation, call UnlockView to allow AGS
to take control back.

Example:

```ags
cEgo.LockViewOffset(12, 1, -1);
cEgo.Animate(1, 5, eOnce, eBlock, eForwards);
cEgo.UnlockView();
```

will change EGO's view to view 12 and animate using loop 1, meanwhile
all frames will be shifted 1 pixel right and 1 pixel up.

*Compatibility:* Optional *StopMovementStyle* parameter is supported
only by **AGS 3.4.1** and later versions.

*See also:* [`Character.LockView`](Character#characterlockview),
[`Character.LockViewAligned`](Character#characterlockviewaligned),
[`Character.UnlockView`](Character#characterunlockview)

---

### `Character.LoseInventory`

*(Formerly known as global function `LoseInventory`, which is now
obsolete)*<br>
*(Formerly known as `LoseInventoryFromCharacter`, which is now obsolete)*

```ags
Character.LoseInventory(InventoryItem *item)
```

Removes one quantity of the specified inventory item from the character's inventory. If
they do not have the item, nothing happens.

To completely remove an item of quantity greater than one will require calling the function InventoryQuantity times.

The parameter is the inventory item's Script Name from the editor.

Example:

```ags
cEgo.LoseInventory(iKey);
```

will make the character EGO lose the inventory item iKey from the
inventory tab.

**NOTE:** Make sure you check the [`Mouse.Mode`](Mouse#mousemode) after taking away an inventory item from a character, especially when the active cursor is the inventory item the player just lost. Something like Mouse.Mode = eModeInteract; in the line after the LoseInventory command helps when you use the 2 click controls.


*See also:* [`Character.AddInventory`](Character#characteraddinventory),
[`Character.InventoryQuantity`](Character#characterinventoryquantity)

---

### `Character.Move`

```ags
Character.Move(int x, int y, optional BlockingStyle,
               optional WalkWhere);
```

Starts the character moving from its current location to (X,Y), but does
not play the character's walking animation.

The parameters to this command are identical to the
[`Character.Walk`](Character#characterwalk) command -- see that page for
more details. The only difference is that *Walk* plays the walking
animation whereas *Move* does not.

In the vast majority of cases, you will use **Character.Walk** instead.

Example:

```ags
cEgo.Move(155, 122, eBlock);
```

will make the character move to 155,122 without playing his walking
animation. The script will not continue until the character has reached
his destination.

*Compatibility:* Supported by **AGS 3.1.0** and later versions.

*See also:* [`Character.AddWaypoint`](Character#characteraddwaypoint),
[`Character.FaceCharacter`](Character#characterfacecharacter),
[`Character.Walk`](Character#characterwalk),
[`MoveCharacterToObject`](Globalfunctions_General#movecharactertoobject),
[`Object.Move`](Object#objectmove),
[`Character.StopMoving`](Character#characterstopmoving)

---

### `Character.MoveStraight`

```ags
Character.MoveStraight(int x, int y, optional BlockingStyle);
```

Moves the character from its current location towards (X,Y) in a straight line as far as is possible before hitting a non-walkable area, without playing a walking animation.

This is useful for keyboard movement, since it guarantees that the character will move in a straight line in the direction specified.

*blocking* determines whether the function waits for the character to finish moving before your script resumes. eNoBlock is the default.

Example:

```ags
cEgo.MoveStraight(cEgo.x + 10, cEgo.y + 10, eBlock);
```

will make the character move in a straight line 10 pixels towards bottom-right, unless hitting a non-walkable area earlier, without playing his walking animation.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:* [`Character.Move`](Character#charactermove),
 [`Character.Walk`](Character#characterwalk),
 [`Character.WalkStraight`](Character#characterwalkstraight)

---

### `Character.PlaceOnWalkableArea`

*(Formerly known as `MoveToWalkableArea`, which is now obsolete)*

```ags
Character.PlaceOnWalkableArea()
```

Places the character in the nearest walkable area to its current
location. If the character is already on a walkable area, nothing
happens.

This is useful for example in the Player Enters Room event of a room, to
make sure the character can move if a ChangeRoom with co-ordinates has
been issued to get there. You could also use this in on_event for
eEventEnterRoomBeforeFadein to use whenever a player enters a room.

**NOTE:** `PlaceOnWalkableArea` ignores any parts of the room beyond room Edges. This was done so, because Edges usually serve like an area boundaries and may trigger player leaving the room.

**NOTE:** historically, prior to **AGS 3.6.2**, if no walkable area was found within a very close range, then `PlaceOnWalkableArea` has been searching whole room in intervals of 5 pixels. This was originally done for performance reasons. Because of that it could fail if walkable areas are too thin. Starting with **3.6.2** it searches each pixel but does so in a more performant way, so should not miss even 1-pixel areas.

Example:

```ags
cEgo.x = Random(320);
cEgo.y = Random(200);
cEgo.PlaceOnWalkableArea();
```

will move character EGO to a random position but make sure that he is on
a walkable area.

*See also:*
[`Room.NearestWalkableArea`](Room#roomnearestwalkablearea)

---

### `Character.RemoveTint`

```ags
Character.RemoveTint()
```

Undoes the effects of calling Tint, and returns the character to using
the room's ambient tint.

Example:

```ags
player.Tint(0, 250, 0, 30, 100);
Wait(40);
player.RemoveTint();
```

will tint the player character green for a second, then turn it back to
normal.

*See also:*
[`Character.HasExplicitTint`](Character#characterhasexplicittint),
[`Character.Tint`](Character#charactertint)

---

### `Character.IsInteractionAvailable`

```ags
Character.IsInteractionAvailable(CursorMode)
```

Checks whether there is an event handler defined for activating the
character in cursor mode MODE.

This function is very similar to RunInteraction, except that rather than
run the event handler script function, it simply returns *true* if
something would have happened, or *false* if unhandled_event would have
been run.

Example:

```ags
if (cNPC.IsInteractionAvailable(eModeTalkto) == 0)
    Display("speaking with this character would not do anything.");
```

*Compatibility:* Supported by **AGS 3.4.0** and later versions.

*See also:* [`IsInteractionAvailable`](Globalfunctions_General#isinteractionavailable),
[`Character.RunInteraction`](Character#characterruninteraction)

---

### `Character.RunInteraction`

*(Formerly known as `RunCharacterInteraction`, which is now obsolete)*

```ags
Character.RunInteraction(CursorMode)
```

Fires the event script as if the player had clicked the mouse on the
character in the specified cursor mode. This is one of the mouse cursor
modes, as defined in your Cursors tab in the editor.

Example:

```ags
cMan.RunInteraction(eModeTalk);
```

will execute the code defined in the MAN's "TALK TO CHARACTER" event.

*See also:* [`Room.ProcessClick`](Room#roomprocessclick),
[`Character.IsInteractionAvailable`](Character#characterisinteractionavailable),
[`Hotspot.RunInteraction`](Hotspot#hotspotruninteraction),
[`InventoryItem.RunInteraction`](InventoryItem#inventoryitemruninteraction)

---

### `Character.Say`

*(Formerly known as `DisplaySpeech`, which is now obsolete)*

```ags
Character.Say(string message, ...)
```

Plays speech by the given character. The actual look and behavior of a speech depends on the speech style settings, which may be configured in [General Settings](GeneralSettings#dialog) or [Speech functions](Speech) in script.

If a character has a valid Speech View assigned to it, that view will be used to animate either character itself or character's portrait.

* **Lucasarts style**: displays the text above the character's head, animates character using Speech View.
* **Sierra style**, with or without background: displays character's portrait and speech in fixed position at the top of the screen. Animates portrait using Speech View.
* **WholeScreen style**: covers whole room with a black overlay, and displays character's portrait and speech in the middle. Animates portrait using Speech View.

The text will remain on screen for a limited time, and the player may or may not be able to click it away depending on the setting of "Allow speech to be skipped" and runtime property [Speech.SkipStyle](Speech#speechskipstyle).

`Character.Say` supports playing a voice file. For that you need to precede the text with `&N`, where N is a voice clip's number. For more information, see the [Voice speech](VoiceSpeech) section.

This function is used by the dialog scrips by default, using it in normal script is fully equivalent to having "charname: message" in the dialog.

You can insert the value of variables into the message. For more information, see the [string formatting](StringFormats) section.

Examples:

```ags
cEgo.Say("My name is Ego.");
```

Will display a simple text.

```
cEgo.Say("My age is %d", age_variable);
```

Will display a text and print some variable in it.

```
cEgo.Say("&5 Hear me talking.");
```

Will display a text and play voice clip named "EGO5".

*See also:* [`Display`](Globalfunctions_Message#display),
[`Character.SayAt`](Character#charactersayat),
[`Character.SayBackground`](Character#charactersaybackground),
[`Character.Think`](Character#characterthink),
[`Speech`](Speech)
[Voice speech](VoiceSpeech)

---

### `Character.SayAt`

*(Formerly known as `DisplaySpeechAt`, which is now obsolete)*

```ags
SayAt(int x, int y, int width, string message, ...)
```

Similar to [`Say`](Character#charactersay), except that the text is
displayed with at the provided position (X,Y), in an area WIDTH wide.

The way the text is aligned to the given position depends on the speech style:
* Lucas-Arts style speech aligns by the left-bottom corner (upwards from the passed Y coordinate);
* Sierra style speech aligns by the left-top corner.

If a *negative* value is passed as either X or Y coordinate, the text will be instead set at the default position for the respective axis:
* Lucas-Arts style speech by default is positioned above the speaking character's head;
* Sierra style speech by default is positioned at the top of the screen, centered horizontally.

You can use this function to write the character's speech text anywhere
you like, and AGS will still play the character's talking animation and
so on if appropriate.

As of **AGS 3.6.1**, you can insert the value of variables into the message. 
For more information, see the [string formatting](StringFormats) section.

**NOTE:** This function does not support Whole-Screen speech.

Example:

```ags
cEgo.SayAt(220, 20, 100, "My name is ego");
```

will display the message in the top right corner of the screen, whilst
playing the character's talking animation.

*See also:* [`Character.Say`](Character#charactersay),
[`Character.SayBackground`](Character#charactersaybackground)

---

### `Character.SayBackground`

*(Formerly known as `DisplaySpeechBackground`, which is now obsolete)*

```ags
Overlay* Character.SayBackground(string message, ...)
```

Similar to Say, except that this function returns immediately and the
game continues while the character is talking. This allows you to have
characters talking in the background while the player does other things.
Note that the character's talking animation is not played if this
function is used.

This command works by creating a text overlay with an automatic removal
time delay. The overlay is returned by this command, so you can save it
for use later with Overlay.IsValid and Overlay.Remove, if you want to
remove the text prematurely.

If background speech is already on-screen for the character, it will be
removed and replaced with the new MESSAGE.

All background speech is automatically removed when a normal Say command
is used (unless you set the global variable
[`game.bgspeech_stay_on_display`](Gamevariables) to 1).

As of **AGS 3.6.1**, you can insert the value of variables into the message. 
For more information, see the [string formatting](StringFormats) section.

Example:

```ags
cMan.SayBackground("Hey, why won't you talk to me?");
```

will display the message above character MAN's head without pausing the
game.

*See also:* [`Character.Say`](Character#charactersay)

---

### `Character.SetAsPlayer`

*(Formerly known as `SetPlayerCharacter`, which is now obsolete)*

```ags
Character.SetAsPlayer()
```

Changes the character which the player controls to the specified
character. This function will also cause the room to change to the room
which the chosen character is currently in (though as with ChangeRoom,
the change won't happen until the end of the script).

Additionally, calling this command will cause the "player" variable to
be updated to point to the specified character.

Example:

```ags
cMan.SetAsPlayer();
```

will change the character that the player controls to character MAN and
also change to the room that MAN is in, if he is not in the current
room.

*See also:* [`Character.ID`](Character#characterid),
[`Character.ChangeRoom`](Character#characterchangeroom)

---

### `Character.SetLightLevel`

```ags
void Character.SetLightLevel(int light_level)
```

Sets individual light level for this character.

The light level is from **-100 to 100**.

In 8-bit games you cannot use positive light level for brightening
effect, but you may still use negative values to produce darkening
effect.

To disable character lighting and tinting effects, call
[`RemoveTint`](Character#characterremovetint).

**NOTE:** Setting a light level will disable any RGB tint set for the
character.

**NOTE:** Character's individual light level OVERRIDES both ambient
light level and local region light level.

Example:

```ags
cEgo.SetLightLevel(100);
```

This will give character EGO maximal individual brightness.

*Compatibility:* Supported by **AGS 3.4.0** and later versions.

*See also:* [`Character.Tint`](Character#charactertint),
[`SetAmbientLightLevel`](Globalfunctions_General#setambientlightlevel),
[`Object.SetLightLevel`](Object#objectsetlightlevel),
[`Region.LightLevel`](Region#regionlightlevel)

---

### `Character.SetIdleView`

*(Formerly known as `SetCharacterIdle`, which is now obsolete)*

```ags
Character.SetIdleView(int idleview, int delay)
```

Changes the character's idle view to IDLEVIEW, with a timeout of DELAY
seconds of inactivity before it is played. Inactivity is defined as when
the character is not moving and not being animated.

Setting DELAY to 0 causes the idle view to be looped continuously when
the character is not moving - this is useful when for example the
character is swimming and they need to tread water when idle.

Pass IDLEVIEW as -1 to disable the idle view completely.

**NOTE:** Due to a quirk in AGS, you cannot set the Idle View to view 1.
In the unlikely event that you created your idle view in View 1, you'll
need to move it to another view number.

Following actions are counted as character's "activity" and will reset idle timer, or stop idle animation if one was playing:
* Walking or moving with a Walk or Move command (but not changing x,y properties directly).
* Regular blocking speech started with a Say command if the SpeechView is set; if SpeechView is not set, then idling timer will not be reset, and idle animation may even play during speech. Also background speech (SayBackground) does not prevent idling timer or animation either.
* Turning on a spot with FaceDirection and similar commands (but not changing Loop property directly).
* Locking the view with LockView command will prevent idle animation completely for the duration of lock. Idling timer will start anew when the view is unlocked with UnlockView.

**NOTE:** There was a historical mistake in the engine because of which FaceDirection and similar commands did not reset idling timer nor prevented idle animation. The newer versions should have this fixed.

Example:

```ags
cEgo.SetIdleView(12, 30);
```

will change/set the character EGO's idle view to 12. The idle view will
be played if the character is idle for 30 seconds.

---

### `Character.SetWalkSpeed`

*(Formerly known as `SetCharacterSpeed`, which is now obsolete)*<br>
*(Formerly known as `SetCharacterSpeedEx`, which is now obsolete)*

```ags
Character.SetWalkSpeed(int x_speed, int y_speed)
```

Changes the character to have a walking speed of X_SPEED in the
horizontal direction and Y_SPEED in the vertical direction. The values
used for X_SPEED and Y_SPEED are identical to those set in the AGS
Editor for walking speed.

Speed values may be either positive or negative.
Positive values mean a speed of N pixels per game frame, while negative values mean a speed of 1/N pixels per game frame (that is - N game frames spent for a single pixel).

X_SPEED and Y_SPEED can be identical, in which case the character
moves with the same speed in any direction (the editor calls this
"Uniform movement speed").

**NOTE:** This function CANNOT be called while the character is moving,
so you must stop him first.

Example:

```ags
cEgo.SetWalkSpeed(10, 10);
```

will change the character EGO's speed to 10 pixels per game frame.

*See also:*
[`Character.AnimationSpeed`](Character#characteranimationspeed),
[`Character.StopMoving`](Character#characterstopmoving),
[`Character.Walk`](Character#characterwalk),
[`Character.WalkSpeedX`](Character#characterwalkspeedx),
[`Character.WalkSpeedY`](Character#characterwalkspeedy)

---

### `Character.StopMoving`

*(Formerly known as global function `StopMoving`, which is now obsolete)*

```ags
Character.StopMoving()
```

Stops the character moving and sets its graphic to the standing frame of
the current loop.

Example:

```ags
if (cEgo.x > 299)
{
    cEgo.StopMoving();
}
```

will stop the character when he reaches the coordinate x=300.

*See also:* [`Character.Walk`](Character#characterwalk),
[`Object.StopMoving`](Object#objectstopmoving)

---

### `Character.Think`

*(Formerly known as `DisplayThought`, which is now obsolete)*

```ags
Character.Think(string message, ...)
```

Displays the text MESSAGE as a thought above the specified character's
head. The text will remain on screen for a limited time, and the user
may or may not be able to click it away depending on the setting of
"Player can't skip speech text".

How this function displays the text depends on a few things: the Speech
Style setting, the "Custom thought bubble GUI" setting, and whether the
character has a thinking animation or not.

If the "Custom thought bubble GUI" setting is not set, then the
thought will be displayed in the same way as normal speech - the
difference being that the character's thinking animation will play (or
no animation if they don't have one).

If you are using Sierra-style speech and the character doesn't have a
thinking animation, the thought bubble will be displayed in
LucasArts-style.

If the "Custom thought bubble GUI" setting has been set, then the thought
will be displayed like normal speech, except that the bubble GUI will be
used for the window background. In LucasArts-style speech this means
above the character's head, in Sierra-style it will be done along the
top of the screen as normal.

If the character has a thinking animation, it will just loop through
once (it won't repeat).

You can insert the value of variables into the message. For more
information, see the [string formatting](StringFormats)
section.

Example:

```ags
cEgo.Think("I wonder what's for dinner.");
```

will display the message above EGO's head and play the character's
thinking animation.

*See also:*
[`Character.BlinkWhileThinking`](Character#characterblinkwhilethinking),
[`Character.Say`](Character#charactersay),
[`Character.Thinking`](Character#characterthinking),
[`Character.ThinkingFrame`](Character#characterthinkingframe),
[`Character.ThinkView`](Character#characterthinkview),
[`game.speech_bubble_width`](Gamevariables)

---

### `Character.Tint`

```ags
Character.Tint(int red, int green, int blue,
               int saturation, int luminance)
```

Tints the character on the screen to (RED, GREEN, BLUE) with SATURATION
percent saturation.

This function applies a tint to a specific character. For the meaning of
all the parameters, see [`SetAmbientTint`](Globalfunctions_General#setambienttint).

The tint set by this function overrides any ambient tint set for the
room. For this reason, passing the SATURATION as 0 to this function does
not turn it off - rather, it ensures that no tint is applied to the
character (even if an ambient tint is set).

To remove the tint set by this function and return to using the ambient
tint for this character, call
[`RemoveTint`](Character#characterremovetint).

**NOTE:** This function only works in hi-color games and with hi-color
sprites.

Example:

```ags
cEgo.Tint(0, 250, 0, 30, 100);
```

will tint the EGO character green.

*See also:*
[`Character.HasExplicitTint`](Character#characterhasexplicittint),
[`Character.RemoveTint`](Character#characterremovetint),
[`SetAmbientTint`](Globalfunctions_General#setambienttint)

---

### `Character.TintBlue`

```ags
readonly int Character.TintBlue
```

Gets the *Blue* setting for the character's current tint.

This property is read-only; to change it, use the
[`Character.Tint`](Character#charactertint) command.

**NOTE:** If the
[`Character.HasExplicitTint`](Character#characterhasexplicittint)
property is false, then this value is meaningless.

*Compatibility:* Supported by **AGS 3.4.1** and later versions.

*See also:* [`Character.Tint`](Character#charactertint),
[`Character.HasExplicitTint`](Character#characterhasexplicittint),
[`Character.TintGreen`](Character#charactertintgreen),
[`Character.TintRed`](Character#charactertintred),
[`Character.TintLuminance`](Character#charactertintluminance)

---

### `Character.TintGreen`

```ags
readonly int Character.TintGreen
```

Gets the *Green* setting for the character's current tint.

This property is read-only; to change it, use the
[`Character.Tint`](Character#charactertint) command.

**NOTE:** If the
[`Character.HasExplicitTint`](Character#characterhasexplicittint)
property is false, then this value is meaningless.

*Compatibility:* Supported by **AGS 3.4.1** and later versions.

*See also:* [`Character.Tint`](Character#charactertint),
[`Character.TintBlue`](Character#charactertintblue),
[`Character.TintRed`](Character#charactertintred),
[`Character.TintSaturation`](Character#charactertintsaturation),
[`Character.TintLuminance`](Character#charactertintluminance)

---

### `Character.TintRed`

```ags
readonly int Character.TintRed
```

Gets the *Red* setting for the character's current tint.

This property is read-only; to change it, use the
[`Character.Tint`](Character#charactertint) command.

**NOTE:** If the
[`Character.HasExplicitTint`](Character#characterhasexplicittint)
property is false, then this value is meaningless.

*Compatibility:* Supported by **AGS 3.4.1** and later versions.

*See also:* [`Character.Tint`](Character#charactertint),
[`Character.TintBlue`](Character#charactertintblue),
[`Character.TintGreen`](Character#charactertintgreen),
[`Character.TintSaturation`](Character#charactertintsaturation),
[`Character.TintLuminance`](Character#charactertintluminance)

---

### `Character.TintSaturation`

```ags
readonly int Character.TintSaturation
```

Gets the *saturation* setting for the character's current tint.

This property is read-only; to change it, use the
[`Character.Tint`](Character#charactertint) command.

**NOTE:** If the
[`Character.HasExplicitTint`](Character#characterhasexplicittint)
property is false, then this value is meaningless.

*Compatibility:* Supported by **AGS 3.4.1** and later versions.

*See also:* [`Character.Tint`](Character#charactertint),
[`Character.TintBlue`](Character#charactertintblue),
[`Character.TintGreen`](Character#charactertintgreen),
[`Character.TintRed`](Character#charactertintred),
[`Character.TintLuminance`](Character#charactertintluminance)

---

### `Character.TintLuminance`

```ags
readonly int Character.TintLuminance
```

Gets the *luminance* setting for the character's current tint.

This property is read-only; to change it, use the
[`Character.Tint`](Character#charactertint) command.

**NOTE:** If the
[`Character.HasExplicitTint`](Character#characterhasexplicittint)
property is false, then this value is meaningless.

*Compatibility:* Supported by **AGS 3.4.1** and later versions.

*See also:* [`Character.Tint`](Character#charactertint),
[`Character.TintBlue`](Character#charactertintblue),
[`Character.TintGreen`](Character#charactertintgreen),
[`Character.TintRed`](Character#charactertintred),
[`Character.TintSaturation`](Character#charactertintsaturation)

---

### `Character.UnlockView`

*(Formerly known as `ReleaseCharacterView`, which is now obsolete)*

```ags
Character.UnlockView(StopMovementStyle=eStopMoving)
```

Allows the engine to automatically control the character's view, as
normal. Use this once you have finished doing the animation which you
started with the LockView command.

*StopMovementStyle* determines what to do if character was moving when
this function is called. You can pass either eStopMoving (in which case
the walking character will stop), or eKeepMoving (in which case the
character will keep moving). The default is eStopMoving.

Example:

```ags
cEgo.LockView(12);
cEgo.Animate(0, 0, eOnce, eBlock, eForwards);
cEgo.UnlockView();
```

will play an animation using loop 0 of view 12, then return the
character to its normal view.

*Compatibility:* Optional *StopMovementStyle* parameter is supported
only by **AGS 3.4.1** and later versions.

*See also:* [`Character.LockView`](Character#characterlockview)

---

### `Character.Walk`

*(Formerly known as `MoveCharacter`, which is now obsolete)*<br>
*(Formerly known as `MoveCharacterBlocking`, which is now obsolete)*<br>
*(Formerly known as `MoveCharacterDirect`, which is now obsolete)*

```ags
Character.Walk(int x, int y, optional BlockingStyle,
               optional WalkWhere);
```

Starts the character moving from its current location to (X,Y), whilst
playing his walking animation.

If *blocking* is eNoBlock (the default) then control returns to the
script immediately, and the character will move in the background.

If *blocking* is eBlock then this command will wait for the character to
finish moving before your script resumes.

If *walkWhere* is eWalkableAreas (the default), then the character will
attempt to get as close a possible to (X,Y) by using the room's walkable
areas.

If *walkWhere* is eAnywhere, then the character will simply walk
directly from its current location to (X,Y), ignoring the room walkable
areas.

If you don't want the character's walking animation to play, you can use
the [`Move`](Character#charactermove) command instead.

**NOTE:** this function only works with characters which are on the
current screen.

**NOTE:** if you need to find out when the character has reached its
destination, use the [`Moving`](Character#charactermoving) property.

Example:

```ags
cEgo.Walk(155, 122, eBlock);
```

will make the character walk to 155,122. The script will not continue
until the character has reached his destination.

*See also:* [`Character.AddWaypoint`](Character#characteraddwaypoint),
[`Character.FaceCharacter`](Character#characterfacecharacter),
[`Character.Move`](Character#charactermove),
[`MoveCharacterToObject`](Globalfunctions_General#movecharactertoobject),
[`Object.Move`](Object#objectmove),
[`Character.StopMoving`](Character#characterstopmoving)

---

### `Character.WalkStraight`

*(Formerly known as `MoveCharacterStraight`, which is now obsolete)*

```ags
Character.WalkStraight(int x, int y, optional BlockingStyle);
```

Moves the character from its current location towards (X,Y) in a
straight line as far as is possible before hitting a non-walkable area.
This is useful for keyboard movement, since
it guarantees that the character will move in a straight line in the
direction specified.

*blocking* determines whether the function waits for the character to finish moving before your script resumes. eNoBlock is the default.

Example:

```ags
cEgo.WalkStraight(cEgo.x + 10, cEgo.y + 10, eBlock);
```

will make the character walk in a straight line 10 pixels towards bottom-right, unless hitting a non-walkable area earlier.

*See also:* [`Character.Move`](Character#charactermove),
 [`Character.MoveStraight`](Character#charactermovestraight)
 [`Character.Walk`](Character#characterwalk)

---

### `Character.ActiveInventory`

*(Formerly known as `SetActiveInventory`, which is now obsolete)*<br>
*(Formerly known as `character[].activeinv`, which is now obsolete)*

```ags
InventoryItem* Character.ActiveInventory
```

Gets/sets the character's current active inventory item. Setting it will
update the mouse cursor if appropriate.

This property is useful in "Use inventory on hotspot/character/etc"
events, to find out which inventory item the player is trying to use on
the target.

To deselect the current inventory, set it to *null*.

Example:

```ags
cEgo.ActiveInventory = iKey;
```

will make the inventory item iKey active (before you use it make sure
that the player has the inventory item)

---

### `Character.Animating`

*(Formerly known as `character[].animating`, which is now obsolete)*

```ags
readonly bool Character.Animating
```

Returns 1 if the character is currently animating.<br>
Returns 0 if the character has finished its animation.

This property is read-only. To change character animation, use the
[`Animate`](Character#characteranimate) command.

Example:

```ags
cEgo.Animate(5, 0);
while (cEgo.Animating) Wait(1);
```

will animate EGO and wait until the animation finishes.

In reality, you would simply use the Blocking parameter of Animate so
you wouldn't need to do this.

*See also:* [`Character.Animate`](Character#characteranimate),
[`Character.Moving`](Character#charactermoving),
[`Character.Speaking`](Character#characterspeaking),
[`Character.Thinking`](Character#characterthinking)

---

### `Character.AnimationSpeed`

*(Formerly known as `character[].animspeed`, which is now obsolete)*

```ags
int Character.AnimationSpeed;
```

Gets/sets the character's animation delay, as set in the editor.

Example:

```ags
player.AnimationSpeed = 4;
```

will change the player character's animation speed to 4.

*See also:* [`Character.SetWalkSpeed`](Character#charactersetwalkspeed),
[`Character.SpeechAnimationDelay`](Character#characterspeechanimationdelay)

---

### `Character.AnimationVolume`

```ags
int Character.AnimationVolume
```

Gets/sets the character's animation sound volume, which is a *relative* volume (0-100) of frame-linked sounds that play during character's animations.

**NOTE:** all the volume properties in Character act as relative factors: these are `AnimationVolume`, "volume" parameter of [`Animate`](Character#characteranimate) function, and character's scale if [`ScaleVolume`](Character#characterscalevolume) is also set. The final frame sound's volume will be equal to `sound volume * AnimationVolume % * scaled volume %` in case of a normal view animation (walking, talking, etc), and `sound volume * AnimationVolume % * scaled volume % * Animate's volume param %` when `Animate` is called.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`Character.Animate`](Character#characteranimate),
[`Character.ChangeView`](Character#characterchangeview),
[`Character.ScaleVolume`](Character#characterscalevolume),
[`Character.View`](Character#characterview)

---

### `Character.Baseline`

*(Formerly known as `SetCharacterBaseline`, which is now obsolete)*

```ags
int Character.Baseline
```

Gets/sets the character's baseline. This allows you to set a specific
base line for the character, which works as its z-order value when sorting characters, room objects and walk-behinds on screen.

The baseline's value can be from 1 to the height of the room.
Setting it to 0 (or lower) will make character go back to using its current y coordinate as the baseline (that's the default).

If you want to get the effective character's baseline, and Baseline's property value is 0, then the baseline is the character's Y co-ordinate.

**IMPORTANT:** If two or more Character (or other objects) have equal Baseline, their draw order is **undefined**, and should not be relied upon. This is because of how sprite sorting is done in the engine.

Example 1:

```ags
cEgo.Baseline = 120;
```

will move the character's baseline (which can be used for testing
collisions, or for walk-behinds) to a line positioned at y coordinate =
120.

Example 2:

```ags
int zorder = cEgo.Baseline;
if (zorder == 0)
    zorder = cEgo.y;
```

will find out the character's effective z-order in the room.

*See also:* [`Object.Baseline`](Object#objectbaseline),
[`SetWalkBehindBase`](Globalfunctions_Room#setwalkbehindbase)

---

### `Character.BlinkInterval`

*(Formerly part of `SetCharacterBlinkView`, which is now obsolete)*

```ags
int Character.BlinkInterval
```

Gets/sets the character's blinking interval, which specifies how long
the game waits between playing the blinking animation. This is specified
in game loops - an interval of 80 would play the blinking animation
about every 2 seconds.

This property has no effect if no
[`BlinkView`](Character#characterblinkview) has been set.

**NOTE:** blinking animation works only for Sierra-style or QFG4-style speech, when the SpeechView is displaying a character's portrait.

Example:

```ags
cEgo.BlinkView = 10;
cEgo.BlinkInterval = 160;
```

will change the character EGO's blink view to view 10, and play the
animation every 4 seconds.

*See also:* [`Character.BlinkView`](Character#characterblinkview),
[`Character.SpeechView`](Character#characterspeechview)

---

### `Character.BlinkView`

*(Formerly part of `SetCharacterBlinkView`, which is now obsolete)*

```ags
int Character.BlinkView
```

Gets/sets the character's blinking view. To stop the character from
blinking, set this to -1.

The [`BlinkInterval`](Character#characterblinkinterval) property sets how
often the blinking animation is played.

**NOTE:** blinking animation works only for Sierra-style or QFG4-style speech, when the SpeechView is displaying a character's portrait.

Example:

```ags
cEgo.BlinkView = 10;
cEgo.BlinkInterval = 160;
```

will change the character EGO's blink view to view 10, and play the
animation every 4 seconds.

*See also:*
[`Character.BlinkInterval`](Character#characterblinkinterval),
[`Character.SpeechView`](Character#characterspeechview)

---

### `Character.BlinkWhileThinking`

```ags
bool Character.BlinkWhileThinking
```

Gets/sets whether the character can blink while thinking. By default
this is set to true, but if your blinking animation only goes with the
talking animation and not the thinking one, you can stop the character
from blinking while Thinking by setting this to false.

**NOTE:** blinking animation works only for Sierra-style or QFG4-style speech, when the ThinkView is displaying a character's portrait.

Example:

```ags
cEgo.BlinkWhileThinking = false;
```

will stop EGO from blinking while his thinking animation is playing.

*See also:* [`Character.BlinkView`](Character#characterblinkview),
[`Character.Think`](Character#characterthink)

---

### `Character.BlockingHeight`

```ags
int Character.BlockingHeight
```

Gets/sets the character's blocking height.

The blocking height determines how large of a blocking rectangle the
character exerts to stop other characters walking through it. If this is
set to 0 (the default), then the blocking rectangle is automatically
calculated to be the character's width, and 5 pixels high.

You can manually change the setting by entering a blocking height in
pixels, which is the size of walkable area that the character
effectively removes by standing on it.

**NOTE:** This property has no effect unless the
[`Solid`](Character#charactersolid) property is set to *true*.

Example:

```ags
cEgo.BlockingHeight = 20;
```

will make EGO block 20 pixels high (10 above and 10 below his baseline)

*See also:*
[`Character.BlockingRectX`](Character#characterblockingrectx),
[`Character.BlockingRectY`](Character#characterblockingrecty),
[`Character.BlockingWidth`](Character#characterblockingwidth),
[`Character.Solid`](Character#charactersolid)

---

### `Character.BlockingRectX`

```ags
int Character.BlockingRectX
```

Gets/sets the X offset of the character's blocking rectangle.

By default, the blocking rectangle is centered around character's position. This property specifies a *relative* offset either to left (negative value) or right (positive value) from character's center.

**NOTE:** This property has no effect unless the [`Solid`](Character#charactersolid) property is set to *true*.

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

*See also:*
[`Character.BlockingRectY`](Character#characterblockingrecty),
[`Character.BlockingHeight`](Character#characterblockingwidth),
[`Character.BlockingHeight`](Character#characterblockingheight),
[`Character.Solid`](Character#charactersolid)

---

### `Character.BlockingRectY`

```ags
int Character.BlockingRectY
```

Gets/sets the Y offset of the character's blocking rectangle.

By default, the blocking rectangle is centered around character's position. This property specifies a *relative* offset either to up (negative value) or down (positive value) from character's center.

**NOTE:** This property has no effect unless the [`Solid`](Character#charactersolid) property is set to *true*.

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

*See also:*
[`Character.BlockingRectX`](Character#characterblockingrectx),
[`Character.BlockingHeight`](Character#characterblockingwidth),
[`Character.BlockingHeight`](Character#characterblockingheight),
[`Character.Solid`](Character#charactersolid)

---

### `Character.BlockingWidth`

```ags
int Character.BlockingWidth
```

Gets/sets the character's blocking width.

The blocking width determines how large of a blocking rectangle the
character exerts to stop other characters walking through it. If this is
set to 0 (the default), then the blocking rectangle is automatically
calculated to be the character's width, and 5 pixels high.

You can manually change the setting by entering a blocking width in
pixels, which is the size of walkable area that the character
effectively removes by standing on it.

**NOTE:** This property has no effect unless the
[`Solid`](Character#charactersolid) property is set to *true*.

Example:

```ags
cEgo.BlockingWidth = 50;
```

will make EGO block 50 pixels wide (25 pixels to the left of his X
co-ordinate, and 25 to the right)

*See also:*
[`Character.BlockingRectX`](Character#characterblockingrectx),
[`Character.BlockingRectY`](Character#characterblockingrecty),
[`Character.BlockingHeight`](Character#characterblockingheight),
[`Character.Solid`](Character#charactersolid)

---

### `Character.Clickable`

*(Formerly known as `SetCharacterClickable`, which is now obsolete)*

```ags
bool Character.Clickable
```

Gets/sets whether the character is recognized as something which the
player can interact with. This allows you to modify the "Clickable"
property set initially in the Editor.

If you set this to *true* then the player can look at, speak to, and so
on the character (as with the old Sierra games). If you set this to
*false*, then if the player clicks on the character it will activate
whatever is behind them (as with the old LucasArts games).

Example:

```ags
cMan.Clickable = 0;
```

will make the game ignore clicks on the character MAN.

*See also:* [`Object.Clickable`](Object#objectclickable),
[`Character.Move`](Character#charactermove),
[`Character.Moving`](Character#charactermoving),
[`Character.Walk`](Character#characterwalk)

---

### `Character.DestinationX`

```ags
readonly int Character.DestinationX
```

Gets the X coordinate of the character's final moving destination. If
character is not walking or moving it is equal to its current position.

*Compatibility:* Supported by **AGS 3.4.0** and later versions.

*See also:* [`Character.DestinationY`](Character#characterdestinationy),
[`Character.Move`](Character#charactermove),
[`Character.Moving`](Character#charactermoving),
[`Character.Walk`](Character#characterwalk)

---

### `Character.DestinationY`

```ags
readonly int Character.DestinationY
```

Gets the Y coordinate of the character's final moving destination. If
character is not walking or moving it is equal to its current position.

*Compatibility:* Supported by **AGS 3.4.0** and later versions.

*See also:* [`Character.DestinationX`](Character#characterdestinationx),
[`Character.Move`](Character#charactermove),
[`Character.Moving`](Character#charactermoving),
[`Character.Walk`](Character#characterwalk)

---

### `Character.DiagonalLoops`

*(Formerly part of `SetCharacterProperty`, which is now obsolete)*

```ags
bool Character.DiagonalLoops
```

Gets/sets whether diagonal walking loops are used for the character. If
this is set to *true*, then loops 4-7 will be used as diagonal walking
loops. If this is set to *false*, then the character will only face in 4
directions and you can use loops 4-7 for other purposes.

Example:

```ags
cEgo.DiagonalLoops = true;
```

will enable diagonal walking loops for character EGO.

---

### `Character.Following`

```ags
readonly Character* Character.Following
```

Gets the character this character is following.

Example:

```ags
void room_AfterFadeIn()
{
  cAlter.FollowCharacter(cEgo);
  cAlter.Say("I am following %s!", cAlter.Following.Name);
}
```

Will have cAlter follow cEgo and say "I am following Ego!", or whatever `cEgo`'s name is.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:* [`Character.FollowCharacter`](Character#characterfollowcharacter)

---

### `Character.Frame`

*(Formerly known as `character[].frame`, which is now obsolete)*

```ags
int Character.Frame
```

Gets/sets the character's current frame number. Usually you won't change
this directly, but will use the Animate command to play an animation.

Example:

```ags
Display("EGO currently using frame %d.", cEgo.Frame);
```

displays EGO's current frame number within his view.

*See also:* [`Character.Animate`](Character#characteranimate),
[`Character.Loop`](Character#characterloop),
[`Character.View`](Character#characterview)

---

### `Character.HasExplicitTint`

```ags
readonly bool Character.HasExplicitTint
```

Returns *true* if the character has a tint set explicitly with the
[`Character.Tint`](Character#charactertint) command.

Returns *false* if the character has no explicit tint, but it may still
be tinted by [`SetAmbientTint`](Globalfunctions_General#setambienttint) or a region
tint.

Example:

```ags
if (player.HasExplicitTint)
{
    player.RemoveTint();
}
```

removes the player's tint if it currently has one.

*Compatibility:* Supported by **AGS 3.1.0** and later versions.

*See also:* [`Character.Tint`](Character#charactertint),
[`Character.RemoveTint`](Character#characterremovetint)

---

### `Character.ID`

```ags
readonly int Character.ID
```

Gets the character's ID number. This is the character's number from the
editor, and is useful if you need to interoperate with legacy code that
uses the character's number rather than name.

Example:

```ags
MoveCharacter(cEgo.ID, 100, 50);
```

uses the obsolete MoveCharacter function to move EGO to (100, 50)

---

### `Character.IdleAnimationDelay`

```ags
int Character.IdleAnimationDelay
```

Gets/sets the character's idle animation delay

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`Character.IdleView`](Character#characteridleview), [`Character.SetIdleView`](Character#charactersetidleview)

---

### `Character.IdleView`

```ags
readonly int Character.IdleView
```

Gets the character's current idle view. If the character doesn't have
one, returns -1.

This property is read-only; to change the view, use the
[`SetIdleView`](Character#charactersetidleview) function.

Example:

```ags
Display("EGO's idle view is currently view %d.", cEgo.IdleView);
```

will display EGO's current idle view number.

*See also:* [`Character.SetIdleView`](Character#charactersetidleview)

---

### `Character.IgnoreLighting`

*(Formerly known as `SetCharacterIgnoreLight`, which is now obsolete)*

```ags
bool Character.IgnoreLighting
```

Allows you to dynamically modify the "ignore lighting" checkbox for the
character. If this is set to 0, the character will be affected by region
light levels and tints; if this is set to 1, then the character will
ignore all region lighting.

Example:

```ags
cEgo.IgnoreLighting = 1;
```

will make the character look the same no matter if he stands on regions
with different light levels.

---

### `Character.IgnoreWalkbehinds`

**This property is obsolete since AGS 3.5.0 and not recommended for use at all.**

*(Formerly known as `SetCharacterIgnoreWalkbehinds`, which is now
obsolete)*

```ags
bool Character.IgnoreWalkbehinds
```

Gets/sets whether the character is affected by walkbehind areas. Passing
*false* (the default setting) means that the character will be placed
behind walk- behind areas according to the relevant baselines.

Passing *true* means that the character will never be placed behind a
walk-behind area. This is useful if for example you want to use the
character as an overlay to display rain or snow onto a scene.

**IMPORTANT:** This property is a "dirty hack" and not recommended for use at all. It breaks the logic of drawing order for room elements, and only works as intended if your game is run using Software graphics driver. We strongly suggest to design your rooms without it and rely on [`Baseline`](Character#characterbaseline) property instead.

---

### `Character.InventoryQuantity`

*(Formerly known as `character[].inv`, which is now obsolete)*

```ags
int Character.InventoryQuantity[]
```

Gets/sets the quantity of the specified inventory item that the
character currently has. The array index is the inventory item number,
from the Inventory pane in the editor.

Usually, you should use the AddInventory and LoseInventory functions to
modify the character's inventory; however, if you need to add or remove
a large number of items in one go, directly changing this array can be
an easier method.

If you change this array directly, the on-screen inventory will not be
updated. In this case, you must call UpdateInventory to see any new or
removed items.

If you just want to quickly check whether the character has a particular
item or not, use the [`HasInventory`](Character#characterhasinventory)
function instead.

Example:

```ags
Display("The player has $%d.", player.InventoryQuantity[iCash.ID]);
```

will display how many inventory items of type iCash the player has.

*See also:* [`UpdateInventory`](Globalfunctions_General#updateinventory),
[`Character.AddInventory`](Character#characteraddinventory),
[`Character.HasInventory`](Character#characterhasinventory),
[`Character.LoseInventory`](Character#characterloseinventory)

---

### `Character.Loop`

*(Formerly known as `character[].loop`, which is now obsolete)*

```ags
int Character.Loop
```

Gets/sets the character's current loop number. Usually you won't change
this directly, but will use the Animate command to play an animation.

Example:

```ags
Display("EGO currently using loop %d.", cEgo.Loop);
```

displays EGO's current loop number within his view.

*See also:* [`Character.Animate`](Character#characteranimate),
[`Character.Frame`](Character#characterframe),
[`Character.View`](Character#characterview)

---

### `Character.ManualScaling`

*(Formerly known as `Character.IgnoreScaling`, which is now obsolete)*<br>
*(Formerly part of `SetCharacterProperty`, which is now obsolete)*

```ags
bool Character.ManualScaling
```

Gets/sets whether the character's scaling level is determined by the
walkable area that he is walking on, or whether it is set manually by
the script. This is equivalent, though opposite, to the "UseRoomAreaScaling" property in the Character pane of the editor.

If this is set to *true*, then the character's scaling level is set
manually by the [`Scaling`](Character#characterscaling) property (by
default this is `100%`). If it is set to *false*, then the character
will be stretched or shrunk automatically as appropriate on walkable
areas.

Example:

```ags
cEgo.ManualScaling = true;
cEgo.Scaling = 50;
```

will tell EGO to ignore walkable area scaling levels and be fixed to
`50%` zoom level.

*See also:* [`Character.Scaling`](Character#characterscaling)

---

### `Character.MovementLinkedToAnimation`

```ags
bool Character.MovementLinkedToAnimation
```

Gets/sets whether the character's movement is linked to their animation.
By default this is *true*, which means that when the character is
walking their movement across the screen will be kept in sync with their
animation frame changing. Without this, the character can appear to
"glide" across the screen.

In some special cases you may wish to turn this off though, and to do so
you can set this property to *false*.

In previous versions of AGS, this setting was known as "Anti-glide mode"
and was a game-wide setting.

Example:

```ags
player.MovementLinkedToAnimation = false;
player.Walk(50, 100, eBlock);
player.MovementLinkedToAnimation = true;
```

will turn off movement-linked animation for the player character, walk
him to (50,100), then turn it back on again.

*Compatibility:* Supported by **AGS 3.1.1** and later versions.

*See also:* [`Character.Move`](Character#charactermove),
[`Character.Moving`](Character#charactermoving),
[`Character.Walk`](Character#characterwalk)

---

### `Character.Moving`

*(Formerly known as `character[].walking`, which is now obsolete)*

```ags
readonly bool Character.Moving
```

Returns *true* if the character is currently moving, or *false* if not.

This property is read-only; to change the character's movement, use the
[`Walk`](Character#characterwalk), [`Move`](Character#charactermove) and
[`StopMoving`](Character#characterstopmoving) commands.

Example:

```ags
cEgo.Walk(125, 40);
while (cEgo.Moving) Wait(1);
```

will move EGO to 125,40 and return control to the player when he gets
there.

*See also:* [`Character.Animating`](Character#characteranimating),
[`Character.Move`](Character#charactermove),
[`Character.Speaking`](Character#characterspeaking),
[`Character.StopMoving`](Character#characterstopmoving),
[`Character.Thinking`](Character#characterthinking),
[`Character.Walk`](Character#characterwalk)

---

### `Character.Name`

*(Formerly known as `character[].name`, which is now obsolete)*

```ags
String Character.Name
```

Gets/sets the name of the character, as set in the AGS Editor. This is
the full name, not the script name.

Example:

```ags
Display("You are controlling %s.", player.Name);
```

will display the name of the player character.

*Compatibility:* character's name was limited to 40 characters prior to **AGS 3.6.1**.

*See also:* [`Character.ScriptName`](Character#characterscriptname)

---

### `Character.NormalView`

*(Formerly known as `character[].defview`, which is now obsolete)*

```ags
readonly int Character.NormalView
```

Gets the character's normal view. This is the character's standard
walking view, that is used when his view is not locked to something
else.

This property is read-only; to change it, use the
[`ChangeView`](Character#characterchangeview) command.

Example:

```ags
if (cEgo.View == cEgo.NormalView) {
    Display("EGO is not animating, not talking and not idle.");
}
```

will display a message if EGO is currently displayed using his normal
view.

*See also:* [`Character.ChangeView`](Character#characterchangeview),
[`Character.View`](Character#characterview)

---

### `Character.on`

```ags
int Character.on
```

Gets/sets whether the character is enabled and visible. This lets you disable any character anytime, including player character. When disabled a character will not be drawn on screen and not updated (not animating, not moving, etc). Disabled characters may still be modified and have their coordinates changed by setting their corresponding properties directly, and also moved to another room with [ChangeRoom](Character#characterchangeroom) function.

Rooms have a property called "ShowPlayerCharacter", which is enabled by default. If this property is disabled, then player character will have their `on` property set to 0 when such room is loaded, and restored it to 1 when leaving that room.

---

### `Character.PreviousRoom`

*(Formerly known as `character[].prevroom`, which is now obsolete)*

```ags
readonly int Character.PreviousRoom
```

Gets the room number that the character was previously in. If the
character is still in the room that they started in, this will be -1.
Otherwise, it will be the room number of the room that they were last
in.

This is a read-only property. It is set automatically by
[`ChangeRoom`](Character#characterchangeroom).

Example:

```ags
Display("EGO's previous room was %d.", cEgo.PreviousRoom);
```

will display the EGO character's previous room.

---

### `Character.Room`

*(Formerly known as `character[].room`, which is now obsolete)*

```ags
readonly int Character.Room
```

Gets the room number that the character is currently in.

This is a read-only property. It is set by
[`ChangeRoom`](Character#characterchangeroom).

Example:

```ags
Display("EGO is in room %d.", cEgo.Room);
```

will display the EGO character's current room.

---

### `Character.ScaleMoveSpeed`

*(Formerly part of `SetCharacterProperty`, which is now obsolete)*

```ags
bool Character.ScaleMoveSpeed
```

Gets/sets whether the character's movement speed is adjusted in line
with his current scaling level. This allows you to modify the "Adjust
speed with scaling" option from the editor.

If you set this to *true*, the character's movement speed will be
adjusted so that he walks at a speed relative to his current scaling
level. If you set this to *false*, the character will always just move
at his normal speed.

Example:

```ags
cEgo.ScaleMoveSpeed = true;
```

will mean that EGO's speed is adjusted in line with his scaling

*See also:* [`Character.ScaleVolume`](Character#characterscalevolume)

---

### `Character.ScaleVolume`

```ags
bool Character.ScaleVolume
```

Gets/sets whether the character's volume is adjusted in line with his
current scaling level. This allows you to modify the "Adjust volume with
scaling" option from the editor.

By default, this is *false*. If you set it to *true*, then any
frame-linked sounds for the character (for example, footstep sounds)
will have their volume automatically adjusted in line with the
character's scaling level. At the normal `100%` zoom level the sounds
will be played at normal volume, but will then get quieter and louder as
appropriate in scaled walkable areas.

**NOTE:** all the volume properties in Character act as relative factors: these are `AnimationVolume`, "volume" parameter of [`Animate`](Character#characteranimate) function, and character's scale if [`ScaleVolume`](Character#characterscalevolume) is also set. The final frame sound's volume will be equal to `sound volume * AnimationVolume % * scaled volume %` in case of a normal view animation (walking, talking, etc), and `sound volume * AnimationVolume % * scaled volume % * Animate's volume param %` when `Animate` is called.

Example:

```ags
cEgo.ScaleVolume = true;
```

will mean that EGO's footstep sounds are adjusted in line with his
scaling.

*See also:*
[`Character.ScaleVolume`](Character#characterscalevolume),
[`Character.ScaleMoveSpeed`](Character#characterscalemovespeed)

---

### `Character.Scaling`

```ags
int Character.Scaling
```

Gets/sets the character's current scaling level, in percents. The permitted value range is 1 to 32767. A character that has the regular size has this property set to 100.

You can only set the value of this property if
[`ManualScaling`](Character#charactermanualscaling) is enabled for the
character; otherwise, the scaling is determined automatically based on
the walkable area that the character is on.

This property returns the actual current scaling at any given moment, whether it was set by you or acquired from the walkable area.

Example:

```ags
cEgo.ManualScaling = true;
cEgo.Scaling = 50;
```

will tell EGO to ignore walkable area scaling levels and be fixed to
`50%` zoom level.

*Compatibility:* Since **AGS 3.6.0** this property's value can be in the range between 1 and 32767.
In the previous versions this property could only have values in the range between 5 and 200.

*See also:* [`Character.ManualScaling`](Character#charactermanualscaling)

---

### `Character.ScriptName`

*(Formerly known as `character[].scrname`, which is now obsolete)*

```ags
readonly String Character.ScriptName
```

Gets the script name of the character, which serves as a unique identifier, as set in the AGS Editor.

This may be useful if you have a pointer to some character stored in your variable, and want to know what it actually is. Normally you don't need a script name, as you have an automatic global variable for each character in the game, but sometimes you may want to save its script name as a text either to display it somewhere for testing purposes, or keep as a reference. You may later use [Character.GetByName](Character#charactergetbyname) function to retrieve the character by the previously saved script name.

*Compatibility:* Supported by **AGS 3.6.1** and later versions.

*See also:* [`Character.Name`](Character#charactername),
[`Character.GetByName`](Character#charactergetbyname)

---

### `Character.Solid`

*(Formerly part of `SetCharacterProperty`, which is now obsolete)*

```ags
bool Character.Solid
```

Gets/sets whether the character can be walked through by other
characters.

If this is set to *true*, then the character is solid and will block the
path of other characters. If this is set to *false*, then the character
acts like a hologram, and other characters can walk straight through
him.

Example:

```ags
cEgo.Solid = true;
```

will mean that EGO blocks the path other characters.

*See also:*
[`Character.BlockingHeight`](Character#characterblockingheight),
[`Character.BlockingWidth`](Character#characterblockingwidth)

---

### `Character.Speaking`

```ags
readonly bool Character.Speaking
```

Returns true if the character is currently talking, or false if not.

This property is read-only. It will **only** return true for the active
talking character; that is, it will not return true for any characters
talking with the SayBackground command.

Since this property will only be true while the character is speaking,
and speaking is a blocking command, this property will probably only be
useful to access from the repeatedly_execute_always or
late_repeatedly_execute_always handlers.

Example:

```ags
if ((cEgo.Speaking) && (!cEgo.Animating)) {
    cEgo.Animate(3, 5, eRepeat, eNoBlock);
}
```

will animate the character using loop 3 while they are talking (only
useful with Sierra-style speech).

*Compatibility*: Prior to **AGS 3.6.2** Character.Speaking used to always return `false` if the character had no SpeechView set.

*See also:* [`Character.Animating`](Character#characteranimating),
[`Character.Moving`](Character#charactermoving),
[`Character.Say`](Character#charactersay),
[`Character.SpeakingFrame`](Character#characterspeakingframe),
[`Character.Thinking`](Character#characterthinking)

---

### `Character.SpeakingFrame`

```ags
readonly int Character.SpeakingFrame
```

Returns the current frame number of the character's talking animation.
This is useful when using Sierra-style speech, if you want to
synchronize events with the progress of the close-up face talking
animation.

This property is read-only. It is only accessible while the character is
speaking; if you attempt to call it when
[`Character.Speaking`](Character#characterspeaking) is *false* then it
will raise an error.

Since speaking is a blocking command, this property will probably only
be useful access from the repeatedly_execute_always or
late_repeatedly_execute_always handlers.

Example:

```ags
if (cEgo.Speaking) {
    if (cEgo.SpeakingFrame == 0) {
        cMan.Move(cMan.x + 10, cMan.y, eNoBlock, eAnywhere);
    }
}
```

will move cMan to the right every time the talking animation loops back
to Frame 0.

*See also:* [`Character.Say`](Character#charactersay),
[`Character.Speaking`](Character#characterspeaking)

---

### `Character.SpeechAnimationDelay`

```ags
int Character.SpeechAnimationDelay;
```

Gets/sets the character's speech animation delay, as set in the editor.
This specifies how many game loops each frame of the character's speech
animation is shown for.

**NOTE:** This property is ignored if lip sync is enabled.

**NOTE:** This property **cannot** be used if the
Speech.UseGlobalSpeechAnimationDelay is set to **true**. In that case,
the Speech.GlobalSpeechAnimationDelay property value is used instead.

Example:

```ags
player.SpeechAnimationDelay = 4;
```

will change the player character's speech animation speed to 4.

*Compatibility:* Supported by **AGS 3.1.2** and later versions.

*See also:*
[`Character.AnimationSpeed`](Character#characteranimationspeed),
[`Character.SpeechView`](Character#characterspeechview),
[`Game.TextReadingSpeed`](Game#gametextreadingspeed),
[`Speech.GlobalSpeechAnimationDelay`](Speech#speechglobalspeechanimationdelay),
[`Speech.UseGlobalSpeechAnimationDelay`](Speech#speechuseglobalspeechanimationdelay),

---

### `Character.SpeechColor`

*(Formerly known as `SetTalkingColor`, which is now obsolete)*

```ags
int Character.SpeechColor
```

Gets/sets the character's speech text color. This is set by default in
the editor.

NEWCOLOR is the color slot index from the Palette Editor. This can be
0-255 for a 256-color game, or one of the hi-color indexes available
from the Palette Editor.

Example:

```ags
cEgo.SpeechColor = 14;
```

will change the character's EGO talking color to yellow.

*See also:* [`Character.SpeechView`](Character#characterspeechview)

---

### `Character.SpeechView`

*(Formerly known as `SetCharacterSpeechView`, which is now obsolete)*<br>
*(Formerly known as `character[].talkview`, which is now obsolete)*

```ags
int Character.SpeechView
```

Gets/sets the character's talking view. If you change it, the new view
number will be used as the character's talking view in all future
conversations.

You can set this to -1 to disable the character's speech view.

Example:

```ags
cEgo.SpeechView = 10;
```

will change the character EGO's speech view to view 10.

*See also:* [`Character.ChangeView`](Character#characterchangeview),
[`Character.BlinkView`](Character#characterblinkview),
[`Character.SpeechAnimationDelay`](Character#characterspeechanimationdelay),
[`Character.SpeechColor`](Character#characterspeechcolor)

---

### `Character.Thinking`

```ags
readonly bool Character.Thinking
```

Returns true if the character is currently thinking, or false if not.

This property is read-only. It will **only** return true for the active
thinking character.

Since this property will only be true while the character is thinking,
and thinking is a blocking command, this property will probably only be
useful to access from the repeatedly_execute_always or
late_repeatedly_execute_always handlers.

Example:

```ags
function repeatedly_execute_always()
{
    if (cEgo.Thinking)
        cEgo.Transparency = 50;
    else
        cEgo.Transparency = 0;
}
```

this will keep character semi-transparent while he is thinking.

*Compatibility:* Supported by **AGS 3.3.4** and later versions.

*See also:* [`Character.Animating`](Character#characteranimating),
[`Character.Moving`](Character#charactermoving),
[`Character.Speaking`](Character#characterspeaking)
[`Character.Think`](Character#characterthink),
[`Character.ThinkingFrame`](Character#characterthinkingframe),

---

### `Character.ThinkingFrame`

```ags
readonly int Character.ThinkingFrame
```

Returns the current frame number of the character's thinking animation.
This is useful when using Sierra-style speech, if you want to
synchronize events with the progress of the close-up face talking
animation.

This property is read-only. It is only accessible while the character is
thinking; if you attempt to call it when
[`Character.Thinking`](Character#characterthinking) is *false* then it
will raise an error.

Since thinking is a blocking command, this property will probably only
be useful access from the repeatedly_execute_always or
late_repeatedly_execute_always handlers.

Example:

```ags
if (cEgo.Thinking) {
    if (cEgo.ThinkingFrame == 0) {
        cMan.Move(cMan.x + 10, cMan.y, eNoBlock, eAnywhere);
    }
}
```

will move cMan to the right every time the thinking animation loops back
to Frame 0.

*Compatibility:* Supported by **AGS 3.3.4** and later versions.

*See also:* [`Character.Think`](Character#characterthink),
[`Character.Thinking`](Character#characterthinking)

---

### `Character.ThinkView`

*(Formerly known as `character[].thinkview`, which is now obsolete)*

```ags
int Character.ThinkView
```

Gets/sets the character's thinking view. This is used to animate the
character when a thought is being displayed.

Example:

```ags
cEgo.ThinkView = 14;
```

will change the character EGO's thinking view to 14.

*See also:* [`Character.Think`](Character#characterthink)

---

### `Character.Transparency`

*(Formerly known as `SetCharacterTransparency`, which is now obsolete)*

```ags
int Character.Transparency
```

Gets/sets the character's transparency. This is specified as a
percentage, from 0 to 100. 100 means fully transparent (i.e. invisible),
and 0 is totally opaque (fully visible). Numbers in between represent
varying levels of transparency.

**NOTE:** Transparency only works in 16-bit and 32-bit color games.

**NOTE:** When using the DirectX 5 driver, a large transparent character
can significantly slow down AGS.

Some rounding is done internally when the transparency is stored --
therefore, if you get the transparency after setting it, the value you
get back might be one out. Therefore, using a loop with
`cEgo.Transparency++;` is not recommended as it will probably end too
quickly.

In order to fade a character in, the best approach is shown in the
example below:

Example:

```ags
int trans = cEgo.Transparency;
while (trans < 100) {
    trans++;
    cEgo.Transparency = trans;
    Wait(1);
}
```

will gradually fade out the character from its current transparency
level to being fully invisible.

*See also:* [`Object.Transparency`](Object#objecttransparency)

---

### `Character.TurnBeforeWalking`

*(Formerly part of `SetCharacterProperty`, which is now obsolete)*

```ags
bool Character.TurnBeforeWalking
```

Gets/sets whether the character turns to face his new direction before
walking. This is equivalent (though opposite) to the editor "Do not turn
before walking" tick-box.

If you set this to `true`, the character will turn on the spot to face his
new direction before setting off on a walk. If you set this to `false`, the
character will instantly face in the correct direction and start
walking.

Example:

```ags
cEgo.TurnBeforeWalking = true;
```

will tell EGO to turn to face his new direction before setting off,
whenever he walks.

*See also:* [`Character.TurnWhenFacing`](Character#characterturnwhenfacing)

---

### `Character.TurnWhenFacing`

```ags
bool Character.TurnWhenFacing
```

Gets/sets whether the character turns on the spot when ordered to face the new standing direction.

If you set this to `true`, any time you tell the the character to face other direction, they will turn on the spot to face this
new direction. If you set this to `false`, the character will instantly face in the set direction.

Example:

```ags
cEgo.TurnWhenFacing= true;
cEgo.FaceDirection(eDirectionLeft, eBlock);
cEgo.FaceDirection(eDirectionRight, eBlock);
```

will tell EGO to turn to face left, and then turn again until they face right.

*Compatibility*: Supported by **AGS 3.6.2** and later versions.

*See also:* [`Character.TurnBeforeWalking`](Character#characterturnbeforewalking)

---

### `Character.View`

```ags
readonly int Character.View
```

Gets the view that the character is currently displayed using.

This property is read-only; to change the view, use the ChangeView and
LockView functions.

Example:

```ags
Display("EGO's view is currently view %d.", cEgo.View);
```

will display EGO's current view number.

*See also:* [`Character.ChangeView`](Character#characterchangeview),
[`Character.Frame`](Character#characterframe),
[`Character.LockView`](Character#characterlockview),
[`Character.Loop`](Character#characterloop),
[`Character.NormalView`](Character#characternormalview)

---

### `Character.WalkSpeedX`

```ags
readonly int Character.WalkSpeedX;
```

Gets the character's walking speed in the X direction. If using uniform
movement, this will be the same as the Y walking speed.

This property is read-only. To change the walking speed, use the
SetWalkSpeed function.

Example:

```ags
Display("player's x speed: %d", player.WalkSpeedX);
```

will display the player's X speed.

*See also:* [`Character.SetWalkSpeed`](Character#charactersetwalkspeed),
[`Character.WalkSpeedY`](Character#characterwalkspeedy)

---

### `Character.WalkSpeedY`

```ags
readonly int Character.WalkSpeedY;
```

Gets the character's walking speed in the Y direction. If using uniform
movement, this will be the same as the X walking speed.

This property is read-only. To change the walking speed, use the
SetWalkSpeed function.

Example:

```ags
Display("player's y speed: %d", player.WalkSpeedY);
```

will display the player's Y speed.

*See also:* [`Character.SetWalkSpeed`](Character#charactersetwalkspeed),
[`Character.WalkSpeedX`](Character#characterwalkspeedx)

---

### `Character.x`

```ags
int Character.x;
```

Gets/sets the character's current X co-ordinate. This is expressed in
normal room co-ordinates, and specifies the bottom-center of the
character's sprite.

**NOTE:** Do **NOT** change this property while the character is moving.
Make sure the character is standing still before changing his
co-ordinates.

Example:

```ags
Display("The player is at %d,%d.", player.x, player.y);
```

displays the player character's current coordinates.

*See also:* [`Character.y`](Character#charactery),
[`Character.z`](Character#characterz)

---

### `Character.y`

```ags
int Character.y;
```

Gets/sets the character's current Y co-ordinate. This is expressed in
normal room co-ordinates, and specifies the bottom-center of the
character's sprite.

**NOTE:** Do **NOT** change this property while the character is moving.
Make sure the character is standing still before changing his
co-ordinates.

Example:

```ags
Display("The player is at %d,%d.", player.x, player.y);
```

displays the player character's current coordinates.

*See also:* [`Character.x`](Character#characterx),
[`Character.z`](Character#characterz)

---

### `Character.z`

```ags
int Character.z;
```

Gets/sets the character's current Z position. This allows the character
to levitate off the ground, whilst still retaining its normal Y
co-ordinate for baseline calculations and regions.

Normally this is set to 0 (ground-level), but you can increase it to
make the character float.

Example:

```ags
while (player.z < 20) {
    player.z++;
    Wait(1);
}
```

gradually levitates the character up to 20 pixels.

*See also:* [`Character.x`](Character#characterx),
[`Character.y`](Character#charactery)
