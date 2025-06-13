## `ShaderProgram` functions and properties

### `ShaderProgram.CreateFromFile`

```ags
static ShaderProgram* ShaderProgram.CreateFromFile(const string filename)
```

Creates a new ShaderProgram by either loading a precompiled shader, or reading source code and compiling one.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `ShaderProgram.CreateInstance`

```ags
ShaderInstance* ShaderProgram.CreateInstance()
```

Creates a new shader instance of this shader program.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [`ShaderInstance`](ShaderInstance)

---

### `ShaderProgram.Default`

```ags
readonly ShaderInstance* ShaderProgram.Default
```

Gets the default shader instance of this shader program.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [`ShaderInstance`](ShaderInstance)