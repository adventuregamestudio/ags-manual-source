## `Dictionary` Functions and Properties

A Dictionary allows you to store key/value string pairs and look up values be referencing their associated key. Within the dictionary each key must be unique, but the value does not have to be. This means that you may have the same value assigned for multiple keys, but not a single key with multiple values.

Because of how dictionaries work internally, searching for a key in dictionary is faster than you'd search for it in a regular array. The advantage increases with the number of items searched. They are also fast to add or remove items.

A Dictionary has two general properties: **sort style** and **compare style**.

**Sort style** defines whether items are stored sorted or unsorted, based on their keys. The unsorted containers are commonly somewhat faster to search in, but the items will be stored in an undefined order.

**Compare style** determines whether string keys are compared as case sensitive or case insensitive. For example, in a case-sensitive dictionary the strings "Parameter" and "parameter" will be seen as two different keys, but in a case-insensitive dictionary they will be seen as the same key. Compare style defines both key uniqueness and sorting.

At the moment a dictionary does not let you directly access all of its internal data, but has [`GetKeysAsArray`](Dictionary#dictionarygetkeysasarray) and [`GetValuesAsArray`](Dictionary#dictionarygetvaluesasarray) functions that will write all items to a dynamic array, which you may parse, print, and otherwise use as you see fit. The order of items in these arrays will match their order within the dictionary, hence if the dictionary was sorted the array will be sorted as well.

To summarize, dictionaries make convenient storage for key/value pairs. If you do not need pairs but rather a sequence of unique values, you may wish to use a [`Set`](Set) instead.

*Compatibility:* Dictionary struct is supported by **AGS 3.5.0** and later versions.

---

### `Dictionary.Create`

```ags
static Dictionary* Dictionary.Create(SortStyle sortStyle, StringCompareStyle compareStyle)
```

Creates a new empty Dictionary of the given properties. If you don't pass any options the Dictionary is unsorted and case-insensitive. Note that you cannot change sorting style and case sensitivity later, you would have to create another Dictionary and then re-add the values there.

- Possible values for sortStyle: `eNonSorted`, `eSorted`.
- Possible values for compareStyle: `eCaseInsensitive`, `eCaseSensitive`.

Example:

```ags
Dictionary* myDictionary = Dictionary.Create();
```

---

### `Dictionary.Clear`

```ags
void Dictionary.Clear()
```

Removes all items from the dictionary.

Example:

```ags
// a new chapter begins, and the old notes are no longer relevant
notes.Clear();
```

will remove all the notes at once. Here `notes` is a global Dictionary* that holds the player's notes, created with Dictionary.Create.

---

### `Dictionary.Contains`

```ags
bool Dictionary.Contains(String key)
```

Returns true if the key is in the dictionary, otherwise - false.

Example:

```ags
Dictionary* myDictionary = Dictionary.Create();
myDictionary.Set("my-key","my-value");
if(myDictionary.Contains("my-key")){
    Display("has my key!");
}
```

This will add "my-key" key, assign "my-value" to that key and then test whether that key was added successfully.

*See also:* [`Dictionary.Get`](Dictionary#dictionaryget),
[`Dictionary.Set`](Dictionary#dictionaryset)

---

### `Dictionary.Get`

```ags
String Dictionary.Get(String key)
```

Gets the value for the given key. Returns null if such a key does not exist.

Example:

```ags
Dictionary* myDictionary = Dictionary.Create();
myDictionary.Set("a-key","a-value");
String myValue = myDictionary.Get("a-key");
```

Here _myValue_ variable will be assigned "a-value" from the dictionary.

*See also:* [`Dictionary.Set`](Dictionary#dictionaryset)

---

### `Dictionary.GetKeysAsArray`

```ags
String[] Dictionary.GetKeysAsArray()
```

Creates a dynamic array filled with keys in the same order as they are stored in the Dictionary, set by `SortStyle` when the Dictionary is created.

**IMPORTANT:** this returns a *copy* of Dictionary contents existing at the time of function call. If you change Dictionary later, that won't affect previously created key array and you would have to call this function again.

Returns null if this Dictionary is empty.

Example:

```ags
Dictionary* myDictionary = Dictionary.Create(eSorted);
myDictionary.Set("my-key1","my-value1");
myDictionary.Set("my-key2","my-value2");
String keys[] = myDictionary.GetKeysAsArray();
for (int i = 0; i < myDictionary.ItemCount; i++)
    Display("#%d: %s", i, keys[i]);
```

In the above example the keys will be displayed on screen one by one, preceded by their index.

*See also:* [`Dictionary.ItemCount`](Dictionary#dictionaryitemcount)

---

### `Dictionary.GetValuesAsArray`

```ags
String[] Dictionary.GetValuesAsArray()
```

Creates a dynamic array filled with values in the same order as they are stored in the Dictionary, set by `SortStyle` when the Dictionary is created.

**IMPORTANT:** this returns a *copy* of Dictionary contents existing at the time of function call. If you change Dictionary later, that won't affect previously created values array and you would have to call this function again.

Returns null if this Dictionary is empty.

Example:

```ags
Dictionary* myDictionary = Dictionary.Create(eSorted);
myDictionary.Set("my-key1","my-value1");
myDictionary.Set("my-key2","my-value2");
String values[] = myDictionary.GetValuesAsArray();
for (int i = 0; i < myDictionary.ItemCount; i++)
    Display("#%d: %s", i, values[i]);
```

In the above example the values will be displayed on screen one by one, preceded by their index.

*See also:* [`Dictionary.ItemCount`](Dictionary#dictionaryitemcount)

---

### `Dictionary.Remove`

```ags
bool Dictionary.Remove(String key)
```

Removes a key/value pair from the dictionary, returns true on success and false if there was no such key.

Example:

```ags
// the lighthouse puzzle is solved, so the note about it is no longer needed
if (notes.Remove("lighthouse"))
{
  player.Say("I can cross that one off my notes.");
}
```

will remove the note stored under the key "lighthouse", and make the player comment on it only if there was such a note. Here `notes` is a global Dictionary* that holds the player's notes, created with Dictionary.Create.

*See also:* [`Dictionary.Set`](Dictionary#dictionaryset)

---

### `Dictionary.Set`

```ags
bool Dictionary.Set(String key, String value)
```

Assigns a value to the given key. If the key did not exist then it will be created, otherwise the old value for this key will be overwritten with a new one.

Example:

```ags
notes.Set("lighthouse", "The keeper hides a key somewhere.");
// ... later, after finding out more:
notes.Set("lighthouse", "The key is under the doormat.");
```

will store a note under the key "lighthouse", and later replace it with a new one, since a dictionary keeps only one value for each key. Here `notes` is a global Dictionary* that holds the player's notes, created with Dictionary.Create.

*See also:* [`Dictionary.Get`](Dictionary#dictionaryget),
[`Dictionary.Remove`](Dictionary#dictionaryremove)

---

### `Dictionary.CompareStyle`

```ags
StringCompareStyle Dictionary.CompareStyle
```

Returns the string comparison method for this dictionary, which determines whether its keys are case-sensitive.

Example:

```ags
function SetFlag(Dictionary* flags, String key, String value)
{
  if (flags.CompareStyle == eCaseSensitive)
  {
    key = key.LowerCase(); // so that "Door" and "door" mean the same key
  }
  flags.Set(key, value);
}
```

will store a value in any dictionary, converting the key to lowercase first if the dictionary would otherwise treat "Door" and "door" as different keys.

*See also:* [`Dictionary.SortStyle`](Dictionary#dictionarysortstyle)

---

### `Dictionary.ItemCount`

```ags
int Dictionary.ItemCount
```

Gets the number of key/value pairs currently in the dictionary.

Example:

```ags
btnNotes.Text = String.Format("Notes (%d)", notes.ItemCount);
```

will show on the button btnNotes how many notes the player has. Here `notes` is a global Dictionary* that holds the player's notes, created with Dictionary.Create.

---

### `Dictionary.SortStyle`

```ags
SortStyle Dictionary.SortStyle
```

Returns the key sorting method for this dictionary.

Example:

```ags
function FillListFromDictionary(ListBox* list, Dictionary* dict)
{
  list.Clear();
  String keys[] = dict.GetKeysAsArray();
  for (int i = 0; i < keys.Length; i++)
  {
    list.AddItem(keys[i]);
  }
  if (dict.SortStyle == eNonSorted)
  {
    list.SortItems(eCaseInsensitive, eSortAscending);
  }
}
```

will fill a list box with the keys of a dictionary, and sort the list only if the dictionary does not already keep its keys sorted.

*See also:* [`Dictionary.CompareStyle`](Dictionary#dictionarycomparestyle)
