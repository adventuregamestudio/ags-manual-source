## Dynamic Arrays

### Overview

Suppose that you're writing a script that you want people to be able to
use in their games. You need to store the Health for every character in
the game, but you don't know how many characters there will be. What do
you do?

Dynamic Arrays are designed for just this purpose. You can declare an
array like this:

`int characterHealth[];`

in your script file. This special notation tells AGS that you don't yet
know how large you want the array to be. Now, before you use the array
(so probably in game_start), you can do this:

`characterHealth = new int[Game.CharacterCount];`

If you forget to do this `new` command, you'll get a Null Pointer Error
if you try to access the array. You can change the size of an array by
simply using another `new` command with a different size; but this will
erase the contents of the array in the process.

You may create dynamic arrays of basic types (int, char, etc), built-in types (String,
Character, etc), custom structs declared as `managed`, and, since AGS 4.0.0, regular (non-managed) custom structs:

```ags
struct Pair
{
    int X, Y;
};

Pair many_pairs[];

many_pairs = new Pair[10];
many_pairs[0].X = 50;
many_pairs[0].Y = 60;
```

Since AGS 4.0.0 you may also have dynamic arrays as members of "managed" structs (see [Managed structs](ScriptManagedStructs#pointers-to-other-managed-objects)).

*Compatibility:* In AGS 3.x versions you cannot have dynamic arrays of regular (non-managed) structs, and cannot have dynamic arrays inside "managed" structs. Dynamic arrays of regular structs also require the new script compiler.

### Empty arrays

You may create a dynamic array of zero length, by specifying 0 as its size:

```ags
int arr[] = new int[0];
```

Such array has nothing in it, but it is a valid array, and not a "null" pointer. This is useful when a function should return an array that may have no elements: then the script that uses it does not have to check for "null", and may simply iterate over the array, using its `Length` (which is 0).

*Compatibility:* Zero-length dynamic arrays are supported by **AGS 4.0.0** and later versions, and only when using the new script compiler.

### Array's Length

Dynamic array's length is its number of elements. In AGS you can access this value using a `Length` property:

```ags
characterHealth = new int[Game.CharacterCount];
<...>
// later in script
int len = characterHealth.Length;
```

*Compatibility:* The dynamic array's Length property is supported by **AGS 3.6.2** and later versions.
In previous versions you would have to store created array's length in a separate integer variable.

### Filling a dynamic array

If you have dynamic array of managed pointers, then after creation all of these pointers will be null. You must also assign individual elements one by one. For example, when you want to fill an array with already existing game objects:

```ags
Character *groupOfChars[] = new Character[4];
groupOfChars[0] = cRoger;
groupOfChars[1] = cMary;
groupOfChars[2] = cThomas;
groupOfChars[3] = cVillager;
```

Similarily, when you want to create completely new objects:

```ags
DynamicSprite *sprites[] = new DynamicSprite[4];
sprites[0] = DynamicSprite.Create(10, 10);
sprites[1] = DynamicSprite.Create(20, 20);
sprites[2] = DynamicSprite.Create(10, 40);
sprites[3] = DynamicSprite.Create(50, 10);
```

Accessing individual elements is as simple as with regular array:

```ags
int health = characterHealth[5];
int charX = groupOfChars[3].x;
sprites[1].Flip(eFlipLeftToRight);
```

You don't *have* to fill all the array indexes of course, you may as well leave some empty (null). Just remember to keep track of that, and if your script may access empty indexes make sure to check if the element is null or not before using it:

```ags
for (int i = 0; i < sprites.Length; i++) {
    if (sprites[i] != null) {
        sprites[i].Flip(eFlipLeftToRight);
    }
}
```

### Resizing a dynamic array

Dynamic arrays cannot be resized on their own. If later you require a bigger array, you may to do following:
* Create a new, larger array and save it in a temporary variable;
* Copy all elements from the old array into the new one;
* Assign new array to your usual array variable.

For example:

```ags
int arrOfInts[]; // declare a dynamic array of ints

void game_start() {
    arrOfInts = new int[100]; // create array of 100 ints
}

void resize_array() {
    int tempArr[] = new int[200]; // create array of 200 ints and save it in a temp var
    for (i = 0; i < arrOfInts.Length && i < 200; i++) {
        tempArr[i] = arrOfInts[i]; // copy contents of the old array
    }
    arrOfInts = tempArr; // assign new array to your usual variable
}
```

Same solution may work when you want to reduce an array to a smaller size, except in that case you'll have to loose some elements.

### Deleting dynamic array

Like most of the dynamically created objects in AGS, dynamic arrays exist so long as it is assigned to at least one pointer. If you have one pointer to array, and you change its value or assign it to null, then the dynamic array will get deleted automatically:

```ags
int arrOfInts[];
arrOfInts = new int[100]; // create and assign an array of 100 ints
arrOfInts = new int[200]; // assign a new array of 200 ints; the previous one is lost and gets deleted
arrOfInts = null; // now the second array is also lost and get deleted
```

Note that if you store a dynamic array in a local function variable, that variable will get removed as soon as function ends, and if array was not reassigned to any global pointer, then it also gets deleted automatically.

```ags
void func() {
    int arrOfInts[] = new int[100];
    // do something with this array
} // as soon as function ends the array gets deleted
```

The array will not be deleted if you *return* it from the function; so long as the returned array is assigned to a pointer of course:

```ags
int[] func_makearr(int size) {
    int arrOfInts[] = new int[size];
    return arrOfInts; // not deleted just yet
}

// somewhere else in code:
int arr[] = func_makearr(100); // the returned array will be assigned to `arr` pointer
```

### Multi-dimensional dynamic arrays

Multi-dimensional dynamic arrays are also known as "jagged arrays". In these each element of all dimensions except the last one is a pointer to another dynamic array, and elements of the last dimension are values of the array's type.
All sub-arrays must be created individually, but this also means that each of them may have an independent length.

Jagged arrays must be declared with all dimension sizes undefined, like:

```ags
int dyn_arr[][][]; // a three-dimensional dynamic array
```

First you must create the "parent" array, then you may create sub-arrays for each of its elements. On each nesting level the sub-array has *one dimension less*. When you create actual arrays, only the *first* dimension must have a fixed size, and following dimensions still have undefined size:

```ags
int dyn_arr[][][];
// create 3-dimensional array of size 10
dyn_arr = new int[10][][];
// for the 5th element of the first dimension,
// create a 2-dimensional array of size 20
dyn_arr[5] = new int[20][];
// for the 6th element of the first dimension,
// create a 2-dimensional array of size 25
dyn_arr[6] = new int[25][];
// for the 15th element of the 5th element,
// create a 1-dimensional array of size 40
dyn_arr[5][15] = new int[40];
// finally we may access the actual integer values
dyn_arr[5][15][35] = 100;
```

Elements which sub-arrays were not created are "null", same as with dynamic arrays of pointers. Each sub-array has its own `Length`, for example: `dyn_arr.Length` is 10, `dyn_arr[5].Length` is 20 and `dyn_arr[5][15].Length` is 40.

*Compatibility:* Multi-dimensional dynamic arrays are supported by **AGS 4.0.0** and later versions, and only when using the new script compiler.

### Mixed arrays

You may also have regular (fixed-size) arrays of dynamic arrays. This is done by declaring an array which first dimensions have fixed size, and the last ones have undefined size. The regular dimensions "end" at the last fixed-sized dimension.

Following is a regular array of 10 dynamic arrays of int:

```ags
int mixed_array[10][];
```

Following is a regular multi-dimensional array, which contains pointers to multi-dimensional dynamic arrays:

```ags
int overcomplicated_array[10][20][][];
```

Each element of such array is initially "null", and you must create dynamic arrays for them, like:

```ags
mixed_array[0] = new int[5];
mixed_array[1] = new int[50];
```

*Compatibility:* Mixed arrays are supported by **AGS 4.0.0** and later versions, and only when using the new script compiler.

*See also:* [Arrays](ScriptKeywords#arrays), [Multidimensional arrays](ScriptKeywords#multidimensional-arrays), [Structs](ScriptKeywords#struct)
