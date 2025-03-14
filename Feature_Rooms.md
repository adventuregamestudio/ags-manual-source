## Rooms

In AGS Rooms are logical parts of the game. A Room may play various roles depending on the game's genre and your needs: it may represent a physical location in a game's world, a world's map or a closeup of an object, a mini game scene, just a placeholder for displaying something else on screen (such as a title screen, main menu or end credits).

A Room may be fixed in its looks and composition, or have its state changed over time completely with the use of scripting. For example, you could have a room showing a game's title, and then scroll sideways to the gameplay position. Or have same Room depict multiple similar locations, by toggling decorations and backdrops. Or depict same location but in different times of a day, and so on.

Rooms are identified simply by a number, ranging from 0 to 999 (number 0 is traditionally not used, but it's still available).

A Room may be "state-saving" and "not state-saving". The former keep their state when you leave them and have it restored when you enter them again. The latter have their state reset to initial setup when they are revisited.

The way it currently works in AGS, Room numbers 0-300 are defined as state-saving, and Room numbers 301-999 are non-state-saving. This makes AGS support up to 1000 rooms in a single game, 301 of them save their state and the rest 699 not.

Non-state-saving rooms help to save on memory, and also they guarantee that upon entering them their state will be exactly the same as you set it in the Editor each time player revisits this room. It's a good practice to have rooms serving as title screens, menus and similar be non-state-saving, because these usually don't need to be restored. Same goes to rooms which you know will only be visited once during a playthrough, such as cutscenes, for instance.

See Also: [Tutorial: Creating your first room](acintro2)

### Changing Rooms

Engine can have only a single Room loaded and active at the same time. The active Room is defined by the player character location. When the game is launched it first loads the room assigned as a player character's "starting room". Changing rooms is done by moving a player character elsewhere using [Character.ChangeRoom](Character#characterchangeroom) script function.

Naturally, only one character is defined as "playable" at any given time. If you assign another character as playable using a script command (see [Character.SetAsPlayer](Character#charactersetasplayer)), and that character happens to be in another room, then the rooms will get changed automatically.

### Displaying Rooms

A Room has a size, it's defined by the size of Room's first background image. Room's coordinate system has a center (0,0) point at its background's top-left corner, X axis pointing from left to right and Y axis pointing from top to bottom, so the right-bottom point of the Room has coordinates (Width - 1, Height - 1).

Only Room Objects and Characters positioned within these coordinates are guaranteed to be visible, although technically they may take positions outside of the room's bounds too. Having objects outside of the room background is one way to keep them out of sight, or prepare them to enter the scene. For instance, it's possible to place a Character at negative coordinates, or at positive coordinates exceeding room's Width or Height on room's "load" event prior to "fade-in", and then on the "after fade-in" event make that character walk onto the room background as a small cutscene. It is similarly possible to make Characters walk outside of the room background before transitioning to another room.

Speaking of displaying a room on screen, AGS uses a concept of Room's Camera and Viewport. You may think of a Camera as of an "eye" located inside the Room and showing it whole or a part of its area, while Viewport is a "monitor" that displays what Camera can see on screen. By default both Camera and the Viewport are automatically set to either the Game's resolution or Room's size - whatever is smaller.

What that means, for example, if the Game's resolution is 320x200, and room is 320x200, then the room will occupy whole game screen. If room is 160x100, then it will occupy only portion of the game screen. In both of these cases the room will be fully seen on screen at all times.

However, if the room is larger than the Game's resolution, say it's 640x200, the room's Viewport will still be restricted by the game's resolution - 320x200, and only half of the room will be visible at once. As the camera moves around the room, different parts of it will be shown on screen. Rooms larger than the game are traditionally called "scrolling rooms", because when the camera moves it looks like the room is scrolling before player's eyes.

By default the camera is centered at player character. When player character walks around the room, the camera will follow as far as it can without crossing the room's bounds. There are ways to control the camera using [relevant script commands](Camera).

See Also: [Camera functions and properties](Camera), [Viewport functions and properties](Viewport)

### Room contents

Besides their basic settings, rooms may have following contents in them:

* Backgrounds
* Edges
* Room Objects
* Hotspots
* Walkable areas
* Regions
* Walk-behinds

See Also: [Room Editor](EditorRoom)

### Room Backgrounds

Backgrounds are images that are displayed at the back of the room. Room must have at least a single background (this is called "main background"), but it does not have to be an actual image, and may be just a bitmap filled with a black color, for instance. It's still necessary to have one, as main background defines the room's size: that is the area that will ever be visible on screen.

It's possible to add up to 5 backgrounds total for a room. The extra backgrounds are commonly meant for two purposes:
1. Displaying different variants of the room. This may be used, for example, to have day and night versions of the same room.
2. Animating room background. That is known as a full room animation, and usually used when the whole background must change as one piece.

Backgrounds are limited in what you may achieve with them alone, not only because their max number is small (5), but also because they are just flat images.
For complex room transformations and visual effects (such as parallax, for instance) one would use a combination of background and multiple room objects, moving and animating simultaneously.

### Room Edges

Room Edges are four lines that define room's playable area (Left, Right, Top and Bottom edges). These are optional, and may be ignored if not needed.
Their purpose is to detect when player character crosses them, in which case they trigger a respective [room event](EventTypes#room-events): "Walk off left edge", "Walk off right edge", and so on. This is commonly used to script exits to other rooms.

One downside of using a Room Edge is that it is a straight line running across the whole room, and will trigger regardless of which point character crosses it at. If the room has a non-trivial layout, then Hotspots or Regions would be more suitable for this purpose.

See Also: [Room events](EventTypes#room-events)

### Room Objects

Room Objects are things in room that have their own position, image, and few other visual properties, and may be interacted with by the player. They may be moved, change their graphic and animate, be turned on and off. They are suitable to represent both static and dynamic things in the room, from decorations to simple characters (although Objects are more limited in what they can do if compared to actual [Characters](Feature_Characters)).

Room Objects cannot change the owning room, they will always stay in one they are created in.

Room Objects may be commanded to move using pathfinding (see also [Walkable areas](Feature_Rooms#walkable-areas)), or freely. You may also just set the new position directly using their X and Y properties. Objects may be marked as "solid", in which case they will prevent moving characters from passing through the location which they occupy (see [Object.Solid](Object#objectsolid) property).

They may be [assigned a View](Object#objectsetview) and animated by a [script command](Object#objectanimate). Unlike Characters, Room Objects do not have a concept of "action animation", nor "facing direction", therefore they never animate automatically, only by your command, and the animation loop does not change on its own either if they, for example, change moving direction. You may achieve similar effects with scripting though, if you want to.

Room Objects may be clicked on by player to trigger one of their interaction events, depending on the active cursor mode.

See Also: [Tutorial: room objects](acintro4#objects), [Object events](EventTypes#object-events), [Object functions and properties](Object)

### Room Areas

There are 4 types of Room Areas: Hotspots, Walkable Areas, Regions, Walk-behinds. They have different uses, and are explained in a more detail below. But what they all have in common is that they are created by drawing areas of different colors on a respective "mask" image, - a sort of a layer that covers whole room but is invisible at runtime. Each area  *type* has got its own mask, where each *area* is associated with its own color on that mask.

Masks are 8-bit images, which means that pixel values are defined as indexes between 0 and 255 (inclusive), and max number of colors on them is technically limited to 256. The actual limit of usable areas in the engine may be lower for historical reasons. Each color index corresponds to an area. Area 0 is treated as "no area" in the engine, and as an "eraser" in the editor.

As there's a single mask per area type in a room, and areas are made of pixels of certain color on that mask:
1. Areas of the same type *cannot overlap*.
2. A single area may consist of multiple disconnected parts and all of these parts will act as one.

### Hotspots

Hotspots are parts of the room background that may be interacted with by the player. They are drawn on a "hotspot mask" room layer, where each color index correspond to a respective hotspot, and Hotspot 0 is treated as "no hotspot".

Hotspots do not have any visual representation of their own, they rather rely on the room background for that. That's why hotspots are commonly used to add interaction to certain areas, or things drawn on the room background that *do not change position or shape*.

Hotspots can react to following:
* Player clicking on them;
* Player character standing on them;
* Cursor moving over them.

Each of the above triggers [corresponding event](EventTypes#hotspot-events), which in turn may run a custom function in script.

A hotspot may be enabled or disabled by a [script command](Hotspot#hotspotenabled). Disabled hotspots do not react to any interactions.

Hotspot may have a Walk-to Point: that's an optional setting that tells where a character will walk to automatically when the player clicks on hotspot.

See Also: [Tutorial: hotspots](acintro3#hotspots), [Hotspot events](EventTypes#hotspot-events), [Hotspot functions and properties](Hotspot)

### Walkable areas

Walkable areas let define parts of the room that are permitted to move through by the Characters and Room Objects. Whenever a Character or a Room Object is commanded to [walk](Character#characterwalk) or [move](Object#objectmove) using walkable areas, the engine's pathfinder searches the shortest path to destination which passes through only the enabled walkable areas that you've drawn in the room.

Walkable areas are drawn on a "walkable mask" room layer, where each color index correspond to a respective hotspot, and Walkable Area 0 is considered to be "non-walkable" (or "blocking").

Walkable areas may be enabled or disabled by [script commands](Globalfunctions_Room#removewalkablearea). Disabled areas are treated as non-walkable. All enabled areas are viewed as one by the pathfinder, thus having different areas is necessary only if you wish to toggle them separately, or apply different effects to them.

Speaking of effects, Walkable Areas may have following configured:

1. Scaling. This lets to scale any Character or Room Object positioned on the area. There are two kinds of scaling: uniform, that applies same scaling in each point of this area, and continuous scaling, that assigns a scaling range from maximum at the area's bottom edge to minimum at the area's topmost edge. Continuous scaling is used to simulate a perspective, where characters walking "away" from the player will become smaller, and those that walk "closer" will appear larger.

2. Area specific view. You can assign a view number, which will be applied to a playable character automatically when it walks on this area.

See Also: [Tutorial: creating your first room](acintro2), [Global functions (room actions)](Globalfunctions_Room)

### Regions

Regions are parts of the room that may apply certain visual effects on the standing or passing characters and room objects. They are drawn on a "region mask" room layer, where each color index correspond to a respective region, and Region 0 is treated as "no region".

Regions may have following effects configured:

1. Color Tint.
2. Light Level.

Regions have a number of events, and react to following:
* Player walking onto a region;
* Player walking off a region;
* Player standing on a region (triggers repeatedly).

Each of the above triggers [corresponding event](EventTypes#region-events), which in turn may run a custom function in script.

See Also: [Region events](EventTypes#region-events), [Region functions and properties](Region)

### Walk-behinds

Walk-behinds are used to create "cut outs" from the room background, that may be "walked behind" by a character or object, hence - "walk-behinds". Walk-behinds are normally used to simulate the three-dimensional perspective in the room, where things that are further "away" from the player's view are visually covered by things that are "closer".

Walk-behinds are drawn on a "walk-behind mask" room layer, where each color index correspond to a respective cut-out, and index 0 is unused.

The one and only property of a walk-behind is the Baseline. Baseline defines this cut-out's drawing order. When characters and objects are positioned in the room, they are sorted among themselves and walk-behinds. Those things that have higher baseline are drawn above, and those that have lower baseline are drawn behind others. By default, character and objects have dynamic baseline equal to their Y coordinate. In AGS Y axis points from top of the room down to bottom. This means that, for example, if a character is standing lower than a walk-behind's baseline, then it has *higher* Y coordinate, and will be displayed *above* a cut-out. While if it is standing higher than a walk-behind's baseline, then it has *lower* Y coordinate, and will be displayed *behind* a cut-out.

Walk-behinds have no interactions or events.

Walk-behinds may be turned off by setting their Baseline to 0.

Walk-behinds are an optional feature, as they may be substituted by room objects, which have certain advantages over them. Because walk-behinds are using parts of the room background, they cannot change their looks dynamically (unless you switch whole room background), cannot animate or change position. Thus they are only suitable for static parts of the scene that never change.

See Also: [Global functions (room actions)](Globalfunctions_Room)
