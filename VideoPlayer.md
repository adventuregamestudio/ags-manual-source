## `VideoPlayer` functions and properties

### `VideoPlayer.Open`

```ags
static VideoPlayer* VideoPlayer.Open(const string filename, bool autoPlay=true, RepeatStyle=eOnce)
```

Returns a VideoPlayer instance from a video file

Example:

```ags
VideoPlayer* video = VideoPlayer.Open("$DATA$/intro.mp4");
if (video != null)
{
  Overlay* screen = Overlay.CreateGraphical(0, 0, video.Graphic);
  while (video.State == ePlaybackOn)
  {
    if (IsKeyPressed(eKeyEscape))
    {
      video.Stop();
    }
    Wait(1);
  }
  screen.Remove();
}
```

will play a video on a screen overlay and make the script wait until it ends, or until the player presses Escape. VideoPlayer does not have a blocking mode, so the loop with Wait(1) is what makes the script wait while the game keeps updating the video.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.Play`

```ags
VideoPlayer.Play()
```

Starts or resumes the playback.

Example:

```ags
VideoPlayer* tvVideo;

function room_Load()
{
  tvVideo = VideoPlayer.Open("$DATA$/news.mp4", false, eRepeat);
  if (tvVideo != null)
  {
    oTV.Graphic = tvVideo.Graphic;
  }
}

function oTV_Interact(Object *theObject, CursorMode mode)
{
  tvVideo.Play();
}
```

will prepare a video for the TV set in the room without starting it, and begin the playback when the player turns the TV on. The video variable is declared outside of the functions, so that the video is not lost when `room_Load` ends.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.Pause`

```ags
VideoPlayer.Pause()
```

Pauses the playback.

Example:

```ags
function on_key_press(eKeyCode keycode, int mod)
{
  if (keycode == eKeySpace && video != null)
  {
    if (video.State == ePlaybackOn)
    {
      video.Pause();
    }
    else if (video.State == ePlaybackPaused)
    {
      video.Play();
    }
  }
}
```

will let the player pause and resume the video with the Space key. Here `video` is a global VideoPlayer* variable holding a video opened with VideoPlayer.Open.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.NextFrame`

```ags
VideoPlayer.NextFrame()
```

Advances video by 1 frame, may be called when the video is paused.

Example:

```ags
function on_key_press(eKeyCode keycode, int mod)
{
  if (keycode == eKeyRightArrow && video != null && video.State == ePlaybackPaused)
  {
    video.NextFrame();
  }
}
```

will let the player step through a paused video frame by frame with the right arrow key. Here `video` is a global VideoPlayer* variable holding a video opened with VideoPlayer.Open.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.SeekFrame`

```ags
int VideoPlayer.SeekFrame(int frame)
```

Changes playback to continue from the specified frame; returns new position or -1 on error.

Example:

```ags
function btnRestart_OnClick(GUIControl *control, MouseButton button)
{
  video.SeekFrame(0);
}
```

will make the video start over from its first frame when the player clicks the button btnRestart. Here `video` is a global VideoPlayer* variable holding a video opened with VideoPlayer.Open.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.SeekMs`

```ags
int VideoPlayer.SeekMs(int position)
```

Changes playback to continue from the specified position in milliseconds; returns new position or -1 on error.

Example:

```ags
function btnForward_OnClick(GUIControl *control, MouseButton button)
{
  video.SeekMs(video.PositionMs + 10000);
}
```

will skip the video 10 seconds forward when the player clicks the button btnForward. Here `video` is a global VideoPlayer* variable holding a video opened with VideoPlayer.Open.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.Stop`

```ags
VideoPlayer.Stop()
```

Stops the video completely.

Example:

```ags
function on_key_press(eKeyCode keycode, int mod)
{
  if (keycode == eKeyEscape && video != null)
  {
    video.Stop();
    video = null;
    videoOverlay.Remove();
    videoOverlay = null;
  }
}
```

will stop the video and remove the overlay it was shown on when the player presses Escape. Here `video` and `videoOverlay` are global variables, holding a video opened with VideoPlayer.Open and the overlay that shows it.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.Frame`

```ags
readonly int VideoPlayer.Frame
```

Gets current frame index.

Example:

```ags
function repeatedly_execute()
{
  if (video != null)
  {
    sldProgress.Max = video.FrameCount - 1;
    sldProgress.Value = video.Frame;
  }
}
```

will make the slider sldProgress show how far the video has played. Here `video` is a global VideoPlayer* variable holding a video opened with VideoPlayer.Open.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.FrameCount`

```ags
readonly int VideoPlayer.FrameCount
```

Gets total number of frames in this video.

Example:

```ags
function repeatedly_execute()
{
  if (video != null)
  {
    sldProgress.Max = video.FrameCount - 1;
    sldProgress.Value = video.Frame;
  }
}
```

will make the slider sldProgress show how far the video has played. Here `video` is a global VideoPlayer* variable holding a video opened with VideoPlayer.Open.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.FrameRate`

```ags
readonly float VideoPlayer.FrameRate
```

Gets this video's framerate (number of frames per second).

Example:

```ags
float seconds = IntToFloat(video.Frame) / video.FrameRate;
lblTime.Text = String.Format("%.1f s", seconds);
```

will convert the current frame number of the video into seconds, and show it in the label lblTime. Here `video` is a global VideoPlayer* variable holding a video opened with VideoPlayer.Open.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.Graphic`

```ags
readonly int VideoPlayer.Graphic
```

Gets the number of sprite this video renders to.

Example:

```ags
VideoPlayer* tvVideo;

function room_Load()
{
  tvVideo = VideoPlayer.Open("$DATA$/news.mp4", true, eRepeat);
  if (tvVideo != null)
  {
    oTV.Graphic = tvVideo.Graphic;
  }
}
```

will play a looping video on the room object oTV, by giving the object the sprite that the video renders its frames to.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.LengthMs`

```ags
readonly int VideoPlayer.LengthMs
```

The length of the currently playing video, in milliseconds.

Example:

```ags
function repeatedly_execute()
{
  if (video != null)
  {
    lblTime.Text = String.Format("%d / %d s", video.PositionMs / 1000, video.LengthMs / 1000);
  }
}
```

will show the current position and the total length of the video, in seconds, in the label lblTime. Here `video` is a global VideoPlayer* variable holding a video opened with VideoPlayer.Open.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.Looping`

```ags
bool VideoPlayer.Looping
```

Gets/sets whether the video should loop.

Example:

```ags
function on_mouse_click(MouseButton button, int mx, int my)
{
  if (titleVideo != null)
  {
    // the title video loops until the player clicks, then it plays to its end and stops
    titleVideo.Looping = false;
  }
}
```

will let the looping video on the title screen finish its current run and stop, as soon as the player clicks. Here `titleVideo` is a global VideoPlayer* variable, holding a video opened with VideoPlayer.Open using eRepeat.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.PositionMs`

```ags
readonly int VideoPlayer.PositionMs
```

The current playback position, in milliseconds.

Example:

```ags
function repeatedly_execute()
{
  if (video != null)
  {
    lblTime.Text = String.Format("%d / %d s", video.PositionMs / 1000, video.LengthMs / 1000);
  }
}
```

will show the current position and the total length of the video, in seconds, in the label lblTime. Here `video` is a global VideoPlayer* variable holding a video opened with VideoPlayer.Open.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.Speed`

```ags
float VideoPlayer.Speed
```

The speed of playing (1.0 is default).

Example:

```ags
function repeatedly_execute()
{
  if (video == null)
  {
    return;
  }
  if (IsKeyPressed(eKeyF))
  {
    video.Speed = 4.0;
  }
  else
  {
    video.Speed = 1.0;
  }
}
```

will play the video four times faster while the player holds down the F key. Here `video` is a global VideoPlayer* variable holding a video opened with VideoPlayer.Open.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `VideoPlayer.State`

```ags
readonly PlaybackState VideoPlayer.State
```

Gets the current playback state (playing, paused, etc).

Example:

```ags
function on_key_press(eKeyCode keycode, int mod)
{
  if (keycode == eKeySpace && video != null)
  {
    if (video.State == ePlaybackOn)
    {
      video.Pause();
    }
    else if (video.State == ePlaybackPaused)
    {
      video.Play();
    }
  }
}
```

will let the player pause and resume the video with the Space key. Here `video` is a global VideoPlayer* variable holding a video opened with VideoPlayer.Open.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

*See also:* [`PlaybackState`](StandardEnums#playbackstate)

---

### `VideoPlayer.Volume`

```ags
int VideoPlayer.Volume
```

The volume of this video's sound, from 0 to 100.

Example:

```ags
function sldVolume_OnChange(GUIControl *control)
{
  video.Volume = sldVolume.Value;
}
```

will set the volume of the video's sound from the slider sldVolume (with its range set from 0 to 100). Here `video` is a global VideoPlayer* variable holding a video opened with VideoPlayer.Open.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---
