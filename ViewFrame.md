
## `ViewFrame` functions and properties

### `ViewFrame.EventName`

```ags
String ViewFrame.EventName
```

Gets/sets a custom frame event tag. If set, this tag will be used as a parameter to the animating object's OnFrameEvent event, whenever this frame is displayed.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [`Button.RunFrameEvent`](Button#buttonrunframeevent),
[`Character.RunFrameEvent`](Character#characterrunframeevent),
[`Object.RunFrameEvent`](Object#objectrunframeevent)

---

### `ViewFrame.Flipped`

*(Formerly part of `GetGameParameter`, which is now obsolete)*

```ags
eFlipDirection ViewFrame.Flipped
```

Gets/sets whether this frame is flipped.

Example:

```ags
ViewFrame *frame = Game.GetViewFrame(WALKING, 2, 4);
if (frame.Flipped) {
    Display("This frame is flipped");
}
else {
    Display("This frame is not flipped");
}
```

*Compatibility:* Since **AGS 4.0.0** it now accepts an `eFlipDirection` value instead of `bool`.

*See also:* [`Game.GetViewFrame`](Game#gamegetviewframe),
[`ViewFrame.Graphic`](ViewFrame#viewframegraphic),
[`eFlipDirection`](StandardEnums#eflipdirection)

---

### `ViewFrame.Frame`

*(Formerly part of `GetGameParameter`, which is now obsolete)*

```ags
readonly int ViewFrame.Frame
```

Returns the frame number represented by this ViewFrame.

Example:

```ags
ViewFrame *frame = Game.GetViewFrame(WALKING, 2, 4);
Display("This ViewFrame is view %d, loop %d, frame %d",
        frame.View, frame.Loop, frame.Frame);
```

*See also:* [`Game.GetViewFrame`](Game#gamegetviewframe),
[`ViewFrame.Loop`](ViewFrame#viewframeloop),
[`ViewFrame.View`](ViewFrame#viewframeview)

---

### `ViewFrame.Graphic`

*(Formerly part of `GetGameParameter`, which is now obsolete)*

```ags
int ViewFrame.Graphic
```

Gets/sets the sprite slot number that this view frame displays.

Example:

```ags
ViewFrame *frame = Game.GetViewFrame(WALKING, 2, 4);
Display("This frame uses sprite %d", frame.Graphic);
```

*See also:* [`Game.GetViewFrame`](Game#gamegetviewframe)

---

### `ViewFrame.LinkedAudio`

*(Formerly known as `ViewFrame.Sound`, which is now obsolete)*<br>
*(Formerly known as `SetFrameSound`, which is now obsolete)*<br>
*(Formerly part of `GetGameParameter`, which is now obsolete)*

```ags
AudioClip* ViewFrame.LinkedAudio
```

Gets/sets the audio clip that plays when this frame comes around in
animations.

If there is no linked sound, this should be *null*.

Example:

```ags
ViewFrame *frame = Game.GetViewFrame(WALKING, 2, 4);
if (frame.LinkedAudio == null)
{
    Display("This frame has no frame-linked audio.");
}
else
{
    frame.LinkedAudio.Play();
}
```

checks view WALKING to see if frame 4 in loop 2 has a linked audio clip;
if so, plays it.

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:* [`Game.GetViewFrame`](Game#gamegetviewframe), [`Character.ScaleVolume`](Character#characterscalevolume)

---

### `ViewFrame.Loop`

*(Formerly part of `GetGameParameter`, which is now obsolete)*

```ags
readonly int ViewFrame.Loop
```

Returns the loop number represented by this ViewFrame.

Example:

```ags
ViewFrame *frame = Game.GetViewFrame(WALKING, 2, 4);
Display("This ViewFrame is view %d, loop %d, frame %d",
        frame.View, frame.Loop, frame.Frame);
```

*See also:* [`Game.GetViewFrame`](Game#gamegetviewframe),
[`ViewFrame.Frame`](ViewFrame#viewframeframe),
[`ViewFrame.View`](ViewFrame#viewframeview)

---

### `ViewFrame.Speed`

*(Formerly part of `GetGameParameter`, which is now obsolete)*

```ags
int ViewFrame.Speed
```

Gets/sets the speed setting of the view frame. This is 0 by default but may
have been changed in the AGS Editor.

Example:

```ags
ViewFrame *frame = Game.GetViewFrame(WALKING, 2, 4);
Display("This frame has speed %d.", frame.Speed);
```

*Compatibility:* the value can be set since **AGS 4.0.0** and later versions.

*See also:* [`Game.GetViewFrame`](Game#gamegetviewframe)

---

### `ViewFrame.View`

*(Formerly part of `GetGameParameter`, which is now obsolete)*

```ags
readonly int ViewFrame.View
```

Returns the view number represented by this ViewFrame.

Example:

```ags
ViewFrame *frame = Game.GetViewFrame(WALKING, 2, 4);
Display("This ViewFrame is view %d, loop %d, frame %d",
        frame.View, frame.Loop, frame.Frame);
```

*See also:* [`Game.GetViewFrame`](Game#gamegetviewframe),
[`ViewFrame.Loop`](ViewFrame#viewframeloop),
[`ViewFrame.Frame`](ViewFrame#viewframeframe)

---

### `ViewFrame.XOffset`

```ags
int ViewFrame.XOffset
```

Gets/sets the relative x offset applied to this frame.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `ViewFrame.YOffset`

```ags
int ViewFrame.YOffset
```

Gets/sets the relative y offset applied to this frame.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.