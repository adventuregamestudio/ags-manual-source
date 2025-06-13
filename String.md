## `String` functions

### `String.Append`

*(Formerly known as global function `StrCat`, which is now obsolete)*

```ags
String.Append(string str2)
```

Appends the string `str2` to the end of the specified string, and returns
the result.

**IMPORTANT:** The result of joining the strings together is returned as
a new string from this command. The original string will **NOT** be
changed. For example, the following script will not do anything:<br>
`mytext.Append("World");`<br>
what you probably want instead is:<br>
`mytext = mytext.Append("World");`

Example:

```ags
String mytext = "Hello";
mytext = mytext.Append("World");
Display(mytext);
```

will display "HelloWorld".

*See also:* [`String.AppendChar`](String#stringappendchar),
[`String.Substring`](String#stringsubstring),
[`String.Truncate`](String#stringtruncate)

---

### `String.AppendChar`

```ags
String.AppendChar(int extraChar)
```

Appends a single character to the end of the specified string, and
returns the result.

**IMPORTANT:** The newly extended text is returned as a new string from
this command. The original string will **NOT** be changed. For example,
the following script will not do anything:<br>
`mytext.AppendChar('o');`<br>
what you probably want instead is:<br>
`mytext = mytext.AppendChar('o');`

Example:

```ags
String mytext = "Hell";
mytext = mytext.AppendChar('o');
Display(mytext);
```

will display "Hello".

*Compatibility:* This functions can take an utf-8 char as `int` since **AGS 3.6.0** and later versions.

*See also:* [`String.Append`](String#stringappend)

---

### `String.CompareTo`

*(Formerly known as global function `StrCaseComp`, which is now
obsolete)*<br>
*(Formerly known as global function `StrComp`, which is now obsolete)*

```ags
String.CompareTo(string str2, optional bool caseSensitive)
```

Compares the specified string to `str2`. *caseSensitive* determines
whether "Dog" and "dog" are equivalent; case sensitivity is off by
default.

Returns 0 if the strings match, a number less than 0 if this string is
earlier in the alphabet than `str2`, and a number greater than 0 if this
string is later in the alphabet than `str2`.

**TIP:** To do a case-sensitive comparison of two strings, it's easier
to just use the == operator.

Example:

```ags
String mytext = "Hello";
if (mytext.CompareTo("hello") == 0) {
    Display("Strings match with case sensitivity off!");
}
else {
    Display("Strings don't match with case sensitivity off!");
}

if (mytext == "hello") {
    Display("Strings match with case sensitivity on!");
}
else {
    Display("Strings don't match with case sensitivity on!");
}
```

will display "Strings match with case sensitivity off!", and then
"Strings don't match with case sensitivity on!".

---

### `String.Copy`

*(Formerly known as global function `StrCopy`, which is now obsolete)*

```ags
String.Copy()
```

Returns a new copy of the specified string. You should not normally need
to use this, since strings can be assigned using the = operator.

Example:

```ags
String mystring = "This is a test string.";
String newstring = mystring.Copy();
Display(newstring);
```

will display "This is a test string".

---

### `String.EndsWith`

```ags
bool String.EndsWith(string lookForText, optional bool caseSensitive)
```

Returns *true* if this string ends with *lookForText*, or *false* if
not.

*caseSensitive* is *false* by default, but you can set it to true so
that the function will only return *true* for an exact-case match.

Example:

```ags
String myString = "Hello from the script!";
if (myString.EndsWith("script!"))
{
    Display("Ends with script!");
}
```

will display the "Ends with script!" message.

*Compatibility:* Supported by **AGS 3.1.0** and later versions.

*See also:* [`String.IndexOf`](String#stringindexof),
[`String.StartsWith`](String#stringstartswith)

---

### `String.Format`

*(Formerly known as global function `StrFormat`, which is now obsolete)*

```ags
static String.Format(string fmt, ...)
```

Processes the string FMT, optionally inserting values into the placeholders inside FMT, and returns the resulting string.
You can insert the numeric or text, as well as value of variables into the string.
Besides other purposes, this function is the method for converting numbers into the text.

For more information on how to work with format string, see the [string formatting](StringFormats) section.

**NOTE:** This function is static, which means you do not call it on an
existing string variable, but use `String.Format()` instead.

Examples:

```ags
// create a text string containing "10".
String text = String.Format("%d", 10);

// create a text string with health stats in it
int health, max_health;
String text = String.Format("Health: %d / %d", health, max_health);

// create a text string with character's name
String text = String.Format("Hello, my name is %s", player.Name);
```

*See also:* [`Display`](Globalfunctions_Message#display)

---

### `String.IndexOf`

*(Formerly known as global function `StrContains`, which is now
obsolete)*<br>
*(Formerly known as `String.Contains`, which is now obsolete)*

```ags
String.IndexOf(string needle, optional StringCompareStyle style, optional int index, optional int count)
```

Checks to see if NEEDLE is contained within the specified string.
Returns the character position of the match if it is, or -1 if it is
not.

This function is not case sensitive; i.e. testing "test string" for
"sTRiN" would match.

Example:

```ags
String haystack = "The haystack had a needle in it somewhere.";
int result = haystack.IndexOf("a needle");

if (result == -1) {
    Display("The string didn't contain the needle.");
}
else {
    Display("a needle was found starting at character %d in the string.", result);
}
```

*Compatiblity:* Optional `style`, `index`, and `count` parameteres are supportet by **AGS 4.0.0** and later.

*See also:* [`String.EndsWith`](String#stringendswith),
[`String.StartsWith`](String#stringstartswith),
[`StringCompareStyle`](StandardEnums#stringcomparestyle)

---

### `String.IsNullOrEmpty`

```ags
static bool String.IsNullOrEmpty(String stringToCheck)
```

Returns whether the supplied string is null or empty. This is simply
shorthand for the following:

```ags
if ((stringToCheck == null) || (stringToCheck == ""))
```

in other words, you can easily use this to check whether a string has
any text in it or not.

**NOTE:** This function is static, which means you do not call it on an
existing string variable, but use `String.IsNullOrEmpty()` instead. See
the example.

Example:

```ags
String myString;
if (String.IsNullOrEmpty(myString))
{
    myString = "Some text";
}
```

will set the myString variable to "Some text" if it is null or empty
(which it is).

*Compatibility:* Supported by **AGS 3.0.1** and later versions.

---

### `String.Join`

```ags
static String.Join(String[], string separator)
```

Joins a dynamic array of strings using the separator in a single string

**NOTE:** This function is static, which means you do not call it on an
existing string variable, but use `String.Join()` instead. 

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `String.LowerCase`

*(Formerly known as global function `StrToLowerCase`, which is now
obsolete)*

```ags
String.LowerCase()
```

Returns a lower case version of the specified string.

**NOTE:** The new string is returned from this function; it does **NOT**
modify the original string.

Example:

```ags
String mystring = "THIS is a test string";
String lowercased = mystring.LowerCase();
Display("Old: %s, new: %s", mystring, lowercased);
```

will display "Old: THIS is a test string, new: this is a test string".

*See also:* [`String.UpperCase`](String#stringuppercase)

---

### `String.Replace`

```ags
String.Replace(string lookForText, string replaceWithText,
               optional bool caseSensitive)
```

Creates a copy of this string, with all instances of *lookForText*
replaced with the *replaceWithText*.

*caseSensitive* is *false* by default, but you can set it to true so
that only case-sensitive matches of the *lookForText* will be replaced.

**NOTE:** The new string is returned from this function; it does **NOT**
modify the original string.

Example:

```ags
String original = "Hello from the script!";
String changed = original.Replace("hello", "goodbye");
Display("Old: %s, new: %s", original, changed);
```

will display "Old: Hello from the script!, new: goodbye from the
script!".

*Compatibility:* Supported by **AGS 3.1.0** and later versions.

*See also:* [`String.ReplaceCharAt`](String#stringreplacecharat)

---

### `String.ReplaceCharAt`

*(Formerly known as global function `StrSetCharAt`, which is now
obsolete)*

```ags
String.ReplaceCharAt(int index, int newChar)
```

Changes the character at INDEX in the string to NEWCHAR.

INDEX is the character index into the string (where 0 is the first
character, and the last allowable value is the string's Length minus 1).

**NOTE:** The new string is returned from this function; it does **NOT**
modify the original string.

Example:

```ags
String mystring = "Hello";
String changed = mystring.ReplaceCharAt(2, 'm');
Display("Old: %s, new: %s", newstring, changed);
```

will display "Old: Hello, new: Hemlo".

*Compatibility:* This functions can take an utf-8 char as `int` since **AGS 3.6.0** and later versions.

*See also:* [`String.Chars`](String#stringchars),
[`String.Replace`](String#stringreplace)

---

### `String.Split`

```ags
String[] String.Split(string separator, optional StringSplitOptions options);
```

Creates a dynamic array made of substrings of the string each time the separator occurred.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [`StringSplitOptions`](StandardEnums#stringsplitoptions)

---

### `String.StartsWith`

```ags
bool String.StartsWith(string lookForText, optional bool caseSensitive)
```

Returns *true* if this string starts with *lookForText*, or *false* if
not.

*caseSensitive* is *false* by default, but you can set it to true so
that the function will only return *true* for an exact-case match.

Example:

```ags
String myString = "Hello from the script!";
if (myString.StartsWith("hello"))
{
    Display("Starts with hello!");
}
```

will display the "Starts with hello!" message.

*Compatibility:* Supported by **AGS 3.1.0** and later versions.

*See also:* [`String.EndsWith`](String#stringendswith),
[`String.IndexOf`](String#stringindexof)

---

### `String.Substring`

```ags
String.Substring(int index, int length)
```

Returns part of the string, starting from character *index* and *length*
characters long.

*index* is the initial character index, where 0 is the first character
and (Length - 1) is the last. *length* specifies how many characters to
retrieve.

Example:

```ags
String mystring = "Hello World!";
String substring = mystring.Substring(3, 5);
Display("Original: %s, Substring: %s", mystring, substring);
```

will display "Original: Hello World!, Substring: lo Wo".

*See also:* [`String.Append`](String#stringappend),
[`String.Chars`](String#stringchars)

---

### `String.Trim`

```ags
String.Trim()
```

Returns a new string without any leading or trailing whitespace.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `String.Truncate`

```ags
String.Truncate(int length)
```

Returns a version of the string that has been truncated down to *length*
characters.

**NOTE:** The new string is returned from this function; it does **NOT**
modify the original string.

Example:

```ags
String mystring = "Hello World!";
String truncated = mystring.Truncate(4);
Display("Original: %s, Truncated: %s", mystring, truncated);
```

will display "Original: Hello World!, Truncated: Hell".

*See also:* [`String.Append`](String#stringappend),
[`String.Substring`](String#stringsubstring)

---

### `String.UpperCase`

*(Formerly known as global function `StrToUpperCase`, which is now
obsolete)*

```ags
String.UpperCase()
```

Returns an upper case version of the specified string.

**NOTE:** The new string is returned from this function; it does **NOT**
modify the original string.

Example:

```ags
String mystring = "THIS is a test string";
String uppercased = mystring.UpperCase();
Display("Old: %s, new: %s", mystring, uppercased);
```

will display "Old: THIS is a test string, new: THIS IS A TEST STRING".

*See also:* [`String.LowerCase`](String#stringlowercase)

---

### `String.AsFloat`

```ags
readonly float String.AsFloat;
```

Converts the string into a float, and returns that value. Returns 0.0 if
the string does not contain a number.

Example:

```ags
String text1, text2;
float number1,number2;
text1 = "57.362";
text2 = "Hello";
number1 = text1.AsFloat;
number2 = text2.AsFloat;
```

will set number1 value to 57.362 and number2 value to 0.0 This function
is useful for processing strings input from the user.

**NOTE:** To convert a float to a string, you can use the
[`String.Format`](String#stringformat) command.

*See also:* [`Game.InputBox`](Game#gameinputbox),
[`String.AsInt`](String#stringasint),
[`String.Format`](String#stringformat)

---

### `String.AsInt`

*(Formerly known as global function `StringToInt`, which is now obsolete)*

```ags
readonly int String.AsInt;
```

Converts the string into an integer, and returns that value. Returns
zero if the string does not present a number.

**NOTE:** This operation takes just the leading sequence of the
characters from the string, if it contains digits and other symbols
which could be a part of number. If the string has the valid number only
in the middle, it will not work.

Example:

```ags
String text1, text2;
int number1,number2;
text1 = "53";
text2 = "Hello";
number1 = text1.AsInt;
number2 = text2.AsInt;
```

will set number1 value to 53 and number2 value to 0. This function is
useful for processing strings input from the user.

**NOTE:** To convert an integer to a string, you can use the
[`String.Format`](String#stringformat) command.

*See also:* [`Game.InputBox`](Game#gameinputbox),
[`String.AsFloat`](String#stringasfloat),
[`String.Format`](String#stringformat)

---

### `String.Chars`

*(Formerly known as global function `StrGetCharAt`, which is now
obsolete)*

```ags
readonly int String.Chars[position];
```

Returns the character at POSITION within the string.

POSITION is the character index (where 0 is the first character, and the
last allowable value is the Length minus 1).

If POSITION is outside the string, this function returns 0.

**NOTE:** The *Chars* array is read-only. If you want to change one of
the characters in the string, use
[`String.ReplaceCharAt`](String#stringreplacecharat).

Example:

```ags
String text = "This is my string.";
Display("The 4th character is: %c", text.Chars[3]);
```

will display "The 4th character is: s".

*Compatibility:* This attribute returns an utf-8 char as `int` since **AGS 3.6.0** and later versions.

*See also:* [`String.Length`](String#stringlength),
[`String.ReplaceCharAt`](String#stringreplacecharat)

---

### `String.Length`

*(Formerly known as global function `StrLen`, which is now obsolete)*

```ags
readonly int String.Length;
```

Returns the length of the string, in characters.

Example:

```ags
String text = "This is my string.";
Display("Length: %d", text.Length);
```

will display "Length: 18".

