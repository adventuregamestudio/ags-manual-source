## Structs

A struct (or "structure") is a way to group variables and functions together in one script object. The primary purpose of struct is better and more convenient organization of data.

A struct is declared as:

```ags
struct MyStruct
{
    // variable declarations
    int Variable;
    int SecondVariable;

    // function import declarations
    import void FunctionInsideStruct();
    import int  AnotherFunction(int param1, int param2);

    // attribute import declarations
    import attribute int Property;
};
```

Struct's variables are called "member variables", and functions - "member functions". Another traditional name for struct's functions is "methods", this term comes from the Object Oriented Programming concept.

The struct's contents are purely optional: you may have structs with only variables in it, or structs with variables and functions, or ones with only functions. You also may have an empty struct, except it will be practically useless.

If you have a struct with *at least one variable* in it, you may then create struct's "instances": these are variables that have this struct as a type. For example:

```ags
MyStruct s1;
MyStruct s2;
```

Above creates two variables of type `MyStruct`: s1 and s2.

Accessing struct's members is done using operator dot (`.`), like this:

```ags
s1.Variable = 10;
Display("Variable is now: %d", s1.Variable);
```

You may also have arrays of struct instances:

```ags
MyStruct lotsOfMyStructs[100];
```

### Member functions

Struct's functions must be declared with an `import` keyword, but the actual function code cannot be put inside the struct. Instead they have to be written outside the struct, below the struct's declaration. Note that if the struct is declared in a script header (`*.ash` file), then normally their functions are written in a script's body (`*.asc` file): that is a common practice for functions declared in headers (see [Exporting and importing a function](ImportingFunctionsAndVariables#exporting-and-importing-a-function)).

When writing a function's body, you have to use special syntax for its name, which includes both struct's name and function's name separated with a double colon symbol (`::`), like this:

```ags
void MyStruct::FunctionInsideStruct()
{
    // function's code here
}

int MyStruct::AnotherFunction(int param1, int param2)
{
    // function's code here
}
```

Struct's functions can have and do everything that regular functions can. They may have parameters and return values, have local variables, access global variables, call other functions, and so forth.

Besides that, member functions can access struct's own members using "this" keyword. "This" is a reference to the struct instance which this function was called for. For example:

```ags
void MyStruct::FunctionInsideStruct()
{
    this.Variable += 2; // increase variable by 2
    Display("My Variable is now: %d", this.Variable);
}
```

Now, let's say you have 2 variables of type MyStruct, and you call this function from both of them:

```ags
MyStruct s1;
MyStruct s2;

s1.FunctionInsideStruct();
s2.FunctionInsideStruct();
```

First time this function is called for variable s1, and so its code will have "this" reference pointing to "s1" object, and will therefore modify and display s1.Variable. The second time its called for variable s2, in which case "this" in its code will reference "s2" object, and s2.Variable will be modified.

Similarly you can call another struct's function from this function:

```ags
void MyStruct::FunctionInsideStruct()
{
    this.AnotherFunction(10, 20); // call another function with some parameters
}
```

Notice how we're using "this" reference here, and this "this" will be propagated further into the called function, meaning that the called function will be accessing same struct instance.

### Static functions

There are two types of member functions: regular and static. Regular functions reference particular struct instance and may use its variables in the code (we observed them in a section above). Static functions do not reference any particular struct instance, but the *struct as a type*. They cannot access normal struct variables in their code, and may only access other static functions or static attributes.

Static functions are declared with `static` keyword, like this:

```ags
struct MyStruct
{
    import static void StaticFunction(int param);
};
```

Static keyword must also be present when defining the function's body:

```ags
static void MyStruct::StaticFunction(int param)
{
    // function's code here
}
```

Static member functions are called differently from regular ones, they are called "for the type" rather than "for the object":

```ags
MyStruct.StaticFunction(50);
```

The script syntax allows to call a static function using the object too, but that does not make much sense and will have no difference compared to calling it from type name.

"This" reference cannot be used inside static function's code, as such function is not called from particular instance so it cannot refer to one.

Since static functions cannot access normal struct's data, you way wonder how these may be useful.

First of all, there may be a case when the function is working only with the input parameters and produces result without needing any persistent struct's data. A good example is Math struct, which groups various math algorithms, which only calculate the result based on input values.

Another example is when you have a struct as a "facade" for functionality hidden inside the script. In such case struct's functions may still access variables declared inside the script.

### Member modifiers

There are additional modifiers that you may apply to struct's members:

* `static` - we covered this modifier in detail in a section above. This modifier is applied to functions and attributes, and tells that they are called "from the type" itself rather than "from struct instance". Note that in AGS variables cannot be static, that's not supported by the script compiler at the moment.
* `protected` - this modifier is applied to variables, attributes or functions, which makes them unusable from any external code. Only this struct's member functions can read, modify or call such members.
* `writeprotected` - this modifier is applied to variables and attributes, and lets *read* them from outside of the struct, but not modify their values. Member functions can still read and modify them normally.
* `readonly` - this modifier is applied to attributes, and makes them only return a value, but not set one. The use case is when the struct should have a property that can only tell something about it, but does not let change anything. Technically, "readonly" may be applied to variables too, but that makes them useless as there will be no way to set their values.

These modifiers may be used in any combinations, except `protected` and `writeprotected` cannot be used together. So you may have, for example, a "static protected readonly attribute".

### Examples

Let's imagine that you need to describe a rectangle: a shape which has a position and a size, - and then store a number of rectangles in your game script. That's a good case for declaring a struct:

```ags
struct Rectangle
{
    int x;
    int y;
    int width;
    int height;
};
```

With this you may declare any number of Rectangle variables, and even array of Rectangles:

```ags
Rectangle firstRect;
Rectangle secondRect;
Rectangle shapes[20]; // 20 rectangles
```

Initializing a rectangle will look like this:

```ags
Rectangle rect;
rect.x = 10;
rect.y = 20;
rect.width = 100;
rect.height = 50;
```

Now, if we have a lot of rectangles initialized like this throughout the game script, writing such code may become tad tedious, and also prone to oversights (what if you forget to initialize a width or a height?). We may solve this by writing a member function that lets us initialize whole struct at once:

```ags
struct Rectangle
{
    int x;
    int y;
    int width;
    int height;
    
    import void Init(int x, int y, int w, int h);
};
```
```ags
void Rectangle::Init(int x, int y, int w, int h)
{
    this.x = x;
    this.y = y;
    this.width = w;
    this.height = h;
}
```

Now a rectangle may be initialized by calling its Init function:

```ags
Rectangle rect;
rect.Init(10, 20, 100, 50);
```

You may still access its variables directly too.

Now, this seems to be fine, but there's a problem: it's possible to initialize a rectangle using invalid values, for instance - by assigning negative width or height. We may fix this in Init function by adding respective checks, like:

```ags
void Rectangle::Init(int x, int y, int w, int h)
{
    this.x = x;
    this.y = y;
    if (w >= 0)
        this.width = w;
    else
        this.width = 0;
    if (h >= 0)
        this.height = h;
    else
        this.height = 0;
}
```

(Let's assume that we allow width or height = 0, which would mean that rectangle has empty size.)

Above fixes the problem in Init function, but unfortunately we still have struct's variables exposed, and these may be changed at will to any invalid value.
Let's prevent these from random modification by making them `writeprotected`. Then the external code may still read them, but not modify them. And in case we want to modify them after the first initialization, let's also add more functions:

```ags
struct Rectangle
{
    writeprotected int x;
    writeprotected int y;
    writeprotected int width;
    writeprotected int height;
    
    import void Init(int x, int y, int w, int h);
    import void SetPos(int x, int y);
    import void SetSize(int w, int h);
};
```
```ags
void Rectangle::SetPos(int x, int y)
{
    this.x = x;
    this.y = y;
}

void Rectangle::SetSize(int w, int h)
{
    if (w >= 0)
        this.width = w;
    else
        this.width = 0;
    if (h >= 0)
        this.height = h;
    else
        this.height = 0;
}
```

We may even modify Init function, making it call these functions, so to reduce code duplication:

```ags
void Rectangle::Init(int x, int y, int w, int h)
{
    this.SetPos(x, y);
    this.SetSize(w, h);
}
```

What else to add? Suppose that we want to tell if our rectangle is empty (has no size):

```ags
struct Rectangle
{
    //.... the rest of the struct contents here
    
    import bool IsEmpty();
}
```
```ags
bool Rectangle::IsEmpty()
{
    if (this.width == 0 || this.height == 0)
        return true;
    else
        return false;
}
```
```ags
Rectangle rect;
rect.Init(0, 0, 0, 0);
if (rect.IsEmpty())
{
    Display("Well, darn...");
}
```

But maybe we also want to have a way to check if input arguments are valid before initializing Rectangle. This operation does not need actual rectangle object, so it will be suitable to make such function "static":

```ags
struct Rectangle
{
    //.... the rest of the struct contents here
    
    import static bool AreParamsValid(int x, int y, int w, int h);
}
```
```ags
static bool Rectangle::AreParamsValid(int x, int y, int w, int h)
{
    // x and y are always valid in this example, so only test the size
    if (w < 0 || h < 0)
        return false;
    else
        return true;
}
```

And just an example of how this function could be used:

```ags
String xstr = Game.InputBox("Input rectangle's x pos:");
String ystr = Game.InputBox("Input rectangle's y pos:");
String wstr = Game.InputBox("Input rectangle's width:");
String hstr = Game.InputBox("Input rectangle's height:");
int x = xstr.AsInt;
int y = ystr.AsInt;
int w = wstr.AsInt;
int h = hstr.AsInt;
if (!Rectangle.AreParamsValid(x, y, w, h))
{
    Display("Invalid parameters!");
}
```

See Also: [Object Oriented Programming](OOProgramming)