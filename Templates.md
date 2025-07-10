## New Game templates

When you choose "Start a new game" in the initial "Welcome to AGS"
dialog box, a window appears with various templates that you can base
your game off.

AGS comes with a few standard templates:

- Empty Game (...which is what it says)
- [Tumbleweed](Tumbleweed) (9-Verb)
- [Sierra-style](TemplateSierraStyle)
- [BASS (two click handler)](TemplateBASS) (like **B**eneath **a** **S**teel **S**ky)
- [Verbcoin](TemplateVerbcoin)

...but you can create your own too.

**NOTE:** the previously available "Default" template is now known as
"Sierra-style", which also contains updated graphics.

### If the list of templates is empty

The templates are normally located in "Templates" folder withing AGS Editor
directory. If the folder is empty, that could happen if AGS Editor installation
went wrong, or you have run from an incomplete package. What you may try:

- Reinstall AGS (preferably downloading an official release),
- Download templates (see below).
- Copy "Templates" folder from any other versions of AGS if you have them on
  your computer, but make sure these versions are not newer than the one you're
  going to use. Also in this case you have to be ready to possibly update the
  game scripts as templates may be using deprecated script commands.

### Using downloaded templates

If you've downloaded a game template from the internet, you should find
a file with a `.AGT` extension. This is the AGS Template File, and you
just need to copy it into the "Templates" folder within the AGS Editor
directory.

### Creating your own template

A game template is basically just an archive containing all of the game
source files, which are then extracted into the new folder when the user
creates a new game.

To create a template, first of all you create a game as normal in the
editor. Once you have everything set up how you want it, select "_Make_
_template from this game_" on the File menu. This will bring a regular "Save file as" dialog where you select a file name and location where you'd wish to save the template to, and then the Editor will compile the template for you.

The Editor has two ways of gathering the game files into the template.

By default it takes following files from your game's folder:

1. Core game files (`Game.agf`, `acsprset.spr`);
2. All script files (`*.ash`, `*.asc`);
3. Room files (`*.crm`);
4. Font files (`agsfnt*.ttf`, `agsfnt*.wfn`);
5. AudioCache directory with imported sound and music files;
6. Speech directory with voice-over files and lipsync files (if these are present);
7. Supported video files (`*.ogv`, `flic*.flc` and `flic*.fli`);
8. Translations (`*.trs`);
9. Text files, in case you'd like to include a `README.TXT` or whatever (`*.txt`, `*.pdf`);
10. Icons (`*.ico`);
11. Preload image file (`preload.pcx`).

The second way is to use "template.files" file, as explained in [the section below](Templates#creating-template-with-the-use-of-the-patterns-file).

If you include a **template.ico** file in your game folder when you make
the template, then it will be used as the icon in the Start New Game
dialog box. Otherwise, the icon will be taken from `user.ico` (if
present), or if not it will get the default AGS icon.

You can also include a `template.txt` file in your game folder. If you
do, then its contents will be displayed to the user in a messagebox
after they create a new game based on the template. You could use this
to explain briefly about any key aspects of the template, or it could
tell them to read your README file. This file should be quite small,
its entire contents need to fit into a standard message box, so keep
it below 255 characters.

NOTE: Do not simply make a template out of a half-finished game. If
you want to make a template, you should start a game from scratch and
make your changes - the user probably doesn't want to already have a
semi-completed game when they use your template. Rather, game templates
are about different types of game controls, gameplay mechanics you want
to share.

### Creating template with the use of the patterns file

Starting with 3.6.2 Patch 2 release of the AGS Editor, you may optionally go second way and write a pattern file for your template. This file must be called `template.files`, be located in the project folder, and contain any number of filepath patterns with wildcards. Following is the example of what a `template.files` contants may look like:

```
# Include the files that match the patterns:
Game.agf
*.asc
*.ash
*.crm
*.ttf
*.txt
*.wfn
acsprset.spr
Speech/
AudioCache/
template.ico
template.txt
template.files

# Exclude any files that match the patterns:
!_Debug/
!Compiled/
!*.bak
!*.user
!*.lock
```

On one hand this method requires writing an explicit list of files or patterns which you like to include, but on another it allows to include any custom files or subfolders and their contents.

*Compatibility:* The `template.files` is supported by **AGS 3.6.2 Patch 2** and later versions.

*See also*: [Setting up the Game](Settingupthegame), [Tutorials Index](StartingOff)
