## `Dialog` functions and properties

### `Dialog.GetByName`

```ags
static Dialog* Dialog.GetByName(string scriptName)
```

Returns a pointer to the Dialog with the specified script name, or null if it does not exist.

Normally you do not need to use this, as there will be a automatically created global script variable for each Dialog which got a script name.
Where GetByName() function may come useful is situation in which you a) do not know exact name, b) had to store object's reference in a string for some reason. Good examples of this are saving object's name in a [custom property](CustomProperties), or a [file](File), then reading it back.

Example:

```ags
String dialogName = cNPC.GetTextProperty("MyDialog");
Dialog* dlg = Dialog.GetByName(dialogName);
if (dlg != null) {
    dlg.Start();
}
```

Retrieves a dialog name from the character's custom property, and gets a pointer to an actual Dialog using that name, then runs it.

*Compatibility:* Supported by **AGS 3.6.1** and later versions.

*See also:* [`Dialog.ScriptName`](Dialog#dialogscriptname), [`Dialog.Start`](Dialog#dialogstart)

---

### `Dialog.Stop`

```ags
static void Dialog.Stop()
```

This command stops the current dialog, if any is running. Note that its effect is not immediate, and engine will wait until the end of the current script before closing the dialog.

This function is a newer OO-style alias of an older [StopDialog()](Dialog#stopdialog) function. For more detailed information on how it works, please refer to that function's description.

Example:

```
function repeatedly_execute_always() {
    if (Dialog.CurrentDialog != null) {
        if (IsKeyPressed(eKeyEscape)) {
            Dialog.Stop();
        }
    }
}
```

Stops running dialog when player presses Escape key.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:* [`Dialog.Start`](Dialog#dialogstart), [`StopDialog`](Dialog#stopdialog)

---

### `Dialog.AreOptionsDisplayed`

```ags
static readonly attribute bool Dialog.AreOptionsDisplayed
```

Returns `true` if dialog options are currently displayed on the screen, or `false` otherwise.

Example:

```ags
function on_event(EventType evt, int data1, int data2)
{
    if (evt == eEventDialogOptionsOpen)
    {
        player.LockView(VIEW_IDLE_WHILE_DIALOG);
    }
    else if (evt == eEventDialogOptionsClose)
    {
        player.UnlockView();
    }
}

void repeatedly_execute_always()
{
    if (Dialog.AreOptionsDisplayed)
    {
        if (!player.Animating && (Random(100) < 2))
        {
            player.Animate(player.Loop, 4, eOnce, eNoBlock);
        }
    }
}
```

In this example the player character will be locked to a view VIEW_IDLE_WHILE_DIALOG when the dialog options are about to be shown on screen, and randomly play animations while they are displayed. When the dialog options are hidden the player character's view will be unlocked.

**Compatibility:** Supported by **AGS 3.6.2** and later versions.

**See also:** [`Dialog.Start`](Dialog#dialogstart),
[`Dialog.DisplayOptions`](Dialog#dialogdisplayoptions), 
[`Dialog.CurrentDialog`](Dialog#dialogcurrentdialog),
[`Dialog.ExecutedOption`](Dialog#dialogexecutedoption),
[`on_event`](Globalfunctions_Event#on_event)

---

### `Dialog.CurrentDialog`

```ags
static readonly attribute Dialog* Dialog.CurrentDialog
```

Gets the currently running dialog. Returns `null` if no dialog is currently active.

"In dialog" is a specific state of the AGS Engine, it means that a Dialog have been run by calling [`Dialog.Start()`](Dialog#dialogstart). This refers to:
* dialog script is running, either an opening or any of the option's scripts;
* dialog options are displayed;

If, on another hand, you want to know when a particular character is speaking, then use [`Speech.SpeakingCharacter`](Speech#speechspeakingcharacter) instead.

Current dialog may change multiple times during the conversation if you have other dialogs started from the dialog script. CurrentDialog will return `null` only when the last dialog has finished and the game returns to the normal state.

Example:

```ags
function on_event(EventType evt, int data1, int data2)
{
    if (evt == eEventDialogStart)
    {
        if (data1 == dShopkeeper.ID)
        {
            SetTimer(1, 300);
        }
    }
    else if (evt == eEventDialogStop)
    {
        if (data1 == dShopkeeper.ID)
        {
            SetTimer(1, 0);
        }
    }
}

void repeatedly_execute_always()
{
    if (Dialog.CurrentDialog == dShopkeeper)
    {
        if (IsTimerExpired(1))
        {
            cThief.ChangeRoom(1, 10, 100);
            cThief.Walk(320, 100, eNoBlock, eAnywhere);
        }
    }
}
```

In this example, the script starts a timer whenever the dShopkeeper dialog starts, and keeps checking for that timer's expiration while it's running. If a timer expires while in dialog, the cThief character will appear in the room and walk across the room. The timer will be cancelled if dialog ended earlier.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

**See also:** [`Dialog.Start`](Dialog#dialogstart),
[`Dialog.AreOptionsDisplayed`](Dialog#dialogareoptionsdisplayed),
[`Dialog.ExecutedOption`](Dialog#dialogexecutedoption),
[`Speech.SpeakingCharacter`](Speech#speechspeakingcharacter),
[`on_event`](Globalfunctions_Event#on_event)

---

### `Dialog.ExecutedOption`

```ags
static readonly attribute int Dialog.ExecutedOption
```

Gets the currently executed dialog option, or `-1` if none is being executed.

**NOTE:** dialog option indexes start with 1, and index 0 refers to the dialog entry (marked with '@S' in dialog script).

Example:

```ags
void repeatedly_execute_always()
{
    if (Dialog.CurrentDialog == dAnxiousTalk && Dialog.ExecutedOption > 0)
    {
        if (!cAnxiousMan.Speaking && !cAnxiousMan.Moving && !cAnxiousMan.Animating)
        {
            if (cAnxiousMan.x < 100)
                cAnxiousMan.Walk(cAnxiousMan.x + 100, cAnxiousMan.y, eNoBlock);
            else
                cAnxiousMan.Walk(cAnxiousMan.x - 100, cAnxiousMan.y, eNoBlock);
        }
        player.FaceCharacter(cAnxiousMan);
    }
}
```

This script will make a cAnxiousMan character continuously walk back and forth during conversation, except the time when he's speaking himself or plays any custom animation.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

**See also:** [`Dialog.CurrentDialog`](Dialog#dialogcurrentdialog),
[`Dialog.GetOptionText`](Dialog#dialoggetoptiontext), 
[`Dialog.GetOptionState`](Dialog#dialoggetoptionstate)

---

### `Dialog.DisplayOptions`

```ags
int Dialog.DisplayOptions(optional DialogOptionSayStyle)
```

Presents the options for this dialog to the user and waits until they
select one of them. The selected option number is returned.

**NOTE:** This command does not run any dialog scripts, it simply
displays the options and waits for the player to choose one. To run the
dialog normally, use the [`Dialog.Start`](Dialog#dialogstart) command
instead.

This command is useful if you want to implement your own dialog system,
but still use the standard AGS dialog option selection screens.

The optional *DialogOptionSayStyle* parameter determines whether the
chosen option is automatically spoken by the player character. The
default is *eSayUseOptionSetting*, which will use the option's "Say"
setting from the dialog editor. You can alternatively use *eSayAlways*,
which will speak the chosen option regardless of its setting in the
editor; or *eSayNever*, which will not speak the chosen option.

If the text parser is enabled for this dialog and the player types
something into it rather than selecting an option, the special value
`DIALOG_PARSER_SELECTED` will be returned, and AGS will have
automatically called [`Parser.ParseText`](Parser#parserparsetext) with
the player's text. Therefore, you can call
[`Parser.Said`](Parser#parsersaid) to process it.

Example:

```ags
int result = dOldMan.DisplayOptions();
if (result == DIALOG_PARSER_SELECTED)
{
    Display("They typed something into the parser!!");
}
else
{
    Display("They chose dialog option %d.", result);
}
```

will show the options for dialog *dOldMan* and display a message
depending on what the player selected.

*Compatibility:* Supported by **AGS 3.0.2** and later versions.

*See also:* [`Dialog.Start`](Dialog#dialogstart),
[`Parser.ParseText`](Parser#parserparsetext)

---

### `Dialog.GetOptionState`

*(Formerly known as global function `GetDialogOption`, which is now
obsolete)*

```ags
Dialog.GetOptionState(int option)
```

Finds out whether an option in a conversation is available to the player
or not.

OPTION is the option number within the dialog, from 1 to whatever the
highest option is for that topic.

The return value can have the following values:

    eOptionOff
      The option is disabled - the player will not see it
    eOptionOn
      The option is enabled - the player can now see and use it
    eOptionOffForever
      The option is permanently disabled - no other command can ever turn
      it back on again.

These are the same as the options passed to Dialog.SetOptionState.

Example:

```ags
if (dJoeExcited.GetOptionState(2) != eOptionOn)
    Display("It's turned off");
```

Will display a message if option 2 of dialog dJoeExcited is not
currently switched on.

*See also:*
[`Dialog.HasOptionBeenChosen`](Dialog#dialoghasoptionbeenchosen),
[`Dialog.SetHasOptionBeenChosen`](Dialog#dialogsethasoptionbeenchosen),
[`Dialog.SetOptionState`](Dialog#dialogsetoptionstate)

---

### `Dialog.GetOptionText`

```ags
String Dialog.GetOptionText(int option)
```

Returns the text for the specified dialog option.

OPTION is the option number within the dialog, from 1 to whatever the
highest option is for that topic.

Example:

```ags
String optionText = dJoeBloggs.GetOptionText(3);
Display("Option 3 of dialog dJoeBloggs is %s!", optionText);
```

will display the text for the third option of the dJoeBloggs dialog.

*Compatibility:* Supported by **AGS 3.0.2** and later versions.

*See also:* [`Dialog.OptionCount`](Dialog#dialogoptioncount),
[`Dialog.GetOptionState`](Dialog#dialoggetoptionstate)

---

### `Dialog.HasOptionBeenChosen`

```ags
bool Dialog.HasOptionBeenChosen(int option)
```

Finds out whether the player has already chosen the specified option in
this dialog. This is mainly useful when drawing your own custom dialog
options display, since it allows you to differentiate options that have
already been chosen.

OPTION is the option number within the dialog, from 1 to whatever the
highest option is for that topic.

Example:

```ags
if (dJoeExcited.HasOptionBeenChosen(2))
    Display("The player has chosen option 2 in dialog dJoeExcited!");
```

will display a message if the player has used option 2 of the dialog
before.

*Compatibility:* Supported by **AGS 3.1.1** and later versions.

*See also:* [`Dialog.GetOptionState`](Dialog#dialoggetoptionstate),
[`Dialog.SetHasOptionBeenChosen`](Dialog#dialogsethasoptionbeenchosen),

---

### `Dialog.ID`

```ags
readonly int Dialog.ID;
```

Gets the dialog ID number from the editor.

This might be useful if you need to interoperate with legacy scripts
that work with dialog ID numbers.

Example:

```ags
Display("dFisherman is Dialog %d!", dFisherman.ID);
```

will display the ID number of the dFisherman dialog

*Compatibility:* Supported by **AGS 3.1.0** and later versions.

---

### `Dialog.OptionCount`

```ags
readonly int Dialog.OptionCount;
```

Gets the number of options that this dialog has.

This might be useful in a script module if you want to iterate through
all the possible choices in the dialog.

Example:

```ags
Display("dFisherman has %d options!", dFisherman.OptionCount);
```

will display the number of options in the dFisherman dialog.

*Compatibility:* Supported by **AGS 3.0.2** and later versions.

*See also:* [`Dialog.GetOptionText`](Dialog#dialoggetoptiontext),
[`Dialog.GetOptionState`](Dialog#dialoggetoptionstate)

---

### `Dialog.OptionsBulletGraphic`

```ags
static attribute int Dialog.OptionsBulletGraphic
```

Gets/sets the sprite to use as a bullet point before each dialog option (0 for none).

**Compatibility:** Supported by **AGS 3.6.3** and later versions.

---

### `Dialog.OptionsFont`

```ags
static attribute int Dialog.OptionsFont
```

Gets/sets the font to use when displaying dialog options.

**Compatibility:** Supported by **AGS 3.6.3** and later versions.

---

### `Dialog.OptionsGap`

```ags
static attribute int Dialog.OptionsGap
```

Gets/sets the vertical gap between dialog options (in pixels).

**Compatibility:** Supported by **AGS 3.6.3** and later versions.

---

### `Dialog.OptionsGUI`

```ags
static attribute GUI* Dialog.OptionsGUI
```

Gets/sets the GUI that will be used to display dialog options; set null to use default options look.

**Compatibility:** Supported by **AGS 3.6.3** and later versions.

---

### `Dialog.OptionsGUIX`

```ags
static attribute int Dialog.OptionsGUIX
```

Gets/sets on-screen X position of dialog options GUI; set to -1 if it should use default placement.

**Compatibility:** Supported by **AGS 3.6.3** and later versions.

---

### `Dialog.OptionsGUIY`

```ags
static attribute int Dialog.OptionsGUIY
```

Gets/sets on-screen Y position of dialog options GUI; set to -1 if it should use default placement.

**Compatibility:** Supported by **AGS 3.6.3** and later versions.

---

### `Dialog.OptionsHighlightColor`

```ags
static attribute int Dialog.OptionsHighlightColor
```

Gets/sets the color used to draw the active (selected) dialog option.

**Compatibility:** Supported by **AGS 3.6.3** and later versions.

---

### `Dialog.OptionsMaxGUIWidth`

```ags
static attribute int Dialog.OptionsMaxGUIWidth
```

Get/sets the maximal width of the auto-resizing GUI on which dialog options are drawn.

**Compatibility:** Supported by **AGS 3.6.3** and later versions.

---

### `Dialog.OptionsMinGUIWidth`

```ags
static attribute int Dialog.OptionsMinGUIWidth
```

Get/sets the minimal width of the auto-resizing GUI on which dialog options are drawn.

**Compatibility:** Supported by **AGS 3.6.3** and later versions.

---

### `Dialog.OptionsNumbering`

```ags
static attribute DialogOptionsNumbering Dialog.OptionsNumbering
```

Gets/sets whether dialog options have numbers before them, and the numeric keys can be used to select them.

**Compatibility:** Supported by **AGS 3.6.3** and later versions.

---

### `Dialog.OptionsPaddingX`

```ags
static attribute int Dialog.OptionsPaddingX
```

Gets/sets the horizontal offset at which options are drawn on a standard GUI.

**Compatibility:** Supported by **AGS 3.6.3** and later versions.

---

### `Dialog.OptionsPaddingY`

```ags
static attribute int Dialog.OptionsPaddingY
```

Gets/sets the vertical offset at which options are drawn on a standard GUI.

**Compatibility:** Supported by **AGS 3.6.3** and later versions.

---

### `Dialog.OptionsReadColor`

```ags
static attribute int Dialog.OptionsReadColor
```

Gets/sets the color used to draw the dialog options that have already been selected once; set to -1 for no distinct color.

**Compatibility:** Supported by **AGS 3.6.3** and later versions.

---

### `Dialog.OptionsTextAlignment`

```ags
static attribute HorizontalAlignment Dialog.OptionsTextAlignment
```

Gets/sets the horizontal alignment of each dialog option's text.

**Compatibility:** Supported by **AGS 3.6.3** and later versions.

---

### `Dialog.OptionsZOrder`

```ags
static attribute int Dialog.OptionsZOrder
```

Gets/sets the z-order of dialog options, relative to GUI and on-screen Overlays..

**Compatibility:** Supported by **AGS 3.6.3** and later versions.

---

### `Dialog.ScriptName`

```ags
readonly String Dialog.ScriptName;
```

Gets the script name of the dialog, which serves as a unique identifier.

This may be useful if you have a pointer to some dialog stored in your variable, and want to know what it actually is. Normally you don't need a script name, as you have an automatic global variable for each dialog in the game, but sometimes you may want to save its script name as a text either to display it somewhere for testing purposes, or keep as a reference. You may later use [Dialog.GetByName](Dialog#dialoggetbyname) function to retrieve the dialog by the previously saved script name.

Example:

```ags
function CustomDialogStart(this Dialog*)
{
  this.Start();
  String dialogName = this.ScriptName;
  System.Log(eLogInfo, "Starting dialog: %s", dialogName);
}
```

Starting a dialog using the `CustomDialogStart()` method, like `dExampleDialog.CustomDialogStart()`, will log its script name for debugging purposes.

*Compatibility:* Supported by **AGS 3.6.1** and later versions.

*See also:* [`Dialog.GetByName`](Dialog#dialoggetbyname), [`Dialog.Start`](Dialog#dialogstart)

---

### `Dialog.SetHasOptionBeenChosen`

```ags
Dialog.SetHasOptionBeenChosen(int option, bool chosen)
```

Changes whether an option in a conversation is marked as previously
chosen by the player. The option is marked as chosen whenever player
selects it during the conversation, and is usually highlighted with
different text color. This function lets you to reset the option state,
or force it change at any random moment.

OPTION is the option number within the dialog, from 1 to whatever the
highest option is for that topic.

Example:

```ags
if (dDialog1.HasOptionBeenChosen(1))
    dDialog1.SetHasOptionBeenChosen(1, false); // reset the option state
```

will mark option 1 of dialog dDialog1 as "not chosen yet".

*Compatibility:* Supported by **AGS 3.3.0** and later versions.

*See also:* [`Dialog.GetOptionState`](Dialog#dialoggetoptionstate),
[`Dialog.HasOptionBeenChosen`](Dialog#dialoghasoptionbeenchosen)

---

### `Dialog.SetOptionState`

*(Formerly known as global function `SetDialogOption`, which is now
obsolete)*

```ags
Dialog.SetOptionState(int option, DialogOptionState)
```

Changes whether an option in a conversation is available to the player
or not. This allows you to add extra options to a conversation once the
player has done certain things.

OPTION is the option number within the topic, from 1 to whatever the
highest option is for that topic.

The DialogOptionState controls what happens to this option. It can have
the following values:

    eOptionOff
      The option is disabled - the player will not see it
    eOptionOn
      The option is enabled - the player can now see and use it
    eOptionOffForever
      The option is permanently disabled - no other command can ever turn
      it back on again.

These are equivalent to the option-off, option-on, and
option-off-forever dialog commands.

Example:

```ags
dFriend1.SetOptionState(2, eOptionOn);
```

will enable option 2 of topic number 4.

*See also:* [`Dialog.GetOptionState`](Dialog#dialoggetoptionstate),
[`Dialog.Start`](Dialog#dialogstart),
[`StopDialog`](Dialog#stopdialog)

---

### `Dialog.ShowTextParser`

```ags
readonly bool Dialog.ShowTextParser;
```

Gets whether this dialog shows a text box allowing the player to type in
text.

This property is initially set in the Dialog Editor.

Example:

```ags
if (dFisherman.ShowTextParser)
{
    Display("dFisherman has a text box!");
}
```

will display a message if dFisherman has the option enabled

*Compatibility:* Supported by **AGS 3.2.1** and later versions.

---

### `Dialog.Start`

*(Formerly known as global function `RunDialog`, which is now obsolete)*

```ags
Dialog.Start()
```

Starts a conversation from the specified topic.

NOTE: The conversation will not start immediately; instead, it will be
run when the current script function finishes executing.

If you use this command from within the dialog_request function, it
will specify that the game should return to this new topic when the
script finishes.

Example:

```ags
dMerchant.Start();
```

will start the conversation topic named dMerchant.

*See also:* [`StopDialog`](Dialog#stopdialog),
[`Dialog.DisplayOptions`](Dialog#dialogdisplayoptions),
[`Dialog.SetOptionState`](Dialog#dialogsetoptionstate)

---

### `StopDialog`

```ags
StopDialog ()
```

This command stops the current dialog, if any is running. But it's effect is not immediate.
- If called in dialog script, or in normal script function being called from a dialog script, then the dialog will stop after current option's script completes.
- If called in "dialog_request" function, then the dialog will stop right after "dialog_request" function finishes.
- If called from regular script while the dialog options are being displayed (i.e. from "repeatedly_execute_always"), then the dialog will stop after the game finishes updating.
- If called from any function which is a part of the [custom dialog options rendering](CustomDialogOptions), then the dialog will stop right after this script function ends.

You can use this function to end the conversation depending on whether the player has/does a certain thing.

Example 1:

```ags
function dialog_request (int dr) {
    if (dr==1) {
        cEgo.AddInventory(iPoster);
        StopDialog();
    }
}
```

will give the player the inventory item iPoster and then end the conversation.

Example 2:

```ags
function repeatedly_execute_always() {
    if (Dialog.AreOptionsDisplayed && Dialog.CurrentDialog == dDialogThatMayBeInterrupted) {
        if (IsKeyPressed(eKeyEscape)) {
            StopDialog();
        }
    }
}
```

will stop a certain dialog if player presses Escape while dialog options are displayed.

*Compatibility:* Prior to **AGS 3.6.2** StopDialog could only work if called from "dialog_request" function.

*See also:* [`Dialog.Start`](Dialog#dialogstart), [`Dialog.Stop`](Dialog#dialogstop),
[`Dialog.SetOptionState`](Dialog#dialogsetoptionstate),
[`dialog_request`](Globalfunctions_Event#dialog_request)
