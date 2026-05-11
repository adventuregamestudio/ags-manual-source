## `ListBox` functions and properties

ListBox is a subclass of [`GUIControl`](GUIControl) and therefore inherits all GUIControl's functions and properties in addition to its own, which are listed below.

---

### `ListBox.AddItem`

*(Formerly known as `ListBoxAdd`, which is now obsolete)*

```ags
ListBox.AddItem(string newitem)
```

Adds NEWITEM to the specified list box. The item will be appended to the
end of the list.

Example:

```ags
String input = txtUserInput.Text;
lstChoices.AddItem(input);
```

will take the input from the user and add it to the listbox.

*Compatibility:* prior to **AGS 3.5.0** ListBox had a limit of 200 items; they are not limited anymore.

*See also:* [`ListBox.Clear`](ListBox#listboxclear),
[`ListBox.FillDirList`](ListBox#listboxfilldirlist),
[`ListBox.InsertItemAt`](ListBox#listboxinsertitemat),
[`ListBox.Items`](ListBox#listboxitems),
[`ListBox.RemoveItem`](ListBox#listboxremoveitem)

---

### `ListBox.Clear`

*(Formerly known as `ListBoxClear`, which is now obsolete)*

```ags
ListBox.Clear()
```

Removes all items from the specified list box.

Example:

```ags
lstNoteBook.Clear();
```

will remove all the items from listbox *lstNoteBook*.

*See also:* [`ListBox.AddItem`](ListBox#listboxadditem)

---

### `ListBox.FillDirList`

*(Formerly known as `ListBoxDirList`, which is now obsolete)*

```ags
void ListBox.FillDirList(string filemask, optional FileSortStyle fileSortStyle, optional SortDirection sortDirection)
```

Fills the list box with a list of filenames matching FILEMASK in the
current directory. This could be useful if you have various data files
and the player can choose which one to load.

*filemask* is a standard wildcard search expression such as `"*.dat"` or `"data*.*"`

*filemask* may include any file tags or location tags, which are described for [`File.Open`](File#fileopen).

*FileSortStyle* determines which file property will be used when sorting the list of filenames. It can be:
* eFileSort_None - don't sort, the order will be unspecified;
* eFileSort_Name - sort by name (this sort is case insensitive for cross-platform compatibility);
* eFileSort_Time - sort by write time (time when this file was created or last written to).

*SortDirection* determines the order of sorting:
* eSortNoDirection - unspecified;
* eSortAscending;
* eSortDescending.

Example:

```ags
lstSaveGames.FillDirList("$SAVEGAMEDIR$/agssave.*");
```

will fill the listbox with the list of the saved games. Note that
actually for this task you would use FillSaveGameList instead.

*Compatibility:* Optional `fileSortStyle` and `sortDirection` parameters are supported only by **AGS 3.6.2** and later versions.

*See also:* [`ListBox.AddItem`](ListBox#listboxadditem),
[`ListBox.Clear`](ListBox#listboxclear),
[`ListBox.FillSaveGameList`](ListBox#listboxfillsavegamelist),
[`File.GetFiles`](File#filegetfiles),
[`FileSortStyle`](StandardEnums#filesortstyle),
[`SortDirection`](StandardEnums#sortdirection)

---

### `ListBox.FillSaveGameList`

*(Formerly known as `ListBoxSaveGameList`, which is now obsolete)*

```ags
bool ListBox.FillSaveGameList(optional int min_slot, optional int max_slot, optional SaveGameSortStyle saveSortStyle, optional SortDirection sortDirection)
```

Fills the specified listbox with the save game list, optionally sorted using certain style and direction.
By default it sorts saves by their time in descending order (with the most recent game at the top of the list).

You can optionally pass `min_slot` and `max_slot` to select the range of save slots
that can be added to the list box. By default it uses a range of 1 to 100.

*SaveGameSortStyle* determines which save's property will be used when sorting the list of saves. It can be:
* eSaveGameSort_None - don't sort, the order will be unspecified;
* eSaveGameSort_Number - sort by the save slot number;
* eSaveGameSort_Time - sort by the save time (the time this save slot was last written at);
* eSaveGameSort_Description - sort by the save's description.

*SortDirection* determines the order of sorting:
* eSortNoDirection - unspecified;
* eSortAscending;
* eSortDescending.

This function returns `true` if all slots in the set range are occupied, and `false` otherwise.

The [`SaveGameSlots`](ListBox#listboxsavegameslots) property is updated
to contain the save game slot number for each index in the list, so that
you can do:

```ags
int index = lstSaveGames.SelectedIndex;
RestoreGameSlot(lstSaveGames.SaveGameSlots[index]);
```

Example:

```ags
lstSaveGames.FillSaveGameList();
```

will fill listbox *lstSaveGames* with the list of the saved games.

*Compatibility:* Optional `min_slot`, `max_slot`, `saveSortStyle` and `sortDirection` parameters are supported only by **AGS 3.6.2** and later versions.
Prior to **AGS 3.6.2** this function would only search for save slots between 0 and 99, but would only list first 50 found.

*See also:* [`ListBox.FillDirList`](ListBox#listboxfilldirlist),
[`ListBox.FillSaveGameSlots`](ListBox#listboxfillsavegameslots),
[`ListBox.ItemCount`](ListBox#listboxitemcount),
[`ListBox.SaveGameSlots`](ListBox#listboxsavegameslots),
[`ListBox.SelectedIndex`](ListBox#listboxselectedindex),
[`Game.GetSaveSlots`](Game#gamegetsaveslots),
[`SaveGameSortStyle`](StandardEnums#savegamesortstyle),
[`SortDirection`](StandardEnums#sortdirection)

---

### `ListBox.FillSaveGameSlots`

```ags
void ListBox.FillSaveGameSlots(int save_slots[], optional SaveGameSortStyle saveSortStyle, optional SortDirection sortDirection)
```

Fills the list box with the current user's saved games using the array of slot indexes, optionally sorted using certain style and direction.
This array must be non-empty. By default the sorting order is unspecified.

You can optionally use `saveSortStyle` and `sortDirection` to set how the list
of save games set to the list box should be sorted. See their explanation in the description to [`ListBox.FillSaveGameList`](ListBox#listboxfillsavegamelist).

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:* [`ListBox.FillDirList`](ListBox#listboxfilldirlist),
[`ListBox.FillSaveGameList`](ListBox#listboxfillsavegamelist),
[`ListBox.ItemCount`](ListBox#listboxitemcount),
[`Game.GetSaveSlots`](Game#gamegetsaveslots),
[`SaveGameSortStyle`](StandardEnums#savegamesortstyle),
[`SortDirection`](StandardEnums#sortdirection)

---

### `ListBox.GetItemAtLocation`

```ags
ListBox.GetItemAtLocation(int x, int y)
```

Determines which item in the list box is at the screen co-ordinates
(X,Y). This allows you to find out which item the mouse is hovering
over, for instance.

Returns the item index (where the first item is 0), or -1 if the
specified co-ordinates are not over any item or are outside the list
box.

Example:

```ags
int index = lstOptions.GetItemAtLocation(mouse.x, mouse.y);
if (index < 0) {
    Display("The mouse is not over an item!");
}
else {
    String selectedItem = lstOptions.Items[index];
    Display("The mouse is over item '%s'.", selectedItem);
}
```

will display the item text that the mouse is currently hovering over.

*See also:* [`ListBox.SelectedIndex`](ListBox#listboxselectedindex)

---

### `ListBox.InsertItemAt`

```ags
ListBox.InsertItemAt(int index, string newitem)
```

Inserts NEWITEM into the specified list box. The item will be inserted
**before** the specified index.

Listbox indexes go from 0 (the first item) to ItemCount - 1 (the last
item). The new item will be inserted before the index you specify.

Example:

```ags
lstChoices.AddItem("First item");
lstChoices.AddItem("Second item");
lstChoices.InsertItemAt(1, "Third item");
```

will insert the Third Item in between the First and Second items.

*Compatibility:* prior to **AGS 3.5.0** ListBox had a limit of 200 items; they are not limited anymore.

*See also:* [`ListBox.AddItem`](ListBox#listboxadditem),
[`ListBox.RemoveItem`](ListBox#listboxremoveitem)

---

### `ListBox.RemoveItem`

*(Formerly known as `ListBoxRemove`, which is now obsolete)*

```ags
ListBox.RemoveItem(int item)
```

Removes ITEM from the specified list box. ITEM is the list index of the
item to remove, starting with 0 for the top item.

If you want to remove all items from the list, then use
[`ListBox.Clear`](ListBox#listboxclear) instead.

**NOTE:** Calling this function causes other items in the list to get
re-numbered, so make sure you don't keep around any references from
ListBox.SelectedIndex and related functions while using this command.

Example:

```ags
lstTest.AddItem("First item");
lstTest.AddItem("Second item");
lstTest.RemoveItem(0);
```

the list box will now just contain "Second item".

*See also:* [`ListBox.Clear`](ListBox#listboxclear),
[`ListBox.FillDirList`](ListBox#listboxfilldirlist)

---

### `ListBox.ScrollDown`

```ags
ListBox.ScrollDown()
```

Scrolls the list box down one row. If it is already at the bottom,
nothing happens.

Example:

```ags
lstTest.ScrollDown();
```

will scroll the *lstTest* list box down one row.

*See also:* [`ListBox.ScrollUp`](ListBox#listboxscrollup)

---

### `ListBox.ScrollUp`

```ags
ListBox.ScrollUp()
```

Scrolls the list box up one row. If it is already at the top, nothing
happens.

Example:

```ags
lstTest.ScrollUp();
```

will scroll the *lstTest* list box up one row.

*See also:* [`ListBox.ScrollDown`](ListBox#listboxscrolldown)

---

### `ListBox.SortItems`

```ags
ListBox.SortItems(StringCompareStyle, SortDirection)
```

Sorts the ListBox's contents, according to the passed string compare style and sort direction. This is a one time operation, the ListBox won't automatically keep items sorted from now on. If you add more items later and want them sorted, then call this function again.

Example:

```ags
lstTest.SortItems(eCaseSensitive, eSortDescending);
```

will arrange the *lstTest* list box's strings, comparing them according to their letter case, in the descending order.

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

---

### `ListBox.Font`

```ags
FontType ListBox.Font
```

Gets/sets the font used by the specified list box.

Example:

```ags
lstSaveGames.Font = eFontSpeech;
```

will change the *lstSaveGames* list box to use Font "Speech".

*See also:* [`Label.Font`](Label#labelfont),
[`TextBox.Text`](TextBox#textboxtext)

---

### `ListBox.HideBorder`

**This property is obsolete since AGS 3.5.0. Use [`ListBox.ShowBorder`](ListBox#listboxshowborder) instead.**

```ags
bool ListBox.HideBorder
```

Gets/sets whether the list box's border is _hidden_.

---

### `ListBox.HideScrollArrows`

**This property is obsolete since AGS 3.5.0. Use [`ListBox.ShowScrollArrows`](ListBox#listboxshowscrollarrows) instead.**

```ags
bool ListBox.HideScrollArrows
```

Gets/sets whether the built-in up/down scroll arrows are _hidden_.

---

### `ListBox.ItemCount`

*(Formerly known as `ListBoxGetNumItems`, which is now obsolete)*

```ags
readonly int ListBox.ItemCount
```

Gets the number of items in the specified listbox. Valid item indexes
range from 0 to (numItems - 1).

This property is read-only. To change the item count, use the AddItem
and RemoveItem methods.

Example:

```ags
int saves = lstSaveGames.ItemCount;
```

will pass the number of saved games to the int saves.

*See also:* [`ListBox.Items`](ListBox#listboxitems)

---

### `ListBox.Items`

*(Formerly known as `ListBoxGetItemText`, which is now obsolete)*<br>
*(Formerly known as `ListBox.GetItemText`, which is now obsolete)*<br>
*(Formerly known as `ListBox.SetItemText`, which is now obsolete)*

```ags
String ListBox.Items[index]
```

Gets/sets the text of the list box item at INDEX.

List box items are numbered starting from 0, so the first item is 0, the
second is 1, and so on. The highest allowable index is ItemCount minus
1.

If you want to add a new item to the listbox, use the
[`ListBox.AddItem`](ListBox#listboxadditem) method.

Example:

```ags
String selectedItemText = lstOptions.Items[lstOptions.SelectedIndex];
```

will get the text of the selected item in the list box.

*See also:* [`ListBox.SelectedIndex`](ListBox#listboxselectedindex),
[`ListBox.ItemCount`](ListBox#listboxitemcount),
[`ListBox.AddItem`](ListBox#listboxadditem)

---

### `ListBox.RowCount`

```ags
readonly int ListBox.RowCount
```

Gets the number of rows that can be shown within the list box. This
depends on the size of the list box, and **does not** depend on how many
items are actually stored in the list box.

This property is read-only. To change the row count, adjust the height
of the list box.

Example:

```ags
Display("You can currently see %d items from the listbox's contents", lstSaveGames.RowCount);
```

will display the number of rows that the listbox can display.

*See also:* [`ListBox.ItemCount`](ListBox#listboxitemcount),
[`ListBox.ScrollDown`](ListBox#listboxscrolldown),
[`ListBox.ScrollUp`](ListBox#listboxscrollup)

---

### `ListBox.SaveGameSlots`

*(Formerly known as global array `savegameindex`, which is now obsolete)*

```ags
readonly int ListBox.SaveGameSlots[];
```

Contains the corresponding save game slot for each item in the list.

This is necessary because the FillSaveGameList command sorts the list of
save games to put the most recent first. Therefore, you can use this
array to map the list box indexes back to the corresponding save game
slot.

**NOTE:** You must use the FillSaveGameList command in order to populate
this array.

Example:

```ags
int index = lstSaveGames.SelectedIndex;
RestoreGameSlot(lstSaveGames.SaveGameSlots[index]);
```

will restore the currently selected game in the list, assuming
FillSaveGameList had been used previously.

*See also:*
[`ListBox.FillSaveGameList`](ListBox#listboxfillsavegamelist),
[`ListBox.SelectedIndex`](ListBox#listboxselectedindex)

---

### `ListBox.SelectedBackColor`

```ags
int ListBox.SelectedBackColor
```

Gets/sets the fill color of the selection rectangle drawn around the selected item.

Example:

```ags
lstSaveGames.SelectedBackColor = Game.GetColorFromRGB(0, 148, 255);
```

*Compatibility:* Supported by **AGS 3.5.0** and later versions.

*See also:* [`ListBox.Font`](ListBox#listboxfont), [`ListBox.SelectedTextColor`](ListBox#listboxselectedtextcolor), [`ListBox.TextColor`](ListBox#listboxtextcolor)

---

### `ListBox.SelectedIndex`

*(Formerly known as `ListBoxGetSelected`, which is now obsolete)*<br>
*(Formerly known as `ListBoxSetSelected`, which is now obsolete)*

```ags
int ListBox.SelectedIndex
```

Gets/sets the index into the list of the currently selected item. The
first item is 0, second is 1, and so on. If no item is selected, this is
set to -1.

You can set this to -1 to remove the highlight (i.e. un-select all
items).

Example:

```ags
String selectedText = lstOptions.Items[lstOptions.SelectedIndex];
```

will get the text of the selected item in the listbox.

---

### `ListBox.SelectedTextColor`

```ags
int ListBox.SelectedTextColor
```

Gets/sets the text color used for the selected item.

Example:

```ags
lstSaveGames.SelectedTextColor = Game.GetColorFromRGB(255, 255, 80);
```

*Compatibility:* Supported by **AGS 3.5.0** and later versions.

*See also:* [`ListBox.Font`](ListBox#listboxfont), [`ListBox.SelectedTextColor`](ListBox#listboxselectedtextcolor), [`ListBox.TextColor`](ListBox#listboxtextcolor)

---

### `ListBox.ShowBorder`

*(Formerly known as `ListBox.HideBorder`, which is now obsolete)*

```ags
bool ListBox.ShowBorder
```

Gets/sets whether the list box's border is shown.

Border is drawn using color from TextColor property.

Note that hiding the border will also implicitly hide the up/down scroll arrows for the list box.

*Compatibility:* Supported by **AGS 3.5.0** and later versions.

*See also:*
[`ListBox.ShowScrollArrows`](ListBox#listboxshowscrollarrows), [`ListBox.TextColor`](ListBox#listboxtextcolor)

---

### `ListBox.ShowScrollArrows`

*(Formerly known as `ListBox.HideScrollArrows`, which is now obsolete)*

```ags
bool ListBox.ShowScrollArrows
```

Gets/sets whether the built-in up/down scroll arrows are shown.

Arrows are drawn using color from TextColor property.

Because the overall appearance of the scroll arrows is not customizable, you may
wish to use this to hide them and provide your own arrows using GUI
Button controls.

**NOTE:** If the list box's "Show Border" setting is disabled, then the
scroll arrows will also be hidden, since "Show Border" supersedes "Show
Scroll Arrows". You only need to use this ShowScrollArrows property if
you want the border to be shown but the arrows hidden.

*Compatibility:* Supported by **AGS 3.5.0** and later versions.

*See also:* [`ListBox.ShowBorder`](ListBox#listboxshowborder), [`ListBox.TextColor`](ListBox#listboxtextcolor)

---

### `ListBox.TextAlignment`

```ags
HorizontalAlignment ListBox.TextAlignment
```

Gets/sets the way a text is aligned inside an item's rectangle. Currently only horizontal alignment is supported, that is: eAlignLeft, eAlignCenter and eAlignRight.

*Compatibility:* Supported by **AGS 3.5.0** and later versions.

*See also:* [Standard Enumerated Types](StandardEnums), [`ListBox.Font`](ListBox#listboxfont), [`ListBox.SelectedBackColor`](ListBox#listboxselectedbackcolor), [`ListBox.SelectedTextColor`](ListBox#listboxselectedtextcolor)

---

### `ListBox.TextColor`

```ags
int ListBox.TextColor
```

Gets/sets the text color used by default, that is - for the non-selected items. Note that the same color is also used for the listbox's border and and scroll arrows.

Example:

```ags
lstSaveGames.TextColor = Game.GetColorFromRGB(80, 80, 200);
```

*Compatibility:* Supported by **AGS 3.5.0** and later versions.

*See also:* [`ListBox.Font`](ListBox#listboxfont),
[`ListBox.SelectedBackColor`](ListBox#listboxselectedbackcolor),
[`ListBox.SelectedTextColor`](ListBox#listboxselectedtextcolor),
[`ListBox.TextOutlineColor`](ListBox#listboxtextoutlinecolor)

---

### `ListBox.TextOutlineColor`

```ags
int ListBox.TextOutlineColor;
```

Gets/sets the color used to display the text's outline, but only if the listbox's Font uses outline. If the font has no outline, then this property has no effect.

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

*See also:* [`ListBox.Font`](ListBox#listboxfont),
[`ListBox.TextColor`](ListBox#listboxtextcolor)

---

### `ListBox.TopItem`

*(Formerly known as `ListBoxSetTopItem`, which is now obsolete)*

```ags
int ListBox.TopItem
```

Gets/sets the top item in the list box. The top item is the first item
that is visible within the list box, so changing this effectively
scrolls the list up and down.

Indexes for TopItem start from 0 for the first item in the list.

Example:

```ags
lstSaveGames.TopItem = 0;
```

will automatically scroll listbox *lstSaveGames* back to the top of the
list.

---

### `ListBox.Translated`

```ags
bool ListBox.Translated
```

Gets/sets whether the list box's items are translated to the selected
game language at runtime.

*Compatibility:* Supported by **AGS 3.3.0** and later versions.
