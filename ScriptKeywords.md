## Script language keywords

### Data types

Type | Description
--- | ---
char | Single byte data type, can store a single character or number 0 to 255
short| 16-bit integer, can store numbers from -32,768 to 32,767
int | 32-bit integer, can store from -2,147,483,648 to 2,147,483,647
String | Stores a string of characters
float | 32-bit floating point number. Accuracy normally about 6 decimal places, but varies depending on the size of the number being stored.
bool | a variable that stores either 'true' or 'false'

You will normally only need to use the **int** and **String** data
types. The smaller types are only useful for conserving memory if you
are creating a very large number of variables.

To declare a variable, write the type followed by the variable name,
then a semicolon. For example:

`int my_variable;`

declares a new 32-bit integer called my_variable

**WARNING:** When using the *float* data type, you may find that the ==
and != operators don't seem to work properly. For example:

```ags
float result = 2.0 * 3.0;
if (result == 6.0) {
    Display("Result is 6!");
}
```

may not always work. This is due to the nature of floating point
variables, and the solution is to code like this:

```ags
float result = 2.0 * 3.0;
if ((result > 5.99) && (result < 6.01)) {
    Display("Result is 6!");
}
```

The way floating point numbers are stored means that 6 might actually be
stored as 6.000001 or 5.999999; this is a common gotcha to all
programming languages so just be aware of it if you use any floating
point arithmetic.

**NOTE:** String literals that are placed next to each other, separated only by whitespace (including line breaks), are joined into one literal by the compiler. This is useful for splitting a long text over several lines:

```ags
player.Say("Hello" " world");  // same as "Hello world"
```

*Compatibility:* Joining string literals is supported by **AGS 4.0.0** and later versions, and only when using the new script compiler.

---

### Arrays

*data_type* *name* `[` *size* `];`

Arrays allow you to easily create several variables of the same type.
For example, suppose you wanted to store a health variable for all the
different characters in the game. One way would be to declare several
different variables like this:

```ags
int egoHealth;
int badGuyHealth;
int swordsmanHealth;
```

but that quickly gets messy and difficult to keep up to date, since you
need to use different script code to update each one. So instead, you
can do this at the top of your script:

```ags
int health[50];
```

This example declares 50 int variables, all called *health*.<br>
You access each separate variable via its **index** (the number in the
brackets). Indexes start from 0, so in this case the *health* array can
be accessed by indexes 0 to 49. If you attempt to access an invalid
index, your game will exit with an error.

Here's an example of using the array:

```ags
health[3] = 50;
health[4] = 100;
health[player.ID] = 10;
```

this sets Health 3 to 50, Health 4 to 100, and the Health index that
corresponds to the player character's ID number to 10.  
You need to do this inside a function.

**NOTE:** AGS currently doesn't support initialization of a range of elements in a single assignment, like many programming languages do.

Oftentimes you may want to do certain action over each element of array, or particular range of elements, such as each element from 5th to 10th, for example. Because array's index may be taken from a variable too (like *health[index]*), the standard way of doing so is using **loops**:

```ags
int i;
while (i < 50) {
    health[i] = 0;
    i++;
}
```

or 

```ags
for (int i = 0; i < 50; i++) {
    health[i] = 0;
}
```

Both examples reset every element of "health" array to a zero.

The array's size must be a constant. With the new script compiler it may also be an expression, so long as the compiler can calculate it, for example when it consists only of literal numbers, [`const`](ScriptKeywords#const) values and macros:

```ags
#define MAX_PLAYERS 4
const int ITEMS_PER_PLAYER = 10;
int items[MAX_PLAYERS * ITEMS_PER_PLAYER + 1];
```

*See also:* [Dynamic arrays](DynamicArrays), [Structs](ScriptKeywords#struct), [while](ScriptKeywords#while), [for](ScriptKeywords#for)

---

### Multidimensional arrays

*data_type* *name* `[` *size1* `,` *size2* ... `];`<br>
*data_type* *name* `[` *size1* `][` *size2* `]` ... `;`

You may declare arrays with multiple dimensions (2D and higher). There are two ways of declaring them, but other than having slightly different syntax these are fully equivalent:

```ags
// Declare two-dimensional array of size 5x3
int a[5, 3];
int b[5][3];
```

You can have any positive number of dimensions, e.g. `int a[2][3][5][7][11];`

Accessing elements is done similarly to declaration, and either syntax may be used regardless of how the array was declared:

```ags
int a[5, 3];
int e11 = a[1, 1];
int e21 = a[2][1];
```

For example, here's a map of tiles, where each value is accessed by its row and column:

```ags
#define MAP_WIDTH   50
#define MAP_HEIGHT  20

int map[MAP_HEIGHT, MAP_WIDTH];

// Fill particular row with identical values
void SetMapRow(int row, int value) {
    for (int col = 0; col < MAP_WIDTH; col++) {
        map[row, col] = value;
    }
}

// Get a map value at particular row and column
int GetMapValue(int col, int row)  {
    return map[row, col];
}
```

[Dynamic arrays](DynamicArrays) may also have multiple dimensions, but these work somewhat differently, see [Multi-dimensional dynamic arrays](DynamicArrays#multi-dimensional-dynamic-arrays).

*Compatibility:* Multi-dimensional arrays are supported by **AGS 4.0.0** and later versions, and only when using the new script compiler.

**Emulating multi-dimensional arrays**

Older versions of AGS (and the legacy script compiler) do not support multi-dimensional arrays natively.
There's, however, a classic method of emulating 2D array using a 1D array, if you create 1D array of size equal to "ARR_WIDTH * ARR_HEIGHT". You may then access its elements using following formula:

    value = array[row * ARR_WIDTH + col];

where "row" and "col" are indexes of row and column respectively. You may think of these as Y and X coordinates, if you prefer.

This method works with both regular arrays and [dynamic arrays](DynamicArrays).

For example:

```ags
#define MAP_WIDTH   50
#define MAP_HEIGHT  20
#define MAP_SIZE    1000 // this is MAP_WIDTH * MAP_HEIGHT

int map[MAP_SIZE];

// Fill whole map with random values from min_value to max_value (inclusive)
void InitRandomMap(int min_value, int max_value) {
    // here we do not care about rows and columns, so iterate this array as a "linear" (1D) array
    for (int i = 0; i < MAP_SIZE; i++) {
        map[i] = Random(max_value - min_value) + min_value;
    }
}

// Fill particular row with identical values
void SetMapRow(int row, int value) {
    for (int col = 0; col < MAP_WIDTH; col++) {
        map[row * MAP_WIDTH + col] = value;
    }
}

// Fill particular column with identical values
void SetMapColumn(int col, int value) {
    for (int row = 0; row < MAP_HEIGHT; row++) {
        map[row * MAP_WIDTH + col] = value;
    }
}

// Get a map value at particular row and column
int GetMapValue(int col, int row)  {
    return map[row * MAP_WIDTH + col];
}
```

---

### Operators

The AGS scripting engine supports the following operators in
expressions. They are listed in order of precedence, with the most
tightly bound at the top of the list.

Operator | Description | Example
--- | --- | ---
`!` | NOT | `if (!a)`
`~` | Bitwise NOT | `a = ~b;`
`*` | Multiply | `a = b * c;`
`/` | Divide | `a = b / c;`
`%` | Remainder | `a = b % c;`
`+` | Add | `a = b + c;`
`-` | Subtract | `a = b - c;`
`++` | Increment by 1 | `a++;` or `++a;`
`--` | Decrement by 1 | `a--;` or `--a;`
`<<` | Bitwise Left Shift | `a = b << c;`
`>>` | Bitwise Right Shift | `a = b >> c;`
`&` | Bitwise AND | `a = b & c;`
<code>&#124;</code> | Bitwise OR | <code>a = b &#124; c;</code>
`^` | Bitwise XOR | `a = b ^ c;`
`==` | Is equal to | `if (a == b)`
`!=` | Is not equal to | `if (a != b)`
`>` | Is greater than | `if (a > b)`
`<` | Is less than | `if (a < b)`
`>=` | Is greater than or equal | `if (a >= b)`
`<=` | Is less than or equal | `if (a <= b)`
`&&` | Logical AND | `if (a && b)`
<code>&#124;&#124;</code> | Logical OR | <code>if (a &#124;&#124; b)</code>
`? :` | Conditional (ternary) | `a = (b > 5) ? c : d;`

This order of precedence allows expressions such as the following to
evaluate as expected:

`if (!a && b < 4)`

which will execute the 'if' block if **a** is 0 and **b** is less than
4.

However, it is always good practice to use parenthesis to group
expressions. It's much more readable to script the above expression like
this:

`if ((!a) && (b < 4))`

**NOTE:** By default, when using operators of equal precedence AGS
evaluates them left-to-right. For example, the expression `a = 5 - 4 - 2;`
evaluates as `a = (5 - 4) - 2;`, which is what you'd commonly expect from
a scripting language today.<br>
For historical reasons AGS also supports right-to-left precedence
mode which may be only useful if you import very old game and do not want to
fix its scripts. To enable this mode find "Left-to-right operator precedence"
option in "Backwards compatibility" section of the General Settings and set it
to "false".

**NOTE:** For signed types (`short` and `int`), the right shift `>>` is an 
*arithmetic* right shift, which means that the most significant bit will be copied over
to fill the shifted spaces (1 for negative values, and 0 for positive ones).<br>
For the purpose of logical bitwise manipulation, bit masking will need to be applied, 
eg: `(0xFE223344 >> 24) & 0xFF` to return `0xFE`.

**Pre- and post-increment**

The increment and decrement operators may be placed either after the variable (`x++`, `x--`)
or before it (`++x`, `--x`). The difference shows when this operation is used inside another expression:

* With post-increment/decrement the *old* variable's value is used in the expression, and the variable is changed afterwards.
* With pre-increment/decrement the variable is changed first, and its *new* value is used in the expression.

```ags
int x = 5;
int a = x++; // a = 5, x becomes 6
int b = ++x; // x becomes 7, b = 7
```

**Bitwise NOT**

The `~` operator inverts each bit in the value: 1 becomes 0 and 0 becomes 1. For example,
`~0` results in `0xFFFFFFFF` (which is `-1` for an `int`), and it may be used to clear particular bits:
`flags = flags & ~0x04;` clears the third bit of `flags`.

**Conditional operator**

The conditional (also called "ternary") operator `? :` selects one of two values depending on a condition:

```ags
int foo = (x > 5) ? 1 : 2; // foo is set to 1 if x > 5, and to 2 otherwise
```

The middle part may be left out, so that `?` and `:` go right next to each other:

```ags
int foo = bar ?: 15;
```

This means that `foo` is normally set to `bar`, but if `bar` is 0 or null, then `foo` is set to 15 instead.
This is particularly handy for specifying a default in case a pointer variable turns out to be null,
for example: `String name = customName ?: "Unknown";`

**Pointer cast operator**

The `as` operator converts a pointer of one type to a pointer of a related type. This works in both
directions: from a child type to a parent type, and from a parent type to a child type. In case of success
it returns a valid pointer of the requested type, and if the object is not of that type it returns `null`:

```ags
GUIControl* someControl = gui[0].Controls[0];
Button* aButton = someControl as Button;
if (aButton != null) {
    aButton.Text = "It's a button!";
}
```

See also: [Casting pointers](ScriptManagedStructs#casting-pointers)

*Compatibility:* Pre-increment and pre-decrement, `~`, `? :` and `as` operators are supported by **AGS 4.0.0** and later versions, and only when using the new script compiler.

**Compile-time evaluation**

The new script compiler calculates expressions of `int` and `float` types at compile time whenever their result
is constant. This lets you use such expressions wherever the syntax requires a constant, such as the size of an
array or the value of a [`const`](ScriptKeywords#const):

```ags
int arr[7 * 9 + 1]; // the size of array will be calculated by compiler
```

---

### Version checking

If you are writing a script module, you may need to check which version
of AGS the user of your module is using.

For this purpose there are two directives:

```ags
#ifver 2.72
// do stuff for 2.72 and above
#endif
#ifnver 2.72
// do stuff for 2.71 and below
#endif
```

Note that this ability was only added in 2.72, so you cannot use the
`#ifver` checks if you want your module to work with earlier versions
than this.

**NOTE:** We recommend to not rely on editor version in most cases, and use [Script API version macros](Constants) instead whenever possible.

*See also:* [Preprocessor](Preprocessor#ifver-ifnver)

---

### if, else statements

**if (** *expression* **)** `{`<br>
*statements1*<br>
`}`<br>
\[ **else** `{`<br>
*statements2*<br>
`}` \]

If *expression* is true, then *statements1* are run.

If *expression* is not true, and there is an **else** clause present,
then *statements2* are run instead.

For example:

```ags
if (player.Room == 10) {
    Display("Player is in the room 10.");
}
else {
    Display("Player is NOT in the room 10.");
}
```

In this example, the first message will be displayed if the return value
from `player.Room` is 10, and the second message will be displayed
if it is not.

**if** statements can be nested inside **else** statements to produce an
"else if" effect. For example:

```ags
if (player.Room == 1) {
    Display("Player is in the room 1.");
}
else if (player.Room == 2) {
    Display("Player is in the room 2.");
}
else {
    Display("Player in neither in room 1 nor room 2.");
}
```

---

### switch, case statements

**switch (** *control_expression* **)** `{`<br>
\[ **case** *match_expression*:<br>
*statements*<br>
\[ **break**; \] \]<br>
\[ **default:**<br>
*statements*<br>
\[ **break**; \] \]<br>
`}`

Compares the result of *control_expression* against the result of
*match_expression* for each **case** label in order. If a match is found,
statements following that label are executed. If there is no matching
label and a **default:** label is present, statements following the
**default:** label are executed.

If a **break** statement is encountered, any statements following it
are skipped and execution continues after the **switch** block.

Unlike many programming languages, AGS allows expression results of any
type (integer, boolean, string, pointers). It also does not require
that *match_expression*s be constant or literal values.

A switch statement is useful if you need to compare one value or
expression against a series of values. The *control_expression*
represents the value you want to compare and each **case** label is
one value in a series to compare it against.

Example:

```ags
switch (player)
{
    case cEgo:
        Display("Hello, my name is Ego.");
        break;
    case cJohn:
        Display("Greetings, I am John.");
        break;
    case cMary:
        Display("Hi there, I am Mary.");
        break;
    default:
        Display("This might be a bug!");
        break;
}
```

In the above example, if the player is cEgo, the game will display "Hello, my
name is Ego." If the player is cJohn, the game will display "Greetings, I am
John." If the player is cMary, the game will display "Hi there, I am Mary." If
the player is none of these characters, the message "This might be a bug!" will
be displayed.

One of the features of a **switch** statement is fall-through. Labels are
ignored once a match is found and indeed execution will continue until the
end of the **switch** block or a **break** statement is encountered.

A **switch** statement that demonstrates this:

```ags
switch (player)
{
    case cJohn:
    case cMary:
        player.Say("I like oranges.")
        break;
    case cEgo:
        player.Say("I like apples.");
    default:
        player.Say("I would like some berries.");
}
```

In the above example, if the player is either cJohn or cMary, s/he will
say "I like oranges.". If the player is cEgo, he will say "I like
apples." and then also "I would like some berries." If the player is any
other character, only the default "I would like some berries." will be
displayed.

Such "fall through" from a non-empty case into the next one is often unintended
(a `break` is missing), so the new script compiler warns when it detects this situation.
To tell the compiler that falling through is *intended*, and suppress the warning,
put a **fallthrough;** statement right in front of the next case:

```ags
switch (inventory_item)
{
case iBlueCup: // no statements inside, falling through, but is fine
case iYellowCup:
    player.Say("I fill the cup with water.");
    break; // break present, all is good
case iVase:
    player.Say("I fill the vase with water.");
    // missing break, will yield a warning
case iBigTowel:
    player.Say("That's my favourite towel. Oh well...");
    fallthrough; // fallthrough keyword tells compiler that it's intended
case iSmallTowel:
    player.Say("The towel is now wet.");
    break;
default:
    player.Say("I think this is useless.");
}
```

*Compatibility:* The `fallthrough` keyword is supported by **AGS 4.0.0** and later versions, and only when using the new script compiler.

A *match_expression* can be any valid AGS expression, including a
function call. The following construction can be useful when implementing
responses to parser values:

```ags
switch (true)
{
    case Parser.Said("take ball"):
        player.AddInventory(iBall);
        break;
    case Parser.Said("drop ball"):
        player.LoseInventory(iBall);
        break;
}
```

In this situation, the *match_expression*s are the results of Parser.Said(). If
*Player.Said("take ball")* is *true*, the ball is added to the player's
inventory. If *Player.Said("drop ball")* is *true*, the ball is removed from the
player's inventory.

---

### while

**while (** *condition* **)** `{`<br>
*statements*<br>
`}`

Runs *statements* continuously, while *condition* is true.

For example:

```ags
while (cEgo.Moving) {
    Wait(1);
}
```

will run the script `Wait(1);` repeatedly, as long as `cEgo.Moving` is
not zero. Once it is zero, the **while** statement will exit at the end
of the loop.

---

### do..while

**do** `{`<br>
*statements*<br>
`}` **while (** *condition* **);**

Similarly to [`while`](ScriptKeywords#while) runs *statements*
continuously, so long as *condition* is true, but unlike **while** it
checks the condition AFTER executing statements, not before. This also
means that the statements will be executed at least once.

For example:

```ags
do
{
    cEgo.Move(cEgo.x + 1, cEgo.y);
}
while (IsKeyPressed(eKeyRightArrow));
```

will run the script `cEgo.Move(cEgo.x + 1, cEgo.y);` once, and then
continue run it repeatedly, as long as the right arrow key is pressed by
player.

---

### for

**for (** \[*initialization*\]**;** \[*condition*\]**;**
\[*iteration*\] **)** `{`<br>
*statements*<br>
`}`

This loop command first performs *initialization* statements, then runs
*statements* inside curved brackets continuously. Each time before
executing these statements it checks whether *condition* is true, and
if not - ends the loop. Each time after statements were executed it
additionally runs *iteration* statements.

*Initialization* is commonly used to declare variables or setting up
existing variable values. If a new variable is declared in
*initialization* - such variable will exist and may be used only inside
the loop. *Iteration* step is usually meant to "move" to the next step,
by changing some variable value. Every part of the command header -
*initialization*, *condition* and *iteration* - is optional: there may
be **for** command without initialization, or without iteration, or even
without conditional expression (in which case loop should be ended with
either [`break`](ScriptKeywords#break) or [`return`](ScriptKeywords#return).

For example:

```ags
for (int i = 0; i < Game.CharacterCount; i++)
{
    Display("My name is %s", character[i].Name);
}
```

will look over every character in game and display their names.

Another example (note missing initialization and iteration):

```ags
for (; cEgo.x < 100;)
{
    Wait(1);
}
```

This will repeat `Wait(1);` until cEgo character does not move beyond
coordinate x = 100.

---

### break

**break**;

`break` statement ends the execution of most inner loop or
[`switch`](ScriptKeywords#switch-case-statements) immediately. After this script
continues running from the next line after loop or switch.

For example:

```ags
while (cEgo.Moving) {
    if (IsKeyPressed(eKeyEscape))
        break;

    Wait(1);
}
```

will run the script `Wait(1);` repeatedly, as long as `cEgo.Moving` is
not zero. If player presses Escape key, the loop is terminated
immediately.

---

### continue

**continue**;

`continue` statement makes the loop skip remaining statements in current
iteration and proceed to the next end-condition check, followed by the
loop restart, if condition is still met, or loop end. If in [`for`](ScriptKeywords#for)
kind of loop, the *iteration* statement is executed right before that.

For example:

```ags
for (int i = 0; i < 100; i++)
{
    // multiple statements here

    if (i > 50)
        continue;

    // more statements following
}
```

will run first part of the loop statements always, and second part only
when `i <= 50`.

---

### function

**function** *name* `(` \[*type1 param1*, *type2 param2*, ... \] `)`

Declares a custom function in your script. A function is a way in which
you can separate out commonly used code into its own place, and thus
avoid duplicating code.

For example, suppose that you quite often want to play a sound and add
an inventory item at the same time. You could write both commands each
time, or you could define a custom function:

```ags
void AddInvAndPlaySound(InventoryItem* item) {
    player.AddInventory(item);
    aInventorySound.Play();
}
```

then, elsewhere in your code you can simply call:

```ags
AddInvAndPlaySound(iKey);
```

to add inventory item *iKey* and play the sound.

Generally, you place your functions in your global script or other script modules. You then need
to add an [`import`](ScriptKeywords#import) line to the respective script header to allow the
function to be called from room scripts.

**Optional parameters**

You can make *int* parameters optional if there is a default value that
the user doesn't need to supply. To do this, change the script header
*import* declaration like this:

```ags
import void TestFunction(int stuff, int things = 5);
```

that declares a function with a mandatory *stuff* parameter, and an
optional *things* parameter. If the caller does not supply the second
parameter, it will default to 5.

**NOTE:** To use optional parameters, you need to have an "import"
declaration for the function in the script header. The default values
cannot be specified in the actual function declaration itself.

---

### return

**return**;

Immediately quits currently run function and returns to the previous
script function current one was called from, if there was any, otherwise
passes execution to engine. **return** can be put in any place in the
the function, no matter if it is inside the if/else statement group,
loop or switch - it will still work as immediate function exit.

If the function is declared with return type other than **void** (or
simply like `function`), then the **return** statement **has** to
specify **return value**.

```ags
int GetHowManyTradeGoodsShopkeeperHas() {
    return 2;
}
```

Alternatively, when function is not supposed to have any return value,
sometimes you may want to break out of current function before it ends
naturally:

```ags
void DoThisAndOptionallyThat(bool do_all) {
    // multiple statements here

    if (!do_all)
        return; // quit the function prematurely

    // more statements following
}
```

---

### struct

**struct** *name* `{`

Declares a custom struct type in your script.<br>
Structs allow you to group together related variables in order to make
your script more structured and readable. For example, suppose that
wanted to store some information on weapons that the player could carry.
You could declare the variables like this:

```ags
int swordDamage;
int swordPrice;
String swordName;
```

but that quickly gets out of hand and leaves you with tons of variables
to keep track of. This is where structs come in:

```ags
struct Weapon {
    int damage;
    int price;
    String name;
};
```

Now, you can declare a struct in one go, like so:

```ags
Weapon sword;
sword.damage = 10;
sword.price = 50;
sword.name = "Fine sword";
```

Much neater and better organized. You can also combine structs with
[arrays](ScriptKeywords#arrays):

```ags
// at top of script
Weapon weapons[10];
// inside script function
weapons[0].damage = 10;
weapons[0].price = 50;
weapons[0].name = "Fine sword";
weapons[1].damage = 20;
weapons[1].price = 80;
weapons[1].name = "Poison dagger";
```

structs are essential if you have complex data that you need to store in
your scripts.

With the new script compiler you may also declare global variables right
after the struct's declaration:

```ags
struct Weapon {
    int damage;
    int price;
    String name;
} sword, dagger; // declares 2 global variables of type Weapon
```

*See also:* [Structs](ScriptStructs)

---

### managed

**managed struct** *name* `{`

**Managed** is a modifier that can be applied to **struct** declaration
to make them managed structs.

Managed structs are special in the way that objects of their types are
created in dynamic pool as opposed to global variables, that exist from
the game start to when the game is shut down, and local variables, that
exist only when their function is run. You cannot declare a variable of
managed struct, but only a pointer variable.

The advantage of such managed (or dynamic) objects is that they are
created only when needed and disposed of when no longer needed. Also,
since you work with pointer to object instead of object itself, you may
assign them to another variable without copying object itself, pass them
to function as parameter, or return from the function.

**IMPORTANT:** in AGS 3.x versions there is a big limitation for
user-defined managed structs, it is that they themselves cannot have
members of pointer types (or dynamic arrays). Since AGS 4.0.0 this
limitation is removed, and managed structs may contain pointers to other
managed objects, dynamic arrays and Strings.

Example:

```ags
managed struct Apple {
    int color;
    int freshness;
};
```

This declares managed struct. To declare a pointer to such struct you
do:

```ags
Apple* my_apple;
```

This creates a pointer variable `my_apple` of managed type `Apple`.
With the new script compiler the `*` may be omitted, and `Apple my_apple;`
will declare a pointer too.

However, this does **not** create an object itself yet, and `my_apple`
is assigned **null** value now. If you try to access struct members
using `my_apple` now, you will get errors. To create an actual object
you need to use a [`new`](ScriptKeywords#new) keyword:

```ags
my_apple = new Apple;
```

The object is now created in the dynamic memory pool, and variable
`my_apple` **points** to it. This lets us access object contents:

```ags
my_apple.color = Game.GetColorFromRGB(255, 0, 0);
my_apple.freshness = 100;
```

You may copy pointer to another variable of same type:

```ags
Apple* apple2 = my_apple;
```

This does **not** copy object itself, only its address in dynamic pool,
meaning both variables - `my_apple` and `apple2` - point to same object!

You may write a function that take such pointer as parameter:

```ags
void DisplayAppleDescription(Apple* apple) {
    String s = String.Format("Apple has color %d and freshness %d", apple.color, apple.freshness);
    Display(s);
}
```

and then call it like:

```ags
DisplayAppleDescription(my_apple);
```

You may write a function that returns pointer to apple:

```ags
Apple* CreateYellowApple(int fresh) {
    Apple* apple = new Apple;
    apple.color = Game.GetColorFromRGB(255, 0, 255);
    apple.freshness = fresh;
    return apple;
}
```

and then use such function just like:

```ags
Apple *my_apple = CreateYellowApple(50);
```

**When does the dynamic object gets destroyed?** After you created
dynamic object as described above, it will exist in memory as long as
*there is at least one pointer variable pointing to it*. As soon as the
last pointer gets destroyed itself (for example, if it was local
function variable, and function ended), or is assigned another object,
or simply assigned `null`, then the dynamic object is removed from your
game forever.

Since AGS 4.0.0 managed structs may also have a constructor, a function that
is called automatically when the object is created with `new`. See
[Constructors](ScriptManagedStructs#constructors).

*See also:* [`new`](ScriptKeywords#new), [Managed Structs](ScriptManagedStructs), [Pointers in AGS](Pointers)

---

### new

*pointer_variable* = **new** *managed_type*;<br>
*pointer_variable* = **new** *managed_type* `(` \[*param1*, *param2*, ... \] `)`;<br>
*array_variable* = **new** *data_type* `[` *size* `]`;

Creates a new dynamic (managed) object of *managed_type* and assigns it
to *pointer_variable*. Also creates [dynamic arrays](DynamicArrays).

If the managed struct has a [constructor](ScriptManagedStructs#constructors),
then the type's name must be followed by brackets with parameters for the
constructor, for example: `new Apple(red, 100)`. If there's no constructor,
then you may either put empty brackets after the type's name, or no brackets at all.
Constructors are supported since AGS 4.0.0, and only with the new script compiler.

Example:

```ags
// Here we declare a managed struct for Apple
managed struct Apple {
    int color;
    int freshness;
};

// ...and declare a global pointer to Apple
Apple* SomeApple;

// At the game start we create a new dynamic object of Apple type
// and assign its address to the pointer variable
void game_start()
{
    SomeApple = new Apple;
}
```

*See also:* [`managed`](ScriptKeywords#managed), [Pointers in AGS](Pointers)

---

### enum

**enum** *name* `{`<br>
*option1* \[ = *value1* \],<br>
*option2* \[ = *value2* \],<br>
...<br>
`};`

Declares an enumeration type. An enumeration allows you to group
together a set of related options, where only one will be true at any
one time -- a bit like the contents of a list box.

For example, if you have a script function, *doStuff*, that can perform
3 different operations, you could do this:

```ags
void doStuff(int param) {
    if (param == 1) {
        // do something
    }
    else if (param == 2) {
        // do something else
    }
    // etc
}
```

but it's hard to read, and when calling the function from elsewhere in
your script, it's not clear what 1 or 2 means. That's where enums come
in:

```ags
enum DoStuffOption {
    BakeCake,
    DoLaundry
};

void doStuff(DoStuffOption param) {
    if (param == BakeCake) {
        // do something
    }
    else if (param == DoLaundry) {
        // do something else
    }
    // etc
}
```

and then the calling code looks like:<br>
`doStuff(BakeCake);`<br>
thus making it perfectly clear what the command will do.

Normally, you would put the enum definition into the script header.

If you don't assign a value for the first enum item, it is assigned the value 1.
Other enum items have the value of the enum before plus 1. Ex: `enum ABC { eA,eB, eC};` get's `eA`=1 `eB`=`eA+1`=2 `eC`=`eB+1`=3, by default.

With the new script compiler you may also declare global variables right after the enum's declaration:

```ags
enum TrafficLightColour {
    red,
    amber,
    green
} col1, col2, col3; // declares 3 global variables of type TrafficLightColour
```

In summary, enums are not an essential part of scripting and you can get
away perfectly well without using them, but in some specific situations
they're very handy.

---

### this

There are two uses for the `this` keyword.

**1. Accessing members of the current struct**

When you are creating custom [structs](ScriptKeywords#struct), you use the "this" keyword
inside member functions to refer to the current struct. For example:

Suppose you had this in your script header:

```ags
struct MyStruct {
    int myValue;

    import void MyMethod();
};
```

Then, in your main script, you could put this:

```ags
void MyStruct::MyMethod()
{
    this.myValue = 5;
}
```

The `MyStruct::MyMethod` tells AGS that you are defining the function
*MyMethod* which belongs to the struct *MyStruct* (the `::` operator
means "belongs to").

The code above will mean that when the MyMethod function is called, it
sets the `myValue` variable to 5.

With the new script compiler `this.` may be omitted, so you may simply write
`myValue = 5;` inside the member function. See
[Struct's member functions](ScriptStructs#member-functions).

**2. Declaring extender functions**

Please see the [Extender functions](ExtenderFunctions) page
for details.

---

### import

**import** [attribute] *declaration* ;

Declares *declaration* as a variable or function which is external to
the current script, but that the script needs access to it. You use this
to provide your room scripts with access to parts of your global script.

For example:

```ags
import int counter;
import int add_numbers (int, int);
```

This imports an integer variable `counter` and the function
`add_numbers` from the global script to enable the current script to
call them. You normally place import statements into the script header
so that all rooms can benefit from them.

In order to import the variable, it must have been exported from the
global script with the [export keyword](ScriptKeywords#export).

**NOTE:** You **MUST** import external variables with the correct type.
If `counter` was declared as a **short** in the global script, you MUST
import it as a short, otherwise your game may crash.

**NOTE:** You cannot import old-style `string` variables (this does not
apply to new-style `String` variables).

As a special case, specifying the optional keyword `attribute` within a struct
definition will define a virtual property, which will also require matching
getter and setter functions.

For example:

```ags
struct Weapon
{
    protected int damage;
    import attribute int Damage;
};
```

Please see the [Object Oriented programming](OOProgramming) page for more
details.

---

### export

**export** *variable* \[, *variable* ... \] ;

Declares that *variable* is exported and is available to access in other scripts,
if declared using the [`import`](ScriptKeywords#import) keyword in those scripts.

For example:

```ags
export my_variable;
export counter, strength;
```

This exports three variables (*my_variable*, *counter*, and *strength*) to be
available for import.

---

### readonly

**readonly** *data_type* *variable* [ = *value* ];

The *readonly* keyword is used when declaring a variable, to indicate that
its value cannot be changed.

For example:

```ags
readonly int zero;
readonly int months = 12;
```

While [`const`](ScriptKeywords#const) defines a compile-time constant, `readonly`
defines a *runtime constant*. This means that its value may be determined at runtime,
and be different each time, but the variable that stores it cannot be modified after
it's initialized. With the new script compiler you may have `readonly` global variables,
local variables and function parameters.

The common use of `readonly` is with function parameters and local variables, as a
"failproof" method to ensure that they are only assigned once, and are never changed
within the function by mistake. For example:

```ags
function MyFunc(readonly int param) {
    readonly int game_speed = GetGameSpeed(); // notice you can assign, but not change later

    param = 10; // compiler will error
    game_speed = 100; // compiler will error
}
```

**NOTE:** `readonly` only tags the variable as not writeable in the current script,
but it does not prevent the engine or another script from modifying it behind the scenes.
For example, you may import a variable as `readonly` in one script module and as not
`readonly` in another.

When applied to struct [attributes](ScriptAttributes#readonly-attributes), `readonly`
means that the attribute can only be read (has a getter, but not a setter).

*Compatibility:* `readonly` local variables and function parameters are supported by
**AGS 4.0.0** and later versions, and only when using the new script compiler.

---

### const

**const** *data_type* *name* = *value*;

The *const* keyword defines a compile-time constant, that is a named value that
always stays the same. Only types which values may be calculated at compile-time
are allowed with `const`, that is: `int` and `float`. The value may be a literal,
or any expression that the compiler can calculate, including other constants.

```ags
const int MainMenuRoom = 301; // main menu room number
const float PI = 3.14159;
const float TWO_PI = PI * 2.0;
const int MAX_ITEMS = 20;
int items[MAX_ITEMS];
```

Unlike macros declared with [`#define`](Preprocessor#define), constants have a type, which
is checked by the compiler.

**NOTE:** there is also a `const string` type, which is a special case: it defines
an unmodifiable string pointer, which may point to any kind of strings: old-style
`string`, new-style `String`, and a char array (e.g. `char text[100];`). This is
mostly used for parameters of built-in functions.

*Compatibility:* `const int` and `const float` are supported by **AGS 4.0.0** and later
versions, and only when using the new script compiler.

---

### writeprotected

**writeprotected** *data_type* *struct_member*;

The *writeprotected* keyword is used to define a property within a struct,
which can only be modified by struct members (including extender functions)
using the [`this`](ScriptKeywords#this) keyword. Reading the value is not
restricted.

For example:

```ags
struct Weapon
{
    writeprotected int Damage;
    import bool SetDamage(int damage);
};
```

---

### protected

**protected** *data_type* *struct_member*;

The *protected* keyword is used to define a property within a struct, which
can only be accessed or modified by struct members (including extender
functions) using the [`this`](ScriptKeywords#this) keyword.

For example:

```ags
struct Weapon
{
    protected int Damage;
    import bool SetDamage(int damage);
};
```

---

### noloopcheck

function **noloopcheck** *function_name* ( *parameters ...* ) `{`

The noloopcheck keyword disables the script loop checking for the
current function.

Normally, if a [`while`](ScriptKeywords#dowhile) loop runs for more than
150,000 loops, AGS will assume that the script has hung and abort the
game. This is to assist scripting since otherwise the game would lock up
if you scripted a loop wrongly.

However, there are some rare situations in which you need a loop to run
several thousand times (for example, when initializing a very large
array). In this case, the *noloopcheck* keyword can be used to stop AGS
aborting your script.

**NOTE:** The *noloopcheck* keyword must be placed between "function"
and the function's name.<br>
If you import the function into the script header, you **do not**
include the "noloopcheck" keyword in the import declaration -- it is
only included in the actual function body.

**NOTE:** If AGS gives you a script iterations error, **DO NOT** just
automatically add this keyword as a way to fix the problem -- more often
than not, it is a fault in your scripting and using this keyword will
mean that the game will hang rather than abort.

For example:

```ags
function noloopcheck initialize_array() {
    char bigarray[200000];
    int a = 0;
    while (a < 200000) {
        bigarray[a] = 1;
        a++;
    }
}
```

without the "noloopcheck" keyword here, AGS would abort that script.
