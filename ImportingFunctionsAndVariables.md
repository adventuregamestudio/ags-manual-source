## Importing functions and variables in other scripts

Let's say you have a function or a variable in one script, but would like to use them in another. Sharing functions and variables between scripts is known as import/export in AGS. You export them from the script where they are defined, and let other scripts use these by declaring an import. An import should be declared where a script can "see" it: usually the most convenient place is a header of a script which does the export.

You also need to remember that currently in AGS a script may only see things declared in its own header and in any header *above* in the list of scripts.

**NOTE:** Room and Dialog scripts can see declarations from all the regular script headers, but not from other rooms or dialogs.

### Exporting and importing a function

All script functions are exported automatically, so they only need an import declaration to let other scripts know that they exist. This is done by declaring a function with an [`import`](ScriptKeywords#import) keyword. The best practice is to place them in the owner script's header.

For example, suppose you have the following function in script MyScript.asc:

```ags
void ScriptAFunction(int param1, int param2)
{
    // some actions here
}
```

Then the import declaration in the MyScript's header (MyScript.ash) should looks like:

```ags
import void ScriptAFunction(int param1, int param2);
```

The name, type and arguments of the function must be the same, otherwise there will be errors either during compilation or at runtime.

### Exporting and importing a variable

Script variables are not exported by default, so that has to be done explicitly inside the script which has them declared, using the [`export`](ScriptKeywords#export) keyword.

Let's suppose that you have the following variable inside MyScript.asc:

```ags
int public_variable;
```

If you want to share this variable with the other scripts, you have put the `export` statement *inside the same script where you have declared the variable* (MyScript.asc), somewhere below the declaration:

```ags
export public_variable;
```

The `export` statement needs _only the variable's name_ and nothing else. It must *not* be put inside a function.

You can export several variables in one statement if you separate their names with commas, for example:

```ags
export public_variable1, public_variable2;
```

Now, import declaration in the MyScript's header (MyScript.ash) should looks like:

```ags
import int public_variable;
```

Both *the name and type* must match the variable's own declaration exactly.<br>
This bit is important. If the variable is `int` then it should be imported as `int`, if it's of the type `Character*` then the import should also be declared as `Character*`.

Imported arrays must have the *correct size*, for example if you have `int arr[10];` then the import declaration must be:

```ags
import int arr[10];
```

(NOTE: the export command still requires its name *only*: `export arr;`)

Similar to the regular variable declaration, you can declare the import of several variables _of the same type_ in one statement if you separate them by commas:

```ags
import int var1, var2;
```

**NOTE:** Most commonly import declarations are put in the *header* of the same script where these variables are defined. But in theory you may not do that and instead place imports inside each particular script where you intend to use these variables.

So, for a final example, you have this in **MyScript.asc**:

```ags
int public_int1, public_int2;
int arr[10];
String someString;
Character* RandomChar;
export public_int1, public_int2, arr, someString, RandomChar;
```

And in **MyScript.ash**:

```ags
import int public_int1, public_int2;
import int arr[10];
import String public_str;
import Character* RandomChar;
```

To sum this up, the import declaration lets other scripts see this variable, and the export command actually connects existing variables to that declaration.

**NOTE:** The AGS Editor has a special menu called "Global Variables" meant for easier creation of public variables which could are accessible everywhere. This may be a better alternative for beginners, as it takes care of all this import/export managing behind the scenes.

*See also:* [import keyword](ScriptKeywords#import), [export keyword](ScriptKeywords#export)
