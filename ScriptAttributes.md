## Attributes

In [structs](ScriptStructs) *variables* store data and *functions* provide functionality. *Attributes* are the third kind of struct members that *look like variables but act like functions*. An attribute is a pseudo-variable that does not store any data itself, but is connected to a pair of "getter" and "setter" functions. A "getter" function is called automatically whenever someone is trying to read attribute's value, and a "setter" function is called whenever a new value is assigned to an attribute. Each attribute has its own "getter"/"setter" pair, these are exclusive and cannot be shared among multiple attributes.

An attribute is declared using `import` and `attribute` keywords:

```ags
struct MyStruct
{
    import attribute int Attribute;
};
```

When you declare an attribute, script compiler will implicitly expect 2 functions that are called "get_Attribute" and "set_Attribute" (where "Attribute" is an attribute's name). The "getter" function must return a value of the attribute's type, and "setter" function must be of "void" type and take 1 parameter of attribute's type (new value). Strictly speaking, you may declare these functions inside a struct yourself, but do not have to, and usually should not:

```ags
struct MyStruct
{
    import attribute int Attribute;
    import protected int get_Attribute(); // Attribute's getter
    import protected void set_Attribute(int value); // Attribute's setter
};
```

If you do, a good idea will be to declare them `protected`, so that they won't be called directly, as that will defeat the purpose of the attribute. Notice how we used a `protected` keyword in their declarations above. The idea is that the external code accesses the attribute but don't use the "get_" and "set_" functions.

In any case you must write the functions themselves in the script body. If you have declared them inside a struct, then these are written just like regular struct's [member functions](ScriptStructs#member-functions):

```ags
int MyStruct::get_Attribute()
{
    // getter code here
}

void MyStruct::set_Attribute(int value)
{
    // setter code here
}
```

There's an alternative, which utilizes the [extender function syntax](ExtenderFunctions). In this case you do not have to declare getter and setter in the struct at all. Then, inside the script body you write two extenders, like this:

```ags
int get_Attribute(this MyStruct*)
{
    // getter code here
}

void set_Attribute(this MyStruct*, int value)
{
    // setter code here
}
```

(We are going to use this latter syntax in code examples below, because it turns out shorter. But you are free to use either in your game.)

Regardless of the chosen style, attribute's getter and setter are treated as regular struct's functions in all regards.

In the script attributes are used similar to struct's variables, where they may be read or set:

```ags
MyStruct s;
s.Attribute = 10;
Display("Attribute's value is: %d", s.Attribute);
```

It is important to note that, while a attribute *looks* like a variable, it does not have a direct connection to any real data, and what it does (or rather - what its get/set functions do) to a struct is entirely defined by you. (See examples of attribute use further below.)

See also: [Struct's member functions](ScriptStructs#member-functions)

### Readonly attributes

Attributes can be declared as `readonly`. In this case they cannot be set, and so the "setter" function should be omited, but "getter" still present:

```ags
struct MyStruct
{
    import attribute int Attribute;
    import readonly attribute int ReadMeOnly;
};
```
```ags
int get_ReadMeOnly(this MyStruct*)
{
    // return something here
}
```

In the script you may still access this attribute for reading, but not setting a value:

```ags
MyStruct s;
Display("ReadMeOnly's value is: %d", s.ReadMeOnly);
s.ReadMeOnly = 20; // this line will cause a error
```

### Static attributes

Attributes can be declared as `static`. In this case they are accessed not from the struct's instance, but from the type itself.

If we have a static attribute declared like this:

```ags
struct MyStruct
{
    import static attribute int StaticAttribute;
};
```

then we will access it like shown in script:

```ags
MyStruct.StaticAttribute = 50;
```

When you write getters and setters for the static attributes you must use `static` keyword as well. In the case when you have these declared as member functions in struct:

```ags
static int MyStruct::get_StaticAttribute()
{
    // return something
}
```

And in the case where you are using extender functions:
```ags
int get_StaticAttribute(static MyStruct)
{
    // return something
}
```

See also: [Struct's static functions](ScriptStructs#static-functions)

### Protection modifiers

In addition to `readonly` and `static`, attributes may have one of two protection modifiers:

* `protected` - tells that the attribute can only be accessed from within the struct's member functions using `this` keyword.
* `writeprotected` - tells that the attribute may be read from the external code, but set only from within struct's member functions using `this` keyword.

See also: [Struct's member modifiers](ScriptStructs#member-modifiers)

### Uses of attribute

What are the common uses of an attribute? The most simple case is when an attribute works as an access to a `protected` variable and provides input value checks. For example:

```ags
struct StoreItem
{
    protected String name;
    protected int    price;
    
    import attribute String Name;
    import attribute int    Price;
};
```
```ags
String get_Name(this StoreItem*)
{
    return this.name;
}

void set_Name(this StoreItem*, String value)
{
    if (!String.IsNullOrEmpty(value))
        this.name = value;
    else
        this.name = "Unknown Item";
}

int get_Price(this StoreItem*)
{
    return this.price;
}

void set_Price(this StoreItem*, int value)
{
    if (value >= 0)
        this.price = value;
    else
        this.price = 0;
}
```

The use of these attributes in script will be like:

```ags
StoreItem item;
item.Name = "Apple";
item.Price = 10;
```

Another example is attributes that work not with the direct value of a variable but a result of a calculation. For example:

```ags
struct StoreItem
{
    protected String name;
    protected int    price;
    protected int    weight; // in ounces
    
    import attribute String Name;
    import attribute int    Price;
    import attribute int    WeightInGramms;
    import attribute int    WeightInOunces;
};
```

```ags
int get_WeightInGramms(this StoreItem*)
{
    return FloatToInt(IntToFloat(this.weight) * 28.34952);
}

void set_WeightInGramms(this StoreItem*, int value)
{
    if (value < 0)
        value = 0;
        
    this.weight = FloatToInt(IntToFloat(value) / 28.34952);
}

int get_WeightInOunces(this StoreItem*)
{
    return this.weight;
}

void set_WeightInOunces(this StoreItem*, int value)
{
    if (value >= 0)
        this.weight = value;
    else
        this.weight = 0;
}
```

Here StoreItem stores the weight in ounces (because it's a smaller unit), but allows to get or set the weight in either gramms or ounces, using two separate attributes.

Following is an example of an attribute that do not directly correspond to a variable, but use them to define the struct's status:

```ags
struct StoreItem
{
    //.... the rest of the struct contents here

    import readonly attribute bool IsWorthless;
}
```
```ags
int get_IsWorthless(this StoreItem*)
{
    return (this.price == 0);
}
```

### Indexed attributes

Where regular attributes emulate a variable, indexed attributes emulate arrays. Although, just like attributes don't have a direct connection to a certain variable, similarly indexed attributes don't have a direct connection to a real array, and may have a special behavior.

Indexed attributes are declared like this:

```ags
struct MyStruct
{
    import attribute int IndexedAttr[];
};
```

Notice that they have a `[]` part which tells that this attribute is accessed using a index, but there's no "size" number between the brackets. That's because indexed attributes do not have a fixed "size" or "number of elements". Usually they should be paired with another attribute that specifies their valid range.

For an indexed attribute their getter and setter functions have a slightly different name: "geti_Attribute" and "seti_Attribute" (notice extra letter 'i'). These functions must have an extra "index" argument in their parameter lists.

The variant with getter and setter declared as struct members:

```ags
struct MyStruct
{
    import attribute int IndexedAttr[];
    import protected int  get_IndexedAttr(int index);
    import protected void set_IndexedAttr(int index, int value);
};
```
```ags
int StoreItem::geti_IndexedAttr(int index)
{
    // return something for the given "index"
}

void StoreItem::seti_IndexedAttr(int index, int value)
{
    // do something for the given "index"
}
```

And the variant with getter and setter defined as extender functions:

```ags
int geti_IndexedAttr(this StoreItem*, int index)
{
    // return something for the given "index"
}

void seti_IndexedAttr(this StoreItem*, int index, int value)
{
    // do something for the given "index"
}
```

Here's an example of their use:

```ags
struct Team
{
    protected int    count;
    protected String names[]; // dynamic array of people names

    import void AllocateTeamSize(int count);
    import attribute int    PeopleCount;
    import attribute String PeopleNames[];
};
```
```ags
void AllocateTeamSize(this Team*, int count)
{
    this.names = new String[count];
    this.count = count;
}

int get_PeopleCount(this Team*)
{
    return this.count;
}

String geti_PeopleNames(this Team*, int index)
{
    if (index < 0 || index >= count)
        return null; // invalid index

    return this.names[index];
}

void seti_PeopleNames(this Team*, int index, String value)
{
    if (index < 0 || index >= count)
        return; // invalid index

    this.names[index] = value;
}
```

As we mentioned previously, an indexed attribute emulates array, but it does not have to correspond to a real array directly. Sometimes it may be used to perform an access to something in struct using a conventional index. Here's a trivial example:

```ags
struct Color
{
    int R; // red component
    int G; // green component
    int B; // blue component
    int A; // alpha component
    
    import attribute int Component[];
};
```
```ags
int geti_Component(this Color*, int index)
{
    switch (index)
    {
    case 0: return this.R;
    case 1: return this.G;
    case 2: return this.B;
    case 3: return this.A;
    default: return 0; // invalid index
    }
}

void seti_Component(this Color*, int index, int value)
{
    switch (index)
    {
    case 0: this.R = value; return;
    case 1: this.G = value; return;
    case 2: this.B = value; return;
    case 3: this.A = value; return;
    default: return; // invalid index
    }
}
```

Here the Color's components may be accessed either directly, or using an indexed attribute and passing index in range 0 to 3.

Of course, in a similar scenario we could do an opposite approach: store components in an array, and have Red, Green etc attributes access particular elements of that array. But this example is here only to illustrate a possibility. In each particular case there's a choice between several solutions, and a solution working best in one case may not be optimal in another.
