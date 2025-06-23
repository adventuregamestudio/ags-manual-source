## Upgrading to AGS 4.0

AGS version 4.0 is a big change to AGS.
The editor and the engine look like you remember from 3.6 versions, but it comes with many changes so it is advised to read this section carefully.

For the past two decades, AGS maintained strong backward compatibility. However, with 4.0, compatibility with versions earlier than 3.6 has been intentionally dropped. This has allowed long-requested features to be implemented. See [Obsolete Script API](ObsoleteScriptAPI) for more details. Additionally 16-bit games are no longer supported, at game creation time you can select 32-bit or 8-bit (with palettes) games.

### New Visual Effects

AGS elements now feature Blend Modes, Rotations, and Shaders. Additionally, GUIs can now be scaled arbitrarily.

Read more about shaders in [`ShaderProgram` functions and properties](ShaderProgram), and [`ShaderInstance` function and propeties](ShaderInstance) pages.

### 32-bit colors everywhere

When referring to colors in script and properties, AGS had its own 16-bit color format, but this now has changed to support 32-bit `AARRGGBB` colors everywhere.

### New Script Compiler and RTTI

While the legacy script compiler is still present for now, a new compiler is available (and it is default in newly created games). This compiler packs new features like multidimensional arrays, nested structs, list initialization, and a multitude of script warnings to help you catch some common issues at compile time.

Beyond this, Run-Time Type Information (RTTI) is added now when using either compilers, which unlocks managed pointers in managed structs and other complex declarations that weren't previously allowed. A new variable watch pane is added when debugging, and RTTI will also enable better type information during debugging.

### New Room Format

Rooms are now stored in the project in XML format and with images in separate png files, which can be tracked in source control. The room `.crm` files are now built from these project files and are no longer required to be added to source control. 

### New Video Player API

Do you want a TV in your room with a video playing while the player walks around? Well, this is now possible, beyond many possibilities, as a new video player API can unlock new ways to use videos in AGS. Before, only a single video could be played and it blocked the entire game, so if you used videos in your game be sure to check this out. The new API is documented in [`VideoPlayer` functions and properties](VideoPlayer) page.

### Pathfinder and Path API

Until now you could only use AGS Pathfinder when ordering a character to walk. Two new APIs let you both search walkable paths in two-dimensional space and also pass an arbitrary path as an array of points to a room element to have it follow a path. You can also create a pathfinder from 8-bit sprites as a navigation mask.

### Joystick and Touch API

New input methods are added to the engine. The engine will now find connected joysticks and make them available through a new struct. In AGS 3, touch devices could only be used with AGS through mouse emulation, but now the touch input can be directly handled, allowing multiple touch points to be supported through scripting. These are documented in [`Joystick` functions and properties](Joystick) and [`Touch` functions and properties](Touch) pages.

### Fonts and Font Files

In AGS 3, each font was directly connected to a font file, so having different sizes or outlines of the same font would require reimporting and packaging multiple copies of the font in the game. AGS now handles these separately, where a font contains description, size and outline information, and can be connected to any imported font file. Font files are now also stored in a Fonts directory within the project root.
