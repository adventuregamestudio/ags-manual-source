## Default Setup

The Default Setup pane lets you create the default runtime configuration for your game. This configuration is saved as "acsetup.cfg" into the Compiled folder.

Since *AGS 3.6.0* the config file in the Compiled folder is created by merging "Default Setup" properties with an optional "acsetup.cfg" file found in the game project's folder, if one is present. This is done to allow game developers provide their own custom settings. In such case the values from "Default Setup" have higher priority, but it does not delete other, custom settings.

For information on possible "acsetup.cfg" contents please refer to the [related topic](EngineConfigFile).

**NOTE:** please be aware that the default game's config cannot be modified using game's [setup utility](Setup) (aka "winsetup.exe"), because that program modifies config file in the user's personal documents folder instead. The setup utility may be useful to quickly test various options, but config file for your game's distribution will be created from "Default Setup" pane anyway.

**IMPORTANT:** what you set on this pane is a default game configuration,
but it may be overridden by a user config. If you have run your game's setup utility at least once and saved, that created a user config file
[in its own location](EngineConfigFile#configuration-file-locations). Since then changing Default Setup won't be enough to affect your game,
you'd also have to change user config by running setup program again. You may also delete user config file (that is safe thing to do), which
will reset settings back to default config and let you test it out.

**IMPORTANT:** the configuration file will only be recreated during next game compilation, so if you change these settings you will need to
rebuild your game one more time to apply them.

### Setup appearance

-   **Title text** - the text that will appear on the title of the setup program window.
    
### Audio

-   **Audio driver** - which audio driver to use. Unfortunately, as the driver depends on the player's system, there's no way to list them here. Instead, there are only two available choices: "Default" and "Disable".
-   **Use voice pack if available** - this option enables voice speech in game, if it's available.

### Environment

-   **Use custom game saves path** - enables **Custom game saves path** option.
-   **Custom game saves path** - defines where game will store its saves
    and individual user's files (e.g. achievements). This path will be
    used to substitute default value of `$SAVEGAMEDIR$` token
    in scripts. Note that when being set up in the Editor, this option
    only accepts relative paths (when used they will be relative to the
    game's location). This has to be done this way to prevent setting
    absolute path that does not exist on the player's machine, so
    typically you have to set this if you want to have saves stored
    locally within the game's folder. Players will be able to modify
    this path in setup program (where they are actually allowed to put
    absolute paths too).
-   **Use custom shared data path** - enables **Custom shared data path** option.
-   **Custom shared data path** - defines where game will store its
    shared data files (e.g. hi-score tables). This path will be used to
    substitute default value of `$APPDATADIR$` token in scripts. Note
    that this option only accepts relative paths, so typically you have
    to set this if you want to have data files created locally within
    the game's folder. Players will be able to modify
    this path in setup program.

### Gameplay

-   **Game language** - which translation to use. The list depends on translations added to your game project.

### Graphics

-   **Anti-alias scaled sprites** - this option will apply anti-aliasing to scaled game objects, such as Characters, Room Objects and Overlays, in order to give them a smoother look after the resizing. This is *NOT* applied to DynamicSprite's operations though.
-   **Fullscreen as borderless window** - if enabled, then in fullscreen mode the game will create a borderless window covering whole desktop, instead of changing display resolution. This is preferred on modern systems.
-   **Fullscreen scaling style** - this option determines the way the game will be scaled when in fullscreen mode. Available options are:
    - **Max round multiplier**. The game will be scaled up using a maximal round integer (2, 3, 4, etc) multiplier with which it still fits inside
the window.
    - **Fill whole screen**. The game will be stretched to fill the whole window. Note that if the game's native aspect ratio is different from the
screen's aspect ratio, the game graphics will appear deformed and indeed stretched.
    - **Stretch, preserving aspect ratio**. The game will be stretched to fill most of the window while respecting the game's native aspect ratio
(width/height proportions). This ensures that the game graphics will always look proportional, but the option can lead to black borders at the left & right or
top & bottom sides of the screen.

-   **Graphics driver** - lets choose a graphics driver. Currently supported are:
    - **Software**. The classic and slower option, but the only driver that fully supports 8-bit games with palette effects.
    - **Direct3D 9**.
    - **Direct3D 11**.
    - **OpenGL**.

-   **Render sprites at screen resolution** - when enabled, any scaled game objects will be scaled in screen pixels rather than game pixels. What this means is that when a low-resolution game is run in a larger window, the sprites will take advantage of this higher resolution and look less pixelated when scaled up or down, especially when scaled to a non-round multiplier.

    **NOTE:** this option may be locked to a certain state in [General Settings](GeneralSettings), in which the game config won't affect it.

-   **Rotation mode** - instruct the mobile device which rotation mode to use when running your game. Available choices are:
    - **Unlocked**. Will depend on physical device's orientation.
    - **Portrait**. Always run in the "portrait" mode.
    - **Landscape**. Always run in the "landscape" mode.

-   **Scaling filter** - tells which algorithm to use when scaling the game up (or down) inside the window. The list of filters depend on "Graphics driver" but two common choices are: "Nearest-neighbour" and "Linear interpolation".
-   **Start in windowed mode** - when enabled starts the game in windowed mode, as opposed to fullscreen.
-   **Vertical sync** - enables or disables vertical sync.
-   **Windowed scaling style** - this option determines the way the game will be scaled when in windowed mode. Available options are matching those of "Fullscreen scaling style", and there's one extra one:
    - **Custom round multiplier** - lets you input an explicit game scaling multiplier.

### Misc

-   **Display FPS on screen** - when enabled, the engine will draw FPS counter on screen while running the game.

### Performance

-   **Sound cache size (in megabytes)** - this option defines the maximum amount of memory that the game will use for caching sounds. Engine has a rule to cache only "small" sound files, typically ones below 1 MB in size, so this cache is mostly meant for small frequent sound effects, such as footsteps and similar.
-   **Sprite cache size (in megabytes)** - this option defines the maximum amount of memory that the game will use for its sprite cache. The sprite cache is used to keep a partition of all the game sprites loaded to the memory, thus reducing loading times between rooms and preventing slowdowns during game play. Of course, higher values make the game use more memory. Usually only high-resolution games with long animations need to have this value increased.
-   **Texture cache size (in megabytes)** - this option defines the maximum amount of **video memory** (of the player's graphics card) that will be used by the texture cache. The texture cache keeps frequently used textures created from sprites by non-Software graphics drivers (Direct3D, OpenGL), and may significantly improve performance in high-resolution games.

### Touch

-   **Touch to Mouse Emulation** - defines the touch control scheme for devices with touch screen. Available options are:
    - **Off**. Completely disabled.
    - **One finger**. In this mode only one finger is detected at once, and touch directly corresponds to holding LMB down.
    - **Two fingers**. In this mode up to 2 fingers are detected. One finger tap is treated as LMB click; one finger held and second finger tap is treated as RMB click. Dragging a finger is treated as mouse move without pressing any buttons. Double tap + drag simulates moving mouse with LMB held down.

-   **Touch to Mouse Motion mode** - tells whether finger motion is interpreted as "Direct", where game cursor will be positioned at a finger, or "Relative", where game cursor will move from its last position proportionally to the finger's move.

### Mouse

-   **Auto-lock mouse to window** - if enabled, the mouse will be locked inside game window whenever the player clicks on it or switches into game. The locked mouse cannot leave the game window, making it impossible to accidentally jump out of the game by mistake (by clicking on the desktop, for example).
-   **Mouse cursor speed** - lets to set up the mouse cursor speed in game. This parameter is only applied in fullscreen mode.

*See also:* [Run-time engine setup](Setup), [Config file locations](EngineConfigFile#configuration-file-locations)