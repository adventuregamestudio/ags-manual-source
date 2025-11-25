## Cursor Editor

The Cursors node of the editor shows you the cursor modes available in the game. These cursor modes are also essentially interaction modes, which may or not have respective visual appearance. Each cursor mode either performs a certain action within the game, or used to represent certain game state (such as "Wait" cursor), so they have several uses.

In the previous versions of AGS the first several cursors have always had fixed roles. This is no longer the case in AGS 4.0, where each cursor is freely customizable. However there's still a list of built-in "Standard Roles" available for assignment, in case you'd like to utilize one of the built-in cursor behaviors provided by the engine.

While in game the cursor's action may be run in one of the three ways:
- Calling [`Room.ProcessClick`](Room#roomprocessclick) function in script.
- Calling `RunInteraction` function for one of the game objects (Character, Room Object, Hotspot or Inventory Item).
- If cursor has one of the "standard roles" assigned, then under some circumstances clicking left or right mouse button will cause its action to trigger.

For the most part, it's advised to trigger cursor actions in script, because that way you'll have most control over what they do.

Back to editing the cursors: right clicking on the "Cursors" node itself allows you to create a new cursor. Creating a new cursor or double clicking an existing one will open it for edit.

The Cursor editor pane allows to preview the cursor's graphic, set its "hotspot" (the point on the cursor's graphic which triggers interaction with underlying objects), as well as configure other properties.

The "StandardMode" option in the property grid tells AGS that this is a "normal" cursor mode. In practice this means that this mode may be switched to when "cycling" through cursor modes using [`Mouse.SelectNextMode`](Mouse#mouseselectnextmode) and [`Mouse.SelectPreviousMode`](Mouse#mouseselectpreviousmode) script functions. Also, when the current mode gets disabled for any reason, then the game will switch to the next enabled standard mode. Note that if you assign a "Use inventory" role to a cursor, then such mode cannot be selected unless the player character has active inventory set (see [`Character.ActiveInventory`](Character#characteractiveinventory)), and gets switched out whenever active inventory is reset.

The "StandardRole" property lets you assign one of the standard built-in interaction modes to this cursor. Such assignment will enable certain automated behavior. Following standard roles are available:
- Role: Walk. Calling ProcessClick with the "walk" mode will command player character to perform a non-blocking walk to the selected point in the room, which is equivalent to calling [`Character.Walk`](Character#characterwalk) function with `eNoBlock` and `eWalkableAreas` parameters. The cursor with "Walk" role will be disabled whenever current Room has a "Show player character" option disabled.
- Role: Look at. If the game has "Handle inventory clicks in script" *disabled*, then right-clicking on an inventory item will trigger an interaction with "Look at" role. Otherwise, this role has no effect.
- Role: Interact. If the game has "Handle inventory clicks in script" *disabled*, then left-clicking on an inventory item will select that item (make it active). Otherwise, this role has no effect.
- Role: Use inventory. Whenever there's a active inventory item set, this cursor mode will display a special image constructed from the active item's "cursor graphic". The mode with this role cannot be selected unless player character has an active inventory item.
- Role: Pointer. The mode with this role will be used during dialog options, and also few built-in gui dialogs (such as built-in save, restore and quit dialogs).
- Role: Wait. This mode will be displayed whenever there's a blocking action or wait run in game. Its purpose is to indicate that the game interface is temporarily disabled.

"Create Event" option lets you add a interaction event associated to this cursor mode. Such event will be available in each Character, Room Object, Hotspot and Inventory Item's event table. The interaction event allows to connect a script function which is going to be run whenever a game object is being interacted with this cursor mode.

"Event Function Name" defines a name used when editor creates a script function for the interaction event. If it's not set, then the cursor mode's name will be used instead.

"Event Label" defines a event's title displayed in the events table. If it's not set, then the cursor mode's name will be used instead.

The "Animate" option allows you to specify that the mouse cursor will animate while it is on the screen.

"AnimateOnlyOnHotspots" option means that the cursor will animate only when over something (hotspot, object or character)

The "AnimateOnlyWhenMoving" box allows you to do a QFG4-style cursor, where it only animates while the player is moving it around.

"Animation Delay" is a extra delay between frames for this cursor's animation.

"Image" property lets you select the static graphic for this cursor, used when it's not animating.

"View" property lets you choose a view number for the cursor animation, and the cursor will animate using the first loop of that view.

"Hotspot X and Y" options let you specify a relative point on the cursor's graphic, which triggers interaction. This point will match the mouse device position on screen, while cursor's image will be drawn with certain offset to that position.
