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
Character, etc), and custom structs declared as `managed`. But currently you cannot have dynamic arrays of regular (non-managed) custom structs.

On another hand, you cannot have dynamic arrays inside "managed" structs as their members, but can have them anywhere else.

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

*See also:* [Arrays](ScriptKeywords#arrays), [Structs](ScriptKeywords#struct)
