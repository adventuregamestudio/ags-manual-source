## Upgrading to AGS 4.0

This is a small guide on how to upgrade your existing game project to AGS 4.

**WARNING:** Please backup your project and read below before proceeding.


### Upgrade to the latest AGS 3

Firstly, it is recommended that you upgrade your game to the latest AGS 3 available - currently, this would be the latest AGS 3.6.2 version, and fix any issues reported by the game build process.

After doing so, still in AGS 3.6.2, go into **General Settings** and adjust as below.

| Setting | Value |
| --- | --- |
| Enforce new-style audio scripting | True |
| Enforce new-style strings | True |
| Enforce post-2.62 scripting | True |
| Left-to-right operator precedence | True |
| Script API version | Latest version |
| Script compatibility level | Latest version |
| Use low-resolution co-ordinates in script | False |
| Use old-style custom dialog options API | False |
| Use old-style keyboard handling | False |

After these adjustments, in General Settings, once you can build and run your game successfully under AGS 3.6.2 with no issues, backup your game, and proceed.


### Incompatibilities with AGS 3

There are a few things that cannot be upgraded at the current time, so we need to warn you about it.

1. **AGSJoy plugin:** Attempts to build your game with this plugin active will not compile with AGS 4 because its Joystick struct will conflict with the AGS own Joystick struct. The new API is relatively simple, so manually rewriting it to be compatible with it should not be a huge problem. The plugin is no longer needed after that.

2. **Speech Center and Translations:** Speech Center with translations will stop working with this AGS 4 update. It won't crash, but it won't see translated strings. This is because Translation storage had changed in the Editor. The plugin will have to be updated to work with AGS 4.

3. 16-bit games are no longer supported. If you wish to maintain the same aesthetics you can achieve it by making a 32-bit color game and shaders.


### Load your game on AGS 4.0

Open AGS 4.0 and load your existing game into it. AGS will do some automatic updates on your game project where possible, but some things will require manual intervention. Read below on those.


### Adjustment of functions with fewer arguments


There are a couple of functions that now have *fewer* arguments compared to previous versions of AGS.

These functions are:

- `DynamicSprite.Create` and `DynamicSprite.CreateFromExisting` no longer have the "has alpha" argument, as in 32-bit games all sprites are considered to have a usable alpha channel now. You can still specify a color format, which is mostly useful for 8-bit color mask creation which can be useful for advanced scripts and custom pathfinder masks.

- `Overlay.CreateGraphical` does not have the "transparent" argument, as it was always useless.

This is an unusual case (commonly arguments are **added**). This may cause script errors if you import a project or a module.

**Quick fix**: If you don't want to manually fix all these function calls in your game, create a script module at the top of the scripts list and place helper extenders there that wrap the actual function calls, like this:

```ags
DynamicSprite* Create(static DynamicSprite, int width, int height, bool unused)
{
    return DynamicSprite.Create(width, height);
}

static DynamicSprite* DynamicSprite.CreateFromExistingSprite(int slot, bool unused)
{
    return DynamicSprite.CreateFromExistingSprite(slot);
}

Overlay* CreateGraphical(static Overlay, int x, int y, int slot, bool unused, bool clone)
{
    return Overlay.CreateGraphical(x, y, slot, clone);
}

Overlay* CreateRoomGraphical(static Overlay, int x, int y, int slot, bool unused, bool clone)
{
    return Overlay.CreateRoomGraphical(x, y, slot, clone);
}
```


### Adjustment of colors in script

AGS 4 uses **32-bit colors** everywhere, including scripting. If you use hex values, the colors are in the `0xAARRGGBB` format, which means they also carry alpha information.

You can use the **color pane** in the Editor to help convert between the old color format and the new one.

Color values in GUI properties and other AGS game elements adjusted through the editor panes should have been **automatically converted** during the upgrade process.


### WalkableArea and Walkbehind script API

These are now accessible through object-oriented APIs, instead of the old ones, you will have to adjust your code to match the new script API if you were accessing it.

