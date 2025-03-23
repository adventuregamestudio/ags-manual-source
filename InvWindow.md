## `InvWindow` functions and properties

InvWindow is a subclass of [`GUIControl`](GUIControl) and therefore inherits all GUIControl's functions and properties in addition to its own, which are listed below.

---

### `InvWindow.ScrollDown`

```ags
InvWindow.ScrollDown()
```

Scrolls the inventory window down one line, if there are more items to
display. If the inventory window is already at the bottom, then nothing
happens.

You would usually use this in response to a GUI button press on a Down
arrow button on your GUI.

Example:

```ags
invMain.ScrollDown();
```

will scroll the *invMain* inv window down one row.

*See also:* [`InvWindow.ScrollUp`](InvWindow#invwindowscrollup),
[`InvWindow.TopItem`](InvWindow#invwindowtopitem)

---

### `InvWindow.ScrollUp`

```ags
InvWindow.ScrollUp()
```

Scrolls the inventory window up one line, if there are more items to
display. If the inventory window is already at the top, then nothing
happens.

You would usually use this in response to a GUI button press on an Up
arrow button on your GUI.

Example:

```ags
invMain.ScrollUp();
```

will scroll the *invMain* inv window up one row.

*See also:* [`InvWindow.ScrollDown`](InvWindow#invwindowscrolldown),
[`InvWindow.TopItem`](InvWindow#invwindowtopitem)

---

### `InvWindow.CharacterToUse`

```ags
Character* InvWindow.CharacterToUse;
```

Gets/sets which character the inventory window is currently displaying
the inventory for. This is either set to a specific character, or it can
be set to *null*, in which case the inventory window will track the
current player character (this is the default).

Example:

```ags
invMain.CharacterToUse = cJack;
```

will change the *invMain* inventory window to display character JACK's
inventory.

---

### `InvWindow.ItemAtIndex`

```ags
readonly InventoryItem* InvWindow.ItemAtIndex[];
```

Gets the inventory item that is currently displayed at the specified
index in this inventory window. The number of items in the window can be
retrieved with the [`ItemCount`](InvWindow#invwindowitemcount) property.
Indexes range from 0 to ItemCount - 1.

If an invalid index is supplied, *null* is returned.

Example:

```ags
String firstOne = invMain.ItemAtIndex[0].Name;
Display("First item is %s.", firstOne);
```

will display the name of the first item displayed in the *invMain*
inventory window.

*See also:* [`InvWindow.ItemCount`](InvWindow#invwindowitemcount)

---

### `InvWindow.ItemCount`

*(Formerly known as `game.num_inv_items`, which is now obsolete)*

```ags
readonly int InvWindow.ItemCount;
```

Gets the total number of items contained in the inventory window. This
will tend to equal the total number of items that the character has
(though it may not if the "Display multiple items multiple times" game
setting is not checked).

Example:

```ags
if (invMain.ItemCount > (invMain.ItemsPerRow * invMain.RowCount)) {
    btnInvUp.Enabled = true;
    btnInvDown.Enabled = true;
}
else {
    btnInvUp.Enabled = false;
    btnInvDown.Enabled = false;
}
```

will enable the GUI buttons *btnInvUp* and *btnInvDown* if there are
more inventory items than will fit in the inventory window, and disable them otherwise.

*See also:* [`InvWindow.ItemAtIndex`](InvWindow#invwindowitematindex),
[`InvWindow.ItemsPerRow`](InvWindow#invwindowitemsperrow),
[`InvWindow.RowCount`](InvWindow#invwindowrowcount)

---

### `InvWindow.ItemHeight`

*(Formerly known as `SetInvDimensions`, which is now obsolete)*

```ags
int InvWindow.ItemHeight;
```

Gets/sets the height of the rows in the inventory window. You should
generally set this up in game_start to the height of your largest
inventory item. The default is 22.

Example:

```ags
invMain.ItemWidth = 50;
invMain.ItemHeight = 30;
```

sets the *invMain* inventory window to use item cells 50x30 large.

*See also:* [`InvWindow.ItemWidth`](InvWindow#invwindowitemwidth),
[`InvWindow.RowCount`](InvWindow#invwindowrowcount)

---

### `InvWindow.ItemWidth`

*(Formerly known as `SetInvDimensions`, which is now obsolete)*

```ags
int InvWindow.ItemWidth;
```

Gets/sets the width of the items in the inventory window. You should
generally set this up in game_start to the width of your largest
inventory item. The default is 40.

Example:

```ags
invMain.ItemWidth = 50;
invMain.ItemHeight = 30;
```

sets the *invMain* inventory window to use item cells 50x30 large.

*See also:* [`InvWindow.ItemHeight`](InvWindow#invwindowitemheight),
[`InvWindow.ItemsPerRow`](InvWindow#invwindowitemsperrow)

---

### `InvWindow.ItemsPerRow`

*(Formerly known as `game.items_per_line`, which is now obsolete)*

```ags
readonly int InvWindow.ItemsPerRow;
```

Gets the number of items that can be displayed in each row of the
inventory window. This is calculated by the width of the inventory
window divided by the individual ItemWidth.

Example:

```ags
Display("The inventory window can show %d items at a time", invMain.ItemsPerRow * invMain.RowCount);
```

displays how many items can be visible in the invMain window at once.

*See also:* [`InvWindow.ItemWidth`](InvWindow#invwindowitemwidth),
[`InvWindow.RowCount`](InvWindow#invwindowrowcount)

---

### `InvWindow.RowCount`

```ags
readonly int InvWindow.RowCount;
```

Gets the number of rows that can be displayed within the inventory
window. This is calculated by dividing the height of the window by the
individual ItemHeight.

Example:

```ags
Display("The inventory window can show %d items at a time", invMain.ItemsPerRow * invMain.RowCount);
```

displays how many items can be visible in the invMain window at once.

*See also:* [`InvWindow.ItemHeight`](InvWindow#invwindowitemheight),
[`InvWindow.ItemsPerRow`](InvWindow#invwindowitemsperrow)

---

### `InvWindow.TopItem`

*(Formerly known as `game.top_inv_item`, which is now obsolete)*

```ags
int InvWindow.TopItem;
```

Gets/sets the index of the first item currently displayed in the
inventory window. The first item is represented by 0, and the last item
is has an index of [`ItemCount`](InvWindow#invwindowitemcount) - 1.

You can use this to work out whether to display scroll arrows or not.

Example:

```ags
if (invMain.TopItem > 0) {
    btnScrollUp.Visible = true;
}
else {
    btnScrollUp.Visible = false;
}
```

makes the *btnScrollUp* button visible or invisible depending on whether
the inventory list can be scrolled up.

*See also:* [`InvWindow.ItemCount`](InvWindow#invwindowitemcount)

