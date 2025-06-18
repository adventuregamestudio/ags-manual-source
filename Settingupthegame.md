## Setting up the game

Now that you know how to create a room, it's time to set up the
game-wide settings. These include inventory items, sprite graphics,
palette setup and other things which do not depend on individual rooms.


---

### Palette setup

The first thing you need to do when you create a new game is to decide
whether you want to use 8-bit (palette-based) color or 32-bit
(true-color). If you want to use 32-bit color, you can
still use 256-color backgrounds and sprites if you want to, but the
engine will only run in a 32-bit color resolution, thus slowing it
down.

If you want to use 8-bit, you need to set up the palette. This is
because all sprite and background scene imports rely on the palette
setup to be the same. You **CANNOT** use true-color
sprites or backgrounds in a 256-color game.

All this means, use 8-bit color only when you know what you are doing or when
you intentionally want to punish yourself with lots of nerdy fumbling around on
settings for colormodes that were not artist's decisions but actual hardware
limitations of the time and every artist is glad these restrictions are no
longer in place.

But you can do really cool color cycling effects only in 8-bit games,
other than that this is really outdated technology, but it still works
of course, and how it works is explained in the next paragraphs.


You set your chosen color depth by opening the General Settings pane
and adjusting the Color Depth setting near the top of the list.

After that double click the "Colours" node in the project tree, which will open a "Colours" pane and let you set up your game's palette. Please continue to the [Colours Editor](ColoursEditor) topic for more information.

---

### Inventory

Most adventure games allow the player to carry a set of objects, which
he can then use to solve puzzles. Adventure Game Studio makes this
inventory easy for you to manage.

Every inventory item which the player may carry during the game at one
time or another is listed under the "[Inventory items](EditorInventoryItems)" node. Here, each
item has a number and a script name which you use in scripts to identify
the object. To create a new item, right-click on the "Inventory items"
node.

Double-click on an inventory item to open it up. On the left you'll see
the graphic used for the object in the inventory window. To change this,
select the "Image" entry in the property grid on the right, and click
the "..." button.

The last thing to do with the inventory items is to define their events:
what happens when the player manipulates them in the inventory window.
Click the "Events" button (the lightning bolt button at the top of the
property grid), which brings up a list which works identically to the
hotspot events. The available events are described in the reference
section.

*NOTE: Each character in the game carries their own set of inventory
items. This means, if you want to create a game like Day of the
Tentacle, where the player can control three different characters, each
character will have a separate inventory.*

You have two choices about how the inventory is displayed to the player
-- a built-in inventory window to get you started, and support for
custom inventory windows when you're ready to make your own.

The default option is the Sierra-style pop-up inventory window, which is
popped up by clicking on the Inventory icon on the icon bar. You can
also have the current inventory item displayed in its own button on the
icon bar by creating a button on the GUI and setting its text to (INV)
which stretches the item picture to the button size, or (INVNS) which
draws the inventory item picture straight onto the button with no
resizing. Finally, (INVSHR) , probably the best option, will draw it at
actual size if it will fit, or shrink it if not.

The other option is a custom inventory window. To use this, you will
need to edit the GUI to add it, so I will explain this later on. While
you are starting off with AGS, it is recommended to use the supplied
standard Sierra-style inventory window.

Finally, you may have noticed a "Hotspot Marker Settings" frame at the
top of the Inventory pane. This allows you to switch on an option so
that when the selects an inventory item, the mouse cursor for it will
have a dot and mini-crosshair drawn on it, to show the player where the
hotspot is. You can enter the color for the center dot and also for the
surrounding 4 pixels.

---

### Importing your own sprite graphics

When you were choosing the graphics for the object earlier in this
tutorial, you probably noticed that most of the graphics available
didn't look up to much. This is no problem, because you can import your
own graphics using the Sprite Manager.

Go to the **Sprites** pane in the editor. Here, you will see the
complete sprite set for the game. There are two ways to import your
graphics - either overwrite an existing slot with your graphic, or
create a new slot for it.

To overwrite an existing sprite, right-click the sprite and select
"Replace sprite from file". To import a new slot, right-click on the
background to the window and choose "Import new sprite".

The graphic you choose to import must be at the same color depth as
your game (i.e. if you are using true-color backgrounds, your sprites must
be true-color, and vice versa). AGS will attempt to convert the image if
possible, but if your game is 256-color then the results of downgrading
a true-color image can be poor.

Then, the Import Sprite window will appear. Here, you need to decide
which portion of the image will be imported. You do this by
right-clicking and dragging in the image, which will produce a yellow
rectangle showing the selection. Once you are happy with it, left-click
to import. Alternatively, you can import the entire image with the
"Import whole image" button.

*NOTE (256-color only): You may well find that the colors on your
graphic look slightly strange in the AGS Editor. This is because the
sprites are only allocated, by default, the first 41 of the palette
colors (see the [palette section](Settingupthegame#palette-setup)), so your graphic
will be remapped to this much smaller palette. If you find that many of
your imported sprites look strange, you can increase the number of
colors assigned to sprites, at the expense of background colors (again
see the section above for information on how to do this).*

If your sprite will only be used in one room then alternatively you can
use the "use background palette" option, which will remap your graphic
to the palette of the room currently loaded, giving much better results.
Note, however, that if you do this, and then try and use the sprite on
another screen, its colors will most likely be screwed up. To use the
room palette, check the "Use room background" check-box. Make sure to
un-check this box before you import any other sprites.

NOTE: The transparent color used by AGS is palette index 0 (for
256-color sprites) and RGB (255,0,255) for hi-color. Any pixels you
draw on imported sprites in these colors will be transparent.

You can group imported sprites into folders. This prevents the main
sprite list from becoming too long. By default, the Sprite Manager
displays the Main folder, which contains some graphics and a sub-folder
called "Defaults". Folders work the same way as Windows folders.
Right-click on a folder in the tree to rename it or make a sub-folder.

You can delete a folder by right-clicking on it and selecting the
"Delete" option; beware though that **this will also delete all the
sprites in the folder**.

For more information, see [Sprite Manager](EditorSprite) topic.

---

### Introduction sequences

You can easily add intro, outro and cutscene sequences to your game.
There is no specific function to do these, but using the provided
animation and script commands you can create almost anything you might
need.

Normally, the game will start in room 1. This is defined by the starting
room number of the player character. To change it, open up the player
character's Character pane, and change the StartingRoom number in the
property grid.

TIP: The starting room facility is also useful when testing your game.
You can make the game start in any room, at the point where you are
testing it, rather than having to keep playing the game through to get
there.

Cutscenes are created using the normal animation script commands, such
as Character.Walk, Object.SetView, and so forth. I would suggest you
leave this until you are more comfortable with AGS, and have some
experience of how to use these functions.

---

### Animations

In most games you will use some sort of animation during the game,
whether it be a flag waving in the breeze or the player bending over to
pick something up. The term "animation" refers to the ability to change
the look of, and move, objects.

Animations in AGS are managed using Views. A "view" is a set of one or
more "loops". A loop is a set of frames which, when put together, give
the effect of movement. Each frame in the view can be set a graphic and
a speed.

Go to the editor's "Views" node, right-click it and select the "New
view" option to create us a new, empty view. Double-click the new view
to open it. Each loop is displayed horizontally with its number at the
left hand side, frames going out to the right. To add a frame, click the
grey "New frame" button. To delete a frame, right-click it.

To change a frame's graphic, double-left-click it. The sprite list
screen will be displayed (you may remember this from the Object graphic
selection) where you can choose the graphic you want to use for this
frame.

Note that for walking animations, the first frame in each loop is
reserved for the standing frame, and when walking it will only cycle
through from the second frame onwards.

You select a frame by left-clicking it -- when you do so, the property
grid will update with information about the frame. One of these settings
is called "Delay", which is the frame's **relative** speed. The larger
the number, the longer the frame stays (i.e. the slower it is). When the
animation is run, an overall animation speed will be set, so the actual
speed of the frame will be: overall_speed + frame_speed . Note that
you can use negative numbers for the frame delay to make it particularly
fast, for example setting it to -3 means that the frame will stay for
hardly any time at all.<br>
Animation speed is specified in Game Loops (i.e. animation speed 4 will
show the frame for 4 game loops - at 40 FPS, that would be 0.1 seconds).

The "Sound" property allows you to enter a sound number that will be
played when this frame becomes visible on the screen. This is especially
useful for footstep sounds.

You run an animation by using the script animation commands, which will
be explained in detail later. Briefly, to animate an object, you first
of all need to set the object's view to the correct view number (use the
Object.SetView script command), and then use the Object.Animate script
command to actually start the animation.

---

### Characters

A character is similar to an object, except that it can change rooms,
maintain its own inventory, and take part in conversations (more on
these later). It can also have its own custom animation speed and
movement speed.

Go to the "Characters" node in the main tree. You will see under it a
list of all the characters in the game. To create a new character,
right-click the "Characters" node and choose the "New character" option.

You will see that there are a lot of options which you can set for each
character. The most immediately obvious one is the "Make this the player
character" button, which allows you to change which character the player
will control at the start of the game. When the game starts, the first
room loaded will be this character's starting room.

The rest of the options are hidden away in the property grid on the
right. Some of them are described below:

The "UseRoomAreaScaling" option allows you to specify whether this
character will be stretched or shrunk in scaling areas of the screen.
You might want to disable this if you have a character who always stands
still in the same place, and you want the graphics on-screen to be the
same size as you drew them, even though he is standing on a scaled area.

The "Clickable" option tells AGS whether you want the player to be able
to click on the character. If Clickable is enabled, then the character
will be interactable, like the way things worked in Sierra games. If it
is not enabled then the character works like the main character did in
LucasArts games - if you move the cursor over him or click to look,
speak, etc, then the game will ignore the character and respond to
whatever is behind him.

To set which room this character starts in, change the "StartingRoom"
property. You can set the character's location within this room by using
the "StartX" and "StartY" properties to type in the X,Y co-ordinates you
want him to start at. These co-ordinates define where the middle of his
feet will be placed.

The "NormalView" is where you set what the character looks like. You
must create a view in the [View Editor](Settingupthegame#animations), and this view
must have either 4 or 8 loops. If you use 4 loops, then when walking
diagonally the closest straight direction is used for the graphics. Each
loop is used for the character walking in one direction, as follows:

     Loop 0 - walking down (towards screen)
     Loop 1 - walking left
     Loop 2 - walking right
     Loop 3 - walking up (away from screen)
     Loop 4 - walking diagonally down-right
     Loop 5 - walking diagonally up-right
     Loop 6 - walking diagonally down-left
     Loop 7 - walking diagonally up-left

To change the rate at which the character animates, change the Animation
Speed box. Here, a smaller number means faster animation. Note that this
does NOT effect the speed at which the character actually moves when
walking.

**NOTE:** The first frame in each loop is the standing still frame. When
walking, the game will cycle through the rest of the frames in the loop.

The "MovementSpeed" option allows you to control how fast the character
moves when walking. This value is in pixels per a single game update. A larger number would make a character walk faster, a lower number makes it slower. If you
find that a movement speed of 1 is still too fast, you can use negative
numbers (e.g. -3) which is interpreted as 1/N pixels per a game update (or N game updates to pass a single pixel).

The "SpeechColor" option specifies which color is used for the text
when this character is talking. It effects all messages that are said by
this character. You can find out the color for each number by going to
the "Colors" pane.

The "IdleView" option allows you to set an idle animation for the
character. To do this, create a new view, with one or more loops of the
character idle (e.g. smoking, reading a book, etc). Then, set the "Idle
view" to this view number. If the player stands still for 20 seconds
(you can change the timeout with the Character.SetIdleView script
function), then the current loop from the idle view will be played.

The "ScriptName" property sets the name by which the character will be
referred to in scripts and in conversation scripting. The difference
from the RealName of the character is that the script name may only
contain letters A-Z and numbers 0-9 (the first character must be a
letter, however). The convention in AGS is that character script names
start with a lower case "c".

To set what happens when the player interacts with the character, click
the "Events" button (this is the lightning bolt button at the top of the
property grid). You will be presented with the events list; select an
event and press the "..." button to allow you to enter some script to
handle the event.

You can also set a talking view for the character. To set one, use the
"SpeechView" property. If you set a talking view, then that view will be
used to animate the character while they are speaking. You should
generally have about 2-3 frames in each loop (the loops are used for the
same directions as in the main view).

There is also an available "Blinking view". This is used to play
intermittent extra animations while the character is talking. You may
want to use this for effects such as blinking (hence the name). If you
set a view here, it will play intermittently while the character talks
(it is drawn on top of the normal talking view). The default time
between it playing is 3-4 seconds, but you can change this with the
Character.BlinkInterval script property.<br>
**NOTE:** the blinking view is currently only supported with
sierra-style speech.

"UseRoomAreaLighting" allows you to tell AGS whether this character will
be affected by light and tint levels set on room regions.

If you disable "TurnBeforeWalking", it will override the General Setting
for turning and tell AGS not to turn this particular character around on
the spot before they move.

"Diagonal loops" specifies that loops 4-8 of the character's view will
be used for the four diagonal directions. If this option is not enabled,
the character will only face 4 ways, and you can use loops 4-8 for other
purposes.

"Adjust speed with scaling" modifies the character's walking speed in
line with their zoom level, as set on the walkable areas.

"Adjust volume with scaling" modifies the volume of any frame-linked
sounds on the character's view (e.g. footstep sounds) with their zoom
level, as set on the walkable areas.

"Solid" specifies that this character is solid and will block other
characters from walking through it. Note that **both** characters must
be solid in order for them to block one another.

AGS allows you to export your characters to a file, and then import the
file into a different game - so you can share the same main character
between games, or create one for distribution on the internet.
Right-click on the character and choose "Export character". The entire
character setup and graphics will be exported to the file, including the
character's walking and talking animations. To import the character into
a different game, load it up, right-click the "Characters" node and
choose "Import Character". The file selector appears, where you find the
CHA file which you exported earlier. A new character slot will be
created and all the settings imported.

*NOTE: Because importing always creates a new slot, you cannot use it to
overwrite an existing character.*

---

### Conversations

While the old Sierra games were mainly based on action and not talking,
the LucasArts games took the opposite approach.

If you want to create a game with conversations where the player can
choose from a list of optional topics to talk about, you can now with
the new Dialog Editor. Go to the "Dialogs" node.

Conversations are made up of Topics. A "topic" is a list of choices from
which the player can choose. You may have up to 30 choices in a topic.
However, not all of them need to be available to the player at the start
of the game - you can enable various options for conversation once the
player has said or done other things.

The Dialog Editor is quite self-explanatory. Double-click a dialog topic
to open up its window. You'll see the list of options for the topic on
the left, and the dialog script on the right. Each option has a couple
of checkboxes to its right:

-   The "Show" column specifies whether that option is available to the
    player at the start of the game.
-   The "Say" column defines whether the character says the option when
    the player clicks it. The default is on, but if you want options
    describing the player's actions rather than the actual words, you
    may want to turn this column off for that dialog.

**Dialog scripts**

You control what happens when the player chooses an option by editing
the script on the right. For more information on how to handle this script
see the [Article about Dialog Scripts](DialogScript).

**Parser input**

You'll notice in the dialog editor, the property grid has an option
called "ShowTextParser". If you enable this, a text box will be
displayed below the predefined options in the game, which allows the
player to type in their own input.

If they type in something themselves, then the dialog_request global
script function will be run, with its parameter being the dialog topic
number that the player was in.

AGS automatically calls ParseText with the text they typed in before it
calls dialog_request, so you can use Said() calls to respond. See the
[text parser](TextParser) section for more info.

---

### The General Settings panel

The General Settings pane contains a list of all the various overall
options that you can set for your game.

Many of these options can be changed at runtime with the script command
SetGameOption.

The available settings are detailed in documentation in the [General Settings](GeneralSettings)
page.

Some settings you may want to adjust right at  the start of your game include
**Developer name**, **Game name** and **Resolution**. Resolution is the most
important option, which determines the size of game area visible on screen at
any given time.

---

### Cursors

The Cursors node of the editor shows you the current mouse cursor modes
available in the game. Each cursor mode performs a different action
within the game. Double-click one to open it up.

The "StandardMode" option in the property grid tells AGS that this is a
'normal' cursor mode - i.e. using this cursor will fire an event on
whatever is clicked on as usual. This mode applies to the standard Walk,
Look, Interact and Talk modes, but you can create others too. Do not
tick it for the Use Inventory mode, since this is a special mode.

The "Animate" option allows you to specify that the mouse cursor will
animate while it is on the screen. Choose a view number, and the cursor
will animate using the first loop of that view. You can make it animate
only when over something (hotspot, object or character) by enabling the
"AnimateOnlyOnHotspots" option.

The "AnimateOnlyWhenMoving" box allows you to do a QFG4-style cursor,
where it only animates while the player is moving it around.

Three of the cursor modes are hard-coded special meanings into AGS:

-   **Mode 4 (Use Inventory)**. This is special because the game decides
    whether to allow its use or not, depending on whether the player has
    an active inventory item selected.
-   **Mode 6 (Pointer)**. This cursor is used whenever a modal dialog is
    displayed (i.e. a GUI that pauses the game). Normally this is a
    standard arrow pointer.
-   **Mode 7 (Wait)**. This cursor is used whenever the player cannot
    control the action, for example during a scripted cutscene. For a
    LucasArts-style game where the cursor disappears completely in this
    state, simply import a blank graphic over the wait cursor.

For the standard modes,

-   Mode 0 will cause the player to walk to the mouse pointer location
    when clicked.
-   Modes 1, 2, 3, 5, 8 and 9 will run the event with the same name as
    the cursor mode.

---

### Fonts

AGS comes with a couple of default fonts, but you can replace the and
add your own. You can use both TrueType (TTF) and SCI fonts (Sierra's
font format).

SCI fonts can be created in two ways:

-   Extract the font from a Sierra game, using the SCI Decoder program
    available on the internet.
-   Create your own font and save it in SCI Font format, using the
    [SCI Graphic Studio program](http://sci.sierrahelp.com/Tools/SCITools.html).

There are also some fonts available on the
[AGS website](https://www.adventuregamestudio.co.uk/site/ags/sci_fonts/).

When choosing whether to use SCI or TTF font following should be taken
into account:

-   SCI font is a raster (bitmap) font, which means that it does not
    scale and always keeps same size and pixel-precise visual shape.
-   TTF font is a vector font, that may be scaled to suit your game (you
    choose its size during import), and may be rendered
    with anti-aliasing.

In general you should prefer using TTF fonts, but SCI fonts may still be
useful in low-resolution games (because not all TTF fonts scale down
well) and situations where you need pixel-precise looks of the drawn
symbols.

Go to the "Fonts" node in the main tree. Here you can see all the
current fonts listed underneath. You can create a new font by
right-clicking the "Fonts" node and choosing "New font". To overwrite an
existing font, open it up and press the "Import over this font" button.

Please refer to the [Font Preview](EditorFont) topic for more information on how the font may be imported and configured for runtime.

**NOTE:** By default font 0 is used as the normal text font for in-game messages, and font 1 is used as
the speech font. To use any other fonts for these purposes, you can set the
[Game.NormalFont](Game#gamenormalfont) and [Game.SpeechFont](Game#gamespeechfont) properties in your script.

Now you are ready to read more about scripting.

Next Reading: [Scripting Tutorial 1](ScriptingTutorialPart1)

Return to Tutorials Index: [Tutorials Index](StartingOff)
