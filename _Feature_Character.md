## Characters

Characters are meant to represent personalities in your game. But they might as well be used for other purposes, such as a replacement for [Room Objects](Feature_Rooms#room-objects) when you need them to do more than Room Object can do.

Characters are global entities, which may freely travel between rooms. When in room they have position in their current room's coordinate system, visual image and properties. They may be moved around, have their looks changed, and may be interacted with by the player. Besides the above, they have several additional behaviors: walking, speaking, idling and inventory. We'll discuss these below.

### Player Character

An AGS game requires to have at least one character set as "player character". You set your starting player character in the editor. Only one character may be declared "player character" at the same time, but it's possible to switch to another character during the game using [Character.SetAsPlayer](Character#charactersetasplayer) script function. Having a character as "player character" has 2 effects.

First, engine always displays a room which has the player character in. Switching rooms is done by moving player character to another room using [Character.ChangeRoom](Character#characterchangeroom) script function. You may hide the character using appropriate method (like making it invisible, or moving offscreen), but it always have to be present in a room if you'd like to have that room loaded and active on screen.

Second, the predefined script variable "player" will always refer to the currently set player character. This variable is very useful, because it lets you to issue commands to the character without having to figure out which character is currently being a protagonist. This is convenient in a game with multiple protagonists which are meant to share same controls, appear in the same rooms, interact with the same objects and so on.

### Non-Player Chararacters

All the characters which are not "player character" (aka NPCs) may perform exact same actions as the protagonist, with the exception of not causing current room to change when they are moved to another room. There's an interesting trick: a NPC may even be moved to a non-existing room (e.g. room -1). That's one of the ways to hide them from the play. Note though that majority of character actions won't work when they are in non-active room.

### Character Views

Unlike Room Objects and other simple entities Characters do not have an explicit "Graphic" property which could be set. Instead they may have Views assigned to them. A "View" is a collection of animations, divided into "Loops", where one Loop is a animation sequence. A character has a number of predefined slots for Views, each meant for the certain behavior, that are run automatically depending on character's current state:

  * Normal View, also known as "walking view", - is a default view which character uses when it's just standing still or walking around. This view is supposed to have walking animations in its loops, each starting with a single standing frame.
  * Speech View - is assigned to give the character some speaking animation. Depending on a game's set speech style, this may be either a animation of character itself (this is known as "Lucas-Arts" style), or its close-up portrait (known as "Sierra-style").
  * Blink View - an optional view which is applied on top of the portrait Speech View and lets add additional "movements" (like blinking eyes) to the character's face both when it's speaking and done speaking.
  * Think View - is a less common optional kind of view, which lets to animate character differently when the [Think](Character#characterthink) function is called in script.
  * Idle View - an optional view which animations are run periodically when the character is not doing anything for a period of time.

What's important is that when a View is assigned to one of these slots, it starts to be interpreted and used as a "directional view". As a character has a direction it faces when standing or walking, the Normal View plays animations from the loops corresponding to the character's facing direction. There are 4 default directions (Left, Right, Up and Down), and 4 diagonal ones which are optional and are only used if the Character has "Use Diagonal Loops" set in the editor.

Idle View also can use directional loops and play different idling animation depending on where the character is looking at.

Speech, Blink and Think Views only use directional loops if the game has "Lucas-Arts" speech style, in which case they animate character's body. But in other speech styles which display a portrait only the first loop of the View is ever used.

**NOTE:** Views are not *owned* by a character. A single View may be assigned to multiple characters at once (and few other types of objects), and even to multiple "slots" in a character (e.g. as both Speech View and Think View).

### Custom Views and Animating

Besides the standard set of Views, Character may be assigned any view for performing a custom animation. This is done by a script function [LockView](Character#characterlockview) followed by a call to function [Animate](Character#characteranimate). When this view is no longer needed, it's unassigned using [UnlockView](Character#characterunlockview), and character returns to its normal looks.

### Moving and Walking

There are two kinds of movement in the engine, which are called "Moving" and "Walking". "Moving" is simply changing position over time, it may be done by a script function [Character.Move](Character#charactermove) or by manually setting [x and y](Character#characterx) character properties.

Walking is moving with animation. It's performed using function [Character.Walk](Character#characterwalk), and uses this character's Normal View to animate character. Engine will switch to the view's loop depending on current character's movement direction, and play it while the character is walking towards same direction. If character turns to another direction, following the movement path, it will switch to another animation loop.

TBD:
- turn when moving/facing
- movement linked to animation

### Speaking

TBD

### Idling

TBD

### Inventory

TBD
