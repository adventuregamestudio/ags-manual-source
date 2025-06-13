## `GUI` functions and properties

### `GUI.BlendMode`

```ags
BlendMode GUI.BlendMode
```

Gets/sets the blending mode for this GUI.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [`BlendMode`](StandardEnums#blendmode)

---

### `GUI.Centre`

*(Formerly known as `CentreGUI`, which is now obsolete)*

```ags
GUI.Centre()
```

Centers the specified GUI in the middle of the screen. Useful if you've
been moving it around with SetPosition and just want to return it to the
center.

Example:

```ags
gControlpanel.Centre();
```

will center the CONTROLPANEL GUI in the middle of the screen.

*See also:* [`GUI.SetPosition`](GUI#guisetposition)

---

### `GUI.Click`

```ags
GUI.Click()
```

Forces GUI's OnClick event. If there is a script function bound to that
event it will be run, otherwise nothing happens.

**NOTE:** GUI.Click should not to be confused with **static** function
GUI.ProcessClick. GUI.Click is called for specific GUI and does not
impose any other conditions, while GUI.ProcessClick is called for "any
GUI element that happen to be at the given coordinates".

Example:

```ags
gMainMenu.Click();
```

triggers OnClick event for gMainMenu.

*Compatibility:* Supported by **AGS 3.4.0** and later versions.

*See also:* [`Button.Click`](Button#buttonclick),
[`GUI.ProcessClick`](GUI#guiprocessclick)

---

### `GUI.GetAtScreenXY`

*(Formerly known as `GetGUIAt`, which is now obsolete)*

```ags
static GUI* GUI.GetAtScreenXY(int x, int y)
```

Checks whether there is currently a GUI at screen co-ordinates (X,Y). If
there is, returns its GUI. If two GUIs overlap, the front-most one will
be returned - this can be changed with the GUI.ZOrder property.

If there is not currently a displayed, clickable GUI at the location
then *null* is returned. If null is returned, do NOT attempt to call any
methods or use any properties of the GUI (since it does not actually
exist).

**NOTE:** This command will not find any GUIs that are set as
Non-Clickable (i.e. the "Clickable" checkbox not checked).

Example:

```ags
GUI *theGui = GUI.GetAtScreenXY(mouse.x, mouse.y);
if (theGui == gInventory) {
    Display("Inventory GUI at mouse location.");
}
else if (theGui == null) {
    Display("No GUI at mouse location");
}
else {
    Display("GUI %d at mouse location.", theGui.ID);
}
```

will display the number of the GUI that the mouse is over.

*See also:*
[`GUIControl.GetAtScreenXY`](GUIControl#guicontrolgetatscreenxy),
[`GUI.ID`](GUI#guiid), [`GUI.ZOrder`](GUI#guizorder)

---

### `GUI.GetByName`

```ags
static GUI* GUI.GetByName(string scriptName)
```

Returns a pointer to the GUI with the specified script name, or null if it does not exist.

Normally you do not need to use this, as there will be a automatically created global script variable for each GUI which got a script name.
Where GetByName() function may come useful is situation in which you a) do not know exact name, b) had to store object's reference in a string for some reason. Good examples of this are saving object's name in a [custom property](CustomProperties), or a [file](File), then reading it back.

Example:

```ags
void ReadGUIConfig() {
    File *file = File.Open("$SAVEGAMEDIR$/guiconfig.dat", eFileRead);
    if (file == null) {
        return;
    }

    while (!file.EOF) {
        String name = file.ReadStringBack();
        int x = file.ReadInt();
        int y = file.ReadInt();

        GUI *gui = GUI.GetByName(name);
        if (gui != null)
        {
            gui.X = x;
            gui.Y = y;
        }
    }

    file.Close();
}
```

Above function opens a custom file called "guiconfig.dat" for reading, reads gui positions, and tries to move game guis to these.

*Compatibility:* Supported by **AGS 3.6.1** and later versions.

*See also:* [`GUI.ScriptName`](GUI#guiscriptname)

---

### `GUI.ProcessClick`

```ags
static void GUI.ProcessClick(int x, int y, MouseButton)
```

Simulates clicking the mouse on GUI on the location (X,Y) using the specified mouse button. This "click" has special behavior in that it
**only affects GUI and GUI controls**. The click will be received by a topmost control or parent GUI under given coordinates. If that control or GUI has functions attached to their events, these will be executed as in case of a normal player's click.
If there's no GUIs under the coordinates, then nothing will happen. Any other game elements (room objects, hotspots, characters) are not affected by this.

Example:

```ags
ProcessClick(100, 50, eMouseLeft);
```

will simulate clicking the mouse on co-ordinates (100, 50) with the left mouse button. Only GUIs will receive this click.

*Compatibility:* Supported by **AGS 3.4.0** and later versions.

*See also:* [`Mouse.Click`](Mouse#mouseclick),
[`Room.ProcessClick`](Room#roomprocessclick)

---

### `GUI.SetPosition`

*(Formerly known as `SetGUIPosition`, which is now obsolete)*

```ags
GUI.SetPosition(int x, int y)
```

Moves the top-left corner of GUI to the new location (X,Y) on the
screen. This allows you to dynamically move GUIs around on the screen
while the game is running. The co-ordinates are screen co-ordinates, not
room co-ordinates, and use the same scale as in the editor.

Example:

```ags
gMyGui.SetPosition(mouse.x, mouse.y);
```

will move the GUI to the position where the cursor is.

*See also:* [`GUI.Centre`](GUI#guicentre),
[`GUI.BackgroundGraphic`](GUI#guibackgroundgraphic),
[`GUIControl.SetPosition`](GUIControl#guicontrolsetposition),
[`GUI.SetSize`](GUI#guisetsize), [`GUI.X`](GUI#guix),
[`GUI.Y`](GUI#guiy)

---

### `GUI.SetSize`

*(Formerly known as `SetGUISize`, which is now obsolete)*

```ags
GUI.SetSize(int width, int height)
```

Changes the GUI to have the new size WIDTH x HEIGHT

This could be useful for initially hiding an 'Advanced' part of an
options screen and such like.

The size is in the normal 320x200-resolution pixels. Setting the size to
320, 200 will cause the GUI to take up the entire screen.

Example:

```ags
gIconbar.SetSize(160, 100);
```

changes the ICONBAR GUI to be the size of half the screen

*See also:* [`GUI.Centre`](GUI#guicentre),
[`GUI.Height`](GUI#guiheight),
[`GUIControl.SetPosition`](GUIControl#guicontrolsetposition),
[`GUI.SetPosition`](GUI#guisetposition),
[`GUI.Width`](GUI#guiwidth)

---

### `GUI.AsTextWindow`

```ags
readonly TextWindowGUI* GUI.AsTextWindow
```

If this GUI is of TextWindow type then returns the TextWindowGUI interface which could be used to access specific properties; otherwise returns null.

Example:

```ags
TextWindowGUI *tw = gMyTextGui.AsTextWindow;
tw.TextColor = Game.GetColorFromRGB(255, 0, 0);
```

*Compatibility:* Supported by **AGS 3.5.0** and later versions.

*See also:* [`TextWindowGUI`](TextWindowGUI)

---

### `GUI.BackgroundColor`

```ags
int GUI.BackgroundColor
```

Gets/sets the background color.

Setting BackgroundColor to 0 will make GUI background fully transparent.

This property is ignored if the GUI.BackgroundGraphic is assigned a sprite number (other than 0).

*Compatibility:* Supported by **AGS 3.5.0** and later versions.

*See also:* [`GUI.BackgroundGraphic`](GUI#guibackgroundgraphic), [`GUI.BorderColor`](GUI#guibordercolor)

---

### `GUI.BackgroundGraphic`

*(Formerly known as `SetGUIBackgroundPic`, which is now obsolete)*

```ags
int GUI.BackgroundGraphic
```

Gets/sets the background image of the GUI.

You can set this to 0 to remove the background image from the GUI.

When this property is assigned a sprite number other than 0 GUI.BackgroundColor and GUI.BorderColor are ignored.

*See also:* [`GUI.SetPosition`](GUI#guisetposition),
[`Button.NormalGraphic`](Button#buttonnormalgraphic)

---

### `GUI.BorderColor`

```ags
int GUI.BorderColor
```

Gets/sets the border color.

Setting BorderColor to 0 will make GUI border fully transparent.

Not applicable to TextWindow GUIs. This property is ignored if the GUI.BackgroundGraphic is assigned a sprite number (other than 0).

*Compatibility:* Supported by **AGS 3.5.0** and later versions.

*See also:* [`GUI.BackgroundColor`](GUI#guibackgroundcolor), [`GUI.BackgroundGraphic`](GUI#guibackgroundgraphic)

---

### `GUI.Clickable`

*(Formerly known as `SetGUIClickable`, which is now obsolete)*

```ags
bool GUI.Clickable
```

Gets/sets whether the GUI is clickable or not. This allows you to modify
the "Clickable" checkbox from the GUI Editor.

If this is set to 1, then the GUI will respond to mouse clicks as
normal.

If this is set to 0, then this GUI cannot be clicked on by the mouse.
This might be useful for a transparent overlay GUI which displays
information, and you want the player to be able to click on whatever is
underneath.

Example:

```ags
gStatusline.Clickable = false;
```

sets the STATUSLINE GUI to no longer respond to mouse clicks.

*See also:* [`GUI.GetAtScreenXY`](GUI#guigetatscreenxy)

---

### `GUI.ControlCount`

```ags
readonly int GUI.ControlCount;
```

Gets the number of controls on the GUI. This is useful if you wish to iterate through
all the GUI's controls, then this property would allow you to determine where to stop.

Example:

```ags
for (int i = 0; i < gMyGUI.ControlCount; i++) {
    gMyGUI.Controls[i].Enabled = false;
}
```

disables all controls on the gMyGUI GUI.

*See also:* [`GUI.Controls`](GUI#guicontrols)

---

### `GUI.Controls`

```ags
GUIControl* GUI.Controls[index]
```

Provides an array which allows you to access controls on the GUI by their index. This may be useful when you need to iterate through all the GUI's controls. And also if, for some reason, you cannot use actual control's name or GUIControl* pointer, and have to use a numeric ID instead.

Returns the GUIControl object for the specified control index, or *null* if you give an invalid control index.

You can cast the GUIControl to the appropriate type using the AsButton, AsLabel, etc methods on it.

Example:

```ags
for (int i = 0; i < gMyGUI.ControlCount; i++) {
    gMyGUI.Controls[i].Enabled = false;
}
```

disables all controls on the gMyGUI GUI.

```ags
int control_number = Random(gMyGUI.ControlCount - 1);
gMyGUI.Controls[control_number].Visible = false;
```

Makes a random control on gMyGUI invisible.

*See also:* [`GUIControl.As\*`](GUIControl#guicontrolastype),
[`GUI.ControlCount`](GUI#guicontrolcount)

---

### `GUI.Height`

```ags
int GUI.Height
```

Gets/sets the height of the GUI. This allows you to dynamically change
the size of the GUI on the screen.

The height is specified in the normal 320-resolution style.

Example:

```ags
Display("The icon bar GUI is %d pixels high.", gIconbar.Height);
```

displays the height of the ICONBAR GUI.

*See also:* [`GUI.SetSize`](GUI#guisetsize),
[`GUI.Width`](GUI#guiwidth)

---

### `GUI.ID`

```ags
readonly int GUI.ID
```

Gets the GUI's ID number. This is the GUI's number from the editor, and
is useful if you need to interoperate with legacy code that uses the
GUI's number rather than object name.

Example:

```ags
SetGUIClickable(gIconbar.ID, 1);
gIconbar.Clickable = false;
```

uses the obsolete SetGUIClickable function to make the ICONBAR GUI
clickable, and then uses the equivalent modern property to stop it being
clickable.

*See also:* [`GUIControl.ID`](GUIControl#guicontrolid)

---

### `GUI.PopupStyle`

```ags
readonly GUIPopupStyle GUI.PopupStyle
```

Gets the style of GUI behavior on screen. Possible values are:

    eGUIPopupNormal       no special behavior
    eGUIPopupMouseYPos    shown when the mouse cursor moves to the top of the screen, past GUI.PopupYPos, and hidden at all other times
    eGUIPopupModal        like normal, but pauses game when shown
    eGUIPopupPersistent   like normal, but not removed when the game's user interface is disabled

**NOTE:** for MouseYPos style [`GUI.Visible`](GUI#guivisible) property does not control visibility directly. The GUI will become visible only when both conditions match:
- GUI.Visible property set to TRUE;
- mouse cursor is closer to the screen top than GUI.PopupYPos value.

[`GUI.Shown`](GUI#guishown) property tells whether GUI is actually displayed, which may be different from GUI.Visible in the case of MouseYPos style.

**NOTE:** To automatically hide a GUI when the user interface is disabled, the
General Settings option `When player interface is disabled, GUIs should` needs
to be set as `Be hidden`.

*Compatibility:* Supported by **AGS 3.5.0** and later versions.

*See also:* [`GUI.PopupYPos`](GUI#guipopupypos), [`GUI.Shown`](GUI#guishown), [`GUI.Visible`](GUI#guivisible)

---

### `GUI.PopupYPos`

```ags
int GUI.PopupYPos
```

Gets/sets the Y co-ordinate at which the GUI will appear when using MouseYPos popup style. The GUI will be automatically shown when the mouse cursor's Y coordinate is *equal or lower* than GUI.PopupYPos (closer to the screen top) and hidden when it is *greater*.

This property is ignored if GUI has a different style.

*Compatibility:* Supported by **AGS 3.5.0** and later versions.

*See also:* [`GUI.PopupStyle`](GUI#guipopupstyle), [`GUI.Shown`](GUI#guishown), [`GUI.Visible`](GUI#guivisible)

---

### `GUI.Shown`

```ags
readonly bool GUI.Shown
```

Tells whether GUI is currently active on screen.

In the common circumstances this property's value is equivalent to checking [`GUI.Visible`](GUI#guivisible). But for GUIs with "Mouse Ypos" popup style the Visible property does not actually determine whether the GUI is displayed, but instead it controls whether the GUI **is allowed to** pop up. In such case it is Shown property that would tell if the GUI is actually displayed on screen at this moment.

**NOTE:** Shown property only reports a logical state, it does not test whether GUI may be seen by the player or not (overlapped by other objects, moved offscreen or fully transparent).

*Compatibility:* Supported by **AGS 3.5.1** and later versions.

*See also:* [`GUI.PopupStyle`](GUI#guipopupstyle), [`GUI.Visible`](GUI#guivisible)

---

### `GUI.ScriptName`

```ags
readonly String GUI.ScriptName;
```

Gets the script name of the gui, which serves as a unique identifier, as set in the AGS Editor.

This may be useful if you have a pointer to some gui stored in your variable, and want to know what it actually is. Normally you don't need a script name, as you have an automatic global variable for each gui in the game, but sometimes you may want to save its script name as a text either to display it somewhere for testing purposes, or keep as a reference. You may later use [GUI.GetByName](GUI#guigetbyname) function to retrieve the gui by the previously saved script name.

*Compatibility:* Supported by **AGS 3.6.1** and later versions.

*See also:* [`GUI.GetByName`](GUI#guigetbyname)

---

### `GUI.Transparency`

*(Formerly known as `SetGUITransparency`, which is now obsolete)*

```ags
int GUI.Transparency
```

Gets/sets the GUI translucency, in percent.

Setting this to 100 means the GUI is totally invisible, and lower values
represent varying levels of translucency. Set it to 0 to stop the GUI
being translucent.

**NOTE:** Transparency only works in 16-bit and 32-bit color games.

Some rounding is done internally when the transparency is stored --
therefore, if you get the transparency after setting it, the value you
get back might be one out. Therefore, using a loop with
`gInventory.Transparency++;` is not recommended as it will probably end
too quickly.

In order to fade a GUI in/out, the best approach is shown in the example
below:

Example:

```ags
int trans = gInventory.Transparency;
while (trans < 100) {
    trans++;
    gInventory.Transparency = trans;
    Wait(1);
}
```

will gradually fade the INVENTORY GUI out until it is invisible.

*See also:* [`GUIControl.Transparency`](GUIControl#guicontroltransparency),
[`Object.Transparency`](Object#objecttransparency)

---

### `GUI.Visible`

*(Formerly known as `GUIOff`, which is now obsolete)*<br>
*(Formerly known as `GUIOn`, which is now obsolete)*<br>
*(Formerly known as `InterfaceOff`, which is now obsolete)*<br>
*(Formerly known as `InterfaceOn`, which is now obsolete)*<br>
*(Formerly known as `IsGUIOn`, which is now obsolete)*

```ags
bool GUI.Visible
```

Gets/sets whether the GUI is visible or not. This property has behaves
differently depending on the GUI popup style.

For "Normal" and "Persistent" GUIs, this property simply switches the
GUI on and off, and has no further effects.

For "Popup modal" GUIs, setting Visible to true causes the game to
become paused until the GUI is removed by setting Visible back to false
(e.g. when the user presses an OK button or something similar).

For "Mouse Ypos" GUIs, the Visible property does not actually determine
whether the GUI is displayed, but instead it controls whether the GUI
**is allowed to** pop up. If Visible is *false*, then moving the mouse
to the top of the screen will not activate the GUI; if it is *true*,
then the GUI will be allowed to be popped up.

**NOTE:** for "Mouse Ypos" style GUIs the way to actually know if GUI is displayed or not is [`GUI.Shown`](GUI#guishown) property.

Example:

```ags
gSettings.Visible = true;
```

will turn on the SETTINGS GUI.

*See also:* [`GUI.Shown`](GUI#guishown), [`IsGamePaused`](Globalfunctions_General#isgamepaused)

---

### `GUI.Width`

```ags
int GUI.Width
```

Gets/sets the width of the GUI. This allows you to dynamically change
the size of the GUI on the screen.

The width is specified in the normal 320-resolution style.

Example:

```ags
gInventory.Width += 5;
```

makes the INVENTORY GUI 5 pixels wider.

*See also:* [`GUI.Height`](GUI#guiheight),
[`GUI.SetSize`](GUI#guisetsize)

---

### `GUI.X`

```ags
int GUI.X
```

Gets/sets the X position of the GUI. This allows you to dynamically
change the position of the GUI on the screen.

The X position is the left-hand side of the GUI, and can be between 0
and 320. The co-ordinates used are screen co-ordinates, not room
co-ordinates, and are in the normal 320-resolution style.

Example:

```ags
gMyGui.X += 5;
```

moves the GUI right 5 pixels.

*See also:* [`GUI.SetPosition`](GUI#guisetposition),
[`GUI.Y`](GUI#guiy)

---

### `GUI.Y`

```ags
int GUI.Y
```

Gets/sets the Y position of the GUI. This allows you to dynamically
change the position of the GUI on the screen.

The Y position is the top edge of the GUI, and can be between 0 and 200
(or 240, depending on room height). The co-ordinates used are screen
co-ordinates, not room co-ordinates, and are in the normal
320x200-resolution style.

Example:

```ags
gMyGui.Y += 5;
```

moves the GUI down 5 pixels.

*See also:* [`GUI.SetPosition`](GUI#guisetposition),
[`GUI.X`](GUI#guix)

---

### `GUI.ZOrder`

*(Formerly known as `SetGUIZOrder`, which is now obsolete)*

```ags
int GUI.ZOrder
```

Gets/sets the drawing z-order of the GUI. This allows you to dynamically change the ordering of GUIs on the screen.

Z-order setting is an arbitrary integer number that can be positive or negative. It tells how the overlapping objects should be sorted.
Those with lower z-order are drawn at the back, and those with higher z-order are drawn at the front.

The GUIs are sorted among themselves and screen Overlays, and thus their ZOrder is relative to other GUIs z-order and Overlays z-order values.

**NOTE:** If two or more GUIs have equal ZOrder, their relative order is then defined by their IDs: lower IDs are drawn at the back and higher IDs at the front.
*This is different from other kinds of game objects* which do not depend on their IDs at all when being sorted on screen.
Normally we don't recommend relying on this though, and suggest setting explicit ZOrder for the scenes where multiple GUIs may overlap each other.

Example:

```ags
gStatusline.ZOrder = 0;
```

sets the STATUSLINE GUI to be behind all other GUIs.

*See also:* [`GUI.GetAtScreenXY`](GUI#guigetatscreenxy)

