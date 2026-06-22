## `Utils` functions and properties

### `Utils.SortFloats`

```ags
static void Utils.SortFloats(float floatArr[], SortDirection)
```

Sorts the provided dynamic array of floats in the requested direction (ascending or descending).

Example:

```ags
float farr[] = new float[5];
farr[0] = 1.2; farr[1] = 0.7; farr[2] = 1.8; farr[3] = -0.4; farr[4] = 2.1;
Utils.SortFloats(farr, eSortAscending);
```

will take 'farr' array of floats and sort it in ascending direction.

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

---

### `Utils.SortInts`

```ags
static void Utils.SortInts(int intArr[], SortDirection)
```

Sorts the provided dynamic array of ints in the requested direction (ascending or descending).

Example:

```ags
int arr[] = new int[5];
arr[0] = 9; arr[1] = 5; arr[2] = 3; arr[3] = 11; arr[4] = 7;
Utils.SortInts(arr, eSortAscending);
```

will take 'arr' array of ints and sort it in ascending direction.

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

---

### `Utils.SortStrings`

```ags
static void Utils.SortStrings(String stringArr[], StringCompareStyle, SortDirection)
```

Sorts the provided dynamic array of Strings using string comparison style in the requested direction (ascending or descending).

Example:

```ags
String strings[] = new String[5];
strings[0] = "Orange";
strings[1] = "Apple";
strings[2] = "Grapefruit";
strings[3] = "Tangerine";
strings[4] = "Plum";
Utils.SortStrings(strings, eCaseSensitive, eSortAscending);
```

will take 'strings' array of Strings and sort it in ascending direction with respect to letter case.

*Compatibility:* Supported by **AGS 3.6.3** and later versions.
