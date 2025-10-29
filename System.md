## `System` functions and properties

### `System.GetEngineInteger`

```ags
static int System.GetEngineInteger(EngineValueID value, optional int index)
```

Gets a runtime engine value represented as integer by the given identifier.

This function, along with [`System.GetEngineString`](System#systemgetenginestring), is meant primarily for diagnostic and debugging purposes. It lets you to find out certain information about the engine's state and configuration.

The list of values that it can return is represented by the [`EngineValueID`](StandardEnums#enginevalueid) enum. The value IDs follow certain pattern, which lets distinguish numeric values from string ones:
- `ENGINE_VALUE_I_*` - this ID refers to a simple numeric value;
- `ENGINE_VALUE_II_*` - this ID refers to an array of numeric values, and you are supposed to pass an index together with this value ID.

Example:

```ags
function repeatedly_execute_always()
{
    int maxSpriteCache = System.GetEngineInteger(ENGINE_VALUE_I_SPRCACHE_MAXNORMAL);
    int curSpriteCache = System.GetEngineInteger(ENGINE_VALUE_I_SPRCACHE_NORMAL);
    lblSpriteCache.Text = String.Format("Sprite cache filled: %d / %d (%d %%)",
        curSpriteCache, maxSpriteCache, curSpriteCache * 100 / maxSpriteCache);
}
```

Above script will get maximal and currently filled amount of the engine's sprite cache, and print them to the Label.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:* [`System.GetEngineString`](System#systemgetenginestring),
[EngineValueID Enum](StandardEnums#enginevalueid)

### `System.GetEngineString`

```ags
static String System.GetEngineString(EngineValueID value, optional int index)
```

Gets a runtime engine value represented as string by the given identifier.

This function, along with [`System.GetEngineInteger`](System#systemgetengineinteger), is meant primarily for diagnostic and debugging purposes. It lets you to find out certain information about the engine's state and configuration.

The list of values that it can return is represented by the [`EngineValueID`](StandardEnums#enginevalueid) enum. The value IDs follow certain pattern, which lets distinguish numeric values from string ones:
- `ENGINE_VALUE_S_*` - this ID refers to a simple string value;
- `ENGINE_VALUE_SI_*` - this ID refers to an array of string values, and you are supposed to pass an index together with this value ID.

Example:

```ags
String engineVersion = System.GetEngineString(ENGINE_VALUE_S_ENGINE_VERSION);
Display("Running on AGS engine %s", engineVersion);
```

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:* [`System.GetEngineInteger`](System#systemgetengineinteger),
[EngineValueID Enum](StandardEnums#enginevalueid)

### `System.Log`

```ags
static void System.Log(LogLevel level, const string format, ...)
```

Prints the message string in `format` to the log, with the defined LogLevel, to the group `script`.

Example:

```ags
System.Log(eLogInfo, "Entering the Room %d", player.Room);
```

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [The run-time engine](RuntimeEngine), [LogLevel Enum](StandardEnums#loglevel)

---

### `System.SaveConfigToFile`

```ags
static void System.SaveConfigToFile()
```

Writes current engine settings to the player's configuration file, so that they may be applied next time the game is launched.

This saves only the options that may be changed at runtime, such as translation and fullscreen/windowed graphic mode.

*Compatibility:* Supported by **AGS 3.5.1** and later versions.

*See also:* [`File.Open`](File#fileopen)

---

### `System.AudioChannelCount`

```ags
readonly static int System.AudioChannelCount;
```

Gets the number of Audio Channels available to the game (in the current
version of AGS this is 16).

This is useful if you want to loop through all the audio channels and
check what is playing on them.

Example:

```ags
Display("There are %d audio channels.", System.AudioChannelCount);
```

will display a message with the number of audio channels.

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:* [`System.AudioChannels`](System#systemaudiochannels)

---

### `System.AudioChannels`

```ags
readonly static AudioChannel* System.AudioChannels[];
```

Gets the AudioChannel instance for the specified channel number. This
allows you to query the audio channel and find out what is playing on
it.

Example:

```ags
AudioChannel *channel = System.AudioChannels[2];
Display("Channel 2's current volume is %d.", channel.Volume);
```

will display a message with Audio Channel 2's current volume.

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:* [`AudioChannel`](AudioChannel),
[`System.AudioChannelCount`](System#systemaudiochannelcount)

---

### `System.CapsLock`

```ags
readonly static bool System.CapsLock;
```

Gets whether Caps Lock is active on the player's system.

You might want to use this to warn the player to switch it off before
typing a password in, for example.

Example:

```ags
if (System.CapsLock)
{
    Display("The CAPS LOCK light is on.");
}
```

will display a message if Caps Lock is on.

*Compatibility:* Supported by **AGS 3.0.1** and later versions.

*See also:* [`System.NumLock`](System#systemnumlock),
[`System.ScrollLock`](System#systemscrolllock)

---

### `System.ColorDepth`

*(Formerly known as `system.color_depth`, which is now obsolete)*

```ags
readonly static int System.ColorDepth;
```

Returns the **native** game's color depth. This is the overall game color depth setting, and it is possible for individual sprites or backgrounds to be different (although not recommended).

**NOTE:** Be aware that for historical reasons, and similar to [`System.ScreenHeight`](System#systemscreenheight) and [`System.ScreenWidth`](System#systemscreenwidth), this is NOT the final color depth of a display mode, but the internal color depth of a game. For instance, a game may have a 8-bit native color depth, but be displayed in a 32-bit mode.

The native color depth may be useful to know, for example, when you are writing a script module performing raw drawing: then you'd know which ranges of colors will work.

Example:

```ags
Display("Game's native resolution is: %d x %d, %d-bit color", System.ScreenWidth,
        System.ScreenHeight, System.ColorDepth);
```

will display the game's native resolution and color depth.

*See also:* [`System.ScreenHeight`](System#systemscreenheight),
[`System.ScreenWidth`](System#systemscreenwidth)

---

### `System.DisplayFPS`

```ags
static bool System.DisplayFPS;
```

Gets/sets whether the current frames per second is displayed on the screen.
This is useful for debugging purposes and quickly understanding performance impacts of the changes you do to your game.

**NOTE:** if a command line flag that forces displaying fps is passed,
this property will always be true, setting it will be ignored.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:* [`System.FPS`](System#systemfps),
[`Debug`](Globalfunctions_General#debug)

---

### `System.FPS`

```ags
static readonly int System.FPS;
```

Gets the current real frames per second ratio of the game.
This is useful for debugging purposes and quickly understanding performance impacts of the changes you do to your game.

Note that System.FPS may or not be equal to the [`Game.Speed`](Game#gamespeed) value. While Game.Speed reports a logical game's frame rate, and is useful for measuring timing relative to the rest of the game, System.FPS tells the real frame rate, which demonstrates rather how well your game performs on a current system, and whether it runs slower than requested.

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

*See also:* [`System.DisplayFPS`](System#systemdisplayfps),
[`Game.Speed`](Game#gamespeed)

---

### `System.Gamma`

```ags
static int System.Gamma;
```

Gets/sets the current screen Gamma level. This is 100 by default, and
you can set it anywhere from 0 (pitch black) to 200 (double normal
brightness).

[`System.SupportsGammaControl`](System#systemsupportsgammacontrol)
must return *true* in order for this property to have any effect.

Because every player's monitor will be different, you should normally
use this property linked to a GUI Slider in order to allow the player to
adjust it to suit their system.

Example:

```ags
if (System.SupportsGammaControl) {
    System.Gamma = 150;
}
```

will turn the screen brightness up to `50%` higher than normal

*See also:*
[`System.SupportsGammaControl`](System#systemsupportsgammacontrol)

---

### `System.HardwareAcceleration`

```ags
readonly static bool System.HardwareAcceleration;
```

Returns whether the game is running with hardware acceleration (e.g.
Direct3D or OpenGL). If this is the case then changing on-screen images with [DrawingSurface](DrawingSurface) functions are likely to be slower,
but sprite scaling, transparency and alpha blending effects, as well as just displaying larger sprites, are likely to be faster, than when the non-accelerated driver is used.

**Cross-Platform Support**

Windows: **Direct3D, OpenGL driver**<br>
Linux: **OpenGL driver**<br>
MacOS: **OpenGL driver**<br>
Android: **OpenGL driver**<br>
iOS: **OpenGL driver**

Example:

```ags
if (System.HardwareAcceleration) {
    Display("Yay, we can draw loads of alpha blended sprites fast!");
}
```

will display a message if the game is being run with hardware
acceleration

*See also:* [AGS Graphics Drivers](GraphicsDriver)

---

### `System.HasInputFocus`

```ags
readonly static bool System.HasInputFocus;
```

Tells whether the game window currently has input focus, meaning it is
active and player can control the game.

If your game is made to continue running in the background, when the
user switches out from game, you may use this property in scripts to
know if that actually happened.

Examples:

```ags
if (!System.HasInputFocus)
    return;
```

skips the rest of the function if player has switched out from the game.

```ags
void repeatedly_execute()
{
    if (!System.HasInputFocus && IsGamePaused() == 0) {
        PauseGame();
    } else if (System.HasInputFocus && IsGamePaused() == 1) {
        UnPauseGame();
    }
}
```

pauses game when player switches out, and unpauses it when player
switches back to game.

*Compatibility:* Supported by **AGS 3.3.5** and later versions.

*See also:* [`SetMultitaskingMode`](Globalfunctions_General#setmultitaskingmode)

---

### `System.NumLock`

```ags
readonly static bool System.NumLock;
```

Gets whether Num Lock is active on the player's system.

You might want to use this to warn the player to switch it off before
using the numeric keypad arrow keys, for example.

Example:

```ags
if (System.NumLock)
{
    Display("The NUM LOCK light is on.");
}
```

will display a message if Num Lock is on.

*Compatibility:* Supported by **AGS 3.0.1** and later versions.

*See also:* [`System.CapsLock`](System#systemcapslock),
[`System.ScrollLock`](System#systemscrolllock)

---

### `System.OperatingSystem`

*(Formerly known as `system.os`, which is now obsolete)*

```ags
readonly static eOperatingSystem System.OperatingSystem;
```

Returns which operating system the game is currently running under. It
can be one of the following values:

    eOSDOS (exists, but no longer supported)
    eOSWindows
    eOSLinux
    eOSMacOS
    eOSAndroid
    eOSiOS
    eOSPSP (exists, but no longer supported)
    eOSWeb
    eOSFreeBSD

Example:

```ags
if (System.OperatingSystem == eOSWindows) {
    Display("Running on Windows!");
}
else {
    Display("Not running on Windows!");
}
```

---

### `System.RenderAtScreenResolution`

```ags
static bool System.RenderAtScreenResolution;
```

Gets/sets whether sprites are rendered at screen resolution (if it's
**true**) or native game resolution (if it's **false**).

When your game has low native resolution and player is running it in
high display mode (for example 320x200 game being run in 1920x1080),
then turning this property on will let engine to take advantage of
higher display resolution and draw characters and objects less pixelated
when they are scaled up or down (e.g. on walkable area with scaling).

**IMPORTANT:** unless you have locked this parameter to certain value in
your game's [General settings](GeneralSettings), players may also change it
in the setup program. In such case it is suggested that you do not
unconditionally modify this property in script, but rather provide a
menu option for toggling it at runtime.

*Compatibility:* Supported by **AGS 3.4.1** and later versions.

---

### `System.RuntimeInfo`

```ags
readonly static String System.RuntimeInfo;
```

Returns the string containing short description of the environment the
game is currently running in, such as engine version, graphics mode, and
available game resources.

This is meant mainly for debug purposes.

**NOTE:** System.RuntimeInfo is a more convenient analogue of Debug(1,0) command,
being more explicit and working on both release and debug
modes of the game.

Example:

```ags
void on_key_press(eKeyCode keycode) {
    if (keycode == eKeyCtrlV) {
        Display(System.RuntimeInfo);
    }
}
```

*Compatibility:* Supported by **AGS 3.4.0** and later versions.

*See also:* [`Debug`](Globalfunctions_General#debug)

---

### `System.ScreenHeight`

**This property is obsolete since AGS 3.5.0. Use [`Screen.Height`](Screen#screenheight) instead.**

*(Formerly known as `system.screen_height`, which is now obsolete)*

```ags
readonly static int System.ScreenHeight;
```

Returns the game's screen height in native coordinates, **not** accounting for the scaling applied by graphic renderer. Historically it also included a size of the black letterbox borders, making it a confusing parameter.

*See also:* [`System.ColorDepth`](System#systemcolordepth), [`Screen.Width`](Screen#screenwidth), [`Screen.Height`](Screen#screenheight),
[`Viewport.Width`](Viewport#viewportwidth), [`Viewport.Height`](Viewport#viewportheight)

---

### `System.ScreenWidth`

**This property is obsolete since AGS 3.5.0. Use [`Screen.Width`](Screen#screenwidth) instead.**

*(Formerly known as `system.screen_width`, which is now obsolete)*

```ags
readonly static int System.ScreenWidth;
```

Returns the game's screen width in native coordinates, **not** accounting for the scaling applied by graphic renderer. Historically it also included a size of the black widescreen side borders, making it a confusing parameter.

*See also:* [`System.ColorDepth`](System#systemcolordepth), [`Screen.Width`](Screen#screenwidth), [`Screen.Height`](Screen#screenheight),
[`Viewport.Width`](Viewport#viewportwidth), [`Viewport.Height`](Viewport#viewportheight)

---

### `System.ScrollLock`

```ags
readonly static bool System.ScrollLock;
```

Gets whether Scroll Lock is active on the player's system.

Note that when running your game under the debugger, the Scroll Lock key
will break out of the game into the debugger, so it is not advised that
you use it for any other purpose in your game.

Example:

```ags
if (System.ScrollLock)
{
    Display("The SCROLL LOCK light is on.");
}
```

will display a message if Scroll Lock is on.

*Compatibility:* Supported by **AGS 3.0.1** and later versions.

*See also:* [`System.CapsLock`](System#systemcapslock),
[`System.NumLock`](System#systemnumlock)

---

### `System.SupportsGammaControl`

```ags
readonly static bool System.SupportsGammaControl;
```

Gets whether the player's PC supports changing the screen's gamma
control settings.

This must return *true* before you try and change the
[`System.Gamma`](System#systemgamma) property. The situations in which
this will be supported are listed below.

**Cross-Platform Support**

Windows: **Full-screen only**<br>
Linux: **No**<br>
MacOS: **No**

Example:

```ags
if (System.SupportsGammaControl) {
    Display("We can change the system gamma level!");
}
```

will display a message if the system supports changing the gamma

*See also:* [`System.Gamma`](System#systemgamma)

---

### `System.Version`

*(Formerly known as `system.version`, which is now obsolete)*

```ags
readonly static String System.Version;
```

Returns the AGS engine version number. This could be useful from within
script modules in order to use features available on a particular engine
version, or work around any known bugs.

The string returned is the full version number, for example "2.71.833".

Example:

```ags
Display("AGS version: %s", System.Version);
```

will display the AGS version number

---

### `System.ViewportHeight`

**This property is obsolete since AGS 3.5.0. Use [`Screen.Height`](Screen#screenheight) instead.**

*(Formerly known as `system.viewport_height`, which is now obsolete)*

```ags
readonly static int System.ViewportHeight;
```

Returns the game's screen height in native coordinates, **not** accounting for the scaling applied by graphic renderer.
Note that before AGS 3.5.0 there was no differentiation between "game's viewport" (as in - full game's screen) and "room viewport", that's why [`Viewport.Height`](Viewport#viewportheight) is **not** a direct equivalent.

*See also:* [`Screen.Width`](Screen#screenwidth), [`Screen.Height`](Screen#screenheight),
[`Viewport.Width`](Viewport#viewportwidth), [`Viewport.Height`](Viewport#viewportheight)

---

### `System.ViewportWidth`

**This property is obsolete since AGS 3.5.0. Use [`Screen.Width`](Screen#screenwidth) instead.**

*(Formerly known as `system.viewport_width`, which is now obsolete)*

```ags
readonly static int System.ViewportWidth;
```

Returns the game's screen width in native coordinates, **not** accounting for the scaling applied by graphic renderer.
Note that before AGS 3.5.0 there was no differentiation between "game's viewport" (as in - full game's screen) and "room viewport", that's why [`Viewport.Width`](Viewport#viewportwidth) is **not** a direct equivalent.

*See also:* [`Screen.Width`](Screen#screenwidth), [`Screen.Height`](Screen#screenheight),
[`Viewport.Width`](Viewport#viewportwidth), [`Viewport.Height`](Viewport#viewportheight)

---

### `System.Volume`

*(Formerly known as `SetDigitalMasterVolume`, which is now obsolete)*<br>
*(Formerly known as `SetMusicMasterVolume`, which is now obsolete)*

```ags
static int System.Volume;
```

Gets/sets the overall system volume, from 0 to 100. This is the master
volume control, that affects all audio in the game. You would usually
attach this to a GUI Slider to enable the player to control the volume
from some sort of Control Panel GUI.

Example:

```ags
System.Volume = 80;
```

will set the overall output volume to 80.

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:* [`AudioChannel.Volume`](AudioChannel#audiochannelvolume),
[`Game.SetAudioTypeVolume`](Game#gamesetaudiotypevolume)

---

### `System.VSync`

*(Formerly known as `system.vsync`, which is now obsolete)*

```ags
static bool System.VSync;
```

Gets/sets whether AGS waits for the vertical retrace before rendering each frame.

When vsync is on, it can help to reduce the "tearing" effect that you can get when the screen scrolls. However, doing so will lock the
game frame rate to the monitor's refresh rate, which will mean you cannot reliably set a game speed higher than 60 fps.

Vertical sync is off by default. When the game starts engine reads vsync option value from the config file, if one exists.

Not all renderers support changing vsync at runtime. If current one does not, then trying to change `System.VSync` will have no effect.

Example:

```ags
if (System.VSync) {
    Display("Vertical retrace sync is enabled!");
}
```

will display a message if vsync is on.

---

### `System.Windowed`

*(Formerly known as `system.windowed`, which is now obsolete)*

```ags
static bool System.Windowed;
```

Gets/sets whether the game is running in a window (*true*) or
full-screen (*false*).

If you set this property at runtime, the game will try to switch to
alternate window mode. If it fails, it will return to previous one.

Example:

```ags
if (System.Windowed) {
    Display("Game is running in a window!");
}
```

will display a message if the game is running in a window

Example:

```ags
void on_key_press(eKeyCode keycode) {
    if (keycode == eKeyEscape) {
        System.Windowed = !System.Windowed;
    }
}
```

will switch from windowed to fullscreen mode and vice-versa whenever
player presses Escape key.

*Compatibility:* This property was read-only before **AGS 3.4.1**; it
can be set in **AGS 3.4.1** and later versions.
