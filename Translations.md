## Translations

### Overview

You can translate your game to multiple languages. In AGS the game translation works like a table of strings, where for each of the original text strings in your game you provide a translated replacement.
This includes speech text in dialogs, any text in double-quotes in regular scripts, and also certain property values (GUI texts, character names, hotspot descriptions and similar).
When you play a game with a translation enabled, AGS will automatically replace the displayed text on GUIs, character speech and text messages. For other cases there's a script function [`GetTranslation`](Globalfunctions_General#gettranslation) that lets you retrieve the translation for the wanted string which you may then use as necessary.

Let's move on to how translations are created in the editor.

![Add a translation to your game project](images/EditorTranslations_01.png)

When you have your game project opened right-click the "Translations" node in the tree, and choose "New translation". Once you've named it, AGS will ask if you want to populate the file now. Say yes.

A new file will be created in the game folder, called NAME.TRS, where NAME is the name you gave to the translation in the editor. This file is simply a text file by format and a .trs suffix, and will be generated with each original line of text separated by a blank line. That blank line is where you enter the translated text.

You can now work on translating these texts yourself or give this file to your translators. The idea is to fill in each blank line with the corresponding translation of the original line above it. If this empty line is left blank, it will simply not be translated and display the original line in the game.

**IMPORTANT:** make sure that the TRS file is saved in a correct encoding. If you want to have Unicode texts in your game then save it as UTF-8 (specifically: UTF-8 without BOM). Otherwise save it as ASCII. You would also have to add a Encoding hint to the translation options, see the ["Additional options"](Translations#additional-options) section about this.

**IMPORTANT:** there are few ways to break your translation file and make it unusable for AGS. The following points are essential to keep in mind:
- DO NOT remove or replace original text lines, they are necessary to find matches.
- DO NOT remove blank lines between original texts, just keep these empty lines if you are not going to translate the text. AGS treats each odd line as an original text and each even line as translated.
- DO NOT add any extra lines in the middle, for the same reason. There are exceptions to this rule - see below.

Once the translation is done, right-click the translation and choose
"Compile". It will be converted into a compiled translation (`.TRA`)
file in the Compiled folder, which can be used with the game engine.

Run the game Setup program, and select the translation from the
drop-down box. Then, run the game, and all the text should be
translated.

While most in-game text is translated automatically, there are a few
instances when this is not possible. These are when a script uses
functions like Append to build up a string, or CompareTo to check some
user input. In these cases, you can use the
[`GetTranslation`](Globalfunctions_General#gettranslation) function to make it work.

[Formatted strings](StringFormats) may also end up in translation file, these are ones that contain input placeholders such as '%s', '%d' and so on. It's *essential* that the translated line has all the same placeholders and has them in the same order. Otherwise this line may cause errors in game.

Please note, there is also an "Update" option when right-clicking a
translation. This is useful if you've got a translated version of your
game, but you want to update the game and add a few bits in. Once you've
updated your game, run the Update Translation option and the translation
file you select will get any new bits of text added to it at the bottom
-- then you can just ask your translator to additionally translate these
lines.

### Additional options

There are two more things you can add to the translation file: comments and font options.

Comment lines should begin with `//`, just like in script, and anything else on the same line will be ignored, and such lines won't be counted when deciding which line is original text and which is the translation.
Comments are allowed to be placed anywhere in the file, and they are a good way to keep notes for yourself or your translators.

Font options let you define certain font substitutes for this particular translation. All options begin with the comment opening and a pound sign: `//#` - followed by `OPTION=VALUE` kind of string where "OPTION" is the corresponding name. Following options are supported:
- **NormalFont** - sets the font used for displaying messages on screen. This corresponds to the [`Game.NormalFont`](Game#gamenormalfont) script property. The value may be either the font's number or `DEFAULT` for no change.
- **SpeechFont** - sets the font used for character speech. This corresponds to the [`Game.SpeechFont`](Game#gamespeechfont) script property. The value may be either the font's number or `DEFAULT` for no change.
- **TextDirection** - sets the direction in which the text is written. This corresponds to calling the [`SetGameOption`](Globalfunctions_General#setgameoption) script function with the OPT_RIGHTTOLEFT argument. The value may be `LEFT` (for left-to-right), `RIGHT` (for right-to-left) or `DEFAULT`.
- **Encoding** - sets the encoding hint that matches the encoding of the file. If you use UTF-8, the file must be saved with **UTF-8 without BOM** encoding. Your game can be set to a different encoding, just make sure that the font used supports the encoding used.
- **GameEncoding** - sets the game encoding hint that tells which locale does the game text use. If the game is made in Unicode mode, then you don't normally need to set this. This option may be useful if the game is done in ASCII/ANSI mode, and base language is not English. In such case the engine needs to know how to interpret game texts when trying to find a translation entry for them. The value of GameEncoding should be set in a format ".NNNN" where "NNNN" is a number of the respective ANSI code page (e.g. ".1252").
- **Language** - lets specify a base game language. This setting is be used whenever engine needs to identify the game text as being of a particular language or using particular alphabet. For example: alphabetic string comparison and sort. In particular, this setting affects "locale aware" string compare styles in script. Assign a standardized language definition in a form of "en_US", or leave unassigned if you don't require this feature.
- **AutoTranslateParserSaid** - can be either "ON" or "OFF" (where OFF is a default). If enabled, then pattern strings passed into Parser.Said function will be translated automatically (if the translation is provided by you). See ["Translating Text Parser"](Translations#translating-text-parser) for more details.

Example:

```
// The normal font to use - DEFAULT or font number
//#NormalFont=4
// The speech font to use - DEFAULT or font number
//#SpeechFont=DEFAULT
// Text direction - DEFAULT, LEFT or RIGHT
//#TextDirection=RIGHT
// Text encoding hint - ASCII or UTF-8
//#Encoding=UTF-8
```

This would set NormalFont to font 4, leave SpeechFont unchanged, and switch text direction to Right-to-left mode. The Encoding hint tells that the translation uses UTF-8 encoding.

**NOTE:** these options are applied to game properties the moment new translation is enabled. But they are not kept permanently while that translation is active. Changing any of these in script would override values set by current translation.

### Translating Text Parser

This feature is available since **AGS 3.6.3**, and must be enabled in [General Settings](GeneralSettings#translation) using "Translate Text Parser" option.

For the Text Parser's overview please see the [respective section](TextParser).

The Text Parser is special in a way that its synonym words cannot be translated individually. That's because another language may not have a direct equivalent to every synonym of the base language, or, on contrary, require more synonyms compared to the base language, for convenience. There also is a chance that certain word is met both in regular game texts and parser's dictionary, and must be translated differently in these two cases.

For that reason, Text Parser's words are not added into the Translation one by one, but instead added as "word group" entries containing whole list of comma-separated synonyms. Similarly, they must be translated as a list of comma-separated synonyms.

Each Text parser's entry is prepended with a "//$PARSERWORD=N" annotation, where N is a word group ID. This annotation must not be removed.

Another thing that you might want decide is whether you will translate Parser.Said patterns. These patterns usually have a certain order of "object" and "verb" words. For example, in English language, the order is "verb object", such as "take apple" for example. Other languages may have either same or different order. If the order is same, then you dont have to translate these (you might still do if you want). If the order is different, then you SHOULD translate these, using exact words from the translated word groups (or players will find that they have to type commands using unusual expressions).

If you translate Parser.Said strings, then consider enabling automatic translation of this function's input using "AutoTranslateParserSaid" option (see [list of options](Translations#additional-options)).

### Font overrides

Starting with **AGS 3.6.3** you can specify font overrides in the translation file. Font override defines replacement of one or more of the game fonts with either another game font, or completely custom font setup. These replacements are applied whenever translation is enabled in game, and are restored to defaults when it's disabled.

Similar to the translation options described above, a font override must be preceded with the comment opening and a pound sign `//#`.
A font override itself can have one of two syntaxes. The first syntax is for the direct font replacement:

  ```FontN=FontX```
  
This syntax simply replaces a game font N with the game font X. This means that whenever font N is referenced in game, a font X will be used instead.
The second syntax is for the custom font setup:

  ```FontN=PROPERTY1=VALUE1;PROPERTY2=VALUE2;PROPERTY3=VALUE3...```

Here properties and their values may be any of the following:

- **File** - font's filename; e.g. "agsfnt10.ttf".
- **Size** - import font size (number).
- **SizeMultiplier** - an integer size multiplier (useful only for bitmap fonts).
- **Outline** - font's outline type, can be: NONE, AUTO or FontX, where X is a outline font's index.
- **AutoOutline** - automatic outline's style: SQUARED or ROUND.
- **AutoOutlineThickness** - automatic outline's thickness, in pixels.
- **HeightDefinition** - how the font's logical height is defined, can be: NOMINAL, REAL or CUSTOM. NOMINAL uses font's Size as its height, REAL uses actual font's height calculated after loading a font, and CUSTOM lets specify a height value.
- **CustomHeight** - custom font's logical height, in pixels.
- **VerticalOffset** - optional vertical offset used when drawing any text with this font.
- **LineSpacing** - custom distance between top of one line of text and top of the next line of text.
- **CharacterSpacing** - custom additional horizontal distance between any two characters of text.

Examples:

```
// Replace font 0 with font 10
//#Font0=Font10
// Replace font 1 with a custom font
//#Font1=File=example.ttf;Size=12;Outline=AUTO;
```

### Other annotations

Since **AGS 3.6.3** the obsolete entries are annotated with "//$OBSOLETE". These will not be included when compiling a translation. What to do with these is entirely up to you; you may safely delete them in any case.

### Troubleshooting

If you have ?s displaying instead of special characters in the translated lines, make
sure the [font](Game#gamespeechfont) you use is imported into AGS correctly and supports the
characters you want to display. In case some letters are mixed up this might be a mapping
problem of the source font, you need to manually edit this fontsheet or just find a new font
on the world wide web that works.

When you play the game with the translation enabled and special characters of the
language are replaced with ?? (2 question marks specifically), this means the actual encoding
of the .trs files uses 2 bytes for each character, probably because you saved the file as
UTF-8 and either set the encoding to ASCII or not set at all. If you wish to use ASCII, make 
sure you save and encode the .trs file to ANSI and NOT in unicode or UTF-8 encoding.

In some wordprocessors changing the encoding of the whole file to ANSI encoding changes the normal
special characters to something even more unrecognisable, so you might have to run a
search and replace function over the whole document for every special character that
is used in that language (make sure to check for capitalized special characters too)
after changing the encoding to ANSI. A lot of wordprocessors use Ctrl+H as hotkey for the
search and replace function.

### Advanced Help

There is a great Editor Plugin called [Speech Center](https://www.adventuregamestudio.co.uk/forums/index.php?topic=45622.0) that makes handling translations easier. You can find a download link for the file in the linked AGS Forum thread.



*See also:*
[`Game.ChangeTranslation`](Game#gamechangetranslation),
[`Game.TranslationFilename`](Game#gametranslationfilename),
[`GetTranslation`](Globalfunctions_General#gettranslation), [`IsTranslationAvailable`](Globalfunctions_General#istranslationavailable)
