## Functions

In AGS script a function is a sequence of script commands grouped under one name. In AGS script all commands must be inside functions, so game scripts are running functions all the time. The script functions may be run by the engine's command on certain events, but may also be run by other functions.

A function has *name*, *parameter list* and a *return value type*. It's declared in script using this syntax:

```ags
type name (parameters)
```

Function's name is any name consisting of permitted characters: basic latin letters, digits and a underscore character ('_'). This name is used to identify this function and distinguish from others.

Function's type (or "return type") determines which result this function returns when it finishes executing. This may be any known type supported by script, similar to the type of variables: `int`, `bool`, `String`, and any other, either standard and custom. If a function does not need to return any result, there's a special type for such occasion called `void` (means: "nothing"). Finally, there's a historical keyword `function` that may be used if you don't care about proper return type, although we still recommended you to designate a correct one. Functions declared with `function` are allowed to return an integer, or not return anything.

Function's parameter (also called "arguments") are values that are passed into the function when it's run from elsewhere. A function can have no parameters, in which case its name is followed by empty brackets (`()`), it may have a single parameter, or multiple ones, in which case the parameters are separated by commas. Each parameter must be declared just like a variable, with type and name.

All the commands inside the function must be enclosed into the curved braces (`{` and `}`). Everything in between is considered function's "body", and will be executed when this function gets run.

Following are examples of function declarations.

A function with default `function` type and no parameters:

```ags
function DoSomething()
{
}
```

A function which returns nothing, and has a single integer parameter:

```ags
void GiveMeInt(int value)
{
}
```

A function which returns an integer, and has 2 integer parameters:

```ags
int SumTwoNumbers(int a, int b)
{
}
```

A function that returns a String and has a `Character*` parameter (a pointer to a Character object):

```ags
String GetMyName(Character* c)
{
}
```

### Function's body

Function insides (also called "function's body"), marked by the opening and closing braces (`{` and `}`) is where you put all the script commands you want it to perform. The commands are executed in the same order as they are written.

For example:

```ags
function DoSomething()
{
    player.Walk(100, 50, eBlock);
	player.Say("Well, that was easy!");
}
```

Here the player character will walk to the new place and say something.

The sequence of commands does not have to be linear. It may include conditional branches and loops, all according the [supported script syntax](ScriptKeywords).

```ags
function DoSomethingOrElse()
{
    if (player.x == 100 && player.y == 50)
	{
	    player.Say("I'm already here.");
	}
	else
	{
		player.Walk(100, 50, eBlock);
		player.Say("Well, that was easy!");
	}

	player.Say("So, what I am supposed to do now?");
}
```

Here the function will execute either the part under "if" or part under "else", depending on how the result of condition: whether the character is already standing at the destination or not. After the conditional part ends, the function will continue normally, and may have more commands. The last speech command will execute always.

### Exiting a function

The closing figure brace (`}`) acts as the natural function's exit. But functions can be exited at any point using `return` keyword. If a function needs to exit only once it reaches the end, then `return` may be omited, unless it also must return a value (see explanation in the next section below).

```ags
function DoSomething()
{
    player.Walk(100, 50, eBlock);
	player.Say("Well, that was easy!");
	// the 'return' keyword here is correct syntax, but is not necessary,
	// because the function exits immediately after anyway
	return;
}
```

Sometimes it may be required to return from a function earlier, before it reaches the end. The common case is when you need to do something, or not do something depending on condition.

```ags
function DoSomethingOrElse()
{
    if (player.x == 100 && player.y == 50)
	{
	    player.Say("What do you know, I'm already here.");
		return;
	}

    player.Walk(100, 50, eBlock);
	player.Say("Well, that was easy!");
}
```

This is another variant of the previously demonstrated scenario, where character should not walk if it's already standing at destination. In this variant the function will exit (`return`) immediately after finding out that there's no need to walk anywhere. Which variant to use (one with if/else blocks, or one with early exit) is a matter your goals and preferred style.

### Calling a function

Some functions are run by the engine on certain events, such as entering a new room or a key press. Engine does this by searching for functions with predefined names in your script. More about this can be learnt in ["Global event handlers"](Globalfunctions_event).

But what if you want to run a function from another function in script? This is called "calling a function", and is done using following syntax:

```ags
name (parameter values)
```

If function has no parameters, then empty brackets are used, but having brackets is required always.

Calling a function without parameters:

```ags
DoSomething();
```

Calling a function with one parameter, requires passing a value for that parameter:

```ags
GiveMeInt(10);
```

Calling a function with multiple parameters, requires to pass matching number of values, separated by commas:

```ags
SumTwoNumbers(10, 20);
```

Following is an example of script where one function calls another function:

```ags

function DoSomethingElse()
{
    Display("You wave your hands, hoping to bring attention to yourself");
}

function DoSomething()
{
    player.Walk(100, 50, eBlock);
	player.Say("Well, that was easy!");
	DoSomethingElse();
}
```

You may have realized this already, but standard script commands, such as Walk, Say, Display and so forth, are also functions. Their only difference is that they are defined not in game script, but by the AGS engine. Other than that, your own functions and these standard ones are used in exactly same way.

**IMPORTANT:** in AGS script you can only call a function if it's declared earlier than you call it. That means: above in the same script, or in another script module placed higher in the list of scripts.

See also: [Importing functions in other scripts](ImportingFunctionsAndVariables)

### Function's parameters

Functions may require parameters. This allows same function to have different result depending on case. Let's investigate a trivial example. Suppose we have a function:

```ags
function Sum10And20AndDisplayResult()
{
    int result = 10 + 20;
	Display("%d", result);
}
```

This function will calculate a sum of 10 and 20 and display the result on screen. But this seems silly, why have a function that can only sum two particular numbers? What if we need to sum different numbers, should we write a separate function for each case? Function parameters solve this problem better:

```ags
function SumTwoNumbersAndDisplayResult(int a, int b)
{
    int result = a + b;
	Display("%d", result);
}
```

Here we declared a function that has 2 parameters: an integer `a` and an integer `b`. Notice how they are declared practically like variables, with obligatory type and name. These parameters will be known under these names within the function, and can be used in its work.

When calling such function, we must pass values for each of its parameters. These values must follow the strict order, just like they are declared in function, and match the types: you cannot pass a String if a function requires an integer, for instance. But you can pass either a direct value, or by value stored in a variable:

```ags
// This calls the function, passing literal values 10 and 20
SumTwoNumbersAndDisplayResult(10, 20);

int x = 30;
int y = 40;
// And here we pass values from existing variables x and y;
// it works just the same
SumTwoNumbersAndDisplayResult(x, y);
```

Furthermore, AGS script allows to perform any kind of calculative expression when passing a value into the function:

```ags
// This will perform the arithmetic operation, and pass result into the function
GiveMeInt(10 + 20 / 4);

int x = 30;
int y = 40;
// Works with variables just the same
SumTwoNumbersAndDisplayResult(x * 2, y / x + 10);
```

Of course, same methods may be used with standard AGS commands. You probably have already noticed the examples of these earlier in this topic and in other topics in this manual, but we will demonstrate here just in case:

```ags
player.Say("This is a literal string passed into the function Character.Say");

String s = "Make a variable 's' and assign this text, then pass to Say";
player.Say(s);

// Sum player character's x and y coordinates (just for example)
SumTwoNumbersAndDisplayResult(player.x, player.y);
```

Which *types* may be the function's parameters be? AGS has certain restriction in this regard: not everything that can be a variable can be a function parameter.

Function's parameters can be:
 - Any simple numeric type: `int`, `float`, `char`, `short`, `bool`.
 - Any managed pointer type: `Character*`, `Object*`, and so forth, as well as pointers of your own [`managed structs`](ScriptManagedStructs).
 - `String` type, which is secretly also a managed pointer type (because String is a managed object).

Function's parameters cannot be:
 - Regular non-managed [structs](ScriptStructs): their instances cannot be passed into the function, as AGS script does not support copying them.
 - Functions.

### Optional parameters

Sometimes you may want to give function's parameter a *default value*, which is used if no value is passed for that parameter. Such parameters are known as *optional parameters*. They are "optional" in the sense that you may skip them when calling a function, but they will still always exist inside the function's body. There's a important restriction: only *latest parameters* in the function's parameter list can be made optional. Multiple parameters can be optional, but only if there's no non-optional parameter between them or after them.

The parameter is made optional by assigning a default value to them right in the function's declaration.

For example:

```ags
function DoSomething(String str_param, int a = 10, int b = 20)
{
}
```

This function has a obligatory parameter `str_param`, and two optional parameters `a` and `b`. This function may be called by passing either 1, 2 or all 3 of its parameter values. Each optional parameter, which value was not passed, will assume its default value instead:

```ags
// assumes a = 10, b = 20
DoSomething("text");
// assumes b = 20
DoSomething("text", 30);
// all parameters are passed explicitly
DoSomething("text", 30, 40);
```

### Returning values

A function can return a value. This may be a result of its calculation, retrieved data, or an indication of its success or failure to do something. The return value must have a *type*, but does not have a name. It can be of same range of types that function parameters can be, but also can be `void` if nothing is returned.

When declaring a function, the type of its returned value is placed before function's name (instead of keyword `function`). When this is done, the function can (actually - should) use `return` command to tell what value it returns.

```ags
int SumTwoNumbers(int a, int b)
{
    return a + b;
}
```

This function calculates the sum of two integer parameters and returns the result. As the function is declared as `int`, this means we should expect it to return a value of that type. Notice that `return` command is followed by expression that results in a integer.

The function must return a value of its type whenever a `return` command is used within. It may return *different* values in different cases, but they all must be of the same type. For example:

```ags
bool IsPlayerStandingAtPosition(int x, int y)
{
    if (player.x == x && player.y == y)
        return true;
    else
        return false;
}
```

The `return` expression can be virtually anything: a literal number, a variable, arithmetic operation, even another function's call; but in the end the final value should be of correct type:

```ags
int SumTwoNumbersAndDivideByThird(int a, int b, int c)
{
    return SumTwoNumbers(a, b) / c;
}
```

The function that has a return type of anything besides `void` can have its return value assigned to a variable, or used in a expression, just like any regular value. This is done by calling a function:

```ags
// Variable 'sum' will have a result of SumTwoNumbers
int sum = SumTwoNumbers(10, 20);

// Variable 'sum2' will have a result of arithmetic operation,
// where result of SumTwoNumbers is multiplied by 2
int sum2 = SumTwoNumbers(10, 20) * 2;

// We can do any kind of calculative expression with the function calls
int weird_number = (SumTwoNumbers(10, 20) * 2) + (SumTwoNumbers(30, 40) / 5) - 10;
```

The `void` type is special, it means that function returns no value. This is a preferred contemporary way to declare a function that does not return any result, instead of a old classic `function`. Please note that even though the function is declared with `void` type, it may still use `return` command whenever necessary, just without any further expression.

To summarize, function's return types can be:
 - `void`, which means no return value.
 - `function`, which is a old-style way to say that function may either return nothing, or return integer (not recommended to use).
 - Any simple numeric type: `int`, `float`, `char`, `short`, `bool`.
 - Any managed pointer type: `Character*`, `Object*`, and so forth, as well as pointers of your own [`managed structs`](ScriptManagedStructs).
 - `String` type, which is secretly also a managed pointer type (because String is a managed object).

Function's return types cannot be:
 - Regular non-managed [structs](ScriptStructs): their instances cannot be passed out of the function, as AGS script does not support copying them.
 - Functions.

### Struct's functions

So far we've been talking about regular functions that are just part of the script. But functions can be part of the `struct` too. For example: `Character` struct has functions like [`Walk`](Character#characterwalk) or [`Say`](Character#charactersay), `Game` struct has function [`DoOnceOnly`](Game#gamedoonceonly), and so forth.

If you write your own structs, you may as well add functions to them. For the detailed explanation please see [Structs](ScriptStructs);
