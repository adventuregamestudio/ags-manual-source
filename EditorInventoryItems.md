## Inventory Items Editor

A place to edit Items that can be added to a character's [inventory](Settingupthegame#inventory) by the player.

![Screenshot Inventory Item Editor](images/EditorInventoryItems_img1.png)

### Cursor Image

The image number from the [Sprite Manager](EditorSprite) that is displayed as the active cursor image when selecting this inventory item from the inventory. This could be to use it on another inventory item or to use it somewhere inside your room. You can also set the active hotspot of the mouse-cursor image, which is where the click from the mouse cursor will be registered when using this image. This can be done by left clicking on the mouse-cursor image itself, or by entering the appropriate into the HotspotX and HotspotY properties (see below).

It is generally good practice to add some kind of crosshair or a single pixel marker in an easy to spot color to the image and align the click hotspot to this. This way the player should be able to tell exactly where they are pointing. Figuring out bad controls is no puzzle!

### Description

The name of the item. This is also the value used by the `@OVERHOTSPOT@` token when the mouse cursor is hovering over this item, whilst still in the inventory.

### Image

The image displayed for the inventory item within the character's inventory. This is typically larger than the mouse cursor image of the inventory item, depending on how the GUI that is used for the inventory window has been configured.

### HotspotAlignment.

This property is available since **AGS 3.6.3**.

Select the automatic alignment for the hotspot of the item's cursor image. The purpose of this setting is to keep the hotspot's relative position when the cursor image changes (whether when editing item in the Editor, or switching in script). HotspotX and HotspotY work as offsets from the aligned position.

Note that prior to **AGS 3.6.3** the only supported alignment was "middle-centered" and it was achieved by setting both HotspotX and HotspotY to 0.

### HotspotX

Select the X offset value for the hotspot of the active cursor image. It's the distance from the position defined by HotspotAlignment, measured in pixels. You can also set this value by clicking on the "Mouse cursor image" display.

Note that prior to **AGS 3.6.3** the X offset was always counted from the left edge of the sprite.

### HotspotY

Select the Y offset value for the hotspot of the active cursor image. It's the distance from the position defined by HotspotAlignment, measured in pixels. You can also set this value by clicking on the "Mouse cursor image" display.

Note that prior to **AGS 3.6.3** the Y offset was always counted from the top edge of the sprite.

### ID

The internal ID of this inventory Item. This value cannot be changed, it can be read only by the script property [`InventoryItem.ID`](InventoryItem#inventoryitemid).

### Name

The script name of the inventory item. Usually the convention is to start inventory item names with an 'i', so a key might be named `iKey`. This convention sure gets tricky when you give your game characters some Apple products...

### PlayerStartsWithItem

This is a boolean value which sets whether the player character starts with this inventory item in their inventory or not. In the majority of cases, where items are collected during gameplay, the default setting of no is probably the one to use.

### Properties

Configure any [custom properties](CustomProperties) for this inventory item. A common use is to use an additional boolean flag to indicate that an inventory item should be 'used' by itself, without needing to combine it with another inventory item.

<hr>

Within the game the player can pick up and store inventory items into his inventory by interacting with the scene or other characters. You add a new inventory item to his inventory with the [`cChar.AddInventory(iInvItem)`](Character#characteraddinventory) function. When the player uses or gives away the item you remove it from the character's inventory with the [`cChar.LoseInventory(iInvItem)`](Character#characterloseinventory) function. Make sure you check the [`Mouse.Mode`](Mouse#mousemode) afterwards, especially when the active cursor is the inventory item the player just lost.

*See also:* [`InventoryItem`](InventoryItem)
