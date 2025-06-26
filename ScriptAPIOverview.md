## Script API Overview

AGS provides a scripting system, where engine functionalities are exposed through the AGS Script API, allowing you to write a mini-program, giving you great control over your game. You can read and modify room elements by using the structs [`Room`](Room), [`Object`](Object), [`Hotspot`](Hotspot), [`Region`](Region), and the *Global functions for Room*. If you need your character to *say* things, *receive or lose an inventory item*, or *walk* somewhere, you can find these in the Character struct. The AGS camera system is available through the use of [`Viewport`](Viewport), [`Camera`](Camera), and [`Screen`](Screen). You can also draw directly with [Dynamic Sprites](DynamicSprite), [Drawing Surface](DrawingSurface) and use [Overlays](Overlay) to have these sprites shown on screen. Browse around the [API topics](Scripting), as there's much more available to you.

**NOTE:** API stands for "Application Programming Interface". This is a common term, it defines all the means a program (such as the AGS engine) provides for the user to control it programmatically.

### API switches

When a new version of AGS is released and comes with new API entries for additional functionality these changes can possibly cause conflicts with previously made scripts. Sometimes a function gets deprecated (outdated) and disabled, or replaced by a different and better one. Sometimes a new function is added that uses a descriptive name which you already used in your script, which can obviously lead to conflicts. Usually it's a good idea to adjust your script and resolve these problems by hand. But if you do not want to or do not have the time to do so just yet - there are API switches.

In a nutshell, API switches let you control which parts of the API are active in your game.

In the editor the API switches are found in the [General Settings](GeneralSettings), in the "Backwards Compatibility" category. The relevant settings are:

* **Script API version** - defines the *topmost level* of the API. Default is the "Highest" level, which uses the topmost supported API by this version of AGS. Setting it to any version will lock the API at that level, disabling any additions (functions and properties) that were introduced to AGS Script later.
* **Script compatibility level** - defines the *lowest level* of the API. Default is the "Highest" level, which again uses only functionality officially supported by the current version of AGS. Setting it to a lower version enables older functions that now are deprecated.
* **Enforce post-2.62 scripting** - this is on by default, turning this off brings back a huge number of obsolete global functions from the era before AGS 2.7. Most of these functions were replaced by the introduction of [object structs](ScriptKeywords#struct) and properties. See [What's new in AGS 2.7](WhatsNewIn27) for more information.
* **Enforce new style strings** - this is on by default, turning this off reactivates old-style string functions and also the deprecated `string` type (the contemporary type is `String` with [capital S](String)) which was used differently and had a 200 character limit.
* **Enforce new-style audio scripting** - this is on by default, turning this off reactivates the old-style audio functions that are deprecated since AGS 3.2. These functions rely on passing clip numbers instead of using their script names (which can be chosen freely and are based on the filename), and support strictly 1 channel per audio type. See [What's new in AGS 3.2](WhatsNewIn32) for more information.
* **Use old-style custom dialog options API** - this is off by default, and enabling this switches to pre-AGS 3.4.0 custom dialog option callbacks. See [What's new in AGS 3.4](WhatsNewIn34) for more information.
* **Use old-style keyboard handling** - this is off by default, and enabling this switches to old-style key input handling style. See [What's new in AGS 3.6](WhatsNewIn#changes-to-key-input-handling) for more information.

Each of the above switches defines a macro or a number of macros which are then used during script compilation and help detect the state of these switches. You can also use these in your script, for example if you want to provide two or more variants of code for different API versions. For more information on this and actual script examples see [Constants](Constants).
