## Upgrading to AGS 4.0

AGS version 4.0 is a big change to AGS.
The editor and the engine look like you remember from 3.6 versions, but it comes with many changes so it is advised to read this section carefully.

For the past two decades, AGS maintained strong backward compatibility. However, with 4.0, compatibility with versions earlier than 3.6 has been intentionally dropped. This has allowed long-requested features to be implemented. See [Obsolete Script API](ObsoleteScriptAPI) for more details.

Also note that 16-bit games are no longer supported. When creating a new game, you can select either 32-bit or 8-bit (palette-based) games.

**WARNING:** If you are upgrading an existing game to AGS 4.0, you should first upgrade to the latest AGS 3.6.2 available, and make sure that in General Settings, under backward compatibility, you have set _Script API Level_ and _Script compatibility level_ both to Latest version, and also with new-style options set to true and old-style options set to false. Only after your game is properly upgraded in AGS 3.6.2 and script adjust should an AGS 4.0 upgrade be attempted. Make sure you backup your project before proceeding.

### 32-bit colors everywhere

When referring to colors in script and properties, AGS had its own 16-bit color format, but this now has changed to support 32-bit `AARRGGBB` colors everywhere. If you are upgrading an AGS 3 game, it should readjust the colors set in GUIs and other game elements that were set through the editor automatically; but for colors you may have set through script you will have to manually convert - the color pane packs a converter that can help you.

### New Visual Effects

AGS elements now feature Blend Modes, Rotations, and Shaders. Additionally, GUIs can now be scaled arbitrarily.

If you're interested in advanced effects, check out the [`ShaderProgram` functions and properties](ShaderProgram), and [`ShaderInstance` function and properties](ShaderInstance) documentation for how to use shaders in your game.

### New Script Compiler

While the legacy script compiler is still present for now, a new compiler is available (and it is default in newly created games). This compiler packs new features like multidimensional arrays, nested structs, list initialization, and a multitude of script warnings to help you catch some common issues at compile time.

### Managed pointers in managed structs and RTTI

This feature finally opens full potential of structs referencing each other with pointers, and allow you to create virtually any kind of data storage in script.

To give an example:
```ags
managed struct Item; // pre-declaration

managed struct Person {
    Item* items[];
};

managed struct Item {
    Person* owner;
};
```

This is possible through the addition of Run-Time Type Information (RTTI) that is now added to the compiled scripts when using either compilers. This also allows watching variables through the newly added watch pane which should be helpful when debugging.

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
