## Extender functions

Extender functions allow you to "add" new functions to existing types, including built-in AGS types, which lets you call them as if they were part of that type.

Let's look at the following example:

```ags
function Scream(Character *c)
{
    c.Say("AAAAARRRRGGGGHHHHHH!!!!");
}
```

This is a regular function that accepts a Character as a parameter, and makes it say something. You would call this function as:

```ags
Scream(cEgo); // pass cEgo character as a function parameter
```

Now, the *extender function* would instead be declared like:

```ags
function Scream(this Character*)
{
    this.Say("AAAAARRRRGGGGHHHHHH!!!!");
}
```

Notice the [`this`](ScriptKeywords#this) keyword. It means that we are addressing a character, *from which* we called this function.

The extender function may now be called as:

```ags
cEgo.Scream(); // call the function Scream from character cEgo
```

Extender functions exist purely for convenience (in programming this is called "syntax sugar"). They do not add any new abilities, rather than to be able to call the function from an object instead of passing this object as a parameter. They use `this` keyword to address the object they were called from. Besides that, extender functions are just like ordinary functions: they may have multiple parameters, and return some value.

The common use-case for extender functions is writing custom behavior for built-in types. Consider following, more sophisticated example:

```ags
function SayAndAnimate(this Character*, String text, int view, int loop)
{
    this.LockView(view);
    this.Animate(loop, 4, eRepeat, eNoBlock);
    Overlay *bg_speech = this.SayBackground(text);
    while (bg_speech.IsValid)
    {
        Wait(1); // keep updating the game
    }
    this.UnlockView();
}
```
```ags
player.SayAndAnimate("Hello, my name is Roger.", VROGERANIMS, 5);
```

Above will make character run a custom animation and display a speech (using Background speech to keep animation going in this example, but there may be other solutions to this).

**NOTE:** just like with any other functions that your write, remember to declare the function as *import* in the script header, if you like it to be usable in other scripts throughout your game. See ["Importing a function"](ImportingFunctionsAndVariables#exporting-and-importing-a-function) for more details.

### Static extenders

Since AGS 3.4.0 you may also create static extender functions, that is
functions that are called directly from a *type*, rather than an actual *object* of that type.
Static extender declaration is a bit different, for example:

```ags
int AbsInt(static Maths, int value)
{
    if (value < 0)
        return -value;
    return value;
}
```

and you use such function as:

```ags
int x = Maths.AbsInt(-10);
```

### Extenders and function overloading

Function overloading is an ability of a scripting language to have multiple functions of same name but with different parameters. AGS script does not support function overloading at this moment, but when using extender functions you may have multiple extenders of the same name attached to different types. This is possible, because compiler treats such functions as members of respective structs, and it is allowed to have identically named functions in different structs.

For example, you may declare functions like this:

```ags
function PickUp(this Object*)
{
    // some code here
}

function PickUp(this Hotspot*)
{
    // some code here
}
```
