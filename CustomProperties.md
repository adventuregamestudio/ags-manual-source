## Custom Properties

**What are custom properties?**

Suppose you're making a LucasArts-style game, and you want all your
hotspots to have a default event (so if the player right-clicks a
window, for example, "Open" will be the default, but if they click on a
pen, "Pick up" will be the default).

To do that, surely you would have to script in lots of code to change
the default mode over each different object?

This is where Custom Properties come to the rescue. You can create a
custom property for "Default event", for example, and then have some
simple code in the global script to use the setting accordingly.

**How do I use them?**

You'll notice that characters, objects, hotspots, rooms and inventory
items all have a "Properties" option in their property grids. Select it
and click the "..." button.

Since you don't yet have any custom properties, you'll get a blank
window, so click the "Edit Schema" button. This takes you to the Schema
Editor, where you can create the various properties you want in your
game. To begin with you are presented with an empty list box, so
right-click in it and choose "Add property".

In the "Add Property" window you can set various options about the new
property:

-   **Name** - this is the name by which you will access the property
    from the script. This cannot be changed after the property has
    been created.
-   **Description** - this is the user-friendly description which will
    be displayed in the custom property editor when you are setting
    the property.
-   **Type** - this specifies what type of property you want. Following types are supported: "Boolean", "Number" and "Text".
    Boolean type's value may be only True or False (or 1 and 0 respectively).
    "Number" can be any integer number supported by a script (from -2 billions to +2 billions).
    "Text" can have any string value.
-   **Default value** - this specifies what the default value of the
    property will be for objects where you have not set it specifically.
-   **Translated** - this tells the Editor to either include or not include the Text property type values in Translation file(s).
    This option is available since **AGS 3.6.3**. AGS 3.6.0 to 3.6.2 were always including Text properties into Translation, and any older versions did not do that at all.

For example, add a new "Boolean" property. Close the schema editor, and
then click the "Properties" button again. You'll now have a window with
a checkbox with the description text you typed in. You can click the
"Edit schema" button there to return to the schema editor if you like.

All types of game item share the same schema. That is, if you create a
"Jibble" property in the schema editor for a hotspot, it will also
appear in the properties window for characters, objects, and so on.

**Getting and setting values in the script**

To access the properties from the script, there are various script
functions. See their descriptions for how they work:

[`Character.GetProperty`](Character#charactergetproperty)<br>
[`Character.GetTextProperty`](Character#charactergettextproperty)<br>
[`Character.SetProperty`](Character#charactersetproperty)<br>
[`Character.SetTextProperty`](Character#charactersettextproperty)<br>
[`Hotspot.GetProperty`](Hotspot#hotspotgetproperty)<br>
[`Hotspot.GetTextProperty`](Hotspot#hotspotgettextproperty)<br>
[`Hotspot.SetProperty`](Hotspot#hotspotsetproperty)<br>
[`Hotspot.SetTextProperty`](Hotspot#hotspotsettextproperty)<br>
[`InventoryItem.GetProperty`](InventoryItem#inventoryitemgetproperty)<br>
[`InventoryItem.GetTextProperty`](InventoryItem#inventoryitemgettextproperty)<br>
[`InventoryItem.SetProperty`](InventoryItem#inventoryitemsetproperty)<br>
[`InventoryItem.SetTextProperty`](InventoryItem#inventoryitemsettextproperty)<br>
[`Object.GetProperty`](Object#objectgetproperty)<br>
[`Object.GetTextProperty`](Object#objectgettextproperty)<br>
[`Object.SetProperty`](Object#objectsetproperty)<br>
[`Object.SetTextProperty`](Object#objectsettextproperty)<br>
[`Room.GetProperty`](Room#roomgetproperty)<br>
[`Room.GetTextProperty`](Room#roomgettextproperty)<br>
[`Room.SetProperty`](Room#roomsetproperty)<br>
[`Room.SetTextProperty`](Room#roomsettextproperty)

**NOTE:** Calling [`ResetRoom`](Globalfunctions_Room#resetroom) will reset that
room's properties to default values, as well as that room's hotspot and
object properties.
