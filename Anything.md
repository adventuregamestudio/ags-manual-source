## `Anything` functions and properties

Anything struct is a super-parent of all the managed types in script. Any other managed struct, whether declared by the engine, or written by the user, will automatically and implicitly inherit (extend) Anything, along with its functions and properties. Because any type inherits Anything, the `Anything*` pointers may be used to store reference to virtually any managed object.

In script you can cast a pointer of any object type to `Anything*`, using implicit type conversion:

```ags
Anything* any1 = someObject;
Anything* any2 = someCharacter;
Anything* any3 = someGUI;
// etc
```

You can try casting `Anything*` variable to certain type using explicit downcast conversion (`as`):

```ags
Object* obj = any1 as Object;
Character* ch = any2 as Character;
GUI* g = any3 as GUI;
```

This will return an actual object of the respective type on success, and null on failure.

*Compatibility:* The Anything struct is supported by **AGS 4.0.0** and later versions.

### `Anything.TypeName`

```ags
readonly String Anything.TypeName
```

Gets the actual name of the type of this object. This may be useful for finding out what type of object is contained in `Anything*` pointer, or for debugging purposes.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.
