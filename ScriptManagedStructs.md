## Managed Structs

Managed struct is a special kind of [struct](ScriptStructs), which instances are allocated not in a regular script memory but in the *dynamic memory pool*, accessed using *pointer* variables, and are *reference counted*. The explanation follows.

When an instance of a regular struct is created by declaring a variable of its type, such instance is allocated either in the global script memory or in a local script memory (in functions). Anything created in a global script memory exists since game start to the game's shutdown. Variables created in a local memory exist until the current function ends.

Managed objects are created in the dynamic memory pool, where they stay as long as you need them to, and may be deleted when no longer needed.

As managed objects are stored in the dynamic memory, they cannot be at the same time stored in regular variables. Instead you are using a ["pointer" type variable](Pointers) to access them. In simple words, "pointer" is a variable that stores not the object itself, but a reference to an object. You may think of it as a "link" to an object. The interesting part here is that a pointer is not a exclusive reference to the object: same managed object may be referenced by multiple pointer variables, and the same pointer variable may be assigned a different object's reference over time. That's like switching a link from one object to another. Pointers may be assigned special value called "null", in which case they point "nowhere". Assigning a pointer to another pointer will copy the reference, making both pointers share a link to the same object.

Engine has to take precaution against deleting a managed object while there are any pointer variables which have it assigned. Would an object get deleted while there are still pointers referencing it, these pointers will point to an invalid space in memory. This problem is solved by a internal system called "reference counting". "Reference count" is a number of active references to a managed object. Whenever you assign an object to a pointer variable, a reference count increases by 1. Whenever you remove such reference, by either replacing with different object, or assigning a "null", a reference count decreases by 1. When reference count becomes 0 the object gets deleted automatically.

There are couple of logical consequences of reference counting.
One is that you must remove all references in order to delete the object. If you have multiple pointers referring the same object, then you must set them all to "null" before it gets deleted. If at least one pointer keeps this reference, the object will be staying in memory.
Another thing is that if you assign the managed object to the only pointer variable, and that is a local variable inside a function, then such variable will get removed when the function ends, and managed object will get deleted automatically. This is handy when you need it to stay only for the duration of a function, but if you want it to stay afterwards, then you will have to either return the pointer from function (if it's called from another function), or copy the reference to a global pointer variable.

Speaking of copying pointers, pointers are one of the few types in AGS script that may be copied (other being trivial types such as `int`, `float`, `char` and `bool`). This makes it possible to use them as function parameters, and function's return values. Which in turn lets pass or return a reference to a managed object, which is a very convenient ability.

**IMPORTANT:** In AGS 3.x versions there's a restriction that makes managed structs somewhat limited (compared to a similar kind of objects in other programming languages). They cannot contain any pointer variables themselves, and this includes dynamic arrays and Strings (which are also secretly pointers). This restriction is removed since **AGS 4.0.0**: managed structs may now contain pointers to other managed objects (including pointers to their own type), dynamic arrays and Strings. See [Pointers to other managed objects](ScriptManagedStructs#pointers-to-other-managed-objects) below.

### Struct's declaration and creating objects

Managed structs are declared similar to regular structs, but with a `managed` keyword, like this:

```ags
managed struct ManagedStruct
{
    // any kind of struct's members
    
    int VarA;
    int VarB;
    
    import void MemberFunction();
    import attribute int SomeProperty;
};
```

Managed structs may contain variables, functions and attributes, just like regular structs, there's no difference in this regard.

Unlike regular structs, you do not declare a variable of a managed struct though, but a pointer TO managed struct (notice a pointer declaration using `*` symbol):

```ags
ManagedStruct* ms;
```

Above does not create an instance of a struct, only a pointer! No object exists yet, and a new pointer is assigned "null".

Since managed structs may only be accessed through pointers, the new script compiler lets you omit the `*` symbol in such declarations, and will understand that this is a pointer anyway. So following is the same as above:

```ags
ManagedStruct ms; // still a pointer, same as "ManagedStruct* ms;"
```

*Compatibility:* Omitting `*` is supported by **AGS 4.0.0** and later versions, and only when using the new script compiler.

An instance of a managed struct is created using a [`new` keyword](ScriptKeywords#new), like this:

```ags
ManagedStruct* ms;
ms = new ManagedStruct;
```

This instructs the engine to create an object of type ManagedStruct and assign its reference to the pointer variable `ms`. If `ms` is a local function variable, you may then combine declaration and object creation:

```ags
ManagedStruct* ms = new ManagedStruct;
```

But that won't work for global pointers, because AGS script cannot execute a `new` command outside of a function.

In any case, after the object was created, and assigned to a pointer, you may access it using that pointer. This looks quite similar to using the regular struct variables:

```ags
ManagedStruct* ms = new ManagedStruct;
ms.VarA = 10;
ms.MemberFunction();
```

Finally, when you no longer need an object, you need to "nullify" all pointers that reference it, like:

```ags
ms = null;
```

**NOTE:** usually you don't have to nullify a pointer declared inside a function, because it will get removed when the function exits. But you may assign "null" by hand if you need to destroy the object earlier for some reason.

### Constructors

A managed struct may have a *constructor*: a special member function that is called automatically whenever a new object of this struct is created with `new`. Constructors are convenient for initializing new objects, and also make sure that an object is never created without proper initialization.

A constructor is declared as a member function that has a name identical to the struct's name:

```ags
managed struct Apple
{
    int Color;
    int Freshness;

    import void Apple(int color, int freshness); // a constructor
};
```
```ags
void Apple::Apple(int color, int freshness)
{
    Color = color;
    Freshness = freshness;
}
```

When the struct has a constructor, `new` must be followed by a list of parameters in brackets, and these parameters are passed into the constructor:

```ags
Apple* apple = new Apple(Game.GetColorFromRGB(255, 0, 0), 100);
```

Following rules apply to constructors:

* A constructor must have the same name as its struct, and must return `void`.
* A constructor may have any parameter list, just like a regular function.
* A constructor cannot be `static` or `protected`.
* A constructor cannot be declared as an [extender function](ExtenderFunctions).
* A constructor cannot be called directly in script, it is only called by `new`.
* A struct inherits the constructor of its parent struct (see [Extending structs](ScriptStructs#extending-structs)), but declaring its own constructor will completely "hide" the parent's one.

The parameters passed to `new` must match the constructor's parameter list. If neither the struct nor any of its parents has a constructor, then you may create it either with an empty parameter list, or without brackets at all, the old way:

```ags
managed struct Stx
{
    int Payload;
    import void Stx(int payload);
};

void Stx::Stx(int payload)
{
    Payload = payload;
}

managed struct Sty
{
    short Payload;
};

function game_start()
{
    Stx *st1 = new Stx(7);      // okay
    Stx *st2 = new Stx(7.0);    // error, parameters don't match
    Stx *st3 = new Stx();       // error, parameter expected but not found
    Stx *st4 = new Stx;         // error, parameter list missing

    Sty *st5 = new Sty();       // empty list is okay, nothing will be called
    Sty *st6 = new Sty;         // okay
    Sty *st7 = new Sty(4);      // error, no constructor declared, so cannot pass parameters
}
```

*Compatibility:* Constructors are supported by **AGS 4.0.0** and later versions, and only when using the new script compiler.

### Pointers to other managed objects

Since AGS 4.0.0 managed structs may have members of pointer types, that is: pointers to other managed structs (both built-in and your own), dynamic arrays and Strings. This lets managed objects reference each other, and allows to create virtually any kind of data storage in script, such as lists, trees, graphs, and so forth.

For example:

```ags
managed struct Item; // pre-declaration, lets use Item before its full declaration

managed struct Person
{
    String Name;
    Character* GameCharacter;
    Item* Items[];   // dynamic array of pointers to Item
};

managed struct Item
{
    String Name;
    Person* Owner;
};
```

A managed struct may even contain a pointer to its own type, which is how linked lists are commonly made:

```ags
managed struct ListNode
{
    int Value;
    ListNode* Next;
};
```

Managed structs may also contain regular (non-managed) structs as members, see [Nested structs](ScriptStructs#nested-structs).

*Compatibility:* Pointers inside managed structs are supported by **AGS 4.0.0** and later versions.

### Casting pointers

If one managed struct [extends](ScriptStructs#extending-structs) another, then a pointer to a "child" type may be assigned to a pointer of a "parent" type directly. The opposite is not always valid, because a pointer of a parent type may refer to an object of a different child type. For such case there's an `as` operator, which checks whether the object is of the requested type, and returns either a pointer of that type, or `null` if the object is of a different type:

```ags
managed struct Animal
{
    String Name;
};

managed struct Dog extends Animal
{
    int Loyalty;
};
```
```ags
Animal* animal = new Dog; // a child type pointer may be assigned to a parent type
Dog* dog = animal as Dog; // cast parent type pointer back to a child type
if (dog != null)
{
    dog.Loyalty = 100;
}
```

The same works with built-in types, for example with [GUIControl](GUIControl) and its child types:

```ags
GUIControl* someControl = gui[0].Controls[0];
Button* aButton = someControl as Button; // will be null if the control is not a Button
```

*Compatibility:* The `as` operator is supported by **AGS 4.0.0** and later versions, and only when using the new script compiler.

### Passing object's reference

As we explained earlier, the pointers may be copied, so you can do this:

```ags
ManagedStruct* another_pointer = ms;
```

This will copy a reference from a previous pointer `ms` to `another_pointer`. This does **NOT** copy the object, but only its reference. Both pointers will now reference same object. If you want a second object, you should be using `new` command again:

```ags
another_pointer = new ManagedStruct;
```

Pointers may be passed as function parameters, for example:

```ags
function GiveMePointers(ManagedStruct* ms1, ManagedStruct* ms2)
{
}
```
```ags
// call the function, passing two pointers
GiveMePointers(ms, another_pointer);
```

Pointers may be returned from a function as well, for example:

```ags
ManagedStruct* MakeObject(int init_value)
{
    ManagedStruct* ms = new ManagedStruct;
    ms.VarA = init_value; // initialize new object with some values
    return ms;
}
```
```ags
// call the function, save its returned object reference in another pointer
ManagedStruct* obj = MakeObject(50);
```

### Arrays of managed objects

It is possible to store *pointers* to managed objects in an array, both regular (fixed-size) and dynamic array. You only have to remember that in such case array's elements will be *pointers*, not objects themselves, and so when you create an array - that will be an array of pointers, each of them initialized with "null". Creating objects for this array will require extra steps.

An example with fixed-size array:

```ags
// This is array of 10 pointers
ManagedStruct* fixed_arr[10];

// Here we created a new object and assigned to array element 0
fixed_arr[0] = new ManagedStruct;

// Let's use a loop to make things faster,
// and fill array with objects
for (int i = 0; i < 10; i++)
{
    fixed_arr[i] = new ManagedStruct;
}
```

An example with dynamic array will be almost the same, except dynamic array itself has to be created first (right, dynamic array is also a managed object!):

```ags
// This is a dynamic array's reference, it's null and there's no real array yet
ManagedStruct* fixed_arr[];

// Here we created a dynamic array of size 10;
// it's full of null-pointers! no actual ManagedStruct objects created yet
fixed_arr = new ManagedStruct[10];

// Let's use a loop to fill array with objects
for (int i = 0; i < 10; i++)
{
    fixed_arr[i] = new ManagedStruct;
}
```

See also: [Dynamic Arrays](DynamicArrays)

### Built-in managed structs

Besides the managed structs that you may write in your game script, there is a collection of structs declared by the engine itself. These define standard game objects, such as Character, GUI, InventoryItem, and so forth.
These structs are declared with a special modifier keyword: `builtin`. This modifier prevents extending a struct, and prevents creating struct's instance using `new` command.
This is done so because these objects have internal parts hidden from script, and are stored in certain special way in the engine's memory, so neither they, not their descendants cannot be trivially created in script. Only engine is allowed to create these, either at game's startup, or when you call certain functions (for example: [`Overlay.CreateGraphical`](Overlay#overlaycreategraphical) creates a `Overlay` managed object and returns a pointer to it).

You should not be using `builtin` keyword yourself. That's not forbidden, but will do no good, as it will prevent you from working with your own managed struct.

### Examples

Let's take a `Rectangle` struct from the [original example](ScriptStructs#examples) and turn it into the managed struct:

```
managed struct Rectangle
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

Creating a rectangle will look a little different now:

```ags
Rectangle* rect = new Rectangle;
rect.Init(10, 20, 100, 50);
```

Because functions can return a pointer, could not we make a function that returns already initialized rectangle? We can! but the function has to be [**static**](ScriptStructs#static-functions), because it's not going to be called from an existing object:

```ags
managed struct Rectangle
{
    //.... the rest of the struct contents here
    
    import static Rectangle* Create(int x, int y, int w, int h);
};
```
```ags
static Rectangle::Create(int x, int y, int w, int h)
{
    Rectangle* rect = new Rectangle;
    rect.Init(x, y, w, h);
    return rect;
}
```

**NOTE:** in programming such functions are also called "factory methods".

**NOTE:** Since AGS 4.0.0 you may use a [constructor](ScriptManagedStructs#constructors) for the same purpose instead of the `Init` function: declare `import void Rectangle(int x, int y, int w, int h);` in the struct, and then create rectangles as `new Rectangle(10, 20, 100, 50)`. Static "factory" functions are still useful when creating an object may fail, and you want to return `null` in such case.

And now we can create rectangles like this:

```ags
Rectangle* rect = Rectangle.Create(10, 20, 100, 50);
Rectangle* rect2 = Rectangle.Create(20, 40, 200, 100);
Rectangle* rect3 = Rectangle.Create(-40, 10, 50, 50);
```

Now, since we have pointers, we may pass them into other functions as well. Let's write a function that displays rectangle's variables as a text message:

```ags
void PrintRectangle(Rectangle* rect)
{
    Display("Rectangle:\n  x = %d\n  y = %d\n  width = %d\n  height = %d",
             rect.x, rect.y, rect.width, rect.height);
}
```

This function may be used with any rectangle:

```ags
PrintRectangle(rect);
PrintRectangle(rect2);
PrintRectangle(rect3);
```

We will finish this tutorial with two functions, one saving rectangle to a file, and another loading rectangle from a file:

```ags
function SaveRectangle(File* f, Rectangle* rect)
{
    f.WriteInt(rect.x);
    f.WriteInt(rect.y);
    f.WriteInt(rect.width);
    f.WriteInt(rect.height);
}
```
```ags
Rectangle* LoadRectangle(File* f)
{
    int x = f.ReadInt();
    int y = f.ReadInt();
    int width = f.ReadInt();
    int height = f.ReadInt();
	return Rectangle.Create(x, y, width, height);
}
```

Here's an example of how these could be used:

```ags
File* f = File.Open("$SAVEGAMEDIR$/rectangles.dat", eFileWrite);
f.WriteInt(3); // write number of rectangles which are going to be saved
SaveRectangle(f, rect);
SaveRectangle(f, rect2);
SaveRectangle(f, rect3);
f.Close();
```
```ags
Rectangle* rect_array[]; // dynamic array of rectangles
int rect_array_size = 0; // will store array length here
File* f = File.Open("$SAVEGAMEDIR$/rectangles.dat", eFileRead);
if (f != null)
{
    rect_array_size = f.ReadInt();
    // create array to store N pointers
    rect_array = new Rectangle[rect_array_size];
    for (int i = 0; i < number; i++)
    {
        // create rectangle for each loaded entry in file,
        // and put its reference into the pointers array
        rect_array[i] = LoadRectangle(f);
    }
    f.Close();
}
```
