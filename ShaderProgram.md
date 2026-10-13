## `ShaderProgram` functions and properties

### `ShaderProgram.CreateFromFile`

```ags
static ShaderProgram* ShaderProgram.CreateFromFile(const string filename)
```

Creates a new ShaderProgram by either loading a precompiled shader, or reading source code and compiling one.

Example:

```ags
ShaderProgram* wave = ShaderProgram.CreateFromFile("$DATA$/Shaders/wave.glsl");
if (wave != null)
{
  Game.Camera.Shader = wave.Default;
}
```

will load the shader from a file, and, if that succeeded, apply its default instance to the primary camera.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `ShaderProgram.CreateInstance`

```ags
ShaderInstance* ShaderProgram.CreateInstance()
```

Creates a new shader instance of this shader program.

Example:

```ags
ShaderProgram* wave = ShaderProgram.CreateFromFile("$DATA$/Shaders/wave.glsl");
ShaderInstance* first = wave.CreateInstance();
ShaderInstance* second = wave.CreateInstance();
Game.Camera.Shader = first;
Screen.Viewport.Shader = second;
```

will create two separate instances of the same shader program, so that each of them can have its own constant values, and apply one to the camera and another to the viewport.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [`ShaderInstance`](ShaderInstance)

---

### `ShaderProgram.Default`

```ags
readonly ShaderInstance* ShaderProgram.Default
```

Gets the default shader instance of this shader program.

Example:

```ags
ShaderProgram* wave = ShaderProgram.CreateFromFile("$DATA$/Shaders/wave.glsl");
Game.Camera.Shader = wave.Default;
```

will apply the default instance of the shader to the primary camera.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [`ShaderInstance`](ShaderInstance)