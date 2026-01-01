## Event Types

In AGS certain types of game objects may have individual events, that is - events that are run for a *particular object* when something happens to it.
These events do not have predefined functions, unlike [Global event handlers](Globalfunctions_Event). Instead each object has an event table, where you may connect a script function to some event.

In the Editor the event tables may be found in the "Events" section of the Properties Window (when clicking the lightning bolt icon).

It's important to note that each event type corresponds to its own function type, which defines function arguments. To give an example, Button's OnClick event demands a function that looks like:

    function ButtonName_Click(GUIControl *control, MouseButton button)

If two or more events have same function type, then it is also possible to connect one function to all of them. Similarily, it is possible to connect one function to events from multiple objects. This may be useful in case you prefer to have a generic behavior for them. In such case the function arguments may usually be used to tell which object and which event called the function.

For example, if multiple Buttons are connected to the same Click function, mentioned above, then you may use `control` argument to refer to the button that was clicked, and `button` argument to know which mouse button performed the click.

Following is a list of events per game object type.

---

### Room events

**Enters room before fade-in**

    function room_Load()

Occurs just after the room is loaded into memory. This event occurs
every time the player enters the screen, and it happens BEFORE the
screen has faded in, which allows you to change object graphics and do
other things to the screen which the player won't notice.

**NOTE:** This event is ONLY meant for adjusting things such as object and
character placement. Do NOT use this event for any sort of automated
intro to the room - use the "Enters Room After Fade In" event for that
instead.

**Enters room after fade-in**

    function room_AfterFadeIn()

Occurs every time the player enters the room, AFTER the screen has
faded-in. Suitable for playing cutscenes.

**First time enters room**

    function room_FirstLoad()

Occurs the first time the player enters the room. This event occurs
AFTER the screen has faded in, and right before the "Player enters room (after fade-in)" event.

**Leaves room before fade out**

    function room_Leave()

Occurs when the player leaves the screen, just BEFORE the screen fades
out.

**Leaves room after fade out**

    function room_Unload()

Occurs when the player leaves the screen, AFTER the screen has faded-out.
This event is suitable for stopping room music and deleting temporary resources, such as Dynamic Sprites.

**Repeatedly execute**

    function room_RepExec()

Occurs repeatedly on every game cycle, at a frequency defined by the game speed (for example if game speed is 40, then this event will occur 40 times per second, about every 25 milliseconds). This event is a equivalent of a global `repeatedly_execute` callback, as `repeatedly_execute` is not run in room scripts.

**Walk off left edge**

    function room_LeaveLeft()

occurs when the player character walks off the left Room Edge.

**Walk off right edge**

    function room_LeaveRight()

occurs when the player walks off the right Room Edge.

**Walk off bottom edge**

    function room_LeaveBottom()

occurs when the player character walks off the bottom Room Edge.

**Walk off top edge**

    function room_LeaveTop()

occurs when the player character walks off the top Room Edge.

---

### Character events

**Look at character**

    function cChar_Look(Character *theCharacter, CursorMode mode)

Occurs when the player clicks on a character while in the "look" mode.

**Interact character**

    function cChar_Look(Character *theCharacter, CursorMode mode)

Occurs when the player clicks on a character while in the "interact"
mode.

**Talk to character**

    function cChar_Talk(Character *theCharacter, CursorMode mode)

Occurs when the player clicks on a character while in the "talk" mode.

**Pick up character**

    function cChar_PickUp(Character *theCharacter, CursorMode mode)

Occurs when the player clicks on a character while in the "pick up" mode.

**Use inventory on character**

    function cChar_UseInv(Character *theCharacter, CursorMode mode)

Occurs when the player uses an inventory object on a character. This
event could be used to allow the player to give items to characters.
You can use the [`player.ActiveInventory`](Character#characteractiveinventory) property
to distinguish which item they used.

**Usermode1 character**

    function cChar_Mode8(Character *theCharacter, CursorMode mode)

Occurs when the player clicks on a character while in the "usermode1" mode.
    
**Usermode2 character**

    function cChar_Mode9(Character *theCharacter, CursorMode mode)

Occurs when the player clicks on a character while in the "usermode2" mode.

**Any click on character**

    function cChar_AnyClick(Character *theCharacter, CursorMode mode)

Occurs when the player clicks on the character in any mode. It's run right after the specific mode event, except if there was a room change command.
This allows you to process custom modes like smell, taste, push, pull, and so on.

---

### Object events

**Look at object**

    function cObject_Look(Object *theObject, CursorMode mode)

Occurs when the player clicks on a object while in the "look" mode.

**Interact object**

    function cObject_Look(Object *theObject, CursorMode mode)

Occurs when the player clicks on a object while in the "interact"
mode.

**Talk to object**

    function cObject_Talk(Object *theObject, CursorMode mode)

Occurs when the player clicks on a object while in the "talk" mode.

**Pick up object**

    function cObject_PickUp(Object *theObject, CursorMode mode)

Occurs when the player clicks on a object while in the "pick up" mode.

**Use inventory on object**

    function cObject_UseInv(Object *theObject, CursorMode mode)

Occurs when the player uses an inventory object on a object.
You can use the [`player.ActiveInventory`](Character#characteractiveinventory) property
to distinguish which item they used.

**Usermode1 object**

    function cObject_Mode8(Object *theObject, CursorMode mode)

Occurs when the player clicks on a object while in the "usermode1" mode.
    
**Usermode2 object**

    function cObject_Mode9(Object *theObject, CursorMode mode)

Occurs when the player clicks on a object while in the "usermode2" mode.

**Any click on object**

    function cObject_AnyClick(Object *theObject, CursorMode mode)

Occurs when the player clicks on the object in any mode. It's run right after the specific mode event, except if there was a room change command.
This allows you to process custom modes like smell, taste, push, pull, and so on.

---

### Hotspot events

**Look at hotspot**

    function cHotspot_Look(Hotspot *theHotspot, CursorMode mode)

Occurs when the player clicks on a hotspot while in the "look" mode.

**Interact hotspot**

    function cHotspot_Look(Hotspot *theHotspot, CursorMode mode)

Occurs when the player clicks on a hotspot while in the "interact"
mode.

**Talk to hotspot**

    function cHotspot_Talk(Hotspot *theHotspot, CursorMode mode)

Occurs when the player clicks on a hotspot while in the "talk" mode.

**Pick up hotspot**

    function cHotspot_PickUp(Hotspot *theHotspot, CursorMode mode)

Occurs when the player clicks on a hotspot while in the "pick up" mode.

**Use inventory on hotspot**

    function cHotspot_UseInv(Hotspot *theHotspot, CursorMode mode)

Occurs when the player uses an inventory hotspot on a hotspot.
You can use the [`player.ActiveInventory`](Character#characteractiveinventory) property
to distinguish which item they used.

**Usermode1 hotspot**

    function cHotspot_Mode8(Hotspot *theHotspot, CursorMode mode)

Occurs when the player clicks on a hotspot while in the "usermode1" mode.
    
**Usermode2 hotspot**

    function cHotspot_Mode9(Hotspot *theHotspot, CursorMode mode)

Occurs when the player clicks on a hotspot while in the "usermode2" mode.

**Any click on hotspot**

    function cHotspot_AnyClick(Hotspot *theHotspot, CursorMode mode)

Occurs when the player clicks on the hotspot in any mode. It's run right after the specific mode event, except if there was a room change command.
This allows you to process custom modes like smell, taste, push, pull, and so on.

**Stands on hotspot**

    function hHotspot_WalkOn(Hotspot *theHotspot)

Occurs repeatedly while the player character is standing on the hotspot.

**Mouse moves over hotspot**

    function hHotspot_MouseMove(Hotspot *theHotspot)

Occurs repeatedly while the mouse cursor is over the hotspot. You can
use this to highlight the cursor, and for other various effects.

---

### Inventory item events

**Look at inventory item**

    function iInvItem_Look(InventoryItem *theItem, CursorMode mode)

Occurs when the player clicks on the inventory item while in the "look"
mode.

**Interact inventory item**

    function iInvItem_Interact(InventoryItem *theItem, CursorMode mode)

Currently, because the Interact mode selects the inventory item, this
event can only be triggered by manually calling the
InventoryItem.RunInteraction script function (i.e. you have to use the
Handle Inv Clicks in Script option).

**Talk to inventory item**

    function iInvItem_Talk(InventoryItem *theItem, CursorMode mode)

Only applies to the LucasArts-style inventory, occurs when the player
clicks the Talk icon on the inventory item.

**Use inventory on this item**

    function iInvItem_UseInv(InventoryItem *theItem, CursorMode mode)

Occurs when the player uses another inventory object on this one. You
can use the
[`player.ActiveInventory`](Character#characteractiveinventory) property
to distinguish which item they used.

This event allows the player to combine items, and so on. For example,
if they had picked up a laptop computer and a battery separately, then
you could use this to allow them to insert the battery into the
computer.

**Other click on inventory**

    function iInvItem_OtherClick(InventoryItem *theItem, CursorMode mode)

Occurs when the player clicks *any other* cursor mode (apart from the ones listed above) on the item.

---

### Region events

**Walks onto region**

    function region_WalksOff(Region *theRegion)

Occurs when the player moves from another region onto this one. Will
also activate on whichever region they start on when they enter the
screen.

**Walks off region**

    function region_WalksOnto(Region *theRegion)

Occurs when the player leaves the current region. Does not occur if they
go to a different room.

**While standing on region**

    function region_Standing(Region *theRegion)

Occurs repeatedly while the player character stands on this region.
