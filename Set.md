## `Set` Functions and Properties

A Set allows you to store a sequence of unique strings and look them up efficiently. When adding a string a Set will automatically test whether such string is already contained and won't make a duplicate.

Because of how a Set work internally, searching for a string in a Set is faster than searching for it in a plain array. Its advantage increases with the number of items. They are also fast in adding and removing items dynamically.

Similar to [`Dictionary`](Dictionary), Set has two general properties: **sort style** and **compare style**.

**Sort style** defines whether items are stored sorted or unsorted. The unsorted containers are typically faster to search in, but the items will be stored in an undefined order.

**Compare style** determines whether strings are compared as case sensitive or case insensitive. For example, in a case-sensitive Set the strings "Parameter" and "parameter" will be seen as two different items, but in a case-insensitive Set they will be seen as identical. Compare style defines both string uniqueness and sorting.

At the moment a Set does not let you directly access all of its internal data at once, but it does have the [`GetItemsAsArray`](Set#setgetitemsasarray) function that will write all items to a dynamic array, which you may parse, print, and otherwise use as you see fit. The order of items in this array will be match the order within the Set, hence if the Set was sorted the array will be sorted as well.

*Compatibility:* Set struct is supported by **AGS 3.5.0** and later versions.

*See also:* [`Dictionary`](Dictionary)

---

### `Set.Create`

```ags
static Set* Set.Create(SortStyle sortStyle, StringCompareStyle compareStyle)
```

Creates a new empty Set of the given properties. If you don't pass any options the Set is unsorted and case-insensitive. Note that you cannot change sorting style and case sensitivity later, you would have to create another Set and move values there.

- Possible values for sortStyle: `eNonSorted`, `eSorted`.
- Possible values for compareStyle: `eCaseInsensitive`, `eCaseSensitive`.

Example:

```ags
Set* mySet = Set.Create();
```

---

### `Set.Add`

```ags
bool Set.Add(String item)
```

Adds an item to the Set. Returns false if the Set already contained the item, otherwise returns true.

Example:

```ags
mySet.Add("item1");
bool wasAdded = mySet.Add("item1"); // wasAdded will be false
```

*See also:* [`Contains`](Set#setcontains), [`Remove`](Set#setremove)

---

### `Set.Clear`

```ags
void Set.Clear()
```

Removes all items from the Set.

Example:

```ags
// a new case begins, so forget the clues found in the previous one
clues.Clear();
```

will remove all the clues at once. Here `clues` is a global Set* that holds the names of the clues the player has found, created with Set.Create.

---

### `Set.Contains`

```ags
bool Set.Contains(String item)
```

Returns whether an item is already in a Set.

Example:

```ags
Set* mySet = Set.Create();
mySet.Add("test");
if (mySet.Contains("test")) {
    Display("Test passed!");
}
```

*See also:* [`Add`](Set#setadd), [`Remove`](Set#setremove)

---

### `Set.GetItemsAsArray`

```ags
String[] GetItemsAsArray()
```

Creates a dynamic array filled with items in same order as they are stored in the Set, set by `SortStyle` when the Set is created.

Returns null if this Set is empty.

Example:

```ags
Set* mySet = Set.Create(eSorted);
mySet.Add("test");
mySet.Add("test2");
String items[] = mySet.GetItemsAsArray();
for (int i = 0; i < mySet.ItemCount; i++)
    Display("#%d: %s", i, items[i]);
```

In the above example the items will be displayed on screen one by one, preceded by their index.

*See also:* [`Set.ItemCount`](Set#setitemcount)

---

### `Set.Remove`

```ags
bool Set.Remove(String item)
```

Removes an item from the Set. Returns false if there was no such item, otherwise returns true.

Example:

```ags
if (activeQuests.Remove("Find the key"))
{
  lblQuestLog.Text = "Quest completed: Find the key";
}
```

will remove the quest from the set of active quests, and report its completion only if the quest was active. Here `activeQuests` is a global Set*, created with Set.Create.

*See also:* [`Add`](Set#setadd), [`Contains`](Set#setcontains)

---

### `Set.CompareStyle`

```ags
StringCompareStyle Set.CompareStyle
```

Returns the string comparison method for this Set.

Example:

```ags
function AddName(Set* names, String name)
{
  if (names.CompareStyle == eCaseSensitive)
  {
    name = name.LowerCase(); // so that "Bob" and "bob" count as the same name
  }
  names.Add(name);
}
```

will add a name to any set, converting it to lowercase first if the set would otherwise treat "Bob" and "bob" as different items.

*See also:* [`SortStyle`](Set#setsortstyle)

---

### `Set.ItemCount`

```ags
int Set.ItemCount
```

Returns the number of items currently in the Set.

Example:

```ags
lblClues.Text = String.Format("Clues found: %d of 10", clues.ItemCount);
```

will show how many clues the player has found. Because a set never contains the same item twice, finding a clue for the second time does not increase the count. Here `clues` is a global Set*, created with Set.Create.

---

### `Set.SortStyle`

```ags
SortStyle Set.SortStyle
```

Returns the sorting method for this Set.

Example:

```ags
function FillListFromSet(ListBox* list, Set* items)
{
  list.Clear();
  String names[] = items.GetItemsAsArray();
  for (int i = 0; i < names.Length; i++)
  {
    list.AddItem(names[i]);
  }
  if (items.SortStyle == eNonSorted)
  {
    list.SortItems(eCaseInsensitive, eSortAscending);
  }
}
```

will fill a list box with the items of a set, and sort the list only if the set does not already keep its items sorted.

*See also:* [`CompareStyle`](Set#setcomparestyle)
