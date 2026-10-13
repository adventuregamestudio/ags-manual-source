## `ShaderInstance` functions and properties

### `ShaderInstance.SetConstantF`

```ags
ShaderInstance.SetConstantF(const string name, float value)
```

Sets a shader's constant value as 1 float.

Example:

```ags
ShaderProgram* wave = ShaderProgram.CreateFromFile("$DATA$/Shaders/wave.glsl");
ShaderInstance* instance = wave.CreateInstance();
instance.SetConstantF("Amplitude", 0.5);
Game.Camera.Shader = instance;
```

will create an instance of the shader, set its constant named "Amplitude" to 0.5, and apply the instance to the primary camera. The constant names depend on the shader's code.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `ShaderInstance.SetConstantF2`

```ags
ShaderInstance.SetConstantF2(const string name, float x, float y)
```

Sets a shader's constant value as 2 floats.

Example:

```ags
ShaderProgram* wave = ShaderProgram.CreateFromFile("$DATA$/Shaders/wave.glsl");
ShaderInstance* instance = wave.CreateInstance();
instance.SetConstantF2("Direction", 1.0, 0.5);
Game.Camera.Shader = instance;
```

will create an instance of the shader, set its constant named "Direction" to a pair of floats, and apply the instance to the primary camera. The constant names depend on the shader's code.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `ShaderInstance.SetConstantF3`

```ags
ShaderInstance.SetConstantF3(const string name, float x, float y, float z)
```

Sets a shader's constant value as 3 floats.

Example:

```ags
ShaderProgram* wave = ShaderProgram.CreateFromFile("$DATA$/Shaders/wave.glsl");
ShaderInstance* instance = wave.CreateInstance();
instance.SetConstantF3("Color", 1.0, 0.5, 0.0);
Game.Camera.Shader = instance;
```

will create an instance of the shader, set its constant named "Color" to three floats (red, green and blue here), and apply the instance to the primary camera. The constant names depend on the shader's code.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `ShaderInstance.SetConstantF4`

```ags
ShaderInstance.SetConstantF4(const string name, float x, float y, float z, float w)
```

Sets a shader's constant value as 4 floats.

Example:

```ags
ShaderProgram* wave = ShaderProgram.CreateFromFile("$DATA$/Shaders/wave.glsl");
ShaderInstance* instance = wave.CreateInstance();
instance.SetConstantF4("Color", 1.0, 0.5, 0.0, 1.0);
Game.Camera.Shader = instance;
```

will create an instance of the shader, set its constant named "Color" to four floats (red, green, blue and alpha here), and apply the instance to the primary camera. The constant names depend on the shader's code.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `ShaderInstance.SetTexture`

```ags
ShaderInstance.SetTexture(int index, int sprite)
```

Sets a secondary shader's input texture, using a sprite number. Only indexes 1-3 are supported.

Example:

```ags
ShaderProgram* blend = ShaderProgram.CreateFromFile("$DATA$/Shaders/blend.glsl");
ShaderInstance* instance = blend.CreateInstance();
instance.SetTexture(1, 300);
Game.Camera.Shader = instance;
```

will create an instance of the shader, give it the sprite 300 as the secondary input texture number 1, and apply the instance to the primary camera. Only the texture indexes 1 to 3 are supported.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `ShaderInstance.Shader`

```ags
readonly ShaderProgram* ShaderInstance.Shader
```

Gets a shader's ShaderProgram.

Example:

```ags
function SetWaveStrength(ShaderInstance* instance, float strength)
{
  // only the instances of the wave shader have the "Amplitude" constant
  if (instance != null && instance.Shader == waveShader)
  {
    instance.SetConstantF("Amplitude", strength);
  }
}
```

will change the "Amplitude" constant of a shader instance, but only if that instance belongs to the wave shader program; the instances of other shaders are left alone. Here `waveShader` is a global ShaderProgram* variable, loaded with ShaderProgram.CreateFromFile.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [`ShaderProgram`](ShaderProgram)