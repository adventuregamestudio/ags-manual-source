## `InventoryItem` functions and properties

### `InventoryItem.GetAtScreenXY`

*(Formerly known as global function `GetInvAt`, which is now obsolete)*

```ags
static InventoryItem* InventoryItem.GetAtScreenXY(int x, int y)
```

Returns the inventory item at SCREEN coordinates (X,Y). Note that this
only detects inventory items on custom Inventory windows (that are
switched on when this function is called), and is intended to allow you
to do Verb Coin style GUIs and so on.

If there is no inventory item there, or if invalid coordinates are
specified, the function returns null.

**NOTE:** The coordinates are SCREEN coordinates, NOT ROOM
coordinates. This means that with a scrolling room, the coordinates
you pass are relative to the screen's current position, and NOT absolute
room coordinates. This means that this function is suitable for use
with the mouse cursor position variables.

Example:

```ags
InventoryItem *item = InventoryItem.GetAtScreenXY(mouse.x, mouse.y);
if (item == null) {
    Display("No inventory item at the mouse coordinates");
}
else {
    Display("Inventory item number %d at the mouse.", item.ID);
}
```

will display the number of the inv item that the mouse is over

*See also:* [`InventoryItem.Name`](InventoryItem#inventoryitemname),
[`Game.GetLocationName`](Game#gamegetlocationname)

---

### `InventoryItem.GetByName`

```ags
static InventoryItem* InventoryItem.GetByName(string scriptName)
```

Returns a pointer to the InventoryItem with the specified script name, or null if it does not exist.

Normally you do not need to use this, as there will be a automatically created global script variable for each InventoryItem which got a script name.
Where GetByName() function may come useful is situation in which you a) do not know exact name, b) had to store object's reference in a string for some reason. Good examples of this are saving object's name in a [custom property](CustomProperties), or a [file](File), then reading it back.

Example:

```ags
String invItemName = Game.InputBox("Script Name of the item");
InventoryItem* item = InventoryItem.GetByName(invItemName);
if (item != null) {
    // If invItemName is an item that exists like "iKey"
    player.AddInventory(item);
}
```

In the example, we have some elements that can be used to build a debug functionality, so the player gets the wanted item by it's script name.

*Compatibility:* Supported by **AGS 3.6.1** and later versions.

*See also:* [`InventoryItem.ScriptName`](InventoryItem#inventoryitemscriptname),
[`InventoryItem.GetAtScreenXY`](InventoryItem#inventoryitemgetatscreenxy)

---

### `InventoryItem.GetProperty`

*(Formerly known as `GetInvProperty`, which is now obsolete)*

```ags
InventoryItem.GetProperty(string property)
```

Returns the custom property setting PROPERTY for the inventory item.

This command works with Number properties (it returns the number), and
with Boolean properties (returns 1 if the box was checked, 0 if not).

Use the equivalent GetTextProperty function to get a text property.

Example:

```ags
if (iStone.GetProperty("Weight") > 200)
    Display("Stone's weight is over 200!");
```

will print the message if inventory item iStone has its "Weight" property set
to more than 200.

*See also:*
[`InventoryItem.GetTextProperty`](InventoryItem#inventoryitemgettextproperty)

---

### `InventoryItem.GetTextProperty`

*(Formerly known as `GetInvPropertyText`, which is now obsolete)*<br>
*(Formerly known as `InventoryItem.GetPropertyText`, which is now
obsolete)*

```ags
String InventoryItem.GetTextProperty(string property)
```

Returns the custom property setting PROPERTY for the inventory item.

This command works with Text properties only. The property's text will
be returned from this function.

Use the equivalent GetProperty function to get a non-text property.

Example:

```ags
String description = iKey.GetTextProperty("Description");
Display("Inv item %s's description: %s", iKey.ScriptName, description);
```

will retrieve inv item iKey's "Description" property and display it.

*See also:*
[`InventoryItem.GetProperty`](InventoryItem#inventoryitemgetproperty)

---

### `InventoryItem.SetProperty`

```ags
bool InventoryItem.SetProperty(const string property, int value)
```

Sets the new *value* for the custom *property* for the specified
inventory item. Returns TRUE if such property exists and FALSE on
failure.

This command works with Number properties (it sets the numeric value),
and with Boolean properties (sets FALSE is value is equal to 0, or TRUE
otherwise).

Use the equivalent SetTextProperty function to set new text property
value.

Example:

```ags
iStone.SetProperty("Weight", 120);
```

will change Stone's "weight" custom property to 120.

*Compatibility:* Supported by **AGS 3.4.0** and later versions.

*See also:*
[`InventoryItem.SetTextProperty`](InventoryItem#inventoryitemsettextproperty)

---

### `InventoryItem.SetTextProperty`

```ags
bool InventoryItem.SetTextProperty(const string property, const string value)
```

Sets the new *value* text for the custom *property* for the specified
inventory item. Returns TRUE if such property exists and FALSE on
failure.

This command works with Text properties only. The property's text will
be changed to new value.

Use the equivalent SetProperty function to set a non-text property.

Example:

```ags
iKey.SetTextProperty("Description", "A rusty key");
```

will change key's "Description" property.

*Compatibility:* Supported by **AGS 3.4.0** and later versions.

*See also:*
[`InventoryItem.SetProperty`](InventoryItem#inventoryitemsetproperty)

---

### `InventoryItem.IsInteractionAvailable`

*(Formerly known as `IsInventoryInteractionAvailable`, which is now
obsolete)*

```ags
InventoryItem.IsInteractionAvailable(CursorMode)
```

Checks whether there is an event handler defined for activating the
inventory item in cursor mode MODE.

This function is very similar to RunInteraction, except that rather than
run the event handler script function, it simply returns *true* if
something would have happened, or *false* if unhandled_event would have
been run.

This is useful for enabling options on a verb-coin style GUI, for
example.

Example:

```ags
if (iKeyring.IsInteractionAvailable(eModeLookat) == 0)
    Display("looking at this item would not do anything.");
```

*See also:* [`IsInteractionAvailable`](Globalfunctions_General#isinteractionavailable),
[`InventoryItem.RunInteraction`](InventoryItem#inventoryitemruninteraction)

---

### `InventoryItem.RunInteraction`

*(Formerly known as `RunInventoryInteraction`, which is now obsolete)*

```ags
InventoryItem.RunInteraction(CursorMode)
```

Runs the event handler as if the player had clicked the mouse on the
inventory item, using the specified cursor mode.

Example:

```ags
if (button == eMouseLeftInv)
    inventory[game.inv_activated].RunInteraction(mouse.Mode);
```

will run the inventory event handler for the current cursor mode when
the player clicks on the item (Handle Inv Clicks needs to be enabled for
this to work)

*See also:*
[`InventoryItem.IsInteractionAvailable`](InventoryItem#inventoryitemisinteractionavailable),
[`Room.ProcessClick`](Room#roomprocessclick),
[`Character.RunInteraction`](Character#characterruninteraction)

---

### `InventoryItem.CursorHotspotX`

```ags
int InventoryItem.CursorHotspotX
```

Gets/sets the X position of hotspot on the graphic, which will be used when this inventory item is selected (see [`InventoryItem.CursorGraphic`](InventoryItem#inventoryitemcursorgraphic). The hotspot is a pixel on the cursor graphic where the click takes effect.

This is the offset into the graphic .

In practice the cursor's image is drawn with opposite, negative offset from the cursor position (which is usually defined by the mouse position).

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

*See also:* [`InventoryItem.CursorHotspotY`](InventoryItem#inventoryitemcursorhotspoty),
[`InventoryItem.CursorGraphic`](InventoryItem#inventoryitemcursorgraphic),
[`InventoryItem.Graphic`](InventoryItem#inventoryitemgraphic),
[`Mouse.ChangeModeHotspot`](Mouse#mousechangemodehotspot)

---

### `InventoryItem.CursorHotspotY`

```ags
int InventoryItem.CursorHotspotY
```

Gets/sets the Y position of hotspot on the graphic, which will be used when this inventory item is selected (see [`InventoryItem.CursorGraphic`](InventoryItem#inventoryitemcursorgraphic). The hotspot is a pixel on the cursor graphic where the click takes effect.

In practice the cursor's image is drawn with opposite, negative offset from the cursor position (which is usually defined by the mouse position).

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

*See also:* [`InventoryItem.CursorHotspotX`](InventoryItem#inventoryitemcursorhotspotx),
[`InventoryItem.CursorGraphic`](InventoryItem#inventoryitemcursorgraphic),
[`InventoryItem.Graphic`](InventoryItem#inventoryitemgraphic),
[`Mouse.ChangeModeHotspot`](Mouse#mousechangemodehotspot)

---

### `InventoryItem.CursorGraphic`

```ags
int InventoryItem.CursorGraphic
```

Gets/sets the sprite slot number of the inventory item's mouse cursor.
This is the sprite used as the mouse cursor when this inventory item is
selected.

**NOTE:** This property is only used if the "Use selected inventory
graphic for cursor" setting in General Settings is turned on.

Example:

```ags
Display("The key's cursor graphic is %d", iKey.CursorGraphic);
```

will display inventory item *iKey*'s cursor graphic.

*Compatibility:* Supported by **AGS 3.1.2** and later versions.

*See also:* [`InventoryItem.CursorHotspotX`](InventoryItem#inventoryitemcursorhotspotx),
[`InventoryItem.CursorHotspotY`](InventoryItem#inventoryitemcursorhotspoty),
[`InventoryItem.Graphic`](InventoryItem#inventoryitemgraphic)

---

### `InventoryItem.Graphic`

*(Formerly known as `GetInvGraphic`, which is now obsolete)*<br>
*(Formerly known as `SetInvItemPic`, which is now obsolete)*

```ags
int InventoryItem.Graphic
```

Gets/sets the sprite slot number of the inventory item. You could use
this with the Object.Graphic property as a means of the player
'dropping' an inventory item, or it may be useful if you want to do a
Raw Drawn inventory window.

**NOTE:** Prior to **AGS 3.6.2**, if you change this property and
the CursorGraphic was assigned the same sprite as the main Graphic,
then the CursorGraphic will be changed too automatically.
Since 3.6.2 this is no longer done, and you have to change both yourself if you need to.

Example:

```ags
int slot = player.ActiveInventory.Graphic;
```

will place the sprite number of the player's current inventory item into
slot.

*See also:*
[`InventoryItem.CursorGraphic`](InventoryItem#inventoryitemcursorgraphic),
[`InventoryItem.GetAtScreenXY`](InventoryItem#inventoryitemgetatscreenxy),
[`InventoryItem.Name`](InventoryItem#inventoryitemname)

---

### `InventoryItem.ID`

```ags
readonly int InventoryItem.ID
```

Gets the inventory item's ID number. This is the item's number from the
editor, and is useful with commands such as Character.AddInventory which
require an inventory number to add.

Example:

```ags
AddInventory(EGO, iShovel.ID);
```

uses the obsolete AddInventory command to add the shovel to EGO's
inventory

*See also:* [`Character.AddInventory`](Character#characteraddinventory),
[`Character.LoseInventory`](Character#characterloseinventory)

---

### `InventoryItem.Name`

*(Formerly known as `GetInvName`, which is now obsolete)*<br>
*(Formerly known as `SetInvItemName`, which is now obsolete)*<br>
*(Formerly known as `InventoryItem.GetName`, which is now obsolete)*<br>
*(Formerly known as `InventoryItem.SetName`, which is now obsolete)*

```ags
String InventoryItem.Name;
```

Gets/sets the name of the inventory item. This is the name which is
initially set in Inventory item's properties in the AGS Editor.

You can change this property if for example you want to change a 'bowl'
to a 'bowl with water in' but want to use the same inventory item for
it.

Example:

```ags
Display("Active inventory: %s", player.ActiveInventory.Name);
```

will display the name of the player's current inventory item.

*Compatibility:* inventory item's name was limited to 24 characters prior to **AGS 3.6.1**.

*See also:*
[`InventoryItem.GetAtScreenXY`](InventoryItem#inventoryitemgetatscreenxy),
[`InventoryItem.Graphic`](InventoryItem#inventoryitemgraphic),
[`Game.GetLocationName`](Game#gamegetlocationname)

---

### `InventoryItem.ScriptName`

```ags
readonly String InventoryItem.ScriptName;
```

Gets the script name of the inventory item, which serves as a unique identifier, as set in the AGS Editor.

This may be useful if you have a pointer to some item stored in your variable, and want to know what it actually is. Normally you don't need a script name, as you have an automatic global variable for each item in the game, but sometimes you may want to save its script name as a text either to display it somewhere for testing purposes, or keep as a reference. You may later use [InventoryItem.GetByName](InventoryItem#inventoryitemgetbyname) function to retrieve the item by the previously saved script name.

Example:

```ags
InventoryItem* activeItem = player.ActiveInventory;
if(activeItem != null) {
  Display("Active inventory item's script name: %s", activeItem.ScriptName);
}
```

Displays the script name of the player's current inventory item.

*Compatibility:* Supported by **AGS 3.6.1** and later versions.

*See also:*
[`InventoryItem.GetByName`](InventoryItem#inventoryitemgetbyname)
