## Inventory Items

Inventory items represent things that a character may acquire, hold and use: a physical object, an idea, an usable skill, and so forth, depending on game mechanics that you want to have. Each character in the game has their own "inventory" which is the set of items they are carrying.

One important thing about items is that when given to a character they are copied rather than change ownership. You may think of the items as "templates", that create "instances" of themselves when added into character's inventory. Any character may possess any number of any particular item. Same kind of item may be held by multiple characters at once. This has to be kept in mind when you want to transfer an item from one character to another: just adding that item to the second character won't remove it from the first. You must do 2 operations: remove from one and add to another (order of these actions does not matter).

In summary, each inventory item has:

  * A unique script name, used to refer to this item in scripts.
  * A textual name, for describing it to a player.
  * A graphic, for displaying it on the inventory GUI.
  * A cursor graphic, for using as a cursor when the item is selected.

What can you do with an inventory item?

  * Add it to a character's inventory (see [AddInventory](Character#characteraddinventory)).
  * Display the character's inventory using a GUI (this can be done with the built-in Inventory Window, a List Box, or a custom GUI).
  * Make the item "active", selected for use by the player (see [ActiveInventory](Character#characteractiveinventory)).
  * Use the item on game entities such as hotspots, room objects, characters, and other inventory items.
  * Remove the item from a character's inventory (see [LoseInventory](Character#characterloseinventory)).

### Displaying Items

Inventory items do not have a defined "position" in game (whether in room or on screen). They can be assigned a graphic, but they are not being displayed on their own. Instead there's a special GUI control called "Inventory Window" that displays inventory contents for particular character. The way items are aligned in this control depend on control's settings. The order in which the items are displayed matches an order in which they were acquired by the character.

Alternatively, you may prefer a textual inventory, in which case you may use "List Box" control and fill it with the names of items belonging to a character.
There are also multiple ways to make a completely custom inventory, which depicts items the way you like. To give an example, you may place several buttons on GUI, and assign item graphics to these buttons.

Inventory item has a optional separate "cursor graphic" property, which is used to display that item on a mouse cursor. This happens when this item is "selected" by a player character, and cursor is switched to "use item" mode. Selecting an item is done by by assigning such item to the player character's [ActiveInventory](Character#characteractiveinventory) property. A character may "select" only an item which it already has in its inventory.

### Using the Item on something else

When the inventory item is selected as "active item" and cursor is set to "use item" mode, then interacting other game object with the cursor will trigger that object's "Use Inventory On" event. Types of objects that have this event are: characters, room objects, hotspots and inventory items themselves.

If you attach a script function to such event, that function will run when the event triggers. Typically you would write this function so that it checks which is the player's active item, and reacts accordingly.

Everything said above describes the built-in behavior, but you may script your custom one, if you require a non-standard way of using items. The contents of character's inventory may be accessed using [Character.InventoryQuantity](Character#characterinventoryquantity) indexed property, the presence of particular item tested using [Character.HasInventory](Character#characterhasinventory) function.
