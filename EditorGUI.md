## GUI Editor

A game's user interface is typically split up into multiple GUIs. Each GUI is a
rectangular region on the screen which is drawn on top of the background scene,
typically configured with one of the following behaviors:

- always be displayed (for example the Sierra status-line)
- pop-up when the mouse moves to a certain position (e.g. Sierra icon-bar)
- pop-up on script command only

The default interface for the Sierra game templates is made up of two GUIs - the
status line, and the icon bar.

Go to the "GUIs" node in the main project tree. This is where all the GUIs in
the game are listed - double-click one to edit it. To create a new one,
right-click on the main "GUIs" node and choose "New GUI".

Once you've opened up a GUI, you'll notice the GUI itself in the main
window, and its settings can be edited in the Properties grid. This
allows you to change the background color of the GUI, set a background
image, and set the position, width, and height amongst other things.

The "Visibility" property allows you to set whether the GUI will be initially
visible or not.

The "PopupStyle" property defines some pre-configured behaviors for managing
GUI visibility:

| Setting | Behavior |
| --- | --- |
| Normal | Can be automatically hidden when user interface is disabled |
| When mouse moves to top of screen | Shown and hidden based on cursor position and "PopupYPos" |
| Pause game when shown | Automatically pause the game whilst the GUI is visible |
| Always shown | Never hidden when the user interface is disabled |

**NOTE:** The user interface is automatically disabled during blocking actions
but the same result is achieved by using the script function
`DisableInterface()`. To automatically hide a GUI during these times, the
General Settings option `When player interface is disabled, GUIs should` needs
to be set as `Be hidden`.

**NOTE:** Depending on how scripts are configured, pausing the game may also
disable the calls to input handling functions, such as `on_mouse_click` and
`on_key_press`.

The "Z-Order" setting allows you to set which order the GUIs are drawn in, i.e.
when there are two GUIs that overlap this determines which one will be drawn in
front of the other. The Z-order setting is an arbitrary number between 0 and
1000. AGS draws the GUIs in order, from the lowest value (0) at the back to the
highest value (1000) at the front.

The "Clickable" setting allows you to set whether the GUI and buttons on
it respond to mouse clicks. This is on by default, but if you turn it
off and the player clicks on the GUI, the game will actually process the
click as if they clicked through the GUI onto the actual screen. This is useful
for transparent GUIs which are only used to display information.

You'll notice that the GUIs also have names. These can be used in the script
in a similar way to using character names. For example, if a GUI is called
"gIconBar", you can use reference it in a script by using this name. For
example, to configure the visibility of GUI when the game starts:

```ags
void game_start()
{
    gIconBar.Visible = true;
}
```

---

### GUI buttons

To provide interactivity with the interface, the most common type of GUI control
to use is a button.

To add a button, click the "Add button" button in the toolbar, and then
drag a rectangle with the mouse to draw one onto the GUI. You will see it
displayed as a text button, with the text "New button" written on it.
Notice that the Properties window is now displaying properties for your new
button rather than the GUI.

Using the Properties window, you can choose an image to use for the button
instead of the text, and you can also set various other self-explanatory
attributes. You set what happens when the player clicks on the button by using
the "Click Action" attribute. This can be set to "Run Script" (the default),
and also "Set mode", which sets the cursor mode to whatever value has been
specified in the "New mode number" property.

To delete a GUI button, right-click it and choose Delete.

---

### Interface text

You can easily display static text on a GUI. For example, the Sierra-style
interface displays the current score in its status bar.

To show text on a GUI, you need a label. Click the "Add label" button in
the toolbar, then drag out a rectangle like you did when adding a
button. You can change the text displayed in the label by editing the
"Text" property. Notice that the text automatically wraps around to fit
inside the rectangle that you've drawn.

As well as typing normal text into the label, you can add some special
values which will automatically be updated during the game. The following
tokens will be replaced with the relevant values in the game:

Token | Description
--- | ---
`@GAMENAME@` | The game's name, specified on the Game Settings pane
`@OVERHOTSPOT@` | The name of the hotspot which the cursor is over

Example: `"Your cursor is over @OVERHOTSPOT@."`

The Properties window also allows you to align the text to the left, to the
right or center it, as well as change its font and color.

---

### Customized Text Windows

If you want to add a personal touch to the standard white text-boxes
which display all the messages during the game, you can create a border
using the GUI Editor. Right-click the "GUIs" node, and choose "New Text
Window GUI".

The element will be resized to about 1/4 of the screen, and you will see
8 images - one in each corner and one on each side. These are the border
graphics. You can change each of these in the normal way, by setting the image
number in the properties panel. You can give every corner and side a name to
more easily identify it, although the current selection will be indicated by
by surround the image with bright red squares.

In the game, the corner graphics will be placed in the respective
corners of the text window, and the side graphics will be repeated along
the edge of the window. To tell the game to use your custom text window
style, go to the General Settings pane, and check the "Text windows use
GUI" box. Then, enter the number of the GUI which you used.

You can also set a background picture for the text window. In the GUI
editor, simply set a background picture for the GUI itself. The graphic
you specify will not be tiled or stretched in the game; however, it will
be clipped to fit the window. You should use a graphic of at least about
250x80 pixels to make sure that it fills up the whole window.

To set the text color in the window, simply set the Foreground Color
of the GUI and that will be used for any text that it displays.

Additionally, you may configure padding - the distance kept between the text
window's border and text inside of it.

For example editing the GUI Borders of the [Display] Command in the [New Sierra Style Template]
looks like this:

![Editing the GUI Borders](https://i.imgur.com/BGB1OQd.png)

After compiling it looks like this in game (with x2 Scaling):

![In-game GUI Borders](https://i.imgur.com/LdJkWPj.png)

---

### Custom inventory

Another option you may have noticed in the GUI editor is the Add
Inventory button. This allows you to drag out a rectangle which will
display the player's current inventory, in the same way as the LucasArts
games did. To make the inventory window scrollable, you will need to add
Up and Down arrow buttons, and attach script code to those buttons to
use the available functions such as
[`InvWindow.ScrollUp`](InvWindow#invwindowscrollup) and
[`InvWindow.ScrollDown`](InvWindow#invwindowscrolldown).

To see a full list of commands available for inventory windows, see the
[GUI Inv Window Functions and Properties](InvWindow)
section.

---

### Sliders

You can now add sliders to your GUIs. This allows you to have a nice
interface for the player to change settings such as volume and game
speed. To add a slider, click the "Add slider" button and drag out its
rectangle just like you would for a button. You can also resize it by
dragging the bottom-right corner out in the same way you would with a button.

Sliders can be either vertical or horizontal. The direction that it is
drawn in is automatic depending on the size that you stretch the slider
box to - if it is wider than it is tall you will get a horizontal
slider, otherwise you'll get a vertical slider.

For the properties of a slider you can set the minimum, maximum and
current values that the slider will have. In the game, the user will be
able to drag the handle from MIN to MAX, and the slider will start off
set to VALUE. For horizontal sliders, MIN is on the left and MAX on the
right, for vertical sliders MAX is at the top and MIN is at the bottom.

Whenever the user moves the handle's position on the slider, the
OnChange event is called. This means that if they continually drag the
handle up and down, the event will get called repeatedly.

Your script can find out the value of the slider using the Slider.Value
script property.

*See also:* [Sliders Functions and Properties](Slider)

---

### Text Boxes

A text box is a simple GUI control that allows the player to type information
into your game. Adding a text box works like adding the other types of
control.

If a text box is on a currently displayed GUI, all standard keypresses
(i.e. letter keys, return and backspace) are diverted to the textbox
instead of being passed to the on_key_press function. When the player
presses Return in the text box, the OnActivate event is called. You can
then use the TextBox.Text property to retrieve what they've typed in.

---

### List Boxes

List box controls allow you to add a list of items to your GUI. These can be
useful for allowing the player to make a selection from a list of options, but
a common use is in the implementation of a custom save/load dialog box.

You use the `ListBox` script object to manipulate the list box - for
example, ListBox.AddItem to add an item, or ListBox.SelectedIndex to get
the current selection.

The ListBox.Translated property defines whether a game's translation to another
language will also be applied to list items or not. It is recommended to disable translation for lists containing saved games.

The OnSelectionChanged event is fired when the player clicks on an item
in the list. You may wish to ignore this or do something useful with it.
