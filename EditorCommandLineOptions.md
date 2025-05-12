## Editor Command Line Options

Running AGS Editor from the command line can be useful for automation purposes.

We will use `cmd.exe` here, but you can call AGS Editor from other command lines, though they may require special handling.

**NOTE:** If you are using the MSYS2 bash prompt on Windows, remember that `/` command flags need two slashes. (e.g. use `AGSEditor.exe //compile my-project/Game.agf`).


### Opening the Editor

You can open the Editor from the command line by simply calling its executable and passing the path to it, for example:

```cmd
C:\Program Files (x86)\Adventure Game Studio 3.6.0\AGSEditor.exe
```

We won't use the full path in the rest of the text, but you still need to either pass it, put your AGS Editor folder in your system PATH, or change the directory to it before running.

You can also open a specific project in the editor by passing your project's `Game.agf` location.

```cmd
AGSEditor.exe path\to\Game.agf
```

As an example, in Windows, you can create a shortcut for the AGS Editor and edit its properties to put your `Game.agf` path there, and then use this shortcut that opens your project in the AGS Editor.

### Compile Flag

This flag will load your project into AGS Editor, compile it, and exit.

```cmd
AGSEditor.exe /compile path\to\Game.agf
```

Outputs that would normally be displayed in a message box or in the output panel will instead be printed into the console. A successful compilation will have exit code 0, and any error will give a non-zero exit code.

This option can be used for automation purposes, you could use it to build your AGS Game in a Continuous Integration pipeline.

### Make template flag

This flag will load your project into AGS Editor, package it as a template, and exit.

```cmd
AGSEditor.exe /maketemplate path\to\Game.agf
```

The template will have a filename "gamename.agt", where "gamename" is a name of your project, and be placed in the current working directory.
