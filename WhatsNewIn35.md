## Upgrading to AGS 3.5

AGS version 3.5 is another big update and it has few serious changes to both editor UI and scripting.

### Room editor and multilayer mode

Room editor has got a new navigation bar on the top of the panel instead of the plain drop-down list where you used to select edit mode. This navigation bar lets you do the following:

* See the full list of elements grouped into layers by type (characters, objects, edges and so forth).
* Select editing mode, and the particular item for edit.
* Toggle each group's and even each item's visibility in any combinations. This lets you display, for example, characters, objects and walkable areas at the same time. The only exception is that you may only display one type of area mask at the same time (that is - either walkable areas, regions, hotspots or walk-behinds).
* Toggle each group's and each item's locked state. When locked the object cannot be moved or have its shape changed (as with mask areas which may be changed by painting).

**NOTE:** you can still only select and edit objects of the chosen group, not all at once.

**NOTE:** in order to remember user preference on visible and locked items Editor saves a new file called **roomNNN.crm.user** per each room, where 'NNN' is a room's number. These files are not part of the game data and are safe to remove (but in that case all item visibility and lock states in the room will be reset).

### New Viewport/Camera system

In the past, AGS had a very simple concept of Viewport: a rectangular "eye" which selected a part of the room to display on the screen. If a room was larger than the game's screen it became a "scrolling room" and you could move a viewport around it, showing different parts of that room.

In 3.5 the concept of viewport has changed a lot to provide more options. First of all, it's now split into two parts: Viewport and Camera.

The new Camera is almost what the old Viewport was: it is a rectangle *inside the room* that acts like an "eye" and tells which part of this room can be seen. Camera's position and size are defined in *room coordinates*. If Camera is smaller than the room, then it can be moved around it to see different parts.<br>
The new Viewport is a different thing, now it defines a rectangle *on a game's screen* where the room will be drawn. Viewport has a link to Camera and displays what camera "sees" at this moment. Viewport's position and size are defined in *screen coordinates*, just like GUIs and Overlays. This means that if a camera is moving inside the room, the viewport will still stay at the same place on the screen, and vice-versa when a viewport is moved around the screen it will still display same part of the room. Of course, moving both viewport and camera will produce more complicated result.<br>
You may think of Viewport as of a TV screen or computer monitor, which translates the image from the camera.

Following is a table of conversion between old viewport functions and new camera functions:

obsolete function | replace with
-- | --
SetViewport(x, y) | Game.Camera.SetAt(x, y)
ReleaseViewport() | Game.Camera.AutoTracking = true
GetViewportX() | Game.Camera.X
GetViewportY() | Game.Camera.Y

Introduction of new Viewport and Camera concepts allows a lot of new effects which were previously impossible or not easy to accomplish. For example, you no longer must display the room covering the whole screen, you may make a smaller "window" to see the room and position it in the middle of the screen.<br>
By the way, this also means that you do not have to keep all your rooms same size, you may create very small rooms in your game now and adjust viewport and camera to display it on screen where and how you like.<br>
By default Viewport and Camera has the same size, but you may make them have different sizes. If a camera is smaller than the viewport then its image will be stretched to fill it causing a "zoom-in" effect. If the camera is larger than the viewport then it will be shrunk to fit in causing a "zoom-out" effect.

The first viewport-camera pair is created automatically when the game starts, but you may create more of them. Viewport's and camera's parameters may be changed anytime, thus you may have different settings per each room and even animations (like moving viewport and camera around, zooming in and out).

It's important to note that any cursor actions will only work on the room if the cursor is on a viewport.

Only rooms and their contents (characters, objects) are affected by the viewport/camera system. GUI and Overlays, including speech and other on-screen text, still work outside of it.

**IMPORTANT:** Another thing which needs a different approach is a conversion between screen and room coordinates. Earlier, if you'd like to know where in the room the player has clicked with a mouse, you only had to apply camera's offset in room (previously known as viewport's offset):

```ags
int roomx = mouse.x + GetViewportX();
int roomy = mouse.y + GetViewportY();
```

Since 3.5 this is no longer enough, because not only viewport itself may have offset on screen, but also camera's image may be scaled inside a viewport. For that reason there are now actual functions that help you do this conversion: [`Screen.ScreenToRoomPoint`](Screen#screenscreentoroompoint), [`Screen.RoomToScreenPoint`](Screen#screenroomtoscreenpoint), [`Viewport.ScreenToRoomPoint`](Viewport#viewportscreentoroompoint), [`Viewport.RoomToScreenPoint`](Viewport#viewportroomtoscreenpoint).<br>
They are used like:

```ags
Point *roompt = Screen.ScreenToRoomPoint(mouse.x, mouse.y);
int roomx = roompt.x;
int roomy = roompt.y;
```

For more information see: [`Camera`](Camera), [`Viewport`](Viewport), [`Game.Cameras`](Game#gamecameras), [`Screen.Viewports`](Screen#screenviewports)

### New Screen struct

There's now a new Screen struct that contains several game screen related functions and properties. Its addition renders some of the older properties from System struct obsolete. Following is a table of conversion between old and new properties:

obsolete property | replace with
-- | --
System.ScreenWidth | Screen.Width
System.ScreenHeight | Screen.Height
System.ViewportWidth | Screen.Width
System.ViewportHeight | Screen.Height

As you may notice, both deprecated ScreenWidth/Height and ViewportWidth/Height properties are corresponding to the single new pair. That is because in practice both System.ScreenWidth and System.ViewportWidth were representing almost same thing, except for specific cases which are no longer valid in modern AGS anyway.

For more information see: [`Screen`](Screen)

### Room sizes and mask resolution

Previously AGS had a restriction that a room background must be at least size of a game resolution. With 3.5 this restriction is gone and you can now make rooms any size. This became possible after the rewrite of the viewport system, as explained above.

Another change is related to the room masks that define walkable areas, hotspots and so forth. Historically only walk-behinds mask was always made equal to the size of room background. Other masks were 1:1 to room background's size only in "low-res" games (below 640x400). In "high-res" games (640x400 and above) hotspots, regions and walkable masks were made x2 smaller resolution than the room, which means that for 4 room pixels (2x2 square) there was 1 mask pixel. That, in turn, caused these masks to be less precise. Originally this was done to reduce both memory size of a room and speed up any operations run against these masks, such as path-finding, but sometimes annoyed users who were unable to draw exactly a region they want.

Since 3.5 each room has a property called MaskResolution which currently ranges from 1:1 to 1:4. This resolution determines the resolution factor for hotspots, regions and walkable masks (but not walk-behinds which is still fixed at 1:1). Default is 1:1, but you may change it anytime for any individual room if you prefer your room to have *less precise* area masks.<br>
To make things more convenient there's also an option in General Settings called "Default mask resolution" which lets you choose a value that will be applied to every newly created room.<br>
Note that when importing older projects the mask resolution is kept unchanged.

### Deprecated relative asset resolutions

This is something users do not know much about now but may stumble upon sometimes. In the long past AGS divided all games into "low-resolution" and "high-resolution" games, "high-res" was defined roughly as 640x400 and higher while "low-res" was 320x200 or 320x240. Similarly AGS allowed certain assets, such as sprites, fonts, and rooms, be tagged as "low-res" or "high-res". The idea behind this was that if a "low-res" asset is used in a "high-res" game then it gets automatically scaled up x2, and vice-versa a "high-res" asset in a "low-res" game would be downscaled x2.

Although such practice is not commonly used for a while, you could still experience this effect if you, for example, take game template or project made in low resolution and convert it to high resolution. Some sprites from the original game which were marked as "low-res" will be scaled up. Sometimes this behavior may be useful - in case you are using these sprites as placeholders but may confuse inexperienced users.

In AGS 3.5 we are deprecating concept of resolution tags and disable these conversions *by default*. We also make asset scaling more explicit in an attempt to prevent its misuse.

1. Rooms no longer have a resolution tag and are always considered having "real" resolution. This is only relevant if you are upgrading from pre-AGS 3.0 era. If you import 2.72 games into AGS 3.5 your rooms will get resized to match game resolution (unless they already do).

2. Sprites retain resolution tag for backward compatibility, but it is now set to "Real" by default and not recommended to change. When you import older project sprites get "promoted" to "real" resolution if their tag matches the game: for example if a sprite is tagged as "low-res" and your game is 320x200, or if a sprite is tagged as "high-res" and your game is 640x400 or higher.

3. "Fonts designed for high resolution" setting was removed because it no longer makes sense. Instead, each font has an individual ScalingMultiplier property. When importing older project each font will be scaled x2 if it was a "high-res" game and this setting was OFF.

### Custom Say function in Dialogs

AGS 3.5.0 adds the possibility of using a custom script function as a substitute instead of regular `Character.Say` commands when building dialogs. This is done using "Custom Say function in dialog scripts" option in the General Settings. Similarly, narration (which is by default done using Display script function) may be substituted with a custom one using "Custom Narrate function in dialog scripts".

These custom functions should be declared as imports in one of your script headers.<br>
"Custom Say" function must have one of the following two declaration forms:<br>
`void MySay(Character* c, const string text); // use ex: MySay(player, "Hello");`<br>
or<br>
`void MySay(this Character*, const string text); // use ex: player.MySay("Hello");`

"Custom Narrate" function must have following declaration form:<br>
`void MyNarrate(const string text);`

**IMPORTANT:** There's currently a limitation that, if Say checkbox for dialog options is checked, it will use regular `Character.Say` despite defining a custom Say function.

### Some script functions replaced and/or deprecated

Some functions from previous Script API were either replaced by new equivalents or deprecated. They are shown in the table below:

obsolete function | replace with
-- | --
GetWalkableAreaAt(x, y) | GetWalkableAreaAtScreen(x, y)
Character.IgnoreWalkbehinds | *Do not use*
Object.IgnoreWalkbehinds | *Do not use*
DrawingSurface.UseHighResCoordinates | *Do not use*

Note that you can still use "Script Compatibility Level" switch in General Settings to enable old functions.

### System limits update

Support for very large files was added to AGS, therefore now package file and every asset file is allowed to exceed 2 GB. One good example to mention is the packed sprites file (acsprset.spr) which 2 GB limit was causing most inconveniences.

Other limits:
* Imported sprites count limit was raised to 90000;
* Total number of sprites in game, which includes both imported and Dynamic Sprites is no longer limited by arbitrary number (in practice you may have around 2 billions of dynamic sprites);
* Font count limit removed;
* Removed length limit on the Button and TextBox text (was 50 and 200 respectively);
* Removed ListBox item count limit (was 200).
* Removed hidden limit for DoOnceOnly token length (was 200 characters).
