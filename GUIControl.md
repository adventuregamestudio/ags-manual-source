## `GUIControl` functions and properties

This section lists the functions and properties common to all types of
GUI control. Each individual control type (Button, ListBox, etc) also
has its own specific section.


---

### `GUIControl.GetAtScreenXY`

*(Formerly known as `GetGUIObjectAt`, which is now obsolete)*

```ags
static GUIControl* GUIControl.GetAtScreenXY(int x, int y)
```

Checks whether there is a GUI control at screen co-ordinates (X,Y).
Returns the control object if there is, or null if there is not. You
probably want to use this in conjunction with GetGUIAtLocation.

Example:

```ags
GUIControl *theControl = GUIControl.GetAtScreenXY(mouse.x, mouse.y);
if (theControl == lstSaveGames) {
    Display("The mouse is over the Save Games list box.");
}
else if (theControl == null) {
    Display("The mouse is not over a control.");
}
else {
    GUI *onGui = theControl.OwningGUI;
    Display("The mouse is over control %d on GUI %d.", theControl.ID, onGui.ID);
}
```

will display what control the mouse is over.

*See also:* [`GUI.GetAtScreenXY`](GUI#guigetatscreenxy)

---

### `GUIControl.GetByName`

```ags
static GUIControl* GUIControl.GetByName(string scriptName)
```

Returns a pointer to the GUI control with the specified script name, or null if it does not exist.

Normally you do not need to use this, as there will be a automatically created global script variable for each control which got a script name.
Where GetByName() function may come useful is situation in which you a) do not know exact name, b) had to store object's reference in a string for some reason. Good examples of this are saving object's name in a [custom property](CustomProperties), or a [file](File), then reading it back.

Example:

```ags
void ReadAndUpdateGUIControl() {
    File *input = File.Open("$SAVEGAMEDIR$/guicontrol_info.dat", eFileRead);
    if (input == null) {
        Display("Error opening file.");
        return;
    }

    String controlName = input.ReadStringBack();
    int controlX = input.ReadInt();
    int controlY = input.ReadInt();

    input.Close();

    GUIControl *controlToUpdate = GUIControl.GetByName(controlName);
    if (controlToUpdate != null) {
        controlToUpdate.SetPosition(controlX, controlY);
        Display("GUI control '%s' updated.", controlName);
    } else {
        Display("GUI control '%s' not found.", controlName);
    }
}
```

This is an example of a function to read the control information from a file previously created by AGS.
The file contains the name of a control and it's position, the function reads it and updates the control.

*Compatibility:* Supported by **AGS 3.6.1** and later versions.

*See also:* [`GUIControl.ScriptName`](GUIControl#guicontrolscriptname), [`GUIControl.SetPosition`](GUIControl#guicontrolsetposition)

---

### `GUIControl.AsType`

```ags
Button*  GUIControl.AsButton;
InvWindow* GUIControl.AsInvWindow;
Label*   GUIControl.AsLabel;
ListBox* GUIControl.AsListBox;
Slider*  GUIControl.AsSlider;
TextBox* GUIControl.AsTextBox;
```

Converts a generic GUIControl\* pointer into a variable of the correct
type, and returns it. If the control is not of the requested type,
returns *null*.

Example:

```ags
Button *theButton = gIconbar.Controls[2].AsButton;
if (theButton == null) {
    Display("Control 2 is not a button!!!!");
}
else {
    theButton.NormalGraphic = 44;
}
```

attempts to set Button 2 on GUI ICONBAR to have NormalGraphic 44, but if
that control is not a button, prints a message.

*See also:* [`GUI.Controls`](GUI#guicontrols)

---

### `GUIControl.BringToFront`

```ags
GUIControl.BringToFront()
```

Brings this control to the front of the Z-order. This allows you to
rearrange the display order of controls within the GUI.

**Applies To**

Inherited by the Button, InvWindow, Label, ListBox, Slider and TextBox.

Example:

```ags
btnBigButton.BringToFront();
```

will move the *btnBigButton* button to be in front of all other controls
on the GUI.

*See also:* [`GUIControl.SendToBack`](GUIControl#guicontrolsendtoback),
[`GUIControl.ZOrder`](GUIControl#guicontrolzorder)

---

### `GUIControl.Clickable`

```ags
bool GUIControl.Clickable
```

Gets/sets whether the GUI control is clickable.

This property determines whether the player can click the mouse on the
control. If it is set to *false*, then any mouse clicks will go straight
through the control onto whatever is behind it. Unlike the Enabled
property though, setting Clickable to false does not alter the
appearance of the control.

Note that disabling the control by setting Enabled to false overrides
this setting -- that is, if Enabled is false then the control will not
be clickable, regardless of the *Clickable* setting.

Also, bear in mind that if you set *Clickable* to false then any mouse
clicks will go through the control onto whatever is behind. On the other
hand, if *Enabled* is set to false then the control "absorbs" the mouse
click but does not do anything with it.

**Applies To**

Inherited by the Button, InvWindow, Label, ListBox, Slider and TextBox.

Example:

```ags
btnSaveGame.Clickable = false;
```

will make the *btnSaveGame* button non-clickable.

*See also:* [`GUIControl.Enabled`](GUIControl#guicontrolenabled)

---

### `GUIControl.Enabled`

*(Formerly known as `SetGUIObjectEnabled`, which is now obsolete)*

```ags
bool GUIControl.Enabled
```

Enables or disables a GUI control.

Normally, all your GUI controls (such as buttons, sliders, etc) are
enabled at all times except during a cutscene, when they are disabled.
This command allows you to explicitly disable a control at your script's
discretion.

If you set this to true, the control will be enabled; set to false to
disable it.

Whether you set it as enabled or not, it will **always** be disabled
during a blocking cutscene, along with all the other controls.

While a control is disabled, it will not respond to mouse clicks. If it
is a button, its mouseover and pushed pictures will not be shown. The
control will be drawn according to the game "When GUI Disabled"
settings, as usual.

**Applies To**

Inherited by the Button, InvWindow, Label, ListBox, Slider and TextBox.

Example:

```ags
btnSaveGame.Enabled = false;
```

will disable the *btnSaveGame* button.

*See also:* [`GUIControl.Clickable`](GUIControl#guicontrolclickable),
[`GUIControl.Visible`](GUIControl#guicontrolvisible)

---

### `GUIControl.Height`

```ags
int GUIControl.Height;
```

Gets/sets the height of the GUI control. This allows you to dynamically
resize GUI controls while the game is running.

**Applies To**

Inherited by the Button, InvWindow, Label, ListBox, Slider and TextBox.

Example:

```ags
btnConfirm.Height = 20;
```

makes the *btnConfirm* button 20 pixels high.

*See also:* [`GUIControl.SetSize`](GUIControl#guicontrolsetsize),
[`GUIControl.Width`](GUIControl#guicontrolwidth)

---

### `GUIControl.ID`

```ags
readonly int GUIControl.ID
```

Gets the GUI control's ID number. This is the control's object number
from the GUI editor, and is useful if you need to interoperate with
legacy code that uses the control's number rather than object name.

**Applies To**

Inherited by the Button, InvWindow, Label, ListBox, Slider and TextBox.

Example:

```ags
SetGUIObjectEnabled(lstSaves.OwningGUI.ID, lstSaves.ID, 1);
lstSaves.Enabled = false;
```

uses the obsolete SetGUIObjectEnabled function to enable the lstSaves
list box, and then uses the equivalent modern property to disable it.

*See also:* [`GUIControl.OwningGUI`](GUIControl#guicontrolowninggui),
[`GUI.ID`](GUI#guiid)

---

### `GUIControl.OwningGUI`

```ags
readonly GUI* GUIControl.OwningGUI
```

Gets the GUI control's owning GUI, which is the GUI that contains the
control.

Returns a GUI, which allows you to use all the usual
[GUI functions and properties](GUI).

**Applies To**

Inherited by the Button, InvWindow, Label, ListBox, Slider and TextBox.

Example:

```ags
GUI *thegui = lstSaves.OwningGUI;
thegui.Visible = false;

lstSaves.OwningGUI.Visible = true;
```

turns off the GUI that contains the lstSaves list box, then turns it on
again using the niftier full pathing approach.

*See also:* [`GUIControl.ID`](GUIControl#guicontrolid),
[`GUI.ID`](GUI#guiid)

---

### `GUIControl.SendToBack`

```ags
GUIControl.SendToBack()
```

Sends this control to the back of the Z-order. This allows you to
rearrange the display order of controls within the GUI.

**Applies To**

Inherited by the Button, InvWindow, Label, ListBox, Slider and TextBox.

Example:

```ags
btnBigButton.SendToBack();
```

will move the *btnBigButton* button to be behind all other controls on
the GUI.

*See also:*
[`GUIControl.BringToFront`](GUIControl#guicontrolbringtofront),
[`GUIControl.ZOrder`](GUIControl#guicontrolzorder)

---

### `GUIControl.SetPosition`

*(Formerly known as `SetGUIObjectPosition`, which is now obsolete)*

```ags
GUIControl.SetPosition(int x, int y)
```

Moves the top-left corner of the GUI control to be at (X,Y). These
co-ordinates are relative to the GUI which contains the control.

This allows you to dynamically move GUI controls around on the screen
while the game is running, and this may well be useful in conjunction
with GUI.SetSize if you want to create dynamically resizable GUIs.

**Applies To**

Inherited by the Button, InvWindow, Label, ListBox, Slider and TextBox.

Example:

```ags
btnConfirm.SetPosition(40, 10);
```

will move the *btnConfirm* button to be positioned at (40,10) within the
GUI.

*See also:* [`GUIControl.Enabled`](GUIControl#guicontrolenabled),
[`GUI.SetPosition`](GUI#guisetposition),
[`GUIControl.SetSize`](GUIControl#guicontrolsetsize),
[`GUIControl.X`](GUIControl#guicontrolx),
[`GUIControl.Y`](GUIControl#guicontroly)

---

### `GUIControl.SetSize`

*(Formerly known as `SetGUIObjectSize`, which is now obsolete)*

```ags
GUIControl.SetSize(int width, int height)
```

Adjusts the specified GUI control to have the new size WIDTH x HEIGHT.

This allows you to dynamically resize GUI controls on the screen while
the game is running, and this may well be useful in conjunction with
GUI.SetSize and GUIControl.SetPosition if you want to create dynamically
resizable GUIs.

**Applies To**

Inherited by the Button, InvWindow, Label, ListBox, Slider and TextBox.

Example:

```ags
invMain.SetSize(160, 100);
```

will resize the *invMain* control to have a size of 160 x 100.

*See also:* [`GUIControl.Height`](GUIControl#guicontrolheight),
[`GUIControl.SetPosition`](GUIControl#guicontrolsetposition),
[`GUI.SetSize`](GUI#guisetsize),
[`GUIControl.Width`](GUIControl#guicontrolwidth),

---

### `GUIControl.Transparency`

```ags
int GUIControl.Transparency
```

Gets/sets the GUIControl translucency, in percent.

Setting this to 100 means the GUI is totally invisible, and lower values
represent varying levels of translucency. Set it to 0 to stop the GUI
being translucent.

**NOTE:** Transparency only works in 16-bit and 32-bit color games.

Some rounding is done internally when the transparency is stored --
therefore, if you get the transparency after setting it, the value you
get back might be one out. Therefore, using a loop with
`gInventory.Transparency++;` is not recommended as it will probably end
too quickly.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`GUI.Transparency`](GUI#guitransparency),
[`Object.Transparency`](Object#objecttransparency)

---

### `GUIControl.Visible`

```ags
bool GUIControl.Visible
```

Gets/sets whether the GUI control is visible. This is *true* by default,
but you can set it to *false* in order to temporarily remove the GUI
control from the GUI.

While the control is invisible, it will not be drawn on the screen, and
will not register clicks or otherwise respond to any user input.

**Applies To**

Inherited by the Button, InvWindow, Label, ListBox, Slider and TextBox.

Example:

```ags
btnSaveGame.Visible = false;
```

will make the *btnSaveGame* button invisible.

*See also:* [`GUIControl.Enabled`](GUIControl#guicontrolenabled)

---

### `GUIControl.Width`

```ags
int GUIControl.Width;
```

Gets/sets the width of the GUI control. This allows you to dynamically
resize GUI controls while the game is running.

**Applies To**

Inherited by the Button, InvWindow, Label, ListBox, Slider and TextBox.

Example:

```ags
btnConfirm.Width = 110;
```

makes the *btnConfirm* button 110 pixels wide.

*See also:* [`GUIControl.Height`](GUIControl#guicontrolheight),
[`GUIControl.SetSize`](GUIControl#guicontrolsetsize)

---

### `GUIControl.X`

```ags
int GUIControl.X;
```

Gets/sets the X position of the GUI control. This specifies its left
edge, and is relative to the GUI which contains the control.

This allows you to dynamically move GUI controls around on their parent
GUI while the game is running.

**Applies To**

Inherited by the Button, InvWindow, Label, ListBox, Slider and TextBox.

Example:

```ags
btnConfirm.X = 10;
```

will move the *btnConfirm* button to be positioned 10 pixels from the
left of its GUI.

*See also:* [`GUIControl.SetPosition`](GUIControl#guicontrolsetposition),
[`GUIControl.Y`](GUIControl#guicontroly)

---

### `GUIControl.Y`

```ags
int GUIControl.Y;
```

Gets/sets the Y position of the GUI control. This specifies its top
edge, and is relative to the GUI which contains the control.

This allows you to dynamically move GUI controls around on their parent
GUI while the game is running.

**Applies To**

Inherited by the Button, InvWindow, Label, ListBox, Slider and TextBox.

Example:

```ags
btnConfirm.Y = 20;
```

will move the *btnConfirm* button to be positioned 20 pixels from the
top of its GUI.

*See also:* [`GUIControl.SetPosition`](GUIControl#guicontrolsetposition),
[`GUIControl.X`](GUIControl#guicontrolx)

---

### `GUIControl.ScriptName`

```ags
readonly String GUIControl.ScriptName
```

Gets the script name of the GUI Control, which serves as a unique identifier, as set in the AGS Editor.

This may be useful if you have a pointer to some dialog stored in your variable, and want to know what it actually is. Normally you don't need a script name, as you have an automatic global variable for each control in the game, but sometimes you may want to save its script name as a text either to display it somewhere for testing purposes, or keep as a reference. You may later use [GUIControl.GetByName](GUIControl#guicontrolgetbyname) function to retrieve the control by the previously saved script name.

Example:

```ags
void ActivateControl(const string controlName)
{
    GUIControl* control = GUIControl.GetByName(controlName);
    if (control != null) {
        control.Visible = true;
        System.Log(eLogInfo, "Activated control: %s", control.ScriptName);
    }
}
```

Activating a control by calling `ActivateControl("btnOptions")` will log its script name for debugging purposes and make it visible if it exists.

*Compatibility:* Supported by **AGS 3.6.1** and later versions.

*See also:* [`GUIControl.GetByName`](GUIControl#guicontrolgetbyname), [`GUIControl.Visible`](GUIControl#guicontrolvisible)

---

### `GUIControl.ZOrder`

```ags
int GUIControl.ZOrder;
```

Gets/sets the control's Z-order relative to other controls within the
same owning GUI. This allows you to precisely arrange the display order
of controls at runtime and to know which position the control had at
certain moment in time.

For AGS GUI Z-order means the order in which controls are displayed from
bottom to top. That means that control at the bottom has Z-order equal
to 0, and control at the top has highest Z-order, equal to (number of
controls - 1).

Setting `GUIControl.ZOrder = 0;` will do same thing as calling
`GUIControl.SendToBack()`, and setting
`GUIControl.ZOrder = GUIControl.OwningGUI.ControlCount - 1;` will do
same thing as calling `GUIControl.BringToFront()`.

If you try to set inappropriate ZOrder, the nearest acceptable one will
be applied instead. This also means that two controls cannot have equal ZOrder.

**Applies To**

Inherited by the Button, InvWindow, Label, ListBox, Slider and TextBox.

*Compatibility:* Supported by **AGS 3.4.0** and later versions.

*See also:*
[`GUIControl.BringToFront`](GUIControl#guicontrolbringtofront),
[`GUIControl.SendToBack`](GUIControl#guicontrolsendtoback)

