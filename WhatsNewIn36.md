## What's new in AGS 3.6

AGS version 3.6 is another big change to AGS.
First of all, this release presents a SDL2-based engine. For about 2 decades the AGS engine was based on the Allegro 4 graphic library, which was discontinued somewhere in the early 2010s, so this is overdue.

Secondly, both the Editor and the Engine have full Unicode support now. This means that you may use any language whatsoever in scripts, object descriptions, and custom properties, as well as translations, - so long as you also provide proper Unicode fonts.

Then, we have expanded a list of platforms that the Editor can directly build the game for: now this includes Android and Web (Emscripten).

Below we'll look into these and other significant changes in more detail.

### Unicode support

AGS can now work in Unicode mode, but we also support ASCII mode for backward compatibility. We recommend using Unicode mode when starting new projects, regardless of the game language.

When in Unicode mode, all game texts and text files, such as scripts, are saved in UTF-8 format.

In the Editor the switch between the two modes is called **"Text format"** and is located in "General Settings".

**IMPORTANT:** This switch is an important choice and should not be played with at random. Whenever you change the text format the Editor will convert all your game files by loading them and saving them back using a new mode. If all your game texts were using the standard Latin alphabet (English, etc), then you should not see any difference, but if you had texts with extended Latin characters or any other non-Latin languages, - these will be converted between ANSI and UTF-8. We strongly recommend doing a full project backup before doing this though.

When in Unicode mode you may type your game texts in any language whatsoever. This refers to the following:
* Human texts in-game options: such as object descriptions, and so on;
* Values of String Custom properties and Global Variables;
* String literals in scripts (any text in double-quotes);
* Character literals in a script (a character in single quotes);
* Speech lines in dialog scripts;
* Translations.

Mixing multiple languages, even in the same string, is fine (but requires having fonts that can display all of these languages).

Where Unicode won't work (and is not supposed to):
* Script names of game objects: characters, guis, views, and so on;
* Names of Custom properties and Global Variables;
* Variable names and function names in script.

All the above must be written strictly in the Latin alphabet.

String variables and related functions should work seamlessly with Unicode texts. Functions that work with characters were changed to have arguments and return values as `int`, to be able to pass any Unicode character. This includes `AppendChar()`, `ReplaceChar()` and `String.Char[]` property. Also, `String.Format("%c", n)` will now be able to print Unicode characters.

The Translations are not affected by the game's setting and have an individual setting of their own. When you open a TRS file made by the new version, it will have an "#Encoding" option in the file's header. This is where you may write either "UTF-8" or "ASCII" by hand to tell the translation compiler (and the engine) how this translation should be interpreted. By default this should be "UTF-8", so you only need to change this if you need the translation to be in ASCII for some reason.
If you have imported an older project and the TRS file does not have this option, then you may add one yourself. Just remember that the TRS options must be preceded with a comment sign, like this:

    //#Encoding=UTF-8

Don't forget to save the TRS in UTF-8 with whatever text editor you are using, if you want it to keep Unicode texts intact.

The engine can run the game in both Unicode and ASCII modes. It uses a game setting to set up the mode on start, but when loading a translation it will switch to the translation's setting. This behavior is meant primarily to allow adding UTF-8 translations for older ASCII games.

**NOTE:** the engine cannot convert older ASCII games to Unicode at runtime, primarily because it does not know the exact codepage its texts were made in. So if you want your older game to fully work in Unicode, the proper way is to upgrade its project and compile the game anew.

### Changes to key input handling

As a part of the Unicode support, and also to make key handling in the script more consistent, the script callbacks for key input were extended.

First of all, the `on_key_press` function now supports a second optional argument `mod`. It's optional, so you don't have to declare it, but if you do then you'll get its benefits.

The extended function definition now is:

```ags
void on_key_press(eKeyCode key, int mod)
```

The `mod` argument describes which key modifiers were enabled when this exact key was pressed. It's different from, for example, using `IsKeyPressed()` in `repeatedly_execute` callback, because the engine may receive several key-presses between two game frames, while `IsKeyPressed` only tells the last state of the key. Using `mod` argument is 100% reliable.

But the `mod` contains a *set of flags*, and is slightly more complicated: as you should not use regular comparison (==, !=) with it, but a bitwise operator (&):

```ags
// these two conditions check that ctrl was pressed (these commands are equivalent)
if (mod & eKeyModCtrl)
if (mod & eKeyModCtrl != 0)

// these two conditions check that ctrl was NOT pressed (again, two variants of the same check)
if (!(mod & eKeyModCtrl))
if (mod & eKeyModCtrl == 0)
```

Here's the full list of mod constants:

constant | meaning
-- | --
eKeyModShiftLeft | left shift key
eKeyModShiftRight | right shift key
eKeyModShift | any of the shift keys
eKeyModCtrlLeft | left control key
eKeyModCtrlRight | right control key
eKeyModCtrl | any of the control keys
eKeyModAltLeft | left alt key
eKeyModAltRight | right alt key
eKeyModAlt | any of the alt keys  
eKeyModNum | numlock key
eKeyModCaps | capslock key

The engine now supports "new style" key handling mode and "old style" mode. There's a **"Use old-style key handling"** option in the "Backwards compatibility" group that lets you choose either old or new key handling mode. This will be "off" (meaning - new mode) by default but will be enabled (meaning - old mode) when you import older projects. If you like to use the new mode you'll need to disable this setting.
What is the difference between the two modes?

**Old (classic) key mode:**
 - lone mod keys (Ctrl, Alt, Shift) are not passed further into the engine nor script, thus `on_key_press` is not called for them;
 - key + mod combinations are merged into one key code for the `on_key_press` callback: e.g. `eKeyCodeCtrlA` and similar key code;

**New key mode:**
 - all keys are passed into the engine and `on_key_press` function as-is, one by one: for example, if you press Ctrl + Z, then you get **two** `on_key_press` calls, one with `eKeyCtrlLeft` and another with `eKeyZ` argument.
 - the new `on_text_input` script function will be active and receive **printable characters**;

In the new mode each key pressed triggers `on_key_press`, and when the pressed keys form a printable character an `on_text_input` function is called. This function must look simply like this:

```ags
void on_text_input(int ch)
```

Its argument (`ch`) contains a Unicode character's code. It may be used, for example, to append to a String, or add to the text field or label.

Similarly to the above, `dialog_options_key_press` had been expanded with the third `mod` argument:

```ags
void dialog_options_key_press(DialogOptionsRenderingInfo *info, eKeyCode keycode, int mod)
```

And there's now `dialog_options_text_input` callback for receiving unicode chars during the custom dialog options state:

```ags
void dialog_options_text_input(DialogOptionsRenderingInfo *info, int ch)
```

### New game package options

AGS now allows to package custom files into your game and read them in script. An option called "Package custom data folder" has been added to the [General Settings - Compiler](GeneralSettings#compiler). This option accepts a comma-separated list of subfolder names, which should be found in your game project's folder. All of the files in these subfolders will be added to the game package under their relative paths (e.g. "UserData/myfile.dat").

Any [File functions](File) that accept file paths can now refer to the game package files using the new `$DATA$` location tag. Virtually any packaged file may be queried this way, whether custom or regular game resource. These files may only be opened for reading however, never for writing.

For example, you may read your custom file from the package as:

```ags
File* f = File.Open("$DATA$/UserData/myfile.dat", eFileRead);
```

### Multiple speech voxes

AGS now supports having multiple speech vox files. This may be useful if, for example, you would like to provide separate voice-over packs for each language (translation) in your game. Creating additional voxes is done by creating subfolders in the "Speech" folder and placing the sound files there. AGS will then generate a file called "sp_NAME.vox" for each existing subfolder, where "NAME" is the subfolder's name.

In script you may switch between the speech packs using new `Game.ChangeSpeechVox()` function. Its value is not linked with the textual translation, which is set by `Game.ChangeTranslation()`. You may enable voice-over matching the language of the displayed text, or on contrary allow the player to choose different languages for the text and for the voice. For example:

```ags
Game.ChangeTranslation("Spanish");
Game.ChangeSpeechVox("English");
```

The commands above will change the textual translation to "Spanish", and the voice-over to the "sp_english.vox" pack.

### New sprite compression options

First of all, the old "Compress the sprite file" option in [General Settings](GeneralSettings) has been replaced with "Sprite file compression", which lets you choose one of the supported compression types. The previously commonly used sprite compression in AGS is called "RLE"; the "LZW" compression method is now new to the selection. It's possible that this list will be extended in the future.

Secondly, there's now an "Enable sprite storage optimization" option, which is enabled by default. This option permits the Editor to repack sprites in a different storage format, reducing their disk size whenever possible. This option may be used together with the sprite compression. Note that this _does not_ change how the sprites work in game, only how they are stored in the game files.

### Multiplatform support extended

The AGS Editor can now produce game builds for Android and Web (using Web/Emscripten port). Both require corresponding components installed alongside with the Editor. In addition, for Android builds you also have to install Android Studio and configure certain settings in the [Editor's Preferences](EditorPreferences) and your game's [General Settings](GeneralSettings). For more information see [Distributing your game](DistGame) and [Building for Android](BuildAndroid).

### TTF fonts behavior

Historically AGS applied a "fixup" to TTF fonts, that shifted letters down when drawn, which, in turn, also increased their graphical height. This behavior is now optional, available purely for backwards compatibility, both as a global and per-font setting called "TTF font adjustment". This setting has two values: "Do nothing" and "Resize ascender to the nominal font height". We recommend having it set to "Do nothing" at all times, except when using TTF fonts specifically created or adjusted for older versions of AGS.

Another global setting is "TTF fonts height used in the game logic". This option sets which value to use when calculating text height, line spacing etc. Its value may be either "Nominal height" or "Full graphical height". The difference is, that due to the font library's behavior, the font's graphical height may end up differently (usually larger) than the nominal height the font is imported with. While "Nominal height" is a historical default, it's up to your preference which setting to use.

A long time requested font feature that is now available is the option to set the thickness of the automatically generated outline of the text. The automatic font outline has always been drawn 1px wide, but that was not enough for high-resolution games. Now fonts have a property called "AutoOutlineThickness" which lets you set any outline width you need, and "AutoOutlineStyle" which toggles between "Squared" and "Rounded" corners.

### Improvement of Overlays

Overlays in AGS are currently the only "game objects" that can be created dynamically in script (there are also [Dynamic Sprites](DynamicSprite), but these cannot be shown without being assigned to *something*). That, and them being a very simple kind of object not requiring extra setup, makes them useful to create temporary visual effects or to create a custom object from ground up. Previously, however, overlays had a number of restrictions, and these are now removed in AGS 3.6.0.

1. Overlays are now unlimited in numbers. Previously there could only be up to 20 simultaneous overlays.
2. You can create both screen and room overlays. Room overlays are created using `Overlay.CreateRoomGraphical` and `Overlay.CreateRoomTextual`; they are like regular overlays but are positioned inside the room, in room coordinates, among room objects.
3. Overlays now have a ZOrder property, similar to GUIs. The z-order may be used to sort overlays among themselves and other game objects on the same layer: the screen overlays are sorted among GUIs, while room overlays are sorted among room objects, characters and walk-behinds.
4. Overlays now have a Graphic property that lets you change the overlay's sprite after it was created.
5. Overlays now have Width and Height properties, that let you freely scale existing Overlays, stretching or shrinking it.
6. The new Transparency property lets you make them translucent, similar to other objects in the game.

Please refer to the corresponding page under Scripting API documentation to learn details about these and other [Overlay's functions and properties](Overlay).

### MIDI playback

While MIDI music files are still supported, on Windows engine is no longer using a synthesizer provided by the operating system. Regardless of the OS, players will have to install a Soundfont and Timidity config if they want to hear MIDI soundtrack. Please refer to [MIDI Playback](MIDI-playback) article for more details.

### Discontinued features

There are a few engine functionalities that were either removed completely, or disabled temporarily for technical reasons.

First of all, _"Windows Game Explorer integration"_ **has been completely removed from AGS**. The Windows Game Explorer was never very popular, and it has been finally been deprecated by Microsoft in Windows 10. The "enhanced save game" feature, that allowed the Windows Explorer to display some meta-data, like screenshots for the AGS game saves, is deprecated since Windows 7. Therefore there are no good reasons to keep this functionality in the engine. Note that it's still possible to register your game in the "Windows Game Explorer" by writing a custom tool or script that does that. We won't cover how to do that in this manual though.

_Hqx graphic filters_ are discontinued. They were previously only usable with the Software renderer, and had performance issues. In theory they may be returned later if there's a demand for them, and if implemented in a more performant way.

_Windows-only DirectMedia video playback_ is discontinued. It allowed you to play AVI/MPEG video files, but was implemented using the DirectMedia interface, thus was supported only on Windows and required particular codecs to be installed on the system. It is probable that these video formats will be supported again at some point in the future. The OGG/OGV video format is the only working format at the moment, and we recommend using it for now.

### Deprecated / replaced script functions

obsolete function | replace with
-- | --
Object.IgnoreScaling | Object.ManualScaling

### System limits update

* Max number of AudioChannels is now 16 (was 8).
* Max number of Room Objects per room is now 256 (was 40).
* Removed game Cursors limit (was 20).
* Removed Overlays limit (was 20).
* Removed limit of simultaneous Button animations (was 15).
* Removed limit of Character followers (was 30).
