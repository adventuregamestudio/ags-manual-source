## `ShaderInstance` functions and properties

### `ShaderInstance.SetConstantF`

```ags
ShaderInstance.SetConstantF(const string name, float value)
```

Sets a shader's constant value as 1 float.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `ShaderInstance.SetConstantF2`

```ags
ShaderInstance.SetConstantF2(const string name, float x, float y)
```

Sets a shader's constant value as 2 floats.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `ShaderInstance.SetConstantF3`

```ags
ShaderInstance.SetConstantF3(const string name, float x, float y, float z)
```

Sets a shader's constant value as 3 floats.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `ShaderInstance.SetConstantF4`

```ags
ShaderInstance.SetConstantF4(const string name, float x, float y, float z, float w)
```

Sets a shader's constant value as 4 floats.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `ShaderInstance.SetTexture`

```ags
ShaderInstance.SetTexture(int index, int sprite)
```

Sets a secondary shader's input texture, using a sprite number. Only indexes 1-3 are supported.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `ShaderInstance.Shader`

```ags
readonly ShaderProgram* ShaderInstance.Shader
```

Gets a shader's ShaderProgram.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [`ShaderProgram`](ShaderProgram)