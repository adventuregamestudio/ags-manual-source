## `VideoPlayer` functions and properties

### `VideoPlayer.Open`

```ags
static VideoPlayer* VideoPlayer.Open(const string filename, bool autoPlay=true, RepeatStyle=eOnce)
```

Returns a VideoPlayer instance from a video file

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.Play`

```ags
VideoPlayer.Play()
```

Starts or resumes the playback.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.Pause`

```ags
VideoPlayer.Pause()
```

Pauses the playback.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.NextFrame`

```ags
VideoPlayer.NextFrame()
```

Advances video by 1 frame, may be called when the video is paused.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.SeekFrame`

```ags
int VideoPlayer.SeekFrame(int frame)
```

Changes playback to continue from the specified frame; returns new position or -1 on error.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.SeekMs`

```ags
int VideoPlayer.SeekMs(int position)
```

Changes playback to continue from the specified position in milliseconds; returns new position or -1 on error.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.Stop`

```ags
VideoPlayer.Stop()
```

Stops the video completely.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.Frame`

```ags
readonly int VideoPlayer.Frame
```

Gets current frame index.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.FrameCount`

```ags
readonly int VideoPlayer.FrameCount
```

Gets total number of frames in this video.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.FrameRate`

```ags
readonly float VideoPlayer.FrameRate
```

Gets this video's framerate (number of frames per second).

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.Graphic`

```ags
readonly int VideoPlayer.Graphic
```

Gets the number of sprite this video renders to.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.LengthMs`

```ags
readonly int VideoPlayer.LengthMs
```

The length of the currently playing video, in milliseconds.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.Looping`

```ags
bool VideoPlayer.Looping
```

Gets/sets whether the video should loop.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.PositionMs`

```ags
readonly int VideoPlayer.PositionMs
```

The current playback position, in milliseconds.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.Speed`

```ags
float VideoPlayer.Speed
```

The speed of playing (1.0 is default).

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.State`

```ags
readonly PlaybackState VideoPlayer.State
```

Gets the current playback state (playing, paused, etc).

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [`PlaybackState`](StandardEnums#playbackstate)

---

### `VideoPlayer.Volume`

```ags
int VideoPlayer.Volume
```

The volume of this video's sound, from 0 to 100.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---
