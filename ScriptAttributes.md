## Attributes

In [structs](ScriptStructs) *variables* store data and *functions* provide functionality. *Attributes* are the third kind of struct members that *look like variables but act like functions*. An attribute itself is a pseudo-variable, that is connected to a pair of "getter" and "setter" functions. A "getter" function is called automatically whenever someone is trying to read attribute's value, and a "setter" function is called whenever a new value is assigned to an attribute. Each attribute has its own "getter"/"setter" pair, these are exclusive and cannot be shared among multiple attributes.

Attribute are declared in structs and must have `import` and `attribute` keywords before them:

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
    import int get_Attribute(); // Attribute's getter
    import void set_Attribute(int value); // Attribute's setter
};
```

Like we said above, normally you should not be declaring these in the struct yourself, as that will defeat the purpose of the attribute. The idea is that the external code should access the attribute but don't use the "get_" and "set_" functions. So there's no point in having them mentioned in the struct.

But in any case you must write the functions themselves in the script body. These are written just like regular struct's [member functions](ScriptStructs#member-functions):

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

Attribute's getter and setter are treated as regular struct's functions in all regards.

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
int MyStruct::get_ReadMeOnly()
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
String StoreItem::get_Name()
{
    return this.name;
}

void StoreItem::set_Name(String value)
{
    if (String.IsNullOrEmpty(value))
        this.name = value;
    else
        this.name = "Unknown Item";
}

int StoreItem::get_Price()
{
    return this.price;
}

void StoreItem::set_Price(int value)
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
int StoreItem::get_WeightInGramms()
{
    return FloatToInt(IntToFloat(this.weight) * 28.34952);
}

void StoreItem::set_WeightInGramms(int value)
{
    if (value < 0)
        value = 0;
        
    this.weight = FloatToInt(IntToFloat(value) / 28.34952);
}

int StoreItem::get_WeightInOunces()
{
    return this.weight;
}

void StoreItem::set_WeightInOunces(int value)
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
int StoreItem::get_IsWorthless()
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

For an indexed attribute their getter and setter functions have a slightly different name: "geti_Attribute" and "seti_Attribute" (notice extra letter 'i'). These functions must have an extra "index" argument in their parameter lists:

```ags
int MyStruct::geti_IndexedAttr(int index)
{
    // return something for the given "index"
}

void MyStruct::seti_IndexedAttr(int index, int value)
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
void Team::AllocateTeamSize(int count)
{
    this.names = new String[count];
    this.count = count;
}

int Team::get_PeopleCount()
{
    return this.count;
}

String Team::geti_PeopleNames(int index)
{
    if (index < 0 || index >= count)
        return null; // invalid index

    return this.names[index];
}

void Team::seti_PeopleNames(int index, String value)
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
int Color::geti_Component(int index)
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

void Color::seti_Component(int index, int value)
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
