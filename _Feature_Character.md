## Characters

Characters are meant to represent personalities in your game. But they might as well be used for other purposes, such as a replacement for [Room Objects](_Feature_Rooms#room-objects) when you need them to do more than Room Object can do.

Characters are global entities, which may freely travel between rooms. When in room they have position in their current room's coordinate system, visual image and properties. They may be moved around, have their looks changed, and may be interacted with by the player. Besides the above, they have several additional behaviors: walking, speaking, idling and inventory. We'll discuss these below.

See Also: [Character Editor](CharacterEditor), [Character functions and properties](Character)

### Player Character

An AGS game requires to have at least one character set as "player character". You set your starting player character in the editor. Only one character may be declared "player character" at the same time, but it's possible to switch to another character during the game using [Character.SetAsPlayer](Character#charactersetasplayer) script function. Having a character as "player character" has 3 effects.

First, engine always displays a room which has the player character in. Switching rooms is done by moving player character to another room using [Character.ChangeRoom](Character#characterchangeroom) script function. You may hide the character using appropriate method (like making it invisible, or moving offscreen), but it always have to be present in a room if you'd like to have that room loaded and active on screen.

Second, the game camera is centering and following the player character by default (this may be disabled in script, see [Camera.Autotracking](Camera#cameraautotracking)).

Third, the predefined script variable "player" will always refer to the currently set player character. This variable is very useful, because it lets you to issue commands to the character without having to figure out which character is currently being a protagonist. This is convenient in a game with multiple protagonists which are meant to share same controls, appear in the same rooms, interact with the same objects and so on.

### Non-Player Chararacters

All the characters which are not "player character" (aka NPCs) may perform exact same actions as the protagonist, with the exception of not causing current room to change when they are moved to another room. There's an interesting trick: a NPC may even be moved to a non-existing room (e.g. room -1). That's one of the ways to hide them from the play. Note though that majority of character actions won't work when they are in non-active room.

### Character Views

Unlike Room Objects and other simple entities Characters do not have an explicit "Graphic" property which could be set. Instead they have Views assigned to them. A "View" is a collection of animations, divided into "Loops", where one Loop is a animation sequence. A character has a number of predefined slots for Views, each meant for the certain behavior, that are run automatically depending on character's current state or action:

  * Normal View, also known as "walking view", - is a default view which character uses when it's just standing still or walking around. This view is supposed to have walking animations in its loops, each starting with a single standing frame.
  * Speech View - is assigned to give the character some speaking animation. Depending on a game's set speech style, this may be either a animation of character itself (this is known as "Lucas-Arts" style), or its close-up portrait (known as "Sierra-style").
  * Blink View - an optional view which is applied on top of the portrait Speech View and lets add additional "movements" (like blinking eyes) to the character's face both when it's speaking and done speaking.
  * Think View - is a less common optional kind of view, which lets to animate character differently when the [Think](Character#characterthink) function is called in script.
  * Idle View - an optional view which animations are run periodically when the character is not doing anything for a period of time.

What's important is that when a View is assigned to one of these slots it starts to be used as a "directional view". Any character has a direction it faces when standing or walking, and the Normal View plays animations from the loops corresponding to the character's facing direction. There are 4 default directions (Left, Right, Up and Down), and 4 diagonal ones which are optional and are only used if the Character has "Use Diagonal Loops" set in the editor.

Idle View also can use directional loops and play different idling animation depending on where the character is looking at.

Speech, Blink and Think Views only use directional loops if the game has "Lucas-Arts" speech style, in which case they animate character's body. But in other speech styles which display a portrait only the first loop of the View is ever used.

**NOTE:** Views are not *owned* by a character. A single View may be assigned to multiple characters at once (and few other types of objects), and even to multiple "slots" in a character (e.g. as both Speech View and Think View).

See Also: [Character Editor](CharacterEditor), [View Editor](ViewEditor)

### Custom Views and Animating

Besides the standard set of Views, Character may be assigned any view for performing a custom animation. This is done by a script function [LockView](Character#characterlockview) followed by a call to function [Animate](Character#characteranimate). When this view is no longer needed, it's unassigned using [UnlockView](Character#characterunlockview), and character returns to its normal looks.

The common use for this is to animate any character actions not covered by default views, such as: picking items up, opening doors, jumping, performing gestures, and so forth. But you may also use custom animate function as a substitute for standard views as well (walking, speech, etc), if their default behavior is not suiting you.

See Also: [Character.LockView](Character#characterlockview), [Character.Animate](Character#animate), [Character.UnlockView](Character#characterunlockview), [View Editor](ViewEditor)

### Moving and Walking

There are two kinds of movement in the engine, which are called "Moving" and "Walking". "Moving" is simply changing position over time, it may be done by a script function [Character.Move](Character#charactermove) or by manually setting [x and y](Character#characterx) character properties.

Walking is moving with default animation. It's performed by a function [Character.Walk](Character#characterwalk), and this will utilize the character's Normal View to animate character. Engine will switch to the view's loop depending on current character's movement direction, and play it while the character is walking towards same direction. If character turns to another direction while following the movement path, it will switch to another respective animation loop.

Note that you may still play an animation while *moving* a character if using LockView and Animate functions. This comes handy in case you need custom movement animation instead of default walk.

Both walking and moving may be done while respecting or ignoring Walkable Areas, this is controlled by passing a proper argument to the Move or Walk function. When ignoring walkable areas, the character will always move directly to destination. When using walkable areas, the engine will run a pathfinding operation first, in order to find a path to destination along the permitted area.

There are couple of additional settings related to how the walking is performed.

"Movement linked to animation" is perhaps one of the most important option one has to consider when configuring character's walk. When this option is enabled the character will only be performing move steps whenever an animation frame advances. On one hand, this establishes a strong connection between animation and movement,  helping to avoid the "gliding" effect when the character moves are disregarding its visible stance. On another hand, it may be seen as "jerky", especially when the camera moves along with the character. There is no best answer here, and this has to be decided to suit your preference and visual game design.

"Turn when moving" option (set per character in the editor) makes character animate a turn in place step by step instead of an immediate switch to the new direction.

**NOTE:** the character does not have to be visible to move or walk. You may turn character invisible by e.g. setting its [Transparency](Character#charactertransparency) property to 100%, and any moving command will still work. Such trick may be used to create an invisible "reference point" that something else, such as another character, should follow.

See Also: [Character.Move](Character#charactermove), [Character.Walk](Character#characterwalk), [Feature: Walkable Areas](Feature_Room#walkableareas)

### Speaking

Characters can be ordered to "speak" by a script function [Character.Say](Character#charactersay), and few other similar functions. Speaking creates three effects, some of which are optional:

 * Displays a text on screen. The text's looks depend on the game's speech style.
 * Animates the character, using its Speech View. This animation is played either over the character's body or by displaying a closeup portrait, according to the speech style.
 * Plays a voice-over.

All three effects are optional and there are conditions under which they will not be present:

 * Text is the most common thing, but it may be hidden if voice-over is present and game is configured to only play voice-over but not show text.
 * Animation will not play if the character does not have any Speech View assigned.
 * Voice-over will not play if it's not defined for this particular speech line (or not available in this game at all).

All those three effects may be achieved by scripting as well, and that's how custom speech styles can be created in AGS.

**NOTE:** the character itself does not have to be seen on screen in order to do speech, it just has to be in the active room, but it may as well be invisible or offscreen. One trick that you may do with an invisible character is to position it at an arbitrary place in the room and make it speak, which will have effect of a speech appearing over that place.

See Also: [Character.Say](Character#charactersay), [Character.Think](Character#characterthink), [Dialog Script](DialogScript)

### Idling

Characters have a setting that tells how long to wait before it enters idling state and animates using an assigned Idle View. After a single animation idle timer resets and character waits again. This timer is also reset whenever character does something else, like walking or speaking.
The idle timer setting may be set to zero, in which case character will be permanently running its "idle" animation, while standing still. This may be used as a kind of a "standing" animation.

See Also: [Character.SetIdleView](Character#charactersetidleview)

### Inventory

Each character in the game has inventory, which is a collection of items that the character possesses. The items may be added and removed from character's inventory at any time using script functions [AddInventory](Character#characteraddinventory) and [LoseInventory](Character#loseinventory). The way inventory items work in AGS, an item is not a unique entity, and when it is given to a character, what is given is an "instance" (a copy) of an item. A character may hold multiple instances of the same item, and multiple characters may hold an instance of same item. This has to be kept in mind when you want to transfer an item from one character to another: just adding that item to the second character won't remove it from the first. You must do 2 operations: remove from one and add to another (order of these actions does not matter).

Each character may have a "selected item", in script this is depicted by [ActiveInventory](Character#characteractiveinventory) property. When this property is set for the player character the cursor mode "use item" displays that item's graphic. This property does not have much meaning for the NPCs though.

See Also: [Feature: Inventory](_Feature_InventoryItems)
