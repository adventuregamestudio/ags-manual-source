## `Walkbehind` functions and properties

### `Walkbehind.GetAtScreenXY`

```ags
static Walkbehind* Walkbehind.GetAtScreenXY(int x, int y)
```

Gets the walk-behind at the specified position on the screen.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Walkbehind.GetAtRoomXY`

```ags
static Walkbehind* Walkbehind.GetAtRoomXY(int x, int y)
```

Returns the walk-behind at the specified position within this room.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Walkbehind.GetByScriptName`

```ags
static Walkbehind* Walkbehind.GetByScriptName(string scriptName)
```

Returns a pointer to the Walkbehind with the specified script name, or null if it does not exist.

Normally you do not need to use this, as there will be a automatically created global script variable for each Walkbehind which got a script name.
Where GetByScriptName() function may come useful is situation in which you a) do not know exact name, b) had to store walk-behind's reference in a string for some reason. Good examples of this are saving walk-behind's name in a [custom property](CustomProperties), or a [file](File), then reading it back.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [`Walkbehind.ScriptName`](Walkbehind#walkbehindscriptname)

---

### `Walkbehind.GetDrawingSurface`

```ags
static DrawingSurface* Walkbehind.GetDrawingSurface()
```

Gets the drawing surface for the 8-bit walk-behind mask

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [DrawingSurface](DrawingSurface)

---

### `Walkbehind.ID`

```ags
readonly int Walkbehind.ID
```

Gets the ID number for this walk-behind.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Walkbehind.Baseline`

```ags
int Walkbehind.Baseline
```

Gets/sets this walk-behind's baseline.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Walkbehind.ScriptName`

```ags
readonly String Walkbehind.ScriptName
```

Gets the script name of the walk-behind, which serves as a unique identifier, as set in the AGS Editor.

This may be useful if you have a pointer to some walk-behind stored in your variable, and want to know what it actually is. Normally you don't need a script name, as you have an automatic global variable for each  walk-behind in the game, but sometimes you may want to save its script name as a text either to display it somewhere for testing purposes, or keep as a reference. You may later use [Walkbehind.GetByScriptName](Walkbehind#walkbehindgetbyscriptname) function to retrieve the  walk-behind by the previously saved script name.

Example:

```
function TestWalkbehindAt(int x, int y)
{
	Walkbehind* wb = Walkbehind.GetAtRoomXY(x, y);
    System.Log(eLogDebug, "There's walk-behind %s at position %d,%d", wb.ScriptName, x, y);
}
```

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [`Walkbehind.GetByScriptName`](Walkbehind#walkbehindgetbyscriptname)

