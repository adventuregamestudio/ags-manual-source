## `Slider` properties

Slider is a subclass of [`GUIControl`](GUIControl) and therefore inherits all GUIControl's functions and properties in addition to its own, which are listed below.

*See also:* [Editing GUI Sliders](EditorGUI#sliders)

---

### `Slider.BackgroundGraphic`

```ags
int Slider.BackgroundGraphic;
```

Gets/sets the sprite used to draw the slider background. This image is
tiled along the length of the slider.

The background graphic can be set to 0, which will draw the default grey
slider background.

Example:

```ags
Display("sldHealth's background is sprite %d", sldHealth.BackgroundGraphic);
```

displays the *sldHealth* slider's background image

*Compatibility:* Supported by **AGS 3.1.0** and later versions.

*See also:* [`Slider.HandleGraphic`](Slider#sliderhandlegraphic)

---

### `Slider.HandleColor`

```ags
int Slider.HandleColor
```

Gets/sets the fill color of the handle. This setting will be ignored if HandleGraphic is set.

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

*See also:* [`Slider.HandleGraphic`](Slider#sliderhandlegraphic)

---

### `Slider.HandleGraphic`

```ags
int Slider.HandleGraphic;
```

Gets/sets the sprite used to draw the handle on the slider. The handle
represents the slider's current position, and can be dragged around by
the player.

The handle graphic can be set to 0, which will draw the default grey
handle.

Example:

```ags
Display("sldHealth's handle is sprite %d", sldHealth.HandleGraphic);
```

displays the *sldHealth* slider's handle image

*Compatibility:* Supported by **AGS 3.1.0** and later versions.

*See also:*
[`Slider.BackgroundGraphic`](Slider#sliderbackgroundgraphic),
[`Slider.HandleOffset`](Slider#sliderhandleoffset)

---

### `Slider.HandleOffset`

```ags
int Slider.HandleOffset;
```

Gets/sets the offset at which the handle image is drawn. This value is
initially set up in the editor.

Example:

```ags
sldHealth.HandleOffset = 2;
```

sets the *sldHealth* slider's handle to be drawn an extra 2 pixels to
the right.

*Compatibility:* Supported by **AGS 3.1.0** and later versions.

*See also:* [`Slider.HandleGraphic`](Slider#sliderhandlegraphic)

---

### `Slider.Max`

```ags
int Slider.Max;
```

Gets/sets the maximum value of the specified GUI slider.

When changing the maximum, the slider's Value will be adjusted if
necessary so that it lies within the Min - Max range.

An error occurs if you try to set the Max to a lower value than the Min.

Example:

```ags
sldHealth.Max = 200;
```

sets the maximum value of the *sldHealth* slider to 200.

*See also:* [`Slider.Min`](Slider#slidermin),
[`Slider.Value`](Slider#slidervalue)

---

### `Slider.Min`

```ags
int Slider.Min;
```

Gets/sets the minimum value of the specified GUI slider.

When changing the minimum, the slider's Value will be adjusted if
necessary so that it lies within the Min - Max range.

An error occurs if you try to set the Min to a higher value than the
Max.

Example:

```ags
sldHealth.Min = 0;
```

sets the minimum value of the *sldHealth* slider to 0.

*See also:* [`Slider.Max`](Slider#slidermax),
[`Slider.Value`](Slider#slidervalue)

---

### `Slider.ShadowColor`

```ags
int Slider.ShadowColor
```

Gets/sets the alternate border color that will be used for 3D effect. It's used for both handle and the slider bar, but only if HandleGraphic and BackgroundGraphic are not set, respectively.

*Compatibility:* Supported by **AGS 3.6.3** and later versions.

*See also:* [`GUIControl.BorderColor`](GUIControl#guicontrolbordercolor),
[`Slider.BackgroundGraphic`](Slider#sliderbackgroundgraphic)

---

### `Slider.Value`

*(Formerly known as `GetSliderValue`, which is now obsolete)*<br>
*(Formerly known as `SetSliderValue`, which is now obsolete)*

```ags
int Slider.Value;
```

Gets/sets the value of the specified GUI slider. You would usually use
this in the slider's OnChange event handler to find out what value the
player has changed the slider to, in order to process their command.

When setting the value, the new value must lie between the MIN and MAX
settings for the slider, as set up in the GUI editor.

Example:

```ags
System.Volume = sldVolume.Value;
```

will set the audio volume to the value of the slider *sldVolume*.

*See also:* [`Label.Text`](Label#labeltext)

