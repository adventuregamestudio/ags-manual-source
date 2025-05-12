## General settings

The General Settings window contains various overall
options that you can set for your game.

Note that some things listed here are explained later in the
documentation, so if you don't understand one of the items in this list,
come back to it later.

Many of these options can be changed at runtime with the script command
SetGameOption.

### Basic properties

-   **Color Depth** - the number of colors your game will use. Default
    is 32-bit, which lets you use all the range of colors contemporary
    devices support. 16-bit is rather a compatibility setting, that will
    reduce the size of your game resources at the cost of color
    precision. 8-bit color mode is a special feature for making
    palette-based games. See also: [Palette setup](Settingupthegame#palette-setup),
    [Palette functions](Globalfunctions_Palette)
-   **Developer name** - this will add the provided string to the game's
    executable properties.
-   **Game file name** - your game's executable and/or data filename. 
    This name will be used when creating game package files on disk.
-   **Game name** - your game's title. This string will be displayed at
    the window title, and also added to the game's
    executable properties.
-   **Maximum possible score** - the maximum score your game has, if you
    are using score mechanics, such as [`GiveScore`](Globalfunctions_General#givescore)
    script function.
-   **Render sprites at screen resolution** - whether characters and
    objects should be scaled in screen pixels rather than game pixels.
    What this means is that when low-resolution game is run in larger
    window, sprites will take advantage of this higher resolution and
    look less pixelated when scaled down. If you prefer to keep your
    game looks in particular style, this option may be locked to always
    "Enabled" or "Disabled"; otherwise setting it to "User defined" will
    let your players toggle it in game setup program to their
    own liking.
-   **Resolution** - the native resolution of your game. This is the
    most important option (on par with Color Depth), which determines
    the size of game area visible on screen at any given time. This is
    also the minimal size that game rooms may have. The window your game
    runs in may still be larger or smaller, depending on choices player
    made in setup program, and in that case game's image will be
    stretched or shrunk accordingly.
-   **Text format** - the format your game texts will be in. The contemporary default is "Unicode", which lets you have practically any language in the game. Another choice is "ASCII / ANSI" mode, but this is left strictly for backwards compatibility, e.g. for when upgrading old projects, and not recommended to be used in new projects at all, as that will make multi-language support in your game much more complicated.

    **NOTE:** please be aware that when switching **Text format** the Editor will convert all the game files (scripts, etc) to a new text encoding and re-save them. Normally this should be safe, but probably a good idea to make a project backup before doing this.
    
### Information

Most of this information is not used right now by the engine, but may be made
accessible in future versions.

-   **Developer website** - Your website.
-   **Game description** - The game hook line.
-   **Genre** - Your game genere.
-   **Release date** - Date on which this game is first released.
-   **Version** - a 4-piece version string of your game, made of numbers 
    separated by three dots.

### Android

-   **App ID** - The Application ID, used in app stores. Also called package
    name, it's usually looks like com.mystudio.mygame, and it's used in store 
    URLs. It must have at least two segments (one or more dots), and each
    segment must start with a letter.
-   **App Version Code** - The version ID used by Google Play Store and others.
    It must be a positive integer, and different from the last one uploaded.
-   **App Version Name** - The version name visible to users in the stores,
    can be any string. If you leave empty, AGS will use the version you set in
    the Information group.
-   **Build Format** - How your android app will be packaged. When testing locally,
    always use ApkEmbedded. Google Play only accepts AAB. Use AabEmbedded if your
    packaged game size is lower than 100MB, and Aab if your packaged game size
    is lower than 500MB. Bigger game sizes are not possible right now under
    current Play Store restrictions - AGS can build, but the submission will
    get rejected.

### Backwards compatibility

-   **Allow relative asset resolutions** - if enabled then your game will scale 
    sprites according to their resolution tags: sprites tagged as "low-res"
    will be scaled up in a "high-res" game (640x400 and higher), sprites tagged
    as "high-res" will be scaled down in a "low-res" game (less than 640x400).
    When disabled resolution tags will be ignored as all sprites displayed with
    their real size (this is default).
-   **Enable mouse wheel support** - if enabled, on_mouse_click can be
    called with the values eMouseWheelNorth and eMouseWheelSouth, which
    signify the user scrolling their mouse wheel north or
    south, respectively.
    
    **NOTE:** Not all mice have mouse wheels, therefore its suggested
    that your game should never require the mouse wheel in order to be
    playable - it should only be used as a handy extra.
-   **Enforce new-style audio scripting** - Puts the script compiler
    into strict mode, where it will not accept the old-style
    (pre-AGS 3.2) audio-related script commands.
-   **Enforce new style strings** - Puts the script compiler into strict
    mode, where it will not accept the old-style (pre-AGS 2.7)
    fixed-length strings.
-   **Enforce post-2.62 scripting** - Puts the script compiler into
    strict mode, where it will not accept the old-style (pre-AGS 2.7)
    script commands. This should preferably be ticked, since you should
    no longer be using the old commands.
-   **Left-to-right operator precedence** - if this is enabled, then
    operators of equal precedence in the script will be evaluated left
    to right. For example, 5 - 4 - 3 could be interpreted as (5 - 4) - 3
    or as 5 - (4 - 3), thus giving different results. You should always
    use parenthesis to clarify expressions like this, so that the
    operator precedence doesn't affect the result.
-   **Old-style letterbox mode** - only available if your game's resolution
    is 320x200 or 640x400. If you enable it, your game will run as
    320x240 and 640x480 game correspondingly, while keeping room
    viewport size at 320x200 or 640x400, and adding black horizontal
    borders above and below. Today this is strictly a compatibility option
    for importing old projects, because since v3.5.0 AGS supports custom room viewports in script.
-   **Script API version** - defines the topmost level of built-in
    script content that you want to enable for your project. It is
    suggested to leave this at the "Highest" value, unless you are
    importing an older project and newest built-in script functions
    conflict with some of your own scripts. In such case you may decide
    between fixing your script or lowering AGS API version. The latter
    will let you compile game scripts without any changes, at the price
    of not being able to use newer built-in functions. You may still
    change it to "Highest" anytime later.
-   **Script compatibility level** - defines the lowest level of
    built-in content. It is useful if you wish to keep using some of the
    old functions that were declared obsolete by newer version of AGS.
    You do so by setting this switch to version that still had those
    functions non-deprecated.
-   **Use low-resolution coordinates in script** - always use 320x200
    coordinate space when scripting your game, regardless of its actual
    resolution. Basically, your game will be treated as if it were
    320x200, but pixels are of larger size. Normally this option should
    be off; it may only be useful when importing really old game project
    where such setting was a norm.
-   **Use old-style custom dialog options API** - switch to using
    pre-AGS 3.4.0 custom dialog options callbacks. The differences
    between old and new APIs [are explained in this topic](UpgradeTo34).
-   **Use old-style keyboard handling** - Uses pre-unicode mode key codes in
    `on_key_press` function, where regular keys were merged with Ctrl and Alt
    modifiers. In newer games, it's recommended to leave this as false, and
    either handle the additional 'mod' parameter or rely on `on_text_input`
    function for properly handled characters when building text input
    interfaces.

### Character movement

-   **Automatically move the player in Walk mode** - normally, when you
    click the mouse in the Walk mode, the main character will move to
    where you clicked. However, if you want to create a game all viewed
    from a 1st-person perspective, and so don't have a main character,
    then disabling this option allows you to use the Walk mode for
    other things. If disabled, then "Character stands on hotspot" events
    are instead triggered by clicking the Walk cursor on the hotspot.
-   **Automatically move to hotspots in Look mode** - controls whether
    the player will walk to "walk-to" spots when the player looks at
    the hotspot. Normally he only walks on use, speak and use-inv.
-   **Characters turn before walking** - specifies that when a character
    starts to walk somewhere, it will first turn round to face the
    correct direction using available animation frames, rather than just
    suddenly switching to face the right way.
-   **Characters turn to face direction** - if set, then when a
    character turns round with the
    [`Character.FaceLocation`](Character#characterfacelocation) or
    [`Character.FaceCharacter`](Character#characterfacecharacter) script
    commands, they will visibly turn around using their available loops.
    If this option is not set, they will immediately appear facing their
    new direction.
-   **Scale Character sprite offsets** - if set, then visual character's offsets will be scaled in proportion to the current character's scaling. This refers to values of [`Character.z`](Character#characterz) property and parameters of [`Character.LockViewOffset`](Character#characterlockviewoffset).
-   **Scale movement speed with room's mask resolution** - Character walking
    and object movement speeds will scale inversely in proportion to the
    current room's Mask Resolution. For example, having 1:2 mask resolution
    will multiply speed by 2. This is a backward compatible setting that should
    not be enabled without real need.

### Compiler

-   **Attach game data to exe** (Windows only) - when enabled the main game
    data will be appended to "gamename.exe" file. This is how AGS games were
    packed traditionally, and is on by default. When disabled game data will
    be packed into the separate "gamename.ags" file and placed alongside with
    the game exe.
    Disabling this option will make the game file structure more transparent
    and, for example, may help to prevent false positive reports from
    antiviruses that often don't like it when AGS engine reads data from exe.
-   **Build target platforms** - a checklist of platforms for which the
    game will be compiled.
-   **Enabled Debug Mode** - whether the debug keys are active. When
    debug mode is on, you can press Ctrl-X to teleport to any room,
    Ctrl-S to give all inventory items, Ctrl-A to display walkable areas
    on the screen, and Ctrl-D to display statistics about the
    current room. When debug mode is off, these do nothing. See the
    [Debugging features](Debuggingfeatures) section for more.
-   **Enable sprite storage optimization** - When possible save sprites in game
    files in a format that requires less storage space. This may reduce the
    compiled game size on disk, but effect may differ depending on number of
    colors used in sprites, and other factors.
-   **Package custom data folder(s)** - A comma-separated list of folders, relative to the game project folder. These folders, *including their subfolders*, and all the files in them will be added to the game package when the game is compiled, and you will be able to refer them in script commands using the `$DATA$` location tag.<br>
    For instance, if you'd like to add files from a subfolder "MyData", then insert "MyData" in this field, and access these as `"$DATA$/MyData/somefile.dat"` in script. If you'd like to have three subfolders "MyData1", "MyData2" and "MyData3" added, then insert a string "MyData1, MyData2, MyData3" into this property.<br>
    For further details on  see [`File.Open`](File#fileopen).
-   **Split resource files into X MB-sized chunks** - see
    [here](DistGame#splitting-resource-files) for information. 
-   **Sprite file compression** - when enabled the sprites will be
    compressed to reduce game size. In theory this may affect runtime performance (sprite loading times), but the actual effect depends on multiple factors including specifics of the system the game is running on, so not easy to predict. On desktop platforms the difference in speed is usually negligible. AGS provides few compression types, and the best choice may depend on the sort of sprites you are using in your game on average.

    - **None** - no compression will be used. This setting is not recommended for the game release.
    - **RLE** - this compression type is best suited for low-resolution and low-detailed graphics with limited number of colors.
    - **LZW** - this compression type is better suited for high-resolution and highly detailed graphics.
    - **Deflate** - high level compression, also used in PNG and ZIP formats.

### Dialog

-   **Allow speech to be skipped by which events** - determines how and
    whether the player can skip speech in-game. This can be set to allow
    the mouse and/or keyboard, or neither, to skip speech in the game.
-   **Custom Narrate function in dialog scripts** - determines which function will be used to substitute standard narration in dialog scripts. For example, if you have

    ```agsdialog
    narrator: The man looks you in the eye.
    ```

    in a dialog script, then normally this is replaced by

    ```ags
    Display("The man looks you in the eye");
    ```

    during compilation. With the above setting you can provide *the name* of your custom function that you've defined in your script. Such function must have one of the following prototype forms:

    ```ags
    function CustomNarrate1(const string text);
    function CustomNarrate2(String text);
    ```

    The return value is actually not essential and may be any type.<br>
    If the field is left empty then the standard Display function is used.
-   **Custom Say function in dialog scripts** - determines which function will be used to substitute standard character cues in dialog scripts. For example, if you have something like

    ```agsdialog
    Roger: Hello, my name is Roger.
    ```

    in a dialog script, then normally this is replaced by

    ```ags
    cRoger.Say("Hello, my name is Roger.");
    ```

    during compilation. With the above setting you can provide *the name* of your custom function that you've defined in your script. Such function must have one of the following prototype forms:

    ```ags
    function CustomSay1(Character *c, const string text);
    function CustomSay2(Character *c, String text);
    function CustomSay3(this Character*, const string text);
    function CustomSay4(this Character*, String text);
    ```

    Last two variants are [extender functions](ExtenderFunctions) for Character struct.<br>
    The return value is actually not essential and may be any type.<br>
    If the field is left empty then the standard Character.Say function is used.

    **IMPORTANT**: this setting currently does not work with the "Say" checkbox in the dialog options list. The workaround is to duplicate option's text as a first cue in the dialog script.
-   **Dialog bullet point image** - defines the number of sprite to use
    as a bullet image before each dialog option.
-   **Game-wide speech animation delay** - defines a game-wide speech animation delay to use instead of individual character settings. This setting is only available if **"Use game-wide speech animation delay"** is enabled.
-   **Gap between dialog options** - defines the gap between the options
    displayed to the player in a conversation. Normally this is 0, which
    means the options are right below each other. Changing it to 1 or 2
    can make the option display look less cluttered; it's a matter of
    personal preference.
-   **Number dialog options** - enables keyboard shortcuts to choose
    dialog options (keys 1-9) and adds an index number before each
    dialog option when they are displayed to the player. For example,

        1. Hello there!
        2. Goodbye

    This allows you to visually show the player which option the
    shortcut keys will choose, as well as separating the options if you
    don't use a bullet point.

-   **Print dialog options upwards** - Normally, if you select a
    non-textwindow GUI for the dialog options, they will be printed from
    the top down. However, if you select this option they will go from
    the bottom of the GUI upwards.
-   **Run game loops while dialog options are displayed** - whether to
    allow game animations to continue in the background while waiting
    for the player to select a dialog option.
-   **Sierra-style portrait location** - if you're using Sierra-style
    speech, then this determines whether the portrait appears on the
    left or the right of the screen. The "alternate" setting means it
    swaps sides whenever a different person talks, and the "Based on X
    position" setting means that the side of the screen is chosen
    depending on where the characters are standing.
-   **Speech style** - in the default LucasArts-style speech, when a
    character talks, the speech text is displayed above their head in
    the game, and the character's talking view is used to animate the
    actual character.<br>
    However, if you set this option to Sierra-style then the talking
    view is used to display an animating portrait separately in the
    top-left of the screen, with the text to the right of it. This is
    similar to the way that Space Quest 5, King's Quest 6 and other
    later Sierra games worked. You can also cycle to another option,
    "Sierra- style with background", which is the same except a text
    window is drawn behind the speech text to make it easier to read.<br>
    "Whole Screen" uses a full-screen character portrait, like the way
    that QFG4 worked.
-   **Use game-wide speech animation delay** - defines whether to use
    game-wide speech animation delay as opposed to using the individual
    character settings.
-   **Use GUI for dialog options** - controls where the player's options
    for dialog are displayed. If set to 0, then in a conversation, the
    options will be displayed at the bottom of the screen. If you type
    in GUI's ID number, then instead the options will be displayed on
    the GUI you specify.

### Inventory

-   **Display multiple icons for multiple items** - normally, if the
    player has two of an inventory item, the item will still only be
    shown once in the Inventory window. If you check this option,
    however, then all the copies of the item that the player has will
    be displayed. Useful for RPG-style inventories.
-   **Inventory item cursor hotspot marker** - whether AGS should
    automatically add a marker to inventory item cursors to help the
    player see where the active hotspot is on the cursor. May either
    draw simple crosshair using told colors, or use specified sprite.
-   **Inventory item cursor hotspot marker crosshair color** - the primary color of the item cursor hotspot marker.
-   **Inventory item cursor hotspot marker dot color** - the secondary color of the item cursor hotspot marker.
-   **Inventory item cursor hotspot marker sprite** - the sprite number to use for the item cursor hotspot marker.
-   **Handle inventory window clicks in script** - AGS has a
    default built-in processing of Inventory Window GUI controls mimicking old Sierra point'n'click games, whereby a
    right-click will Look at the item, and a left click will select it
    if the cursor mode is Interact. However, if you enable this option,
    then clicking on an inventory item in an Inventory Window will call
    your `on_mouse_click` function with eMouseLeftInv, eMouseMiddleInv
    or eMouseRightInv, and you then need to process it yourself. You can
    use the `game.inv_activated` variable to find out the ID of an item the player clicked on.
-   **Use selected inventory graphics for cursors** - normally, when you
    select an inventory item the mouse cursor is changed into that item.
    However, if you want to create a LucasArts-style game (where the
    inventory cursor is always a cross-hair), disable this option and it
    won't be changed.

### Rooms

-   **Default mask resolution** - sets default value for MaskResolution property which will be applied for each new room in your game. Mask resolution defines the factor between room masks' sizes and room background size. Common is 1:1, but you can choose other options for less precise masks, which may reduce data size and slightly improve performance in high-resolution games.

### Saved Games

-   **Save games extension** - determines the special extension for your
    save files.
-   **Save games folder name** - determines the name of folder created
    in the user's Saved Games location to store your game's saves. If
    left blank, then the game's title is used as folder's name. You
    might need to change this only if your game's title conflict with
    some other game.
-   **Save screenshots in save games** - Saves a mini-screenshot of the
    player's current position into the save game file. This will create
    larger save game files, but it will mean that you can use a save
    game thumbnails GUI to make the save/load interface
    more professional.
    
There used to be an option for Save Game integration with Windows Vista, it
was called **Enhanced save games**. Since this feature support was removed in modern versions of
Windows, this option has been removed since AGS 3.6.0 as well.

### Sound

-   **Play sound when the player gets points** - controls whether a
    sound effect is played when the player scores points. If so, you can
    select an audio clip here (but you have to have some imported in your game before doing this).

### Text output

-   **Always display text as speech** - if you select this option, then
    all normal text in the game will be displayed above the main
    character's head as speech text, much like the way the LucasArts
    games worked. If this option is not checked, then normal text
    appears in a pop-up message box, like the way that the Sierra
    games worked.
-   **Anti-alias TTF fonts** - If enabled, any TTF fonts you have in
    your game will be rendered to the screen anti-aliased. This can make
    them look a lot better, but it has two drawbacks - firstly,
    anti-aliasing is significantly slower than normal rendering, so you
    might want an option to allow the player to turn it off. Second,
    anti-aliasing only works in hi-color games (in 256-color games, the
    output will look blurred and unreadable).
-   **Custom text-windows GUI** - allows you to customize the standard
    text window appearance in the game, using the specified interface
    element. See [here](EditorGUI#customized-text-windows) for more
    information.
-   **Custom thought bubble GUI** - Determines which text window GUI is
    used for displaying thoughts with
    [`Think`](Character#characterthink).
-   **TTF fonts adjustment defaults** - Automatic adjustment of the true-type
    font metrics; primarily for backward compatibility.
    
    This option will be used as a default value for each new imported font, but
    you may also customize it in the Font's properties.
-   **TTF fonts height used in the game logic** - How the true-type font height
    will be defined whenever it is required by the script of game logic.  
-   **Write game text Right-to-Left** - in-game text will be written
    right-to-left, i.e. line breaks are worked out from the end of the
    sentence going backwards, and the last words are displayed first.
    This is used by languages such as Arabic and Hebrew.

### Visual

-   **Default transition when changing rooms** - defines what type of
    screen transition is used when moving from one room to another.
    Various options are available.
-   **GUI alpha rendering style** - determines which rendering method to
    use in 32-bit games when a GUI Control is drawn over GUI. The
    "Proper alpha blending" choice is meant for full alpha blending
    support, other options exist for compatibility with older versions
    of AGS only.
-   **GUI controls clip their contents** - determines whether the GUI controls will clip their graphical content, such as text, preventing it from being drawn outside of their rectangle. This setting has few exceptions, please see notes below.

    **NOTE**:
    - Button images are clipped using individual button's property called "ClipImage".
    - Label with a wrapped text will only ever display next line if there's at least 1 extra pixel of height available past the line spacing.
    - Sliders DO NOT clip as of AGS 3.6.0. This is left so for the time being, because the positions of their elements historically are calculated to be outside of their bounds.
-   **Pixel-perfect click detection** - tells how the AGS should detect when there's an object or character under cursor, or when you run a script command that looks for an object under certain coordinates. By default AGS checks if the coordinates are within a rectangular area of each character and object on the screen (this is also called "bounding box"). However, if this option is enabled, then it will further check whether the coordinates are on an actual non-transparent pixel of the object graphic. With Pixel-perfect mode the fully transparent pixels will be ignored (sort of "clicked through").

    **NOTE:** currently Pixel-perfect mode works only for objects and characters, and nothing else in game.
-   **Sprite alpha rendering style** - determines which rendering method
    to use in 32-bit games when an image is drawn over
    [drawing surface](DrawingSurface). The "Proper alpha
    blending" choice is meant for full alpha blending support, "Classic"
    style exists for compatibility with older versions of AGS only.
-   **When player interface is disabled, GUI should** - determines what
    happens to buttons on your GUIs while the game interface is
    disabled (e.g. during a cutscene).
