## Engine Plugin Run-time API

### Run-time events

#### AGS_EngineStartup
```cpp
DLLEXPORT void AGS_EngineStartup (IAGSEngine *lpGame);
```

Called by the game engine when the game is loading. This function is called once AGS has loaded everything it needs to into memory, but before it loads the global script.

Your plugin should use this opportunity to register any text script functions that it provides, and to request hooks for any game events that it wants to handle.

Once this function completes, the plugin will only regain program control when one of its text script functions is called, or when one of the hooks it registered is executed.

**NOTE:** This function is called before the graphics subsystem is started, so functions like GetScreenDimensions and GetGraphicsDriverID will not work if called from this event.

#### AGS_EngineShutdown
```cpp
DLLEXPORT void AGS_EngineShutdown();
```

Called by the game engine just before it exits. This gives you a chance to free any memory and do any cleanup that you need to do before the engine shuts down.

_(Shutdown is only called by engine interface version 13 and above.)_

#### AGS_EngineOnEvent
```cpp
DLLEXPORT int AGS_EngineOnEvent (int event, int data);
```

Called by the game engine when one of the events which the plugin has requested via RequestEventHook is triggered.  
`data` is an event-specific parameter - see RequestEventHook below for more information.

Return 0 to tell the engine to continue processing the event as normal. Return 1 to tell the engine not to call any other handlers, whether in other plugins or the text script, for this event.

For example, if you were handling a Keypress event, and you did something in response to the keystroke, you may wish to return 1 to stop anything else also dealing with the keystroke.

**IMPORTANT:** Only return 1 when absolutely necessary since it may interfere with other plugins operation.

#### AGS_EngineDebugHook
```cpp
DLLEXPORT int AGS_EngineDebugHook(const char * scriptName, int lineNum, int reserved);
```

Called by the game engine when a new line of text script is about to be executed. This will only be called if you have requested the AGSE_SCRIPTDEBUG event with the RequestEventHook function (see below).

`scriptName` contains which script is running (either "Global script" or "Room XX script" (where XX is the room number)). `lineNum` contains the line number that is about to be run.

This callback is designed to allow you to create some form of script debugger using a plugin. Note that just having this hook enabled will cause a performance hit in the game - so if the plugin is not actively debugging, it should use `UnregisterEventHook` to unregister itself until such time as it needs to know again.

This callback is also called with `lineNum` set to 0 when a script finishes running.

Return 1 if you handled the debug, or 0 to allow other plugins to have a go.

_(Supported by engine interface version 13 and above)_

#### AGS_EngineInitGfx
```cpp
DLLEXPORT void AGS_EngineInitGfx(const char *driverID, void *data);
```

Called by AGS just before the graphics driver is initialized. This allows you to make changes to how the driver starts up.

`driverID` is the ID of the graphics driver in use. This can either be "D3D9" if the player is using the Direct3D driver, or "DX5" if they are using the standard DirectDraw driver.

If `driverID` is "D3D9", then the `data` parameter is a pointer to the `D3DPRESENT_PARAMETERS` structure that AGS is about to pass into CreateDevice. If you need to enable extra features such as the Auto Depth Stencil, then you can do so by changing the member variables at this point.

For other graphic drivers then the `data` parameter will currently be passed as `NULL`.

**NOTE:** This event is called just as the graphics subsystem is starting, so functions that like GetScreenDimensions that rely on the screen being initialized will not work if called from this event.

**NOTE:** This event can be called more than once. It is called every time AGS tries to initialize the screen, so if the first resolution is not supported for example then it will get called again as AGS retries.

_(Supported by engine interface version 23 and above)_

### The IAGSEngine interface

This interface is provided to the plugin as the way of calling back to the editor. It provides the following methods.

**IMPORTANT:** Very little error checking is performed on these functions for performance reasons, as they provide direct entry into AGS - therefore, mistakes in your code can crash the engine.

The text script currently supports `char`, `short` and `int` types. These are 1, 2 and 4 byte integers respectively. Text script strings are implemented as standard C-style `char*` pointers, but with preallocated buffers of 200 bytes per string. Therefore, if you write a function which takes a string from the text script, make sure that if you add anything to it you don't write past 200 characters, or problems will result.

#### IAGSEngine.version
```cpp
int version;
```

Specifies the interface version. You should check this to determine what version of the engine is being used, because certain interface methods will have been added in later versions, and attempting to use them with an earlier version will crash the system.

AGS v2.51 (the first version to support plugins) was distributed with interface version **7**, so you can reasonably expect that as a minimum for your plugin.

#### IAGSEngine.AbortGame
```cpp
void AbortGame (const char * reason);
```

Aborts the game, presenting the standard AGS "An error has occured, please correct the problem" message box to the user, with the error description being the string supplied.

To indicate that it was a user error (for example, they provided an invalid parameter to your text script function), prepend a ! to the message. For example,

```cpp
AbortGame ("!MyScriptFunction: Not a valid color");
```

This will use the "Please contact the game author for support" heading message.

If you need to exit the game, you should always use this instead of the standard VC++ exit() functions.

_Added in version: 1_

#### IAGSEngine.GetEngineVersion
```cpp
const char* GetEngineVersion ();
```

Returns the full build number of the engine, for example "2.51.400". You can use this with strcmp if your plugin requires a specific engine version.

_Added in version: 1_

#### IAGSEngine.RegisterScriptFunction
```cpp
void RegisterScriptFunction (const char *name, void *address);
```

Allows you to add your own text script functions to AGS. `name` is the name of the function as the script knows it, for example "AddNumbers". `address` is a pointer to your function that implements the command. For example:

```cpp
int AddNumbers (int a, int b) { return a + b; }
...
RegisterScriptFunction ("AddNumbers", AddNumbers);
```

Note that it is possible to override the built-in functions with this command. If you do, for example,
```cpp
RegisterScriptFunction ("GetLocationType", AddNumbers);
```
then any calls from the script to `GetLocationType` will be intercepted by your plugin (and in this case a stupid result returned).

**IMPORTANT:** If you want to override built-in functions, make sure that yours have identical parameters, otherwise the stack could get messed up and crash the engine.

AGS 2.7 and later support object-based scripting. In order to export an object from your plugin, you'll need to include its struct definition in the RegisterScriptHeader editor call. Then, you export functions as follows:

```cpp
int PluginObject_DoSomething(PluginObject *obj, int a) {
  return a + 10;
}
...
RegisterScriptFunction("PluginObject::DoSomething^1",
                         PluginObject_DoSomething);
```

In other words, the function takes the object as its first parameter. The exported name is `"structname::functionname^numparams"`, where `numparams` is the number of parameters it takes (excluding the object itself).

_Added in version: 1_

#### IAGSEngine.GetWindowHandle
```cpp
HWND GetWindowHandle ();
```

**IMPORTANT:** This function is only supported on Windows.

Returns the window handle of the main game window.

_Added in version: 1_

#### IAGSEngine.GetGraphicsDriverID
```cpp
const char* GetGraphicsDriverID();
```

Returns an ID string representing the current graphics driver. This can either be "D3D9" if the player is using the Direct3D driver, or "DX5" if they are using the standard DirectDraw driver.

If the engine interface version is less than 19, this function is not available; however, in that case you can safely assume that the DX5 driver is in use since earlier versions of AGS only supported that driver.

**IMPORTANT:** Your `AGS_EngineStartup` event is called before the graphics system is initialized. Therefore, you cannot call `GetGraphicsDriverID` from within `AGS_EngineStartup` as no driver is loaded at that point.

**NOTE:** When the Direct3D driver is in use, the screen is redrawn each frame using 3D accelerated blits, and therefore there is no virtual screen. You can only manipulate the virtual screen when running with the DX5 driver.

_Added in version: 19_

#### IAGSEngine.GetDirectDraw2
```cpp
LPDIRECTDRAW2 GetDirectDraw2 ();
```

**IMPORTANT:** This function is only supported on Windows.

Returns the main IDirectDraw2 interface that the game is using.

This function is only supported if the player is using the DX5 graphics driver.

_Added in version: 1_

#### IAGSEngine.GetBitmapSurface
```cpp
LPDIRECTDRAWSURFACE2 GetBitmapSurface (BITMAP *bmp);
```

**IMPORTANT:** This function is only supported on Windows.

Returns the IDirectDrawSurface2 interface associated with the specified bitmap.

This function is only supported if the player is using the DX5 graphics driver.

**IMPORTANT:** This is the Allegro BITMAP structure, not the Win32 GDI BITMAP. Do not try and use a win32 BITMAP with this function, as it will crash the system.

_Added in version: 1_

#### IAGSEngine.GetScreen
```cpp
BITMAP * GetScreen ();
```

Returns a reference to the main screen bitmap (as an _Allegro_ `BITMAP`). You can use this via DDraw with the `GetBitmapSurface` method, or use the various AGS drawing functions which are documented later.

This function is only supported if the player is using the DX5 graphics driver.

_Added in version: 1_

#### IAGSEngine.RequestEventHook
```cpp
void RequestEventHook (int event);
```

Requests that the engine calls the plugin's `AGS_EngineOnEvent` function when the specified event occurs in future. Possible values are:

##### IAGSEngine event AGSE_KEYPRESS

triggered when the user presses a key on the keyboard.

**data value:** ASCII value of keystroke, as text script on_key_press (but lower case a-z are passed as 97..122)

##### IAGSEngine event AGSE_MOUSECLICK

triggered when the user clicks a mouse button.

**data value:** mouse button pressed (1=left, 2=right, 3=middle)

##### IAGSEngine event AGSE_POSTSCREENDRAW

triggered every frame after the entire game has been drawn each frame, but before the mouse cursor is painted on top.

**data value:** _\*\* See Note 1 below_

##### IAGSEngine event AGSE_PRESCREENDRAW

_(version 4 and later only)_

triggered every frame right after the room background has been drawn, but before anything else.

**data value:** _\*\* See Note 1 below_

##### IAGSEngine event AGSE_SAVEGAME

_(version 5 and later only)_

triggered when the game position is saved. You can then use the interface `FWrite` function to write your own data. The script engine is in an invalid state at this point, so do not call any script functions.

**data value:** file handle of save game

##### IAGSEngine event AGSE_RESTOREGAME

_(version 5 and later only)_

triggered when the game position is loaded. You then use the `FRead` function to read the data back. You **must** read back the same number of bytes as you wrote. The script engine is in an invalid state at this point, so do not call any script functions.

**data value:** file handle of save game

##### IAGSEngine event AGSE_PREGUIDRAW

_(version 6 and later only)_

triggered every frame once the room viewports have been constructed, but before the GUIs and screen overlays have been drawn over.

**data value:** _\*\* See Note 1 below_

##### IAGSEngine event AGSE_LEAVEROOM

_(version 6 and later only)_

triggered when the player leaves the current room, after the Player Leaves Screen event but before the screen fades out.

**data value:** old room number

##### IAGSEngine event AGSE_ENTERROOM

_(version 6 and later only)_

triggered when the player enters a new room, before the screen fades in.

**data value:** new room number

##### IAGSEngine event AGSE_TRANSITIONOUT

_(version 6 and later only)_

triggered when the screen is about to fade out as the player changes rooms. If you return 1, AGS will not perform its screen transition.

**data value:** _(unused, currently 0)_

##### IAGSEngine event AGSE_TRANSITIONIN

_(version 6 and later only)_

triggered when the screen is about to fade in as the player changes rooms. If you return 1, AGS will not perform its screen transition.

**data value:** _(unused, currently 0)_

##### IAGSEngine event AGSE_FINALSCREENDRAW

_(version 12 and later only)_

triggered every frame after the mouse cursor is painted and immediately before the game is presented on the screen.

**data value:** _\*\* See Note 1 below_

##### IAGSEngine event AGSE_TRANSLATETEXT

(version 12 and later only)

triggered whenever there is some in-game text that might need translating. If you return 0, AGS will translate it as usual; otherwise, you can return a `char *` pointer with the replacement text (the return pointer must point to some static memory since AGS does not take a copy)

**data value:** the text to translate (cast this parameter to `const char *`)

##### IAGSEngine event AGSE_SCRIPTDEBUG

_(version 13 and later only)_

triggers the AGS\_ EngineDebugHook function every time the game script advances to a new line.

**data value:** _N/A_

##### IAGSEngine event AGSE_SPRITELOAD

_(version 18 and later only)_

triggered whenever a sprite is loaded into memory. This allows you to modify sprites as soon as they are loaded into the sprite cache. Do **not** attempt to access any other sprites from this event, since the sprite cache may be in an inconsistent state.

**data value:** sprite number

##### IAGSEngine event AGSE_PRERENDER

_(version 21 and later only)_

triggered every frame after the game logic has run but before anything is drawn.

**data value:** _(unused, currently 0)_

##### IAGSEngine event AGSE_PRESAVEGAME

_(version 24 and later only)_

triggered just before a save game is created. The script engine is still working at this point, so you can perform any tasks that you need to prior to the game data being saved

**data value:** _(unused, currently 0)_

##### IAGSEngine event AGSE_POSTRESTOREGAME

_(version 24 and later only)_

triggered after a save game has been restored. By the time this event occurs, all the game data has been restored and the script engine is operational again.

**data value:** _(unused, currently 0)_

##### IAGSEngine event AGSE_POSTROOMDRAW

_(version 26 and later only)_

triggered every frame *per each room camera* once the room contents have been drawn. This event allows to draw over whole room graphic layer.

**data value:** _\*\* See Note 1 below_

**NOTE:** Each frame (or _game loop_), AGS redraws the screen from scratch. The sequence is as follows:

1.  Call `AGSE_PRERENDER`
2.  Update in-memory cache of current character and object images
3.  Begin constructing scene (Direct3D renderer calls `BeginScene`)
4.  Per each room viewport-camera pair:

    4.1.  Draw current background scene<br>
    4.2.  Call `AGSE_PRESCREENDRAW`<br>
    4.3.  Draw room objects, characters and room overlays<br>
    4.4   Call `AGSE_POSTROOMDRAW`<br>

7.  Call `AGSE_PREGUIDRAW`
8.  Draw GUIs and screen overlays
9. Call `AGSE_POSTSCREENDRAW`
10. Draw mouse cursor and finalize.
11. Call `AGSE_FINALSCREENDRAW`
12. Ends constructing scene and presents new frame on screen (Direct3D renderer calls `EndScene`)

**\*\* Note 1**: The `data` parameter to these events is 0 when using the Software driver; when using the D3D9 driver it is a pointer to the IDirect3DDevice9 that is currently being used for rendering. You can render extra primitives by calling all the normal methods on this interface.  
(Interface version 20 and above only).

_Added in version: 2_

#### IAGSEngine.GetSavedData
```cpp
int GetSavedData (char *buffer, int bufsize);
```

Gets the plugin data that was originally saved with AGS_EditorSaveGame. If your plugin had options that the user could set in the editor, this is how to retrieve them.

`buffer` is a byte array of `bufsize`, which the engine will copy the saved data into. It returns the number of bytes actually used.

_Added in version: 2_

#### IAGSEngine.GetVirtualScreen
```cpp
BITMAP * GetVirtualScreen ();
```

Returns a reference to the virtual screen bitmap (as an _Allegro_ BITMAP) which you may draw upon.

There's a lot of difference depending on a active graphics renderer. If the game is being run using Software renderer, then this function will return the actual virtual screen of the game, with all the game frame drawn on it. In such case you might both "read" game frame's contents from it, and perform blending by combining pixels of your images with the game's image underneath.

On the other hand, if this is a texture-based renderer (Direct3D, OpenGL), then this function will return a "stage screen", which is an empty screen filled with nothing but transparent pixels. After you are done drawing on it, this "stage screen" will be added to the collection of textures to be rendered. The position of this texture among other sprites depends on the render stage event you called this function upon, such as AGSE_PRESCREENDRAW, AGSE_PREGUIDRAW and so forth.

_Added in version: 3_


#### IAGSEngine.GetRawBitmapSurface
```cpp
unsigned char ** GetRawBitmapSurface (BITMAP *bmp);
```

Gets a reference to the linear bank of memory that contains the surface of `bmp`. This allows you to directly plot pixels and so forth, simply by writing to the surface as an array.

This function locks the surface for you, which means you **must** call ReleaseBitmapSurface to release it when you are done. Failure to do so will most likely hang the engine.

**IMPORTANT:** If the bitmap color depth is 15/16 bit, the return value should be cast to `unsigned short**`.  
If the bitmap color depth is 32 bit, the return value should be cast to `unsigned long**`.  
If the bitmap color depth is 24 bit, the return value needs to be accessed in blocks of 3 bytes. The best method of doing this is left up to you.

**IMPORTANT:** If you change any areas of the virtual screen bitmap via this raw surface, you should call `MarkRegionDirty` with the affected area, so that AGS redraws that part of the screen.

**IMPORTANT:** **Be careful** when using this surface, since writing outside it will crash the engine. Make sure you use `GetBitmapDimensions` to find out how big the bitmap is.

**IMPORTANT:** This function cannot be used with the real screen bitmap - it can only be used with the virtual screen and any other bitmaps you may have access to.

_Added in version: 3_

#### IAGSEngine.ReleaseBitmapSurface
```cpp
void ReleaseBitmapSurface (BITMAP *bmp);
```

Releases a bitmap previously locked with `GetRawBitmapSurface`.

_Added in version: 3_

#### IAGSEngine.GetScreenDimensions
```cpp
void GetScreenDimensions (int *width, int *height, int *coldepth);
```

Fills in the supplied variables with the game's **native** screen resolution. You can pass any of the parameters as NULL if you don't want to know them. Color depth is reported in bits (8, 16 or 32).

**NOTE:** this is native resolution of the game, NOTE the final display mode. For example, 320x200 games may be run scaled up to 1920x1080, and 8-bit and 16-bit games may be displayed in 32-bit display mode. This function only tells the game's internal parameters, and therefore is mostly useful if your plugin is doing drawing over a "virtual screen" bitmap (see [GetVirtualScreen()](EnginePluginRun-timeAPI#iagsenginegetvirtualscreen)).

_Added in version: 3_

#### IAGSEngine.DrawText
```cpp
void DrawText (int x, int y, int font, int color, char *text);
```

Writes the string `text` to the current virtual screen at (`x`, `y`), using game font number `font` in color `color`.

**NOTE:** The `x` and `y` parameters are in actual screen co-ordinates, unlike the text script commands which always use 320-scale co-ordinates. You will need to use `GetScreenDimensions` and multiply up the co-ordinates accordingly if you wish to draw to a specific location.

_Added in version: 3_

#### IAGSEngine.GetMousePosition
```cpp
void GetMousePosition (int *x, int *y);
```

Fills the supplied variables with the current mouse cursor co-ordinates. Note that these are real screen size co-ordinates, and so at 640x480 resolution these variables can go up to 640 and 480, unlike the text script system.

_Added in version: 3_

#### IAGSEngine.GetBitmapDimensions
```cpp
void GetBitmapDimensions (BITMAP *bmp, int *width, int *height, int *coldepth);
```

Fills in the supplied integer variables with the properties of the specified bitmap. You can pass any of the last three parameters as NULL if you don't want to know them.

`width` is the width of the bitmap, in pixels. `height` is the height of the bitmap, in pixels. `coldepth` is the color depth of the bitmap. Note that this is not necessarily the same as the game color depth, since it is possible to use 256-color backgrounds in a hi-color game. Before doing any direct bitmap drawing, you should check this to make sure that you cast the pointer correctly.

_Added in version: 4_

#### IAGSEngine.GetNumBackgrounds
```cpp
int GetNumBackgrounds ();
```

Returns the number of background frames that the current room has (a normal room will return 1).

_Added in version: 4_

#### IAGSEngine.GetCurrentBackground
```cpp
int GetCurrentBackground ();
```

Returns the number of the currently displayed background frame. (0 is the default frame, 1 the first animated background, and so forth).

_Added in version: 4_

#### IAGSEngine.GetBackgroundScene
```cpp
BITMAP * GetBackgroundScene (int frame);
```

Returns a reference to the specified background frame (0 is the default frame, 1 the first animated background, and so on).

_Added in version: 4_

#### IAGSEngine.GetCurrentRoom
```cpp
int GetCurrentRoom ();
```

Returns the room number that the game is currently in.

_Added in version: 4_

#### IAGSEngine.FRead
```cpp
int FRead (void *buffer, int length, int handle);
```

Similar to the C function `fread`, this reads `length` bytes of `buffer` from file `handle`, returning the number of bytes actually read. The handle you use here **must** have been passed to you by AGS. For any other I/O, you can simply use the standard C functions.

_Added in version: 5_

#### IAGSEngine.FWrite
```cpp
int FWrite (void *buffer, int length, int handle);
```

Similar to the C function `fwrite`, this writes `length` bytes of `buffer` to file `handle`, returning the number of bytes actually written. The handle you use here **must** have been passed to you by AGS. For any other I/O, you can simply use the standard C functions.

_Added in version: 5_

#### IAGSEngine.SetVirtualScreen
```cpp
void SetVirtualScreen (BITMAP *bmp);
```

Sets `bmp` as the current virtual screen, so that all drawing routines will draw onto it. You should restore the virtual screen to its original setting before returning from your plugin function. (Use GetVirtualScreen first to save the old reference).

_Added in version: 5_

#### IAGSEngine.DrawTextWrapped
```cpp
void DrawTextWrapped (int x, int y, int width, int font, int color, const char *text);
```

Prints `text` to the current virtual screen at (`x`, `y`), using `width` pixels wide to do so, wrapping the text to ensure it does not exceed the width.

_Added in version: 5_

#### IAGSEngine.BlitBitmap
```cpp
void BlitBitmap (int x, int y, BITMAP *bmp, int masked);
```

Draws graphic `bmp` to the current virtual screen at (`x`,`y`).  
If `masked` is 0, the image is copied straight across.  
If `masked` is 1, then transparent pixels are skipped.

_Added in version: 5_

#### IAGSEngine.LookupParserWord
```cpp
int LookupParserWord (const char *word);
```

Checks the text parser dictionary for `word`, and returns its word number. All synonyms of a word have the same word number, as shown in the Text Parser pane in the AGS Editor.  
Returns -1 if the word was not in the dictionary.

_Added in version: 5_

#### IAGSEngine.PollSystem
```cpp
void PollSystem ();
```

Updates the internally stored mouse co-ordinates, and checks for mouse clicks and keypresses (and triggers the relevant plugin events). Also runs the music decoder to make sure that there is enough data to play.

Call this function if you have a long plugin function which takes more than half a second to execute, or if you need to get mouse and keypress events while still inside your function.

**NOTE:** AGS is single-threaded; this means that any background music has to be played by updating the buffer at regular intervals. Therefore, if your function takes a while to execute you should call this to make sure the music doesn't skip.

_Added in version: 5_

#### IAGSEngine.GetNumCharacters
```cpp
int GetNumCharacters ();
```

Returns the number of characters in the current game. Valid character ID's range from 0 to the return value minus 1.

_Added in version: 6_

#### IAGSEngine.GetCharacter
```cpp
AGSCharacter * GetCharacter (int charid);
```

Returns the character state struct for character `charid`. This struct is documented in the header file, but is mostly identical to the `character[]` text script variable.

**IMPORTANT:** Beware when modifying character state structs. There are some caveats to watch out for, such as altering the X and Y values while the character is moving.  
Also, remember that all view numbers stored are **1 less** than the numbers shown in the editor and used in the text script.

_Added in version: 6_

#### IAGSEngine.GetGameOptions
```cpp
AGSGameOptions * GetGameOptions ();
```

Returns a reference to the game state structure. This is similar to the text script `game.` variables.

**NOTE:** One handy member here is the `AGSGameOptions.fast_forward` variable. This is set to 1 while a cut-scene is being skipped, so if you have any lengthy graphical effects you should not perform them if this is set. All operations which update any game state **must** still be performed however, or skipping the cut-scene will desync the game.

_Added in version: 6_

#### IAGSEngine.GetPalette
```cpp
AGSColor * GetPalette ();
```

Returns a reference to the current palette. This is an array of 256 elements of the `AGSColor` struct.

**NOTE:** In a game with animating backgrounds, the palette can change quite often, so you should generally only rely on the palette remaining consistent for the length of your function.

_Added in version: 6_

#### IAGSEngine.SetPalette
```cpp
void SetPalette (int start, int finish, AGSColor *palette);
```

Sets the current screen palette to `palette`, which must be an array of 256 `AGSColor` structs. The elements from `start` to `finish` are updated. Pass start as 0 and finish as 255 to update the entire palette.

_Added in version: 6_

#### IAGSEngine.GetPlayerCharacter
```cpp
int GetPlayerCharacter ();
```

Returns the number of the current player character.

_Added in version: 7_

#### IAGSEngine.GetNumObjects
```cpp
int GetNumObjects ();
```

Returns the number of objects in the current room.

_Added in version: 7_

#### IAGSEngine.GetObject
```cpp
AGSObject * GetObject (int number);
```

Returns the state structure for object `number`.

**IMPORTANT:** Unlike the character structs, object state pointers are not constant throughout the game. Any time the room changes, these structs get reallocated. Therefore, do not keep a pointer that you get with this command after your function ends.

_Added in version: 7_

#### IAGSEngine.RoomToViewport
```cpp
void RoomToViewport (int *x, int *y);
```

Converts the room co-ordinates (`x`, `y`) into current viewport co-ordinates. The input co-ordinates are room co-ordinates using the 320x200-style scheme that the text script uses and that object and character co-ordinates are stored as.

When you call this function, the variables you pass will be converted to hold the equivalent viewport co-ordinates. These are real resolution-sized co-ordinates within the current viewable area of the screen.

**NOTE:** You should check the resulting co-ordinates with the screen size to determine whether they are currently visible on the screen.

_Added in version: 7_

#### IAGSEngine.ViewportToRoom
```cpp
void ViewportToRoom (int *x, int *y);
```

Converts the viewport co-ordinates (`x`, `y`) into room co-ordinates. The input co-ordinates are screen resolution scale co-ordinates within the current viewable area of the room.

When you call this function, the variables you pass will be converted to hold the equivalent room co-ordinates, such as stored in the character and object structs, and used in the text script.

_Added in version: 7_

#### IAGSEngine.GetSpriteGraphic
```cpp
BITMAP * GetSpriteGraphic (int slot);
```

Returns the bitmap with the graphic for sprite slot number `slot`.

**NOTE:** This method will automatically load the sprite from disk into the sprite cache if it is not already present.

**IMPORTANT:** The AGS Sprite Cache may destroy any sprite from memory at any time. Therefore, **never** keep a pointer obtained with this command. Always call this method again if you need to perform multiple operations with the bitmap.

_Added in version: 7_

#### IAGSEngine.CreateBlankBitmap
```cpp
BITMAP * CreateBlankBitmap (int width, int height, int coldepth);
```

Creates a blank bitmap sized `width` x `height` pixels at `coldepth`-bit color. The bitmap will initially be filled with the transparent color for the color depth.

Returns NULL if the bitmap could not be created. You should always check the return value to avoid crashes.

**NOTE:** To draw onto the bitmap either use `GetRawBitmapSurface`, or the `SetVirtualScreen` command followed by one of the drawing commands such as `DrawText` or `BlitBitmap`.

**IMPORTANT:** The bitmap will remain in memory until you explicitly free it with `FreeBitmap`. Use this command sparingly, as each bitmap you create eats into the engine's available memory.

_Added in version: 7_

#### IAGSEngine.FreeBitmap
```cpp
void FreeBitmap (BITMAP *bmp);
```

Frees the bitmap `bmp` from memory.

**IMPORTANT:** **Only** use this command with bitmaps you created with `CreateBlankBitmap`. Using it with an AGS system bitmap, such as one obtained with `GetVirtualScreen`, `GetScreen` or `GetSpriteGraphic` will crash the engine.

_Added in version: 7_

#### IAGSEngine.GetRoomMask
```cpp
BITMAP * GetRoomMask (int which);
```

Returns a reference to the requested mask for the current room. `which` is one of the following values:
`MASK_WALKABLE`  
`MASK_WALKBEHIND`  
`MASK_HOTSPOT`  
`MASK_REGIONS`

The walk-behind mask is exactly the same size as the room. The walkable area, hotspot and region masks can be the size of the current room if a Mask Resolution of 1:1 was set in the Editor when making the game, but it can be smaller if a different resolution was set.

**All** of the masks are bitmaps with 8-bit color depth, irrespective of what color depth the game is running at.

**IMPORTANT:** The area masks are re-allocated when the player changes rooms, so do **not** keep a pointer returned by this function longer than you need it.

_Added in version: 8 (MASK_REGIONS requires version 11)._

#### IAGSEngine.GetViewFrame
```cpp
AGSViewFrame* GetViewFrame (int view, int loop, int frame);
```

Returns the view frame structure for the specified frame. `view` is the number you get from the editor (that's used in `Character.ChangeView` and so forth), not the view minus 1.

- **Interface version 10 and later:**
  If `frame` is not valid (e.g. there aren't that many frames in the specified loop), returns NULL.
- **Interface version 9:**
  If `frame` is not valid (e.g. there aren't that many frames in the specified loop), AGS will exit with an error message.

If `view` or `loop` are invalid, AGS will exit with an error message.

_Added in version: 9_

#### IAGSEngine.GetWalkbehindBaseline
```cpp
int GetWalkbehindBaseline (int area);
```

Returns the baseline for walk-behind area number `area`. This is always in character-resolution, i.e. the maximum value is 200 in all resolutions.

_Added in version: 9_

#### IAGSEngine.GetScriptFunctionAddress
```cpp
void* GetScriptFunctionAddress (const char *funcName);
```

Returns the memory address of the text script exported function `funcName`. This allows you to call any of the functions which AGS makes available to the text scripts.

Returns NULL if you specify a function that doesn't exist.

The `funcName` is formed using following syntax:
* For regular, global functions: `FunctionName^N`;
* For struct's member functions: `StructName::FunctionName^N`;
* For struct's attribute: `StructName::get_AttributeName`, `StructName::set_AttributeName`.
* For struct's indexed (array) attribute: `StructName::geti_AttributeName`, `StructName::seti_AttributeName`.

The `^N` suffix is specifying the expected number of arguments, and is useful if there are several variants of the same function with shorter and longer argument list. It may be omitted, but that is only safe if the script function does not have variants.

Attribute's getters and setters do not need `^N`, because their argument list is always exact:
* Regular attribute's getter has no args; setter has 1 arg: the new value;
* Indexed attribute's getter has 1 arg, which is the index; setter has 2 args: the index and the new value.

**IMPORTANT:** You **must** cast the result to the correct function type in order to call it - i.e. you must know how many parameters the function expects, and call it appropriately. Failure to do so will corrupt the stack and likely crash the engine.

**IMPORTANT:** If you specify the name of an actual function in the script, for example repeatedly_execute, it will return an address - however, this will be a script address and attempting to call it will crash the game. **Only** use this feature to obtain the address of AGS's script functions as documented in the manual.

_Added in version: 9_

#### IAGSEngine.GetBitmapTransparentColor
```cpp
int GetBitmapTransparentColor (BITMAP *bmp);
```

Returns the pixel color which is skipped when rendering the bitmap. This is 0 in 256-color bitmaps and RGB (255,0,255) in hi-color bitmaps, so this function returns the pixel value to check for if you need to know.

_Added in version: 9_

#### IAGSEngine.GetAreaScaling
```cpp
int GetAreaScaling (int x, int y);
```

Returns the walkable area scaling level at the specified co-ordinates. This ranges from 5 to 200, and specifies the scaling percentage, as set in the editor. The co-ordinates are specified as room co-ordinates.

_Added in version: 9_

#### IAGSEngine.IsGamePaused
```cpp
int IsGamePaused ();
```

Equivalent to the text script `IsGamePaused` function. This returns non-zero if the game is paused, or 0 if it is not paused.

_Added in version: 9_

#### IAGSEngine.GetRawPixelColor
```cpp
int GetRawPixelColor (int color);
```

Converts `color`, as specified in the AGS Editor, to the appropriate raw pixel color for the current graphics mode.

If the user is running 8-bit color, the value is returned unchanged.

If the user is running 16-bit color, the value is returned unchanged _unless_ it is less than 32, in which case it is converted to be the appropriate locked color from the editor.

If the user is running 15-bit color, the value is converted to an appropriate 15-bit RGB pixel value.

_Added in version: 10_

#### IAGSEngine.GetSpriteWidth
```cpp
int GetSpriteWidth (int slot);
```

Returns the width, in pixels, of sprite number `slot`.

This is faster than using `GetBitmapDimensions` with `GetSpriteGraphic`, since the image does not need to be retrieved from the sprite cache.

_Added in version: 11_

#### IAGSEngine.GetSpriteHeight
```cpp
int GetSpriteHeight (int slot);
```

Returns the height, in pixels, of sprite number `slot`.

This is faster than using `GetBitmapDimensions` with `GetSpriteGraphic`, since the image does not need to be retrieved from the sprite cache.

_Added in version: 11_

#### IAGSEngine.GetTextExtent
```cpp
void GetTextExtent (int font, const char *text, int *width, int *height);
```

Calculates the width and height, in pixels, of displaying `text` in game font number `font`.

`width` or `height` can be NULL if you only want to know the other one.

_Added in version: 11_

#### IAGSEngine.PrintDebugConsole
```cpp
void PrintDebugConsole (const char *message);
```

This function used to prints `message` to the in-game debug console (which the user can pop up with the ~ key). But that console was poorly designed and removed in **AGS 3.6.1**. Instead this function prints directly to the engine log now. Prefer to use [IAGSEngine.Log](EnginePluginRun-timeAPI#iagsenginelog).

_Added in version: 11_

#### IAGSEngine.IsChannelPlaying
```cpp
int IsChannelPlaying (int channel);
```

Equivalent to the text script function of the same name, returns 1 if the channel is currently in use, 0 if not.

_Added in version: 11_

#### IAGSEngine.PlaySoundChannel
```cpp
void PlaySoundChannel (int channel, int soundType, int volume, int loop, const char *filename);
```

Plays a new sound on the sound channel number `channel`. `volume` can be from 0 to 255, `loop` is 0 or 1.

`soundType` determines which sound driver is used to load the file:

|    _soundType_   |                                        |
| ---------------- | -------------------------------------- |
| `PSND_WAVE`      | Uncompressed .WAV file                 |
| `PSND_MP3STREAM` | MP3, streamed from disk                |
| `PSND_MP3STATIC` | MP3, loaded into memory before playing |
| `PSND_OGGSTREAM` | OGG, streamed from disk                |
| `PSND_OGGSTATIC` | OGG, loaded into memory before playing |
| `PSND_MIDI`      | MIDI file                              |
| `PSND_MOD`       | MOD/XM/S3M file                        |

**NOTE:** You should normally just use the [`AudioClip.Play`](AudioClip#audioclipplay) function (via GetScriptFunctionAddress) to play sounds - only use this function in specific circumstances.

**IMPORTANT:** Only one MIDI file, and one MOD/XM file can be playing at a time. You should generally keep away from these two sound types unless you are sure about what you are doing.

_Added in version: 11_

#### IAGSEngine.MarkRegionDirty
```cpp
void MarkRegionDirty(int left, int top, int right, int bottom);
```

Marks the rectangle (`left`, `top`) - (`right`, `bottom`) on the virtual screen as dirty. Use this if you make any changes to the virtual screen via its raw bitmap surface, to tell AGS to repaint that area of the screen.

**NOTE:** All engine API functions such as `DrawText` and `BlitBitmap` call this automatically, so you only need to call it if you manually modify the bitmap surface.

_Added in version: 12_

#### IAGSEngine.GetMouseCursor
```cpp
AGSMouseCursor* GetMouseCursor(int cursor);
```

Returns the mouse cursor struct which AGS uses to store details about the specified cursor mode.

**IMPORTANT:** The returned struct is read-only. You should not modify any members of it directly; rather, call the script functions such as [`Mouse.ChangeModeGraphic`](Mouse#mousechangemodegraphic) which ensure that the game state is updated properly.

_Added in version: 12_

#### IAGSEngine.GetRawColorComponents
```cpp
void GetRawColorComponents(int coldepth, int color,int *red, int *green, int *blue, int *alpha);
```

Given a raw color (from a bitmap's raw surface, or from GetRawPixelColor) at a given color depth, returns the red, green, blue and alpha components, as values from 0-255.

You can pass any of the pointers as NULL if you don't want to know that value. The `alpha` value only has meaning for 32-bit pixels; for all other color depths, it will just be returned as 0.

_Added in version: 12_

#### IAGSEngine.MakeRawColorPixel
```cpp
int MakeRawColorPixel(int coldepth, int red, int green, int blue, int alpha);
```

Given a color depth, and RGB values (from 0-255), this function returns the raw pixel value needed to display the requested color on a bitmap of `coldepth` bits per pixel.

The `alpha` value is only meaningful for 32-bit color alpha channel bitmaps, and it allows you to set the alpha value for the pixel. For bitmaps without an alpha channel, pass `alpha` as 0.

_Added in version: 12_

#### IAGSEngine.GetFontType
```cpp
int GetFontType(int fontNum);
```

Returns what type of font `fontNum` is, as one of the following values:

| value         | description                  |
| ------------- | ---------------------------- |
| `FNT_INVALID` | invalid font number supplied |
| `FNT_SCI`     | font is a SCI font           |
| `FNT_TTF`     | font is a TTF font           |

_Added in version: 12_

#### IAGSEngine.CreateDynamicSprite
```cpp
int CreateDynamicSprite(int coldepth, int width, int height);
```

Creates a new dynamic sprite with the supplied color depth, width and height. A dynamic sprite is not controlled by the AGS Sprite Cache, and so is not removed from memory until you manually call `DeleteDynamicSprite`. Thus, you can write to its bitmap surface without a problem.

You must remember to free the memory when you are done with the sprite, by calling `DeleteDynamicSprite`.

Returns 0 if the sprite could not be created, or the sprite slot number on success.

_Added in version: 12_

#### IAGSEngine.DeleteDynamicSprite
```cpp
void DeleteDynamicSprite(int slot);
```

Removes the previously created dynamic sprite from memory. It can no longer be used in the game.

_Added in version: 12_

#### IAGSEngine.IsSpriteAlphaBlended
```cpp
int IsSpriteAlphaBlended(int slot);
```

Determines whether the supplied sprite has an alpha channel or not. Returns 1 if it does, 0 if it does not.

This function is useful for determining whether you can ignore the alpha value when doing raw bitmap manipulations. If a sprite does have an alpha channel, you cannot ignore it since the default alpha value of 0 means totally invisible and the pixel would disappear.

Only 32-bit bitmaps can have an alpha channel. For all other color depths, this function will always return 0.

_Added in version: 12_

#### IAGSEngine.UnrequestEventHook
```cpp
void UnrequestEventHook(int event)
```

Tells the engine not to call this plugin any longer when the specified `event` occurs. The values of `event` are the same as for `RequestEventHook`.

This is useful to call for performance reasons if you no longer require notification of an event.

_Added in version: 13_

#### IAGSEngine.BlitSpriteTranslucent
```cpp
void BlitSpriteTranslucent(int x, int y, BITMAP *spr, int trans);
```

Draws graphic `spr` to the current virtual screen at (`x`,`y`), with translucency over the background.

`trans` is the translucency level, from 0 to 255 (where 0 is fully transparent, and 255 is fully opaque). Pixels in the bitmap's transparent color are skipped completely.

_Added in version: 13_

#### IAGSEngine.BlitSpriteRotated
```cpp
void BlitSpriteRotated(int x, int y, BITMAP *spr, int angle);
```

Draws graphic `spr` to the current virtual screen at (`x`,`y`), but rotated to `angle`. Pixels in the bitmap's transparent color are skipped completely.

The sprite is positioned with its upper-left corner at (`x`,`y`), then it is rotated about its center by `angle`. The angle is specified as a number from 0 to 255, where 128 would turn it 180 degrees, and 64 would turn it 90 degrees.

_Added in version: 13_

#### IAGSEngine.GetDirectSound
```cpp
LPDIRECTSOUND GetDirectSound();
```

**IMPORTANT:** This function is only supported on Windows.

Returns the main `IDirectSound` interface that the engine is using. This command does **not** `AddRef` the pointer it returns, so you don't need to Release it.

This function returns `NULL` if sound is disabled, or if the player is using the WaveOut driver rather than the DirectSound one.

_Added in version: 14_

#### IAGSEngine.DisableSound
```cpp
void DisableSound();
```

Disables AGS's sound system completely. Use this if you want your plugin to take over all audio functionality.

_Added in version: 14_

#### IAGSEngine.CanRunScriptFunctionNow
```cpp
int CanRunScriptFunctionNow();
```

Determines whether a game script function can be run now. Returns 0 if there is currently a script running, or 1 if there isn't.

_Added in version: 14_

#### IAGSEngine.CallGameScriptFunction
```cpp
int CallGameScriptFunction(const char *name, int globalScript, int numArgs, int arg1 = 0, int arg2 = 0, int arg3 = 0);
```

Runs a script function in the game's script. `name` is the name of the function to call. If `globalScript` is 1, it will call the function in the global script; if it is 0, the function will be run in the room script.

`numArgs` specifies how many arguments the function takes - this can be from 0 to 3. The following parameters then allow you to pass the contents of the function parameters. They are all 32-bit ints, so the user should declare their script function as taking `int` parameters.

This function returns 0 on success, and a non-zero value otherwise. This will always fail when `CanRunScriptFunctionNow` returns 0.

_Added in version: 14_

#### IAGSEngine.QueueGameScriptFunction
```cpp
void QueueGameScriptFunction(const char *name, int globalScript, int numArgs, int arg1 = 0, int arg2 = 0);
```

Queues a script function to be run in the game's script. `name` is the name of the function to call. If `globalScript` is 1, it will call the function in the global script; if it is 0, the function will be run in the room script.

`numArgs` specifies how many arguments the function takes - this can be from 0 to 2. The following parameters then allow you to pass the contents of the function parameters. They are all 32-bit ints, so the user should declare their script function as taking `int` parameters.

If no scripts are currently running, the requested script will be run straight away. If there are scripts running, however, then the requested script will be queued up to run when the current one finishes.

_Added in version: 15_

#### IAGSEngine.NotifySpriteUpdated
```cpp
void NotifySpriteUpdated(int slot);
```

Notifies the engine that the specified sprite has been updated by the plugin. You would call this if you changed the contents of a dynamic sprite, to make sure that the screen is updated to reflect your changes.

_Added in version: 15_

#### IAGSEngine.SetSpriteAlphaBlended
```cpp
void SetSpriteAlphaBlended(int slot, int isAlphaBlended);
```

Allows you to specify whether the sprite is alpha blended or not. This allows you to create dynamic 32-bit RGBA sprites. Pass `isAlphaBlended` as 1 to enable the sprite's alpha channel, or pass it as 0 to disable the alpha channel.

**IMPORTANT:** This function should only be used with 32-bit dynamic sprites. Using it with sprites of lower color depths, or of sprites created in the AGS Editor, can have unpredictable results.

_Added in version: 15_

#### IAGSEngine.RegisterManagedObject
```cpp
int RegisterManagedObject(const void *object, IAGSScriptManagedObject *callback);
```

Registers a new managed object with the script engine. `object` points to the actual object itself, and `callback` points to the class implementing the `IAGSScriptManagedObject` interface, which will be called to perform operations on the object.

`callback` and `object` can, but do not have to, point to the same thing (i.e. it is possible to have the actual object implement the interface; but if you do so, beware that you should not expose any normal variables from the class due to the vtable occupying the first part of the memory).

The `object` is added to the managed object pool, with a reference count of zero. Once this function has been called, `object` can be returned to the script and will be automatically picked up by the script engine.

See further down this page for a description of the `IAGSScriptManagedObject` interface.

From engine interface version 16 and later, this function returns the managed pool key for the object. You will not normally need this, but it can be useful to link a parent and child object if your structure is complex.

_Added in version: 15_

#### IAGSEngine.AddManagedObjectReader
```cpp
void AddManagedObjectReader(const char *typeName, IAGSManagedObjectReader *reader);
```

Registers the specified managed object type with the script engine, and supplies an `IAGSManagedObjectReader`, which provides the functionality for de-serializing objects of that type. This is used when the user restores a save game, in order to recreate any managed objects that were in existence when the game was saved.

See further down this page for a description of the `IAGSManagedObjectReader` interface.

**IMPORTANT:** AGS only keeps a pointer to `typeName` and does not copy the string, so make sure you do not supply a temporary or local variable as the type name.

_Added in version: 15_

#### IAGSEngine.RegisterUnserializedObject
```cpp
void RegisterUnserializedObject(int key, const void *object, IAGSScriptManagedObject *callback);
```

Used from within the `IAGSManagedObjectReader`, this function re-registers a managed object after it has been read back from disk (for example, when the player restores a save game).

The `key` parameter is the object's key, supplied to the `ObjectReader`, which allows AGS to match up the object internally. You should pass the key in here exactly as it is passed to the `ObjectReader`.

_Added in version: 15_

#### IAGSEngine.GetManagedObjectKeyByAddress
```cpp
int GetManagedObjectKeyByAddress(const char *address);
```

Gets the managed pool key of the specified object address. You will not normally need to use this function, however it can be useful to link up related objects after de-serialization.

Returns -1 if there is no such object in the pool.

This function is quite slow so you should avoid using it wherever possible.

_Added in version: 16_

#### IAGSEngine.GetManagedObjectAddressByKey
```cpp
void* GetManagedObjectAddressByKey(int key);
```

Gets the memory address of the specified object from the managed pool. You will not normally need to use this function, however it can be useful to link up related objects after de-serialization.

Returns `NULL` if the key is invalid.

_Added in version: 16_

#### IAGSEngine.CreateScriptString
```cpp
const char * CreateScriptString(const char *fromText);
```

Creates a new script String object, containing a copy of `fromText`. This allows you to declare your plugin functions as returning a String - simply return the result of **CreateScriptString** to the calling script.

The new string becomes part of the AGS Managed Object Pool and will be freed automatically when there are no longer any script references to it. Note that it creates a copy of `fromText`, so it is safe for you to free the contents of `fromText` after calling this function.

_Added in version: 17_

#### IAGSEngine.IncrementManagedObjectRefCount
```cpp
int IncrementManagedObjectRefCount(const char *address);
```

Increments the reference count of the specified object in the managed object pool. This should be used if you are storing a reference to a managed object between script function calls, in order to ensure that the object does not get deleted.

Returns the new reference count of the object.

**IMPORTANT:** Be **very careful** when using this function. If you adjust the reference count incorrectly, you could cause a memory leak or the game could crash.

_Added in version: 18_

#### IAGSEngine.DecrementManagedObjectRefCount
```cpp
int DecrementManagedObjectRefCount(const char *address);
```

Decrements the reference count of the specified object in the managed object pool. This should be used if you are storing a reference to a managed object between script function calls, in order to ensure that the object does not get deleted.

Returns the new reference count of the object.

**IMPORTANT:** Be **very careful** when using this function. If you adjust the reference count incorrectly, you could cause a memory leak or the game could crash.

_Added in version: 18_

#### IAGSEngine.SetMousePosition
```cpp
void SetMousePosition(int x, int y);
```

Changes the mouse cursor position to (x,y). As with _GetMousePosition_, these co-ordinates are specified in real resolution units, so in a 640x480 game the co-ordinates do go up to 640,480 (unlike the script function of the same name).

_Added in version: 18_

#### IAGSEngine.SimulateMouseClick
```cpp
void SimulateMouseClick(int button);
```

Simulates the mouse being clicked by the player. `button` is which mouse button (1=left, 2=right, 3=middle) to simulate. The effect should be identical to the user clicking the equivalent button on their mouse.

_Added in version: 18_

#### IAGSEngine.GetMovementPathWaypointCount
```cpp
int GetMovementPathWaypointCount(int pathId);
```

Gets the number of waypoints on the specified movement path.

The `GetMovementPath` family of functions allow you to access the paths for moving characters and objects in the game; and therefore allow you to anticipate where they will end up. The `pathId` is the value found in the `AGSCharacter.walking` and `AGSObject.moving` variables (but only if these variables are greater than 0; 0 or less means they are not moving).

_Added in version: 18_

#### IAGSEngine.GetMovementPathLastWaypoint
```cpp
int GetMovementPathLastWaypoint(int pathId);
```

Gets the last waypoint that was passed on this movement path. The starting point is waypoint 0, and the destination is the `WaypointCount` minus 1 (though this can never be returned, since the move is marked as finished once the last waypoint is reached).

_Added in version: 18_

#### IAGSEngine.GetMovementPathWaypointLocation
```cpp
void GetMovementPathWaypointLocation(int32 pathId, int32 waypoint, int32 *x, int32 *y);
```

Gets the X and Y co-ordinates of the specified waypoint on the specified path.

**IMPORTANT:** No error checking is done on these functions for performance reasons, so make sure you supply a valid `pathId` and `waypoint`.

_Added in version: 18_

#### IAGSEngine.GetMovementPathWaypointSpeed
```cpp
void GetMovementPathWaypointSpeed(int32 pathId, int32 waypoint, int32 *xSpeed, int32 *ySpeed);
```

Gets the X and Y speed at which the character/object will move from the specified waypoint to the next. The speeds are returned as 32-bit **fixed point** numbers. This means that the high 16 bits is the whole part of the number, and the low 16 bits is the fractional part. For example, if `xSpeed` is 0x00038000, that would represent a speed of 3.5 pixels per frame.

_Added in version: 18_

#### IAGSEngine.IsRunningUnderDebugger
```cpp
int IsRunningUnderDebugger();
```

Returns whether the game is currently running under the AGS Editor's debugger. If it is, then you are able to use the `BreakIntoDebugger` command.

_Added in version: 22_

#### IAGSEngine.BreakIntoDebugger
```cpp
void BreakIntoDebugger();
```

Tells AGS to stop and break out into the editor's debugger. This will not happen immediately, but will happen when the next line of script code is run. This function will only work if `IsRunningUnderDebugger` returns 1.

_Added in version: 22_

#### IAGSEngine.GetPathToFileInCompiledFolder
```cpp
void GetPathToFileInCompiledFolder(const char *fileName, char *buffer)
```

Fills in `buffer` with a path that you can use to open the specified file that resides in the Compiled folder.

If the game is running under the debugger, `buffer` will end up containing `Compiled/fileName` .
Otherwise, `buffer` will simply be set to a copy of `fileName`.

You should allocate `buffer` at least as `MAX_PATH` bytes long.

**IMPORTANT:** this function is considered unsafe to use, and is deprecated since interface version 27. Prefer to use [ResolveFilePath](EnginePluginRun-timeAPI#iagsengineresolvefilepath) instead.

_Added in version: 22_

#### IAGSEngine.GetDirectInputKeyboard
```cpp
LPDIRECTINPUTDEVICE GetDirectInputKeyboard();
```

**IMPORTANT:** This function is only supported on Windows.

Returns a pointer to the `IDirectInputDevice` interface that AGS is using to control the keyboard. This command does **not** `AddRef` the pointer it returns, so you don't need to Release it.

_Added in version: 23_

#### IAGSEngine.GetDirectInputMouse
```cpp
LPDIRECTINPUTDEVICE GetDirectInputMouse();
```

**IMPORTANT:** This function is only supported on Windows.

Returns a pointer to the `IDirectInputDevice` interface that AGS is using to control the mouse. This command does **not** `AddRef` the pointer it returns, so you don't need to Release it.

_Added in version: 23_

#### IAGSEngine.ReplaceFontRenderer
```cpp
IAGSFontRenderer* ReplaceFontRenderer(int fontNumber, IAGSFontRenderer* newRenderer)
```

Replaces the font renderer that AGS is currently using to draw the specified font with one of your own. The old renderer is returned.

This allows you to override AGS's built-in font rendering with your own system. For example, if you didn't want to use SCI or TTF fonts, this allows you to provide a custom implementation. See further down this page for a description of the `IAGSFontRenderer` interface.

**IMPORTANT:** This function is superceded by [`ReplaceFontRenderer2`](EnginePluginRun-timeAPI#iagsenginereplacefontrenderer2). We recommend you use the new function, and implement the `IAGSFontRenderer2` interface instead.

_Added in version: 23_

#### IAGSEngine.GetRenderStageDesc
```cpp
void GetRenderStageDesc(AGSRenderStageDesc* desc)
```

Fills the provided `AGSRenderStageDesc` struct with the current render stage description. `AGSRenderStageDesc` is declared as:

```cpp
struct AGSRenderMatrixes {
  float WorldMatrix[16];
  float ViewMatrix[16];
  float ProjMatrix[16];
};

// Render stage description
struct AGSRenderStageDesc {
  int Version;
  AGSRenderMatrixes Matrixes;
};
```

AGSRenderStageDesc's first field is `Version`, which must be filled by the plugin itself *before* calling this function. This field's value corresponds to the required engine's interface and tells which struct's fields should be assigned by the engine. It should be 25 or higher for this function to have any effect at all.

As of interface version 25 the only other meaningful field in `AGSRenderStageDesc` is `Matrixes` of type `AGSRenderMatrixes`. This is another struct that contains three arrays, 16 floating point values each. They represent three main transformation matrixes for the current render stage: World, View and Projection. The matrix format will correspond to the *internal format of the current renderer* that engine runs (OpenGL, Direct3D, and so on). Thus, if your plugin works with the same renderer as the engine it may safely use these matrixes with that renderer's functions.

**NOTE:** You may know which renderer is run by the engine if you call [`GetGraphicsDriverID`](EnginePluginRun-timeAPI#iagsenginegetgraphicsdriverid).

**NOTE:** Software renderer will always fill these matrixes with zeroes.

**NOTE:** This function is only guaranteed to return useful data if you call it in one of the [render stage events](EnginePluginRun-timeAPI#iagsenginerequesteventhook).

**IMPORTANT:** Plugin *must* fill the struct's Version field before passing it into the function.

_Added in version: 25_

#### IAGSEngine.GetGameInfo
```cpp
void GetGameInfo(AGSGameInfo* ginfo)
```

Fills the provided `AGSGameInfo` struct with the game's description. `AGSGameInfo` is declared as:

```
// Game info
struct AGSGameInfo {
  // Which version of the plugin interface the struct corresponds to;
  // this field must be filled by a plugin before passing the struct into the engine!
  int Version;
  // Game title (human-readable text)
  char GameName[50];
  // Game's GUID
  char Guid[40];
  // Random key identifying the game (deprecated)
  int UniqueId;
};
```

AGSGameInfo's first field is `Version`, which must be filled by the plugin itself *before* calling this function. This field's value corresponds to the required engine's interface and tells which struct's fields should be assigned by the engine. It should be 26 or higher for this function to have any effect at all.

The rest of the fields are for informational purposes. GameName is a human-readable game's title, which is displayed on the game window's caption. Its value corresponds to the [`Game.Name`](Game#gamename) script property. Guid is a unique string identifying the game. UniqueId is a deprecated game's identifier, which was used in the older versions of AGS. It may be refered to in case Guid is empty.

**IMPORTANT:** Plugin *must* fill the struct's Version field before passing it into the function.

_Added in version: 26_

#### IAGSEngine.ReplaceFontRenderer2
```cpp
IAGSFontRenderer* ReplaceFontRenderer2(int fontNumber, IAGSFontRenderer2* newRenderer)
```

Replaces the font renderer that AGS is currently using to draw the specified font with one of your own. The previously used renderer is returned.

This allows you to override AGS's built-in font rendering with your own custom implementation.

This function supercedes [`ReplaceFontRenderer`](EnginePluginRun-timeAPI#iagsenginereplacefontrenderer) and allows to install an extended renderer type, which implements [`IAGSFontRenderer2`](EnginePluginRun-timeAPI#iagsfontrenderer2-interface) interface (see further down this page for its description).

**NOTE:** The old renderer is still returned as a pointer to the base interface `IAGSFontRenderer`. This is to keep things compatible in case someone sets an old interface, so that this pointer could be used to set it back if necessary (in which case you need to use `ReplaceFontRenderer`). The engine is capable of detecting when the pointer refers to one of the internal renderers, and convert it as necessary. If you would like to tell if the returned pointer refers to one of the plugin's own renderers, you may do so by comparing the memory addresses, for example.

_Added in version: 26_

#### IAGSEngine.NotifyFontUpdated
```cpp
void NotifyFontUpdated(int fontNumber)
```

Notifies the engine about the certain font being modified. This is useful when you implement your own custom font loading and rendering (see [`ReplaceFontRenderer2`](EnginePluginRun-timeAPI#iagsenginereplacefontrenderer2)). When you tell the engine that the font has changed, it recalculates font metrics by quering the active font renderer about font's parameters, and forces game GUI to redraw.

_Added in version: 26_

#### IAGSEngine.ResolveFilePath

```cpp
size_t ResolveFilePath(const char *script_path, char *buf, size_t buf_len);
```

Resolves a script path to a system filepath, same way as script command File.Open does. Caller should provide an output buffer and buffer's length in bytes. Returns a number of bytes written into the buffer, or 0 on failure. Passing NULL instead of a buffer pointer will make function calculate and return length necessary to store a resulting path (in bytes).

_Added in version: 27_

#### IAGSEngine.OpenFileStream

```cpp
IAGSStream* OpenFileStream(const char *script_path, int file_mode, int work_mode);
```

Opens a data stream, resolving a script path and returns IAGSStream object pointer, or NULL on failure. Script path may contain same location tokens as script command File.Open. File mode should contain one of the AGSSTREAM_FILE_* values, work mode should contain flag set of the AGSSTREAM_MODE_* values.
The returned stream object is owned by the caller, and must be deleted by calling its Dispose() method.

**IMPORTANT:** Plugin *must* call IAGSStream.Dispose() for streams returned by OpenFileStream() when these are no longer needed, otherwise stream objects will remain in memory undeleted.

File mode may be one of the following values:
* `AGSSTREAM_FILE_OPEN` - Opens existing file, fails if there's no such file.
* `AGSSTREAM_FILE_CREATE` - Opens existing file, but creates one if it did not exist.
* `AGSSTREAM_FILE_CREATEALWAYS` - Always creates a new blank file.

Work mode is a combination of following values:
* `AGSSTREAM_MODE_READ` - Stream supports reading data.
* `AGSSTREAM_MODE_WRITE` - Stream supports writing data.
* `AGSSTREAM_MODE_SEEK` - Stream supports seeking (that is - changing cursor position).

**NOTE:** only `AGSSTREAM_MODE_READ` and `AGSSTREAM_MODE_WRITE` are essential to be used with the `OpenFileStream()`. `AGSSTREAM_MODE_SEEK` value is ignored, and only used by [`IAGSStream::GetMode()`](EnginePluginRun-timeAPI#iagsstreamgetmode) to indicate seekable streams.

_Added in version: 28_

#### IAGSEngine.GetFileStreamByHandle

```cpp
IAGSStream* GetFileStreamByHandle(int32 fhandle);
```

Returns IAGSStream object identified by the given stream handle, or NULL if handle is invalid. This lets to retrieve IAGSStream object from a handle received in a event callback, such as AGSE_SAVEGAME and AGSE_RESTOREGAME. The returned stream's ownership is NOT passed to the caller, and should be kept and used only in the duration of the event callback.

**IMPORTANT:**: A stream received from GetFileStreamByHandle() should NOT be closed or disposed, doing so will lead to errors in the engine.

_Added in version: 28_

#### IAGSEngine.Log

```cpp
void Log(int level, const char *fmt, ...);
```

Prints a formatted message to the engine's log, using told message level. Message level can be:
* AGSLOG_LEVEL_ALERT
* AGSLOG_LEVEL_FATAL
* AGSLOG_LEVEL_ERROR
* AGSLOG_LEVEL_WARN 
* AGSLOG_LEVEL_INFO 
* AGSLOG_LEVEL_DEBUG

The formatting is done identical to the standard C "printf" function formatting, also used by a script functions such as String.Format and few others. See [String formatting](StringFormats).

_Added in version: 29_

#### IAGSEngine.CreateDynamicArray

```cpp
void* CreateDynamicArray(size_t elem_count, size_t elem_size, bool is_managed_type);
```

Creates a new dynamic array, allocating space for the given number of elements of the given size. Optionally instructs to create an array for managed handles, in which case the element size must be sizeof(int32). Returns a pointer to the beginning of array's data (or the first element), which may be used to fill array or passed into the game script. Returns a null pointer in case creation failed, or the input arguments were incorrect.

You should be using this function when creating dynamic arrays for plugin's script API, because when the engine creates dynamic arrays for the script it adds certain meta data to it, and expects its presence when working with such arrays. For that reason you cannot just allocate a generic C array and return it into the script.

**IMPORTANT:** If you are planning to use this array to store managed object handles, then you MUST correctly pass 'is_managed_type = true', because otherwise engine won't know that it must release their references, which may lead to memory leaks. When writing handles into this array, you MUST increment reference count for each one of them (see [IncrementManagedObjectRefCount](EnginePluginRun-timeAPI#iagsengineincrementmanagedobjectrefcount)), otherwise these objects may get disposed before the array itself, making these handles invalid!

Example:

```cpp
void *CreateArrayForScript()
{
    // create a dynamic array for 100 integers
    void *dyn_arr = engine->CreateDynamicArray(100, sizeof(int), false);
    // fill it with sequential integer values
    int *p_int_arr = static_cast<int*>(dyn_arr);
    for (int i = 0; i < 100; ++i) { p_int_arr[i] = i; }
    return dyn_arr;
}
```

_Added in version: 30_

#### IAGSEngine.GetDynamicArrayLength

```cpp
size_t GetDynamicArrayLength(const void *arr);
```

Retrieves dynamic array's length, which is a number of elements. You should pass a dynamic array object either received from the engine in your registered script function, or created by you with [CreateDynamicArray](EnginePluginRun-timeAPI#iagsenginecreatedynamicarray).

_Added in version: 30_

#### IAGSEngine.GetDynamicArraySize

```cpp
size_t GetDynamicArraySize(const void *arr);
```

Retrieves dynamic array's size, which is a total capacity *in bytes*. You should pass a dynamic array object either received from the engine in your registered script function, or created by you with [CreateDynamicArray](EnginePluginRun-timeAPI#iagsenginecreatedynamicarray).

_Added in version: 30_

### IAGSScriptManagedObject interface

The `IAGSScriptManagedObject` interface is implemented for objects which the plugin wants to return to the game script. It allows a callback mechanism for AGS to request various operations on the object. You must implement the following:

#### IAGSScriptManagedObject.Dispose
```cpp
int Dispose(const char *address, bool force);
```

Called when the reference count of the specified object reaches zero. The `address` parameter points to the particular object that is no longer referenced.

If `force` is zero, then you are merely being informed that the reference count has reached zero. Either you can destroy the object (in which case you should return 1), or you can leave it (return 0). If you leave it, then this function will be called again at various intervals to give you another opportunity to destroy the object.

If `force` is non-zero, then this is the object's last chance to free its memory and it will not be called again. This is the case if the player is about to restore a saved game, or is quitting the game, and so forth.

You must return 1 from this function if you destroy the object at `address` - in which case, it will be removed from the managed object pool. Return 0 if you do not destroy the object, in which case it will stick around.

_Added in version: 15_

#### IAGSScriptManagedObject.GetType
```cpp
const char* GetType();
```

Returns the type name of the object. This must match the type name supplied to `AddManagedObjectReader` for the corresponding reader.

_Added in version: 15_

#### IAGSScriptManagedObject.Serialize
```cpp
int Serialize(const char *address, char *buffer, int bufsize);
```

Called when the player saves the game. You must write any persistent data for the object at `address` into the supplied `buffer`. The buffer is `bufsize` bytes long, and you must not write any more data than that.

You must return the number of bytes written to the buffer.

_Added in version: 15_

### IAGSManagedObjectReader interface

The IAGSManagedObjectReader interface is implemented for each managed object type, to enable the objects to be reconstructed after being read back from disk. You must implement the following:

#### IAGSManagedObjectReader.Unserialize
```cpp
void Unserialize(int key, const char *serializedData, int dataSize);
```

Called for each managed object when a saved game is restored. `serializedData` is the buffer that you wrote to in Serialize, and `dataSize` is the length of this buffer.

You should re-construct the object from the serialized data, and then re-register it using the `RegisterUnserializedObject` function. The `key` supplied to you here should be passed on unmodified to `RegisterUnserializedObject` in order for the object to be fully restored.

_Added in version: 15_

### IAGSFontRenderer interface

The `IAGSFontRenderer` interface can be implemented to override AGS's built-in text rendering with a custom system. The methods on the interface are as follows:

#### IAGSFontRenderer.LoadFromDisk
```cpp
bool LoadFromDisk(int fontNumber, int fontSize);
```

Used internally by AGS to load the font from disk. However, you currently cannot insert a custom renderer early enough in the process to override this method, therefore it will never be called.

_Added in version: 23_

#### IAGSFontRenderer.FreeMemory
```cpp
void FreeMemory(int fontNumber);
```

Called when the engine is shutting down, to free the memory associated with this font. You should perform your own clean-up here, and then call onto the `FreeMemory` method of the old renderer that you replaced.

_Added in version: 23_

#### IAGSFontRenderer.SupportsExtendedCharacters
```cpp
bool SupportsExtendedCharacters(int fontNumber);
```

AGS will call this method to determine whether the specified font supports extended characters (i.e. characters with a ASCII value of 128 or above). Return `true` or `false`, depending on your implementation.

**IMPORTANT:** If you return false from this function, AGS will not necessarily strip extended characters out of text. If you do not support extended characters, then you should implement the `EnsureTextValidForFont` method (below) to strip them out.

_Added in version: 23_

#### IAGSFontRenderer.GetTextWidth
```cpp
int GetTextWidth(const char *text, int fontNumber);
```

AGS will call this method to find out the width, in pixels, of drawing the specified text in the specified font. You should return the number of pixels that this will take up (assuming the text is all on one line and does not wrap).

_Added in version: 23_

#### IAGSFontRenderer.GetTextHeight
```cpp
int GetTextHeight(const char *text, int fontNumber);
```

AGS will call this method to find out the height, in pixels, of drawing the specified text in the specified font. You should return the number of pixels that this will take up (assuming the text is all on one line and does not wrap).

_Added in version: 23_

#### IAGSFontRenderer.RenderText
```cpp
void RenderText(const char *text, int fontNumber, BITMAP *destination, int x, int y, int color);
```

AGS calls this method to render text to a bitmap. `destination` is the bitmap surface that AGS is rendering to (you can use this with other functions like `GetBitmapDimensions` and `GetRawBitmapSurface`), and you need to draw the `text` starting with its top-left position at (`x`, `y`) on this surface.

`color` is a raw pixel color, not an AGS color Number.

_Added in version: 23_

#### IAGSFontRenderer.AdjustYCoordinateForFont
```cpp
void AdjustYCoordinateForFont(int *ycoord, int fontNumber);
```

This method is a workaround introduced for TTF fonts, which allows you to adjust the Y-position of the rendered text. You should implement this method so that it does nothing unless you have problems with text positioning.

_Added in version: 23_

#### IAGSFontRenderer.EnsureTextValidForFont
```cpp
void EnsureTextValidForFont(char *text, int fontNumber);
```

AGS calls this method prior to actually rendering text, and allows you to check that all the characters in the text are supported by the font. If not, `text` is a writable buffer and you could replace them with question marks.

By default, you should implement this method so that it does nothing.

_Added in version: 23_

### IAGSFontRenderer2 interface

The `IAGSFontRenderer2` interface is an extension to the `IAGSFontRenderer`, therefore contains all the parent's methods, but also adds a number of its own:

#### IAGSFontRenderer2.GetVersion
```cpp
int GetVersion();
```

Tells the engine API version to which this renderer implementation corresponds to. This is a very important method, as engine will use it to know what functionality of this interface is safe to use. The return value must not be lower than 26 (as this is the first version in which this interface was introduced).

**IMPORTANT:** You must make sure your renderer returns a correct version, corresponding to which version of interface you implement, otherwise engine may try calling non-existing methods and crash.

_Added in version: 26_

#### IAGSFontRenderer2.GetRendererName
```cpp
const char *GetRendererName();
```

Returns the name of this renderer implementation. This is for informational purposes only. Engine may use this method, for example, to print current active font renderer names into the log.

_Added in version: 26_

#### IAGSFontRenderer2.GetFontName
```cpp
const char *GetFontName(int fontNumber);
```

Returns the name of the given font, if it's supported. Should return an empty string or null otherwise. This is for informational purposes only.

_Added in version: 26_

#### IAGSFontRenderer2.GetFontHeight
```cpp
int GetFontHeight(int fontNumber);
```

Returns the font's height. This method supercedes `IAGSFontRenderer.GetTextHeight`, and does not require to pass a string. The returned height should be a maximal height the font may occupy.

_Added in version: 26_

#### IAGSFontRenderer2.GetLineSpacing
```cpp
int GetLineSpacing(int fontNumber);
```

Returns the font's custom line spacing, if any. Return 0 to tell that the font does not have any particular line spacing set, in which case font's height will be used instead.

_Added in version: 26_

### IAGSStream interface

The `IAGSStream` interface represents an open stream for reading or writing data. This interface may be implemented by the engine or plugin. The interface has following methods:

#### IAGSStream.GetMode
```cpp
int GetMode();
```

Tells which mode the stream is working in. The mode defines supported i/o operations, such as reading, writing, seeking. Returns combination of AGSSTREAM_MODE_* flags. Invalid or non-functional streams return 0.

See [`IAGSEngine.OpenFileStream`](EnginePluginRun-timeAPI#iagsengineopenfilestream) for the list of potential mode values.

_Added in version: 28_

#### IAGSStream.GetPath
```cpp
const char *GetPath();
```

Returns an optional stream's source description. This may be a file path, or a resource name, or anything of that kind, and is meant purely for diagnostic purposes.

**NOTE:** you should not rely on this method to return anything in particular. It is allowed to return empty string, or NULL pointer.

_Added in version: 28_

#### IAGSStream.EOS
```cpp
bool EOS();
```

Tells whether this stream's position is at its end. Unlike standard C `feof` this does not wait for a read attempt past the stream end, and reports positive when stream's position = length.

_Added in version: 28_

#### IAGSStream.GetError
```cpp
bool GetError();
```

Tells if there were errors during previous i/o operation(s). The call to GetError() *resets* the error record, so if there are two consecutive GetError() calls, the second will always return "no error".

**NOTE:** this method is mostly intended for the cases when the stream is being used to read many chunks of data in a loop, or read data periodically with waits in between. In such case you may call GetError periodically too, to find out whether stream operations are still being successful.

_Added in version: 28_

#### IAGSStream.GetLength
```cpp
int64_t GetLength();
```

Returns the total stream's length in bytes.

_Added in version: 28_

#### IAGSStream.GetPosition
```cpp
int64_t GetPosition();
```

Returns stream's position. Position is valid in range 0 to (length-1).

_Added in version: 28_

#### IAGSStream.Read
```cpp
size_t Read(void *buffer, size_t len)
```

Reads number of bytes into the provided buffer. Returns number of bytes that were actually copied. If return value is less than `len` parameter, that means that either end of stream is reached, or there has been an error.

_Added in version: 28_

#### IAGSStream.ReadByte
```cpp
int32_t ReadByte()
```

Reads a single byte and returns either its value (0-255), or -1 on end of stream or error. This method is intended for getting data byte-by-byte.

_Added in version: 28_

#### IAGSStream.Write
```cpp
size_t Write(const void *buffer, size_t len)
```

Writes number of bytes, copying them from the provided buffer. Returns number of bytes that were actually written. If return value is less than `len` parameter, that means that there has been an error.

_Added in version: 28_

#### IAGSStream.WriteByte
```cpp
int32_t WriteByte(uint8_t b)
```

Writes a single byte (0-255) and returns either its value, or -1 on error. This method is intended for putting data byte-by-byte.

_Added in version: 28_

#### IAGSStream.Seek
```cpp
int64_t Seek(int64_t offset, int origin)
```

Moves stream cursor to the requested offset, counting from the "origin", where "origin" defined by AGSSTREAM_SEEK_* constants:
* `AGSSTREAM_SEEK_SET` - offset from beginning of the stream, basically, from 0. Offset must be strictly positive.
* `AGSSTREAM_SEEK_CUR` - offset relative to the current position. Offset may be either positive (go forward) or negative (go backward).
* `AGSSTREAM_SEEK_END` - offset from end of stream. Offset must be strictly negative.

Returns new stream position, or -1 on error.

**NOTE:** seeking to forbidden positions (outside of valid stream range) is not counted as error. In such case the stream will simply clamp the destination offset to the valid range and return the actual position it ended on. Error is only reported if there was a failure to seek at all. Stream's implementation guarantees that stream's position remains unchanged in case of error.

_Added in version: 28_

#### IAGSStream.Flush
```cpp
bool Flush()
```

Flushes stream, forcing it to write any buffered data to the underlying device. Does nothing in read-only mode. Returns if operation was successful.

Some streams may have a internal memory buffer where they keep recently written data instead of passing them to the device (file, etc) right away. This is done for performance reasons. In such case the written data may not get to the device until the internal buffer is full. There are situations where you may want to force the stream to write to device at once. For example, if you are writing a log, you would like the log messages to appear in the file instantly, so that you could read them. This is where you need to use Flush().

**NOTE:** when stream is being closed, it is also flushed automatically, so you don't have to call it yourself then.

_Added in version: 28_

#### IAGSStream.Close
```cpp
void Close()
```

Flushes and closes the stream, but does not delete the stream object just yet. Usually you do not have to call this, and should use Dispose() to close and delete stream object instead.

_Added in version: 28_

#### IAGSStream.Dispose
```cpp
void Dispose()
```

Flushes, closes the stream, and deletes the stream object, freeing all the associated memory.

After calling this the IAGSStream pointer becomes INVALID and should not be used (unless assigned a new stream object later on).

_Added in version: 28_