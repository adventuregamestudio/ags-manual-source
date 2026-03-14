## Dialog Script

Dialog scripts are scripts that run when a dialog starts or one of the options is executed, for example, when player selects it in the list of available options. Dialog scripts are streamlined for conversations and use an extended variant of script syntax that simplifies several common commands.

With a newly created dialog topic, all you will see in the script is a
number of lines starting with an '@' symbol. In the dialog script, these
signify the starting points of the script for each option. For example,
when the player clicks on option 3, the script will begin on the line
following "@3". There is also a special starting point, called "@S".
This is run when the conversation starts, before any choices are given
to the player. This could be used to display a "Hello" message or
something similar.

To display some speech, you begin the line with the character's SCRIPT
NAME (not full name), followed by a colon, then a space, and then what
you want them to say. For example, if my main character's script name is
EGO, I would write

```agsdialog
ego: "I am very happy today because it's my birthday."
```

The character name is used by the system to choose the correct color
for the text.

**IMPORTANT:** Do **NOT** include the "c" at the start of the
character's script name here.

You can also use the special character name "narrator", which displays
the text in the pop-up message box instead of as speech text; and the
alias "player", which will say it as the current player character -
useful if you don't know which character the player will be controlling
when they speak the conversation.

If you just use "..." as the text for a character to say, the game will
pause briefly as if they are stopping to think, and nothing will be
displayed. If, on another hand, you'd like to actually display three dots,
then the trick is to add any number of extra spaces into this string.

To signal the end of the script for this option, place a "return"
command on the last line of it. For example,

```agsdialog
@1
ego: "Hello. How are you?"
narrator: The man looks you in the eye.
otherman: ...
otherman: "I'm fine."
return
```

"return" tells AGS to go back and display the choices again to the
player. If you use "stop" instead of return, then the conversation is
ended. Alternatively, you can use "goto-dialog" or "goto-previous",
which abort the current dialog script and transfer control to the new
dialog.

**NOTE:** Do **NOT** indent these lines with spaces or tabs. Indented
lines signify that AGS should interpret the line as a normal scripting
command rather than a dialog scripting command (see below).

### Dialog commands

The dialog commands available are:

-   **goto-dialog X**<br>
    Switches the current topic to Topic X, and displays the current list
    of choices for that topic.
-   **goto-previous**<br>
    Returns to the previous topic that this one was called from. If the
    dialog started on this topic, then the dialog will be stopped.
-   **option-off X**<br>
    Turns option X for the current topic off, meaning it won't be
    displayed in the list of choices next time.
-   **option-off-forever X**<br>
    Turns option X off permanently. It will never again be displayed,
    not even if an "option-on" command is used.
-   **option-on X**<br>
    Turns option X for the current topic on, including it in the list of
    choices to the player next time they are displayed.
-   **run-script X**<br>
    Calls [dialog_request](Globalfunctions_Event#dialog_request) event handler if one is present in the game script, passes `X` as an integer parameter.
-   **return**<br>
    Stops the script and returns to the list of choices.
-   **stop**<br>
    Stops the conversation and returns the player to the game.

### Substituting dialog speech with custom functions

By default all of the character dialog lines are executed using the standard function Character.Say. Since AGS 3.5.0 it is possible to define a custom script function as a substitute instead. This is done using "Custom Say function in dialog scripts" option in the General Settings. Similarly, narration (which is by default done using Display script function) may be substituted with a custom one using "Custom Narrate function in dialog scripts".

These custom functions should be declared as imports in one of your script headers.<br>
"Custom Say" function must have one of the following two declaration forms:<br>
`void MySay(Character* c, const string text); // use ex: MySay(player, "Hello");`<br>
or<br>
`void MySay(this Character*, const string text); // use ex: player.MySay("Hello");`

"Custom Narrate" function must have following declaration form:<br>
`void MyNarrate(const string text);`

**IMPORTANT:** There's currently a limitation that, if Say checkbox for dialog options is checked, it will use regular `Character.Say` despite defining a custom Say function.<br>
If you still want to have player pronounce dialog option text, one of the solutions is to add a call to your custom speech function as the first line in every dialog option script:

```agsdialog
@X
    player.MySay(this.GetOptionText( X )); \\ where X is the actual option index
```


If you wonder how such function will work in dialogs, see the following topic below.

### Using regular scripting commands in dialogs

Often the provided dialog scripting commands won't be enough for what
you want to do in the dialog. You might want to give the player an
inventory item or add some points to their score, for example.

AGS now lets you put normal scripting commands in your dialog script, by
indenting the line with spaces or tabs. For example:

```agsdialog
@1
ego: "Hello. How are you?"
narrator: The man looks you in the eye.
    player.AddInventory(iKey);
    Display("This line is displayed from a normal script command");
otherman: "I'm fine."
return
```

Here, you can see dialog script commands being used, but also then a
couple of normal scripting commands have been inserted, on indented
lines.

When working with dialog scripts, the **this** keyword allows you to
access the currently running dialog.

If you want to conditionally break out of the dialog script, the special
tokens `RUN_DIALOG_GOTO_PREVIOUS`, `RUN_DIALOG_RETURN` and
`RUN_DIALOG_STOP_DIALOG` are available which you can `return` from
inside a script block. For example:

```agsdialog
@1
ego: "Hello. How are you?"
narrator: The man looks you in the eye.
    if (player.HasInventory(iKey)) {
        player.Say("Actually, I'd better go.");
        return RUN_DIALOG_STOP_DIALOG;
    }
otherman: "Here's a key for you."
return
```

