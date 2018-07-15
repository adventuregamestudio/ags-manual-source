ViewFrame functions and properties
----------------------------------

[Flipped property](#Flipped)\
[Frame property (view frame)](#Frame)\
[Graphic property (view frame)](#Graphic)\
[LinkedAudio property (view frame)](#LinkedAudio)\
[Loop property (view frame)](#Loop)\
[Speed property (view frame)](#Speed)\
[View property (view frame)](#View)

---

### Flipped property

*(Formerly part of GetGameParameter, which is now obsolete)*

    readonly bool ViewFrame.Flipped

Gets whether the frame was set to Flipped in the editor.

Example:

    ViewFrame *frame = Game.GetViewFrame(WALKING, 2, 4);
    if (frame.Flipped) {
      Display("This frame is flipped");
    }
    else {
      Display("This frame is not flipped");
    }

*See Also:* [Game.GetViewFrame](Game#GetViewFrame),
[ViewFrame.Graphic](ViewFrame#Graphic)

---

### Frame property (view frame)

*(Formerly part of GetGameParameter, which is now obsolete)*

    readonly int ViewFrame.Frame

Returns the frame number represented by this ViewFrame.

Example:

    ViewFrame *frame = Game.GetViewFrame(WALKING, 2, 4);
    Display("This ViewFrame is view %d, loop %d, frame %d",
      frame.View, frame.Loop, frame.Frame);

*See Also:* [Game.GetViewFrame](Game#GetViewFrame),
[ViewFrame.Loop](ViewFrame#Loop),
[ViewFrame.View](ViewFrame#View)

---

### Graphic property (view frame)

*(Formerly part of GetGameParameter, which is now obsolete)*

    int ViewFrame.Graphic

Gets/sets the sprite slot number that this view frame displays.

Example:

    ViewFrame *frame = Game.GetViewFrame(WALKING, 2, 4);
    Display("This frame uses sprite %d", frame.Graphic);

*See Also:* [Game.GetViewFrame](Game#GetViewFrame)

---

### LinkedAudio property (view frame)

*(Formerly known as ViewFrame.Sound, which is now obsolete)*\
*(Formerly known as SetFrameSound, which is now obsolete)*\
*(Formerly part of GetGameParameter, which is now obsolete)*

    AudioClip* ViewFrame.LinkedAudio

Gets/sets the audio clip that plays when this frame comes around in
animations.

If there is no linked sound, this should be *null*.

Example:

    ViewFrame *frame = Game.GetViewFrame(WALKING, 2, 4);
    if (frame.LinkedAudio == null)
    {
      Display("This frame has no frame-linked audio.");
    }
    else
    {
      frame.LinkedAudio.Play();
    }

checks view WALKING to see if frame 4 in loop 2 has a linked audio clip;
if so, plays it.

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See Also:* [Game.GetViewFrame](Game#GetViewFrame)

---

### Loop property (view frame)

*(Formerly part of GetGameParameter, which is now obsolete)*

    readonly int ViewFrame.Loop

Returns the loop number represented by this ViewFrame.

Example:

    ViewFrame *frame = Game.GetViewFrame(WALKING, 2, 4);
    Display("This ViewFrame is view %d, loop %d, frame %d",
      frame.View, frame.Loop, frame.Frame);

*See Also:* [Game.GetViewFrame](Game#GetViewFrame),
[ViewFrame.Frame](ViewFrame#Frame),
[ViewFrame.View](ViewFrame#View)

---

### Speed property (view frame)

*(Formerly part of GetGameParameter, which is now obsolete)*

    readonly int ViewFrame.Speed

Gets the speed setting of the view frame. This is 0 by default but may
have been changed in the AGS Editor.

Example:

    ViewFrame *frame = Game.GetViewFrame(WALKING, 2, 4);
    Display("This frame has speed %d.", frame.Speed);

*See Also:* [Game.GetViewFrame](Game#GetViewFrame)

---

### View property (view frame)

*(Formerly part of GetGameParameter, which is now obsolete)*

    readonly int ViewFrame.View

Returns the view number represented by this ViewFrame.

Example:

    ViewFrame *frame = Game.GetViewFrame(WALKING, 2, 4);
    Display("This ViewFrame is view %d, loop %d, frame %d",
      frame.View, frame.Loop, frame.Frame);

*See Also:* [Game.GetViewFrame](Game#GetViewFrame),
[ViewFrame.Loop](ViewFrame#Loop),
[ViewFrame.Frame](ViewFrame#Frame)
