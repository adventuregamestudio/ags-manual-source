## Upgrading to AGS 3.6.3

3.6.3 is the "quality of life" update for the 3.6 version, which adds small improvements to the Editor's interface, new means to customize some of the previously hardcoded object parameters, and more game translation options. There are no breaking changes to the game settings or script. Here we mention few most noteable additions.

### Expanded GUI control visual styles

All the GUI controls now have properties that let create a frame border of certain thickness ([ShowBorder](GUIControl#guicontrolshowborder), [BorderColor](GUIControl#guicontrolbordercolor), [BorderWidth](GUIControl#guicontrolborderwidth)), set background color ([SolidBackground](GUIControl#guicontrolsolidbackground), [BackgroundColor](GUIControl#guicontrolbackgroundcolor)) and padding ([PaddingX](GUIControl#guicontrolpaddingx) and [PaddingY](GUIControl#guicontrolpaddingy)). This may be used for various purposes including uncommon ones, for example you may utilize a textless label with a border to create a box around other controls.

Buttons have [ColorStyle](Button#buttoncolorstyle) that let switch between emulated 3D and Flat looks, as well as a group of properties that change their colors for all 3 states (normal, hovered and pushed).

All of these parameters may be freely changed both in the Editor and in script.

### Translations can override fonts

You can now specify font overrides in the translation. This effectively replaces one font with another in game without a need to change anything in game properties or scripts; meaning that whenever game is instructed to draw a text with font A, it will instead use a replacement font B. Furthermore, this feature allows to "generate" new font instances just for this translation, using a set of parameters such as font file, size, outline style, and so forth. The usage of this feature is explained [on the respective page](Translations#font-overrides).

### Translating Text Parser

If your game features a text parser, it is now possible to translate its words dictionary so that players could type commands in the language of their chosing. See ["Translating Text Parser"](Translations#translating-text-parser) for more details.

### System limits update

* Max number of AudioChannels is now 32 (was 16).
* Removed limit of Dialog's options per topic (was 30).
