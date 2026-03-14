## `AudioChannel` functions and properties

The AudioChannel instance represents a currently playing audio file. You
can use this instance to check the status of playing sounds, and adjust
them.

---

### `AudioChannel.Pause`

```ags
AudioChannel.Pause()
```

Pauses the playback on this channel

Example:

```ags
AudioChannel *channel = aMusic.Play();
Display("After this message the audio will pause, but we may unpause soon")
channel.Pause();
Display("After this message the audio will keep playing")
channel.Resume();
```

will start playing the *aMusic* audio clip, and then pause after the first message and resume after the second message.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`AudioChannel.Resume`](AudioChannel#audiochannelresume)

---

### `AudioChannel.Resume`

```ags
AudioChannel.Resume()
```

Resumes the paused audio channel.

Example:

```ags
AudioChannel *channel = aExplosion.Play();
Wait(20);
channel.Pause();
Wait(20);
channel.Resume();
```

will start playing the *aExplosion* audio clip, wait 20 ticks, pause, wait 20 ticks in silence, then continue playing.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`AudioChannel.Pause`](AudioChannel#audiochannelpause)

---

### `AudioChannel.Seek`

*(Formerly known as `SeekMIDIPosition`, which is now obsolete)*<br>
*(Formerly known as `SeekMODPattern`, which is now obsolete)*<br>
*(Formerly known as `SeekMP3PosMillis`, which is now obsolete)*

```ags
AudioChannel.Seek(int position)
```

Seeks the audio clip that is currently playing on this channel to
*position*.

What *position* represents depends on the FileType of the audio clip:

-   **MIDI** - the beat number
-   **MOD/XM/S3M** - the pattern number
-   **WAV/VOC** - the sample number (e.g. in a 22050 Hz sound, 22050 =
    1 second)
-   **OGG/MP3** - milliseconds offset

Example:

```ags
AudioChannel *channel = aExplosion.Play();
Wait(40);
channel.Seek(0);
```

will start playing the *aExplosion* audio clip, wait for a second, then
seek it back to the start.

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:* [`AudioChannel.SeekMs`](AudioChannel#audiochannelseekms), [`AudioChannel.Position`](AudioChannel#audiochannelposition), [`AudioChannel.PositionMs`](AudioChannel#audiochannelpositionms)

---

### `AudioChannel.SeekMs`

```ags
AudioChannel.SeekMs(int position)
```

Seeks the audio clip that is currently playing on this channel to
*position* in milliseconds.

Example:

```ags
AudioChannel *channel = aMusic.Play();
Wait(40);
channel.SeekMs(10000);
```

will start playing the *aMusic* audio clip, wait for a second, then
jump to 10 seconds position and continue from there.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`AudioChannel.Seek`](AudioChannel#audiochannelseekms), [`AudioChannel.Position`](AudioChannel#audiochannelposition), [`AudioChannel.PositionMs`](AudioChannel#audiochannelpositionms)

---

### `AudioChannel.SetRoomLocation`

*(Formerly part of `PlayAmbientSound`, which is now obsolete)*

```ags
AudioChannel.SetRoomLocation(int x, int y, optional int maxDistance)
```

Sets the currently playing audio to be a directional sound, emanating
from (x,y). Optionally sets "maxDistance" at which the volume should reach 0. If no maxDistance value is provided, then the maxDistance will be calculated as the distance from the location to the farthest room's side.

The volume of the channel will be dynamically adjusted depending on how
close the player character is to the co-ordinates. Therefore, as the
player walks closer the volume will increase, and as they walk away the
volume will decrease.

The channel's Volume setting sets the maximum possible volume when the
player is standing on the specified co-ordinates.

Pass the co-ordinates as (0,0) to remove the directional effect and
return this channel to playing at its normal volume.

Example:

```ags
AudioChannel *channel = aMachine.Play();
channel.SetRoomLocation(oMachine.X, oMachine.Y);
```

will start playing the *aMachine* audio clip, and set it at the location
of the *oMachine* room object.

*Compatibility:* Supported by **AGS 3.2.0** and later versions. The optional "maxDistance" parameter is supported by **AGS 3.6.3** and later versions.

*See also:* [`AudioChannel.Volume`](AudioChannel#audiochannelvolume),
[`Character.ScaleVolume`](Character#characterscalevolume)

---

### `AudioChannel.Speed`

```ags
int AudioChannel.Speed
```

Gets/sets the playing audio clip's playback speed. The value is defined
in clip's milliseconds per second: 1000 is default, meaning 1000 of
clip's ms in 1 real second (scale 1:1). Set < 1000 for slower play
and > 1000 for faster play.

Example:

```ags
AudioChannel *channel = aFunnyTalk.Play();
channel.Speed = 2000;
```

plays *aFunnyTalk* clip at the double speed.

*Compatibility:* Supported by **AGS 3.4.0** and later versions. Prior to **AGS 3.6.0** was working only for MP3 and OGG clips.

---

### `AudioChannel.Stop`

*(Formerly known as `StopAmbientSound`, which is now obsolete)*<br>
*(Formerly known as `StopChannel`, which is now obsolete)*

```ags
AudioChannel.Stop()
```

Stops the sound that is currently playing on this audio channel.

Example:

```ags
AudioChannel *channel = aExplosion.Play();
Wait(40);
channel.Stop();
```

will start playing the *aExplosion* audio clip, wait for a second, then
stop it.

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:* [`Game.StopAudio`](Game#gamestopaudio)

---

### `AudioChannel.ID`

```ags
readonly int AudioChannel.ID
```

Gets the Channel ID of this audio channel. You will not normally need to
use this, but it can be used for inter-operating with legacy commands
such as StopChannel.

Example:

```ags
AudioChannel *channel = aExplosion.Play();
Display("Explosion playing on channel %d", channel.ID);
```

will start playing the *aExplosion* audio clip, and display which
channel it is playing on.

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

---

### `AudioChannel.IsPaused`

```ags
readonly bool AudioChannel.IsPaused
```

Gets whether this audio channel is currently paused. Returns
*true* if it is, or *false* if it is not.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`AudioChannel.Pause`](AudioChannel#audiochannelpause), [`AudioChannel.Resume`](AudioChannel#audiochannelresume)

---

### `AudioChannel.IsPlaying`

*(Formerly known as `IsChannelPlaying`, which is now obsolete)*

```ags
readonly bool AudioChannel.IsPlaying
```

Gets whether this audio channel is currently playing a sound. Returns
*true* if it is, or *false* if it is not.

Example:

```ags
AudioChannel *channel = aExplosion.Play();
while (channel.IsPlaying) Wait(1);
Display("Finished playing the explosion");
```

will start playing the *aExplosion* audio clip, and wait until it
finishes.

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:* [`AudioClip.Play`](AudioClip#audioclipplay)

---

### `AudioChannel.LengthMs`

```ags
readonly int AudioChannel.LengthMs
```

Gets the length of the audio playing on this channel, in milliseconds.

This is supported by all file types, but with MIDI music it is only
accurate to the nearest second.

If this channel is not currently playing any audio, returns 0.

Example:

```ags
AudioChannel *channel = aExplosion.Play();
Display("The Explosion sound is %d ms long.", channel.LengthMs);
```

will start playing the *aExplosion* audio clip, then display its length.

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:* [`AudioChannel.PositionMs`](AudioChannel#audiochannelpositionms)

---

### `AudioChannel.Panning`

```ags
int AudioChannel.Panning
```

Gets/sets the panning of this audio channel.

Panning allows you to adjust the stereo balance of the audio. The
default is 0, which is centered and will play at the same volume on both
speakers. However you can adjust this between -100 (fully left) to 100
(fully right) to adjust the balance between the speakers.

**IMPORTANT:** only mono clips support panning at the moment.

Example:

```ags
AudioChannel *channel = aExplosion.Play();
channel.Panning = -100;
```

will start playing the *aExplosion* audio clip on the left speaker only.

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:* [`AudioClip.Play`](AudioClip#audioclipplay)

---

### `AudioChannel.PlayingClip`

*(Formerly known as `GetCurrentMusic`, which is now obsolete)*

```ags
readonly AudioClip* AudioChannel.PlayingClip
```

Gets the audio clip that is playing on this channel. This allows you to
find out the type of the clip, and other information.

Returns *null* if there is no sound currently playing on this channel.

Example:

```ags
AudioChannel *channel = System.AudioChannels[2];
if (channel.PlayingClip == null)
{
    Display("Nothing is playing on channel 2");
}
else
{
    Display("Channel 2 is playing a clip of type %d", channel.PlayingClip.Type);
}
```

will display what is currently playing on audio channel 2.

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:* [`AudioClip.Play`](AudioClip#audioclipplay),
[`System.AudioChannels`](System#systemaudiochannels)

---

### `AudioChannel.Position`

*(Formerly known as `GetMIDIPosition`, which is now obsolete)*<br>
*(Formerly known as `GetMODPattern`, which is now obsolete)*<br>
*(Formerly known as `GetMP3PosMillis`, which is now obsolete)*

```ags
readonly int AudioChannel.Position
```

Gets the current position of the audio playing on this channel.

What *position* represents depends on the FileType of the audio clip:

-   **MIDI** - the beat number
-   **MOD/XM/S3M** - the pattern number
-   **WAV/VOC** - the sample number (e.g. in a 22050 Hz sound, 22050 =
    1 second)
-   **OGG/MP3** - milliseconds offset

This property is read-only. If you want to change the current playback
position within the audio file, use the
[`AudioChannel.Seek`](AudioChannel#audiochannelseek) or [`AudioChannel.SeekMs`](AudioChannel#audiochannelseekms) function.

Example:

```ags
AudioChannel *channel = aExplosion.Play();
Wait(40);
channel.Seek(channel.Position + 1000);
```

will start playing the *aExplosion* audio clip, wait for a second, then
seek it ahead one second (if it is OGG/MP3/WAV).

**NOTE:** For historical reasons this property works differently during skipped cutscene: in such case it will return a very large arbitrary number instead. This was originally done to avoid game script locking up in do/while loops that check for clip position reaching certain value, because it might never reach it when game is "fast-forwarding". If this is a problem for your script, make sure to test for [`Game.SkippingCutscene`](Game#gameskippingcutscene).

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:*
[`AudioChannel.PositionMs`](AudioChannel#audiochannelpositionms),
[`AudioChannel.Seek`](AudioChannel#audiochannelseek),
[`AudioChannel.SeekMs`](AudioChannel#audiochannelseekms)

---

### `AudioChannel.PositionMs`

```ags
readonly int AudioChannel.PositionMs
```

Gets the current position of the audio playing on this channel, in
milliseconds.

This is supported by all file types except MIDI, and returns the current
offset into the sound in milliseconds. MIDI files will always return 0.

This property is read-only. If you want to change the current playback
position within the audio file, use the
[`AudioChannel.Seek`](AudioChannel#audiochannelseek) or [`AudioChannel.SeekMs`](AudioChannel#audiochannelseekms) function.

Example:

```ags
AudioChannel *channel = aExplosion.Play();
Wait(40);
Display("After 1 second, offset is %d ms.", channel.PositionMs);
```

will start playing the *aExplosion* audio clip, wait for a second, then
display its position.

**NOTE:** For historical reasons this property works differently during skipped cutscene: in such case it will return a very large arbitrary number instead. This was originally done to avoid game script locking up in do/while loops that check for clip position reaching certain value, because it might never reach it when game is "fast-forwarding". If this is a problem for your script, make sure to test for [`Game.SkippingCutscene`](Game#gameskippingcutscene).

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:*
[`AudioChannel.LengthMs`](AudioChannel#audiochannellengthms),
[`AudioChannel.Position`](AudioChannel#audiochannelposition),
[`AudioChannel.Seek`](AudioChannel#audiochannelseek),
[`AudioChannel.SeekMs`](AudioChannel#audiochannelseekms)

---

### `AudioChannel.Volume`

*(Formerly known as `SetChannelVolume`, which is now obsolete)*<br>
*(Formerly known as `SetMusicVolume`, which is now obsolete)*

```ags
int AudioChannel.Volume
```

Gets/sets the volume of this audio channel, from 0 to 100. This allows
you to dynamically adjust the volume of a playing sound.

This command adjusts the volume of this channel relative to the other
channels. It is still constrained within the overall volume, set by the
System.Volume property.

**NOTE:** This command only affects the current sound being played on
the channel. When a new audio clip starts playing on this channel, the
volume will be set to the DefaultVolume of the new audio clip.

**NOTE:** The volume returned by this property is the channel's base
volume, i.e. it does not include the effects of any directional audio set
with SetRoomLocation, or any temporary volume drop while speech is
playing.

Example:

```ags
AudioChannel *channel = aExplosion.Play();
Wait(40);
channel.Volume = 20;
```

will start playing the *aExplosion* audio clip, wait for a second, then
reduce its volume.

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:*
[`AudioChannel.SetRoomLocation`](AudioChannel#audiochannelsetroomlocation),
[`Game.SetAudioTypeVolume`](Game#gamesetaudiotypevolume),
[`System.Volume`](System#systemvolume),
[`Character.ScaleVolume`](Character#characterscalevolume)
