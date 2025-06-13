## `Mouse` functions and properties

### `Mouse.ChangeModeGraphic`

*(Formerly known as `ChangeCursorGraphic`, which is now obsolete)*

```ags
Mouse.ChangeModeGraphic(CursorMode, int slot)
```

Changes the specified mouse cursor mode's cursor graphic to SLOT. This
allows you to dynamically change what a mouse cursor looks like.

**NOTE:** To change what the Use Inventory cursor looks like, you should
change the active inventory item's
[`CursorGraphic`](InventoryItem#inventoryitemcursorgraphic) property instead
of using this function.

Example:

```ags
mouse.ChangeModeGraphic(eModeLookat, 120);
```

will change the cursor's graphic for look mode to the image that's
imported in the sprite's manager slot 120.

*See also:*
[`Mouse.ChangeModeHotspot`](Mouse#mousechangemodehotspot),
[`Mouse.ChangeModeView`](Mouse#mousechangemodeview),
[`Mouse.GetModeGraphic`](Mouse#mousegetmodegraphic),
[`Mouse.Mode`](Mouse#mousemode)

---

### `Mouse.ChangeModeHotspot`

*(Formerly known as `ChangeCursorHotspot`, which is now obsolete)*

```ags
Mouse.ChangeModeHotspot(CursorMode, int x, int y)
```

Permanently changes the specified mouse cursor mode's hotspot on the
cursor graphic to (X,Y). This is the offset into the graphic where the
click takes effect. (0,0) is the upper left corner of the cursor
graphic.

Example:

```ags
mouse.ChangeModeHotspot(eModeWalkto, 10, 10);
```

will change the cursor's hotspot for walk mode to coordinates 10,10.

*See also:*
[`Mouse.ChangeModeGraphic`](Mouse#mousechangemodegraphic),
[`Mouse.ChangeModeView`](Mouse#mousechangemodeview)

---

### `Mouse.ChangeModeView`

```ags
Mouse.ChangeModeView(CursorMode, int view)
```

Changes the specified mouse cursor mode's animation view to VIEW.

You can pass *view* as -1 to stop the cursor from animating.

This allows you to dynamically change the view used for the cursor's
animation while the game is running.

Example:

```ags
mouse.ChangeModeView(eModeLookat, ROLLEYES);
```

will change the Look cursor's view to ROLLEYES.

*See also:*
[`Mouse.ChangeModeGraphic`](Mouse#mousechangemodegraphic),
[`Mouse.ChangeModeHotspot`](Mouse#mousechangemodehotspot)

---

### `Mouse.Click`

```ags
static void Click(MouseButton)
```

Fires mouse click event at current mouse position (at mouse.x /
mouse.y). This is in all aspects identical to what would happen if a
player clicked actual mouse button. This function may be useful to
simulate player actions in game, or create automatic demonstrations
(like tutorials).

Example:

```ags
Mouse.SetPosition(100, 100);
Mouse.Click(eMouseLeft);
```

This will simulate user click at (100,100).

*Compatibility:* Supported by **AGS 3.4.0** and later versions.

*See also:* [`GUI.ProcessClick`](GUI#guiprocessclick),
[`Room.ProcessClick`](Room#roomprocessclick)

---

### `Mouse.CursorShader`

```ags
static ShaderInstance* Mouse.CursorShader
```

Gets/sets the shader applied to the mouse cursor.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Mouse.DisableMode`

*(Formerly known as `DisableCursorMode`, which is now obsolete)*

```ags
Mouse.DisableMode(int mode)
```

Disables the mouse cursor MODE. Any attempts to set the cursor to this
mode while it is disabled (such as using UseModeGraphic) will fail. This
function also greys out and disables any interface buttons whose
left-click command is set as "Set mode X", where X is equal to MODE.

If the current cursor mode is MODE, then the engine will change it to
the next enabled standard cursor.

Example:

```ags
mouse.DisableMode(eModeWalkto);
```

will make the walk mode unavailable until it's enabled again.

*See also:* [`Mouse.EnableMode`](Mouse#mouseenablemode),
[`Mouse.IsModeEnabled`](Mouse#mouseismodeenabled)

---

### `Mouse.EnableMode`

*(Formerly known as `EnableCursorMode`, which is now obsolete)*

```ags
Mouse.EnableMode(int mode)
```

Re-enables the mouse cursor mode MODE. This function also enables any
interface buttons which were disabled by the DisableMode command.

Example:

```ags
mouse.EnableMode(eModeWalkto);
```

will enable cursor mode walk which was disabled before.

*See also:* [`Mouse.DisableMode`](Mouse#mousedisablemode),
[`Mouse.IsModeEnabled`](Mouse#mouseismodeenabled)

---

### `Mouse.IsModeEnabled`

```ags
static bool Mouse.IsModeEnabled(int mode)
```

Returns whether the specified mouse cursor is currently enabled.

Example:

```ags
if (mouse.IsModeEnabled(eModeWalkto)) {
    mouse.Mode = eModeWalkto;
}
```

will change the the "WalkTo" cursor mode, but only if it's currently
enabled.

*See also:* [`Mouse.EnableMode`](Mouse#mouseenablemode),
[`Mouse.DisableMode`](Mouse#mousedisablemode)

---

### `Mouse.GetModeGraphic`

```ags
static int Mouse.GetModeGraphic(CursorMode)
```

Returns the sprite slot number of the specified mouse cursor mode.

This could be useful if you want to change it manually with
ChangeModeGraphic but be able to remember what it was before.

Example:

```ags
Display("The current mouse cursor sprite is %d.", mouse.GetModeGraphic(mouse.Mode));
```

will display the sprite slot number of the current mouse cursor.

*See also:* [`Mouse.ChangeModeGraphic`](Mouse#mousechangemodegraphic)

---

### `Mouse.IsButtonDown`

*(Formerly known as global function `IsButtonDown`, which is now
obsolete)*

```ags
Mouse.IsButtonDown(MouseButton)
```

Tests whether the user has the specified mouse button down. BUTTON must
either be eMouseLeft, eMouseRight or eMouseMiddle.

Returns *true* if the button is currently pressed, *false* if not. This
could be used to test the length of a mouse click and similar effects.

Example:

```ags
int timer=0;  // (at top of script file)
if (mouse.IsButtonDown(eMouseRight)) {
    if (timer == 40) {
        Display("You pressed the right button for 1 sec");
        timer = 0;
    }
    else {
        timer++;
    }
}
```

will display the message if the player presses the right button for 1
sec.

*Compatibility:* *eMouseMiddle* supported by **AGS 3.2.0** and later
versions.<br>
*eMouseLeft* and *eMouseRight* supported by all versions.

*See also:* [`IsKeyPressed`](Globalfunctions_General#iskeypressed)

---

### `Mouse.SaveCursorUntilItLeaves`

*(Formerly known as `SaveCursorForLocationChange`, which is now obsolete)*

```ags
Mouse.SaveCursorUntilItLeaves()
```

Saves the current mouse cursor, and restores it when the mouse leaves
the current hotspot, object or character.

This allows you to temporarily change the mouse cursor when the mouse
moves over a hotspot, and have it automatically change back to the old
cursor when the player moves the mouse away.

**NOTE:** You must call this **BEFORE** you change to your new temporary
cursor, or the new cursor will be saved by this command.

Example:

```ags
mouse.SaveCursorUntilItLeaves();
mouse.Mode = eModeTalk;
```

will change the cursor mode to Talk for as long as the mouse is over the
current object

*See also:* [`Mouse.Mode`](Mouse#mousemode)

---

### `Mouse.SelectNextMode`

*(Formerly known as `SetNextCursorMode`, which is now obsolete)*

```ags
Mouse.SelectNextMode()
```

Selects the next enabled mouse cursor mode. This is useful for
Sierra-style right-click cycling through modes. This function will
choose the next mode marked as a Standard Mode, and will also use the
Use Inventory mode if the player has an active inventory item.

*See also:* [`Mouse.Mode`](Mouse#mousemode)

---

### `Mouse.SelectPreviousMode`

```ags
Mouse.SelectPreviousMode()
```

Selects the previous enabled mouse cursor mode. This is useful for
Sierra-style right-click cycling through modes. This function will
choose the previous mode marked as a Standard Mode, and will also use
the Use Inventory mode if the player has an active inventory item.

*See also:* [`Mouse.Mode`](Mouse#mousemode)

---

### `Mouse.SetBounds`

*(Formerly known as `SetMouseBounds`, which is now obsolete)*

```ags
Mouse.SetBounds(int left, int top, int right, int bottom)
```

Restricts the area where the mouse can move on screen. The four
parameters are the relevant pixel co-ordinates of that edge of the
bounding rectangle. They are in the usual range (0, 0) - (Screen.Width, Screen.Height).

You can pass (0,0,0,0) to disable the bounding rectangle and allow the
mouse to move everywhere, as usual.

*NOTE:* The effect of this function only lasts until the player leaves
the screen, at which point the cursor bounds will be reset.

Example:

```ags
mouse.SetBounds(160, 100, 320, 200);
```

will restrict the mouse cursor to the bottom-right quarter of the
screen of a game that has screen resolution of 320x200.

*See also:* [`Mouse.SetPosition`](Mouse#mousesetposition)

---

### `Mouse.SetPosition`

*(Formerly known as `SetMousePosition`, which is now obsolete)*

```ags
Mouse.SetPosition(int x, int y)
```

Moves the mouse pointer to screen co-ordinates (X,Y). They are in the
usual range (0, 0) - (Screen.Width,Screen.Height). The mouse.x and mouse.y variables
will be updated to reflect the new position.

**NOTE:** Only use this command when absolutely necessary. Moving the
mouse cursor for the player is a sure way to irritate them if you do it
too often during the game.

Example:

```ags
mouse.SetPosition(Screen.Width/2, Screen.Height/2);
```

will place the mouse cursor in the center of the screen.

*See also:* [`Mouse.SetBounds`](Mouse#mousesetbounds)

---

### `Mouse.Update`

*(Formerly known as `RefreshMouse`, which is now obsolete)*

```ags
Mouse.Update();
```

Updates the global variables "mouse.x" and "mouse.y" with the current
position of the mouse. Normally, these variables are set just before
each script function is executed. However, if you have a very long
script where the mouse may have moved since the start of the function,
and you need the exact current location, then this command will update
the variables.

Example:

```ags
Display("The mouse was at: %d, %d.", mouse.x, mouse.y);
mouse.Update();
Display("The mouse is now at: %d, %d.", mouse.x, mouse.y);
```

will display the mouse position just before each dialog box is displayed

---

### `Mouse.UseDefaultGraphic`

*(Formerly known as `SetDefaultCursor`, which is now obsolete)*

```ags
Mouse.UseDefaultGraphic()
```

Changes the appearance of the mouse cursor to the default for the
current cursor mode. Use this to restore the cursor picture after you
changed it with the UseModeGraphic function.

*See also:* [`Mouse.UseModeGraphic`](Mouse#mouseusemodegraphic)

---

### `Mouse.UseModeGraphic`

*(Formerly known as `SetMouseCursor`, which is now obsolete)*

```ags
Mouse.UseModeGraphic(CursorMode)
```

Changes the appearance of the mouse cursor to use the specified cursor.
Unlike the Mouse.Mode property, this does not change the mode used if
the user clicks on a hotspot. This is useful for displaying a "wait"
cursor temporarily.

Example:

```ags
mouse.UseModeGraphic(eModeWait);
```

will change the mouse cursor to the cursor 'Wait' specified in the
Cursors tab.

*See also:*
[`Mouse.ChangeModeGraphic`](Mouse#mousechangemodegraphic),
[`Mouse.Mode`](Mouse#mousemode),
[`Mouse.UseDefaultGraphic`](Mouse#mouseusedefaultgraphic)

---

### `Mouse.AutoLock`

```ags
static bool Mouse.AutoLock;
```

Gets/sets if the system mouse cursor is automatically locked in the game window when the player clicks in it. The "locked" cursor will not be able to leave the bounds of the window. This is useful when playing a game in a windowed mode, preventing occasional misclicks outside that could lead to switching out of the game.

**NOTE:** it is strongly advised to not set this property unconditionally, but instead make a menu option in game where player could set this to their preference.

**NOTE:** the player may toggle mouse lock on and off anytime using **Ctrl + Alt** key combination; but that does not disable autolock on click.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

---

### `Mouse.ControlEnabled`

```ags
static bool Mouse.ControlEnabled;
```

Gets/sets if the mouse cursor movement has to be controlled by AGS
engine; otherwise it is controlled by operation system. Note that in practice mouse is only controlled when the game is run in fullscreen mode.

This setting may be useful to know if custom mouse speed setting is
applied or not.

Example:

```ags
sldMouseSpeed.Visible = Mouse.ControlEnabled;
```

makes mouse speed slider visible only if the mouse is controlled by the
game.

*Compatibility:* Supported by **AGS 3.3.5** and later versions.<br>
Settable since **AGS 3.5.0**.

*See also:* [`Mouse.Speed`](Mouse#mousespeed)

---

### `Mouse.Mode`

*(Formerly known as `GetCursorMode`, which is now obsolete)*<br>
*(Formerly known as `SetCursorMode`, which is now obsolete)*

```ags
int Mouse.Mode;
```

Gets/sets the current mode of the mouse cursor. This is one of the
cursor modes from your Cursors tab (but with eMode prepended). For
example, a cursor mode called "Walk to" on your cursors tab would be
eModeWalkto.

Setting this changes both the appearance of the cursor and the Cursor
Mode used if the player clicks on a hotspot.

Example:

```ags
if (mouse.Mode == eModeWalkto)
{
    // code here
}
```

will execute the code only if the current cursor mode is MODE 0 (WALK).

*See also:*
[`Mouse.SaveCursorUntilItLeaves`](Mouse#mousesavecursoruntilitleaves),
[`Mouse.UseModeGraphic`](Mouse#mouseusemodegraphic),
[`Mouse.SelectNextMode`](Mouse#mouseselectnextmode)

---

### `Mouse.Speed`

```ags
static float Mouse.Speed;
```

Gets/sets in-game mouse cursor speed. 1.0 is default, values in the
range 0-1 make cursor movement slower, values higher than 1 make cursor
movement faster.

Mouse speed changed this way will be saved to the configuration file
when player exits the game. Next time game starts it will restore the
last value you applied, unless player has modified it in the setup
program.

The custom mouse speed is only applied if mouse cursor movement is
currently being controlled by the game, which is usually true only when
game is run in fullscreen mode. Setting this property has no effect
otherwise. You can use
[`Mouse.ControlEnabled`](Mouse#mousecontrolenabled) to know if speed
will be applied.

**NOTE:** It is strongly advised to **NEVER** use this parameter to
achieve gameplay effect (such as character's loss of coordination, and
so forth).

Example:

```ags
Mouse.Speed = IntToFloat(sldMouseSpeed.Value) / 10.0;
```

converts slider control's position into mouse speed.

*Compatibility:* Supported by **AGS 3.3.5** and later versions.

*See also:* [`Mouse.ControlEnabled`](Mouse#mousecontrolenabled),
[`Mouse.SetPosition`](Mouse#mousesetposition)

---

### `Mouse.Visible`

*(Formerly known as `HideMouseCursor`, which is now obsolete)*<br>
*(Formerly known as `ShowMouseCursor`, which is now obsolete)*

```ags
bool Mouse.Visible;
```

Gets/sets whether the mouse cursor is visible. This is initially true by
default, but setting this to false is useful for briefly hiding the
cursor on occasions when you don't want it around.

If you want the LucasArts-style where the mouse cursor is never visible
during cutscenes then a much easier solution is simply to import a
transparent graphic over the default wait cursor, so that the Wait
cursor becomes invisible.

Example:

```ags
mouse.Visible = false;
Wait(40);
mouse.Visible = true;
```

hides the mouse, waits for a second, then turns it back on again

*See also:* [`Mouse.UseModeGraphic`](Mouse#mouseusemodegraphic)
