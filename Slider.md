GUI Slider properties
---------------------

[BringToFront (inherited)](GUIControl#BringToFront)\
[Clickable property (inherited)](GUIControl#Clickable)\
[Enabled property (inherited)](GUIControl#Enabled)\
[Height property (inherited)](GUIControl#Height)\
[ID property (inherited)](GUIControl#ID)\
[OwningGUI property (inherited)](GUIControl#OwningGUI)\
[SendToBack (inherited)](GUIControl#SendToBack)\
[SetPosition (inherited)](GUIControl#SetPosition)\
[SetSize (inherited)](GUIControl#SetSize)\
[Visible property (inherited)](GUIControl#Visible)\
[Width property (inherited)](GUIControl#Width)\
[X property (inherited)](GUIControl#X)\
[Y property (inherited)](GUIControl#Y)\
[ZOrder property (inherited)](GUIControl#ZOrder)

[BackgroundGraphic property (slider)](#BackgroundGraphic)\
[HandleGraphic property](#HandleGraphic)\
[HandleOffset property](#HandleOffset)\
[Max property](#Max)\
[Min property](#Min)\
[Value property](#Value)

---

### BackgroundGraphic property (slider)

    int Slider.BackgroundGraphic;

Gets/sets the sprite used to draw the slider background. This image is
tiled along the length of the slider.

The background graphic can be set to 0, which will draw the default grey
slider background.

Example:

    Display("sldHealth's background is sprite %d", sldHealth.BackgroundGraphic);

displays the *sldHealth* slider's background image

*Compatibility:* Supported by **AGS 3.1.0** and later versions.

*See Also:* [Slider.HandleGraphic](Slider#HandleGraphic)

---

### HandleGraphic property

    int Slider.HandleGraphic;

Gets/sets the sprite used to draw the handle on the slider. The handle
represents the slider's current position, and can be dragged around by
the player.

The handle graphic can be set to 0, which will draw the default grey
handle.

Example:

    Display("sldHealth's handle is sprite %d", sldHealth.HandleGraphic);

displays the *sldHealth* slider's handle image

*Compatibility:* Supported by **AGS 3.1.0** and later versions.

*See Also:*
[Slider.BackgroundGraphic](Slider#BackgroundGraphic),
[Slider.HandleOffset](Slider#HandleOffset)

---

### HandleOffset property

    int Slider.HandleOffset;

Gets/sets the offset at which the handle image is drawn. This value is
initially set up in the editor.

Example:

    sldHealth.HandleOffset = 2;

sets the *sldHealth* slider's handle to be drawn an extra 2 pixels to
the right.

*Compatibility:* Supported by **AGS 3.1.0** and later versions.

*See Also:* [Slider.HandleGraphic](Slider#HandleGraphic)

---

### Max property

    int Slider.Max;

Gets/sets the maximum value of the specified GUI slider.

When changing the maximum, the slider's Value will be adjusted if
necessary so that it lies within the Min - Max range.

An error occurs if you try to set the Max to a lower value than the Min.

Example:

    sldHealth.Max = 200;

sets the maximum value of the *sldHealth* slider to 200.

*See Also:* [Slider.Min](Slider#Min),
[Slider.Value](Slider#Value)

---

### Min property

    int Slider.Min;

Gets/sets the minimum value of the specified GUI slider.

When changing the minimum, the slider's Value will be adjusted if
necessary so that it lies within the Min - Max range.

An error occurs if you try to set the Min to a higher value than the
Max.

Example:

    sldHealth.Min = 0;

sets the minimum value of the *sldHealth* slider to 0.

*See Also:* [Slider.Max](Slider#Max),
[Slider.Value](Slider#Value)

---

### Value property

*(Formerly known as GetSliderValue, which is now obsolete)*\
*(Formerly known as SetSliderValue, which is now obsolete)*

    int Slider.Value;

Gets/sets the value of the specified GUI slider. You would usually use
this in the slider's OnChange event handler to find out what value the
player has changed the slider to, in order to process their command.

When setting the value, the new value must lie between the MIN and MAX
settings for the slider, as set up in the GUI editor.

Example:

    System.Volume = sldVolume.Value;

will set the audio volume to the value of the slider *sldVolume*.

*See Also:* [Label.Text](Label#Text)
