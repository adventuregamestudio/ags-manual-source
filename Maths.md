## `Maths` functions and properties

### `FloatToInt`

```ags
int FloatToInt(float value, optional RoundDirection)
```

Converts the supplied floating point value into an integer.

This function is necessary because implicit conversions in the script
are not supported.

RoundDirection can be either *eRoundDown* (the default), *eRoundUp* or
*eRoundNearest*, which specifies what direction to round the floating
point number in.

* eRoundDown - finds the nearest integer which is <= value. For example:
    * +1.5 = +1,
    * -1.5 = -2;
* eRoundUp - finds the nearest integer which is >= value. For example:
    * +1.5 = +2,
    * -1.5 = -1;
* eRoundNearest - finds the actual nearest integer. Middle cases are rounded away from zero. For example:
    * +1.2 = +1,
    * +1.8 = +2,
    * +1.5 = +2,
    * -1.2 = -1,
    * -1.8 = -2,
    * -1.5 = -2.

Example:

```ags
Display("Round down: %d", FloatToInt(10.7));
Display("Round up: %d", FloatToInt(10.7, eRoundUp));
Display("Round nearest: %d", FloatToInt(10.7, eRoundNearest));
```

displays:
* Round down: 10
* Round up: 11
* Round nearest: 11

*See also:* [`IntToFloat`](Maths#inttofloat)

---

### `IntToFloat`

```ags
float IntToFloat(int value)
```

Converts the supplied integer value into a floating point number.

This function is necessary because implicit conversions in the script
are not supported.

Example:

```ags
float number = IntToFloat(10);
```

loads 10.0 into the variable *number*.

*See also:* [`FloatToInt`](Maths#floattoint)

---

### `Maths.ArcCos`

```ags
float Maths.ArcCos(float value)
```

Calculates the arc-cosine, in radians, of the specified value.

To convert an angle in radians to degrees, use
[`Maths.RadiansToDegrees`](Maths#mathsradianstodegrees).

Example:

```ags
float angle = Maths.ArcCos(1.0);
```

calculates the arc-cosine of 1.0 and places it into variable *angle*.

*See also:* [`Maths.Cos`](Maths#mathscos),
[`Maths.DegreesToRadians`](Maths#mathsdegreestoradians),

---

### `Maths.ArcSin`

```ags
float Maths.ArcSin(float value)
```

Calculates the arc-sine, in radians, of the specified value.

To convert an angle in radians to degrees, use
[`Maths.RadiansToDegrees`](Maths#mathsradianstodegrees).

Example:

```ags
float angle = Maths.ArcSin(0.5);
```

calculates the arc-sine of 0.5 and places it into variable *angle*.

*See also:* [`Maths.Sin`](Maths#mathssin),
[`Maths.DegreesToRadians`](Maths#mathsdegreestoradians),

---

### `Maths.ArcTan`

```ags
float Maths.ArcTan(float value)
```

Calculates the arc-tan, in radians, of the specified value. Return value is in range -PI/2 to +PI/2.

To convert an angle in radians to degrees, use
[`Maths.RadiansToDegrees`](Maths#mathsradianstodegrees).

Example:

```ags
float angle = Maths.ArcTan(0.5);
```

calculates the arc-tan of 0.5 and places it into variable *angle*.

*See also:* [`Maths.ArcTan2`](Maths#mathsarctan2),
[`Maths.DegreesToRadians`](Maths#mathsdegreestoradians),
[`Maths.Tan`](Maths#mathstan)

---

### `Maths.ArcTan2`

```ags
float Maths.ArcTan2(float y, float x)
```

Calculates the arctangent of y/x. This is well defined for every point
other than the origin, even if x equals 0 and y does not equal 0. The
result is returned in radians. Return value is in range -PI to +PI.

To convert an angle in radians to degrees, use
[`Maths.RadiansToDegrees`](Maths#mathsradianstodegrees).

Example:

```ags
float angle = Maths.ArcTan2(-862.42, 78.5149);
```

calculates the arc-tan of -862.42 / 78.5149 and places it into variable
*angle*.

*See also:* [`Maths.DegreesToRadians`](Maths#mathsdegreestoradians),
[`Maths.ArcTan`](Maths#mathsarctan)

---

### `Maths.Cos`

```ags
float Maths.Cos(float radians)
```

Calculates the cosine of the specified angle (in radians).

To convert an angle in degrees to radians, use
[`Maths.DegreesToRadians`](Maths#mathsdegreestoradians).

Example:

```ags
float cosine = Maths.Cos(Maths.DegreesToRadians(360.0));
```

calculates the cosine of 360 degrees (which is 1.0) and places it into
variable *cosine*.

*See also:* [`Maths.ArcCos`](Maths#mathsarccos),
[`Maths.Cosh`](Maths#mathscosh),
[`Maths.DegreesToRadians`](Maths#mathsdegreestoradians),
[`Maths.Sin`](Maths#mathssin), [`Maths.Tan`](Maths#mathstan)

---

### `Maths.Cosh`

```ags
float Maths.Cosh(float radians)
```

Calculates the hyperbolic cosine of the specified angle (in radians).

To convert an angle in degrees to radians, use
[`Maths.DegreesToRadians`](Maths#mathsdegreestoradians).

Example:

```ags
float hcos = Maths.Cosh(Maths.DegreesToRadians(360.0));
```

calculates the hyperbolic cosine of 360 degrees and places it into
variable *hcos*.

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:* [`Maths.Cos`](Maths#mathscos),
[`Maths.DegreesToRadians`](Maths#mathsdegreestoradians),
[`Maths.Sinh`](Maths#mathssinh), [`Maths.Tanh`](Maths#mathstanh)

---

### `Maths.DegreesToRadians`

```ags
float Maths.DegreesToRadians(float degrees)
```

Converts the supplied angle in degrees, to the equivalent angle in
radians.

Since the trigonometric functions such as Sin, Cos and Tan work in
radians, this function is handy if you know the angle you want in
degrees.

Example:

```ags
float cosine = Maths.Cos(Maths.DegreesToRadians(360.0));
```

calculates the cosine of 360 degrees (which is 1.0) and places it into
variable *cosine*.

*See also:* [`Maths.Cos`](Maths#mathscos),
[`Maths.RadiansToDegrees`](Maths#mathsradianstodegrees),
[`Maths.Sin`](Maths#mathssin), [`Maths.Tan`](Maths#mathstan)

---

### `Maths.Exp`

```ags
float Maths.Exp(float x)
```

Returns the exponential value of the floating-point parameter, x

The result is e to the power x, where e is the base of the natural
logarithm. On overflow, the function returns infinite and on underflow,
returns 0.

Example:

```ags
float expValue = Maths.Exp(2.302585093);
```

calculates Exp of 2.302585093 (which should be 10) and places it into
the variable.

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:* [`Maths.Log`](Maths#mathslog),
[`Maths.Log10`](Maths#mathslog10)

---

### `Maths.Log`

```ags
float Maths.Log(float x)
```

Returns the natural logarithm (base e) of x.

x must be a positive non-zero number.

Example:

```ags
float logVal = Maths.Log(9000.0);
```

calculates Log of 9000 (which should be 9.104980) and places it into the
variable.

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:* [`Maths.Exp`](Maths#mathsexp),
[`Maths.Log10`](Maths#mathslog10)

---

### `Maths.Log10`

```ags
float Maths.Log10(float x)
```

Returns the base-10 logarithm of x.

x must be a positive non-zero number.

Example:

```ags
float logVal = Maths.Log(9000.0);
```

calculates Log10 of 9000 (which should be 3.954243) and places it into
the variable.

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:* [`Maths.Exp`](Maths#mathsexp),
[`Maths.Log`](Maths#mathslog)

---

### `Maths.RadiansToDegrees`

```ags
float Maths.RadiansToDegrees(float radians)
```

Converts the supplied angle in radians, to the equivalent angle in
degrees.

Since the trigonometric functions such as Sin, Cos and Tan work in
radians, this function is handy to convert the results of one of those
functions back to degrees.

Example:

```ags
float halfCircle = Maths.RadiansToDegrees(Maths.Pi);
```

converts *PI* radians into degrees (which is 180).

*See also:* [`Maths.Cos`](Maths#mathscos),
[`Maths.DegreesToRadians`](Maths#mathsdegreestoradians),
[`Maths.Sin`](Maths#mathssin), [`Maths.Tan`](Maths#mathstan)

---

### `Maths.RaiseToPower`

```ags
float Maths.RaiseToPower(float base, float exponent)
```

Calculates the value of *base* raised to the power *exponent*.

This means that *base* is multiplied by itself *exponent* times.

Example:

```ags
float value = Maths.RaiseToPower(4.0, 3.0);
```

calculates 4 to the power 3 (which is 64).

*See also:* [`Maths.Sqrt`](Maths#mathssqrt)

---

### `Maths.Random`

```ags
static int Maths.Random(int limit)
```

Returns a random number in the range of 0 to LIMIT-1.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Maths.RandomFloat`

```ags
static float Maths.RandomFloat()
```

Returns a random float in the range of [0.0, 1.0) (including 0 but excluding 1).

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Maths.Round`

```ags
static float Maths.Round(float value, RoundDirection)
```

Rounds the given float towards chosen direction, and returns a result as a new float.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `Maths.Sin`

```ags
float Maths.Sin(float radians)
```

Calculates the sine of the specified angle (in radians).

To convert an angle in degrees to radians, use
[`Maths.DegreesToRadians`](Maths#mathsdegreestoradians).

Example:

```ags
float sine = Maths.Sin(Maths.DegreesToRadians(360.0));
```

calculates the sine of 360 degrees (which is 0) and places it into
variable *sine*.

*See also:* [`Maths.ArcSin`](Maths#mathsarcsin),
[`Maths.Sinh`](Maths#mathssinh),
[`Maths.DegreesToRadians`](Maths#mathsdegreestoradians),
[`Maths.Cos`](Maths#mathscos), [`Maths.Tan`](Maths#mathstan)

---

### `Maths.Sinh`

```ags
float Maths.Sinh(float radians)
```

Calculates the hyperbolic sine of the specified angle (in radians).

To convert an angle in degrees to radians, use
[`Maths.DegreesToRadians`](Maths#mathsdegreestoradians).

Example:

```ags
float hsine = Maths.Sinh(Maths.DegreesToRadians(360.0));
```

calculates the hyperbolic sine of 360 degrees and places it into
variable *hsine*.

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:* [`Maths.ArcSin`](Maths#mathsarcsin),
[`Maths.Sin`](Maths#mathssin),
[`Maths.DegreesToRadians`](Maths#mathsdegreestoradians),
[`Maths.Cosh`](Maths#mathscosh), [`Maths.Tanh`](Maths#mathstanh)

---

### `Maths.Sqrt`

```ags
float Maths.Sqrt(float value)
```

Calculates the square root of the supplied value.

The square root is the number which, when multiplied by itself, equals
*value*.

Example:

```ags
Display("The square root of 4 is %d!", FloatToInt(Maths.Sqrt(4.0)));
```

displays the square root of 4 (rounded down to the nearest integer).

*See also:* [`Maths.Cos`](Maths#mathscos),
[`Maths.RaiseToPower`](Maths#mathsraisetopower),
[`Maths.Sin`](Maths#mathssin), [`Maths.Tan`](Maths#mathstan)

---

### `Maths.Tan`

```ags
float Maths.Tan(float radians)
```

Calculates the tangent of the specified angle (in radians).

To convert an angle in degrees to radians, use
[`Maths.DegreesToRadians`](Maths#mathsdegreestoradians).

Example:

```ags
float tan = Maths.Tan(Maths.DegreesToRadians(45.0));
```

calculates the tan of 45 degrees (which is 1.0) and places it into
variable *tan*.

*See also:* [`Maths.ArcTan`](Maths#mathsarctan),
[`Maths.Tanh`](Maths#mathstanh),
[`Maths.DegreesToRadians`](Maths#mathsdegreestoradians),
[`Maths.Cos`](Maths#mathscos), [`Maths.Sin`](Maths#mathssin)

---

### `Maths.Tanh`

```ags
float Maths.Tanh(float radians)
```

Calculates the hyperbolic tangent of the specified angle (in radians).

To convert an angle in degrees to radians, use
[`Maths.DegreesToRadians`](Maths#mathsdegreestoradians).

Example:

```ags
float htan = Maths.Tanh(Maths.DegreesToRadians(45.0));
```

calculates the hyperbolic tan of 45 degrees and places it into variable
*htan*.

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:* [`Maths.ArcTan`](Maths#mathsarctan),
[`Maths.Tan`](Maths#mathstan),
[`Maths.DegreesToRadians`](Maths#mathsdegreestoradians),
[`Maths.Cos`](Maths#mathscos), [`Maths.Sin`](Maths#mathssin)

---

### `Maths.Pi`

```ags
readonly float Maths.Pi
```

Gets the value of Pi (defined as 3.14159265358979323846).

Example:

```ags
Display("Pi is %f!", Maths.Pi);
```

displays the value of Pi.

*See also:* [`Maths.Cos`](Maths#mathscos),
[`Maths.Sin`](Maths#mathssin), [`Maths.Tan`](Maths#mathstan)

