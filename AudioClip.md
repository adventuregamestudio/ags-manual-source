## `AudioClip` functions and properties

AudioClips are created when you import files in the AGS Editor. The
commands in this section allow to play them.


---

### `AudioClip.GetByName`

```ags
static AudioClip* AudioClip.GetByName(string scriptName)
```

Returns a pointer to the AudioClip with the specified script name, or null if it does not exist.

Normally you do not need to use this, as there will be a automatically created global script variable for each AudioClip which got a script name.
Where GetByName() function may come useful is situation in which you a) do not know exact name, b) had to store object's reference in a string for some reason. Good examples of this are saving object's name in a [custom property](CustomProperties), or a [file](File), then reading it back.

Example:

```ags
void PlayDefaultSound(Object *obj) {
    String soundName = obj.GetTextProperty("default-sound");
    AudioClip *clip = AudioClip.GetByName(soundName);
    if (clip != null) {
        clip.Play();
    }
}
```

Retrieves the audio clip with the script name stored in an object's custom property, and plays it.

*Compatibility:* Supported by **AGS 3.6.1** and later versions.

*See also:* [`AudioClip.ScriptName`](AudioClip#audioclipscriptname), [`AudioClip.Play`](AudioClip#audioclipplay)

---

### `AudioClip.Play`

*(Formerly known as `PlayAmbientSound`, which is now obsolete)*<br>
*(Formerly known as `PlayMP3File`, which is now obsolete)*<br>
*(Formerly known as `PlayMusic`, which is now obsolete)*<br>
*(Formerly known as `PlaySound`, which is now obsolete)*<br>
*(Formerly known as `PlaySoundEx`, which is now obsolete)*<br>
*(Formerly known as `SetMusicRepeat`, which is now obsolete)*

```ags
AudioChannel* AudioClip.Play(optional AudioPriority, optional RepeatStyle)
```

Plays the audio clip.

Optionally you can supply a [AudioPriority](StandardEnums#audiopriority) and [RepeatStyle](StandardEnums#repeatstyle) setting; if you do not
supply these, the defaults set for the audio clip in the editor will be
used.

This command searches through all the available audio channels to find
one that is available for this type of audio. If no spare channels are
found, it will try to find one that is playing a clip with a lower or
equal priority, and interrupt it to replace it with this new sound.

If all audio channels are busy playing higher priority sounds, then this
new audio clip will not be played.

This command returns the AudioChannel instance that the new sound is
playing on, or *null* if it did not play for any reason.

Example 1:

```ags
aExplosion.Play();
```

plays the *aExplosion* audio clip once using default priority.

Example 2:

```ags
aExplosion.Play(eAudioPriorityHigh);
```

plays the *aExplosion* audio clip once using "High" priority. This will replace any currently playing sound with lower priority, if a spare channel was not found.

Example 3:

```ags
aIntroMusic.Play(eAudioPriorityNormal, eRepeat);
```

plays *aIntroMusic* audio clip repeatedly using default priority.

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:* [`AudioClip.PlayFrom`](AudioClip#audioclipplayfrom),
[`AudioClip.PlayQueued`](AudioClip#audioclipplayqueued),
[`AudioClip.Stop`](AudioClip#audioclipstop),
[`ViewFrame.LinkedAudio`](ViewFrame#viewframelinkedaudio),
[`AudioPriority`](StandardEnums#audiopriority),
[`RepeatStyle`](StandardEnums#repeatstyle)

---

### `AudioClip.PlayFrom`

```ags
AudioChannel* AudioClip.PlayFrom(int position, optional AudioPriority,
                                 optional RepeatStyle)
```

Plays the audio clip, starting from *position*. For the meaning of the
position, see the [`AudioChannel.Seek`](AudioChannel#audiochannelseek) help
page.

Otherwise, this command behaves identically to
[`AudioClip.Play`](AudioClip#audioclipplay). Please see that help page
for more information.

Example:

```ags
aExplosion.PlayFrom(1000);
```

plays the *aExplosion* audio clip, starting from a 1 second offset (if
it is OGG/MP3).

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:* [`AudioClip.Play`](AudioClip#audioclipplay)

---

### `AudioClip.PlayQueued`

*(Formerly known as `PlayMusicQueued`, which is now obsolete)*

```ags
AudioChannel* AudioClip.PlayQueued(optional AudioPriority, optional RepeatStyle)
```

Plays the audio clip, or queues it to be played later if it cannot be
played now.

This command behaves identically to
[`AudioClip.Play`](AudioClip#audioclipplay), except that if there are no
available audio channels, it will queue this audio clip to be played
when a channel becomes available.

Additionally, unlike the Play command, using PlayQueued will not
interrupt an existing audio clip with an equal priority; it will only
interrupt clips with a lower priority.

You can queue up to 10 tracks in the audio queue. Note that if you queue
audio clips to be played after a repeating audio clip, they will never
be played.

Example:

```ags
aExplosion.Play();
aAftermath.PlayQueued();
```

plays the *aExplosion* audio clip, and queues the *aAftermath* sound to
be played afterwards.

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:* [`AudioClip.Play`](AudioClip#audioclipplay)

---

### `AudioClip.PlayOnChannel`

```ags
AudioChannel* AudioClip.PlayOnChannel(int channel, optional AudioPriority, optional RepeatStyle)
```

Plays this audio clip, explicitly putting it on the particular channel, starting with 1 (as channel 0 is reserved for Speech). The maximal number of channels may be found on a [System limits](SystemLimits) page, or using [`System.AudioChannelCount`](System#systemaudiochannelcount) at runtime.

This function disregards any audio type rules, so you may script your own channel logic. 

If the selected audio channel is busy playing higher priority sounds, then this
new audio clip will not be played.

This command returns the AudioChannel instance that the new sound is
playing on, or *null* if it did not play for any reason.

Example:

```ags
aBeep.PlayOnChannel(2, eAudioPriorityHigh, eOnce);
```

plays the *aBeep* audio clip on channel 2.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:* [`AudioClip.Play`](AudioClip#audioclipplay),
[`AudioClip.PlayFrom`](AudioClip#audioclipplayfrom),
[`AudioClip.PlayQueued`](AudioClip#audioclipplayqueued),
[`AudioClip.Stop`](AudioClip#audioclipstop)

---

### `AudioClip.Stop`

```ags
AudioClip.Stop()
```

Stops all currently playing instances of this audio clip.

Example:

```ags
aExplosion.Play();
Wait(40);
aExplosion.Stop();
```

plays the *aExplosion* audio clip, waits 1 second and then stops it
again.

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:* [`AudioClip.Play`](AudioClip#audioclipplay)

---

### `AudioClip.ID`

```ags
readonly int AudioClip.ID
```

Gets the ID of this audio clip. This could be used for diagnostic purposes as well to find same clip in the Game.AudioClips[] array.

Example:

```ags
for (int i = 0; i < System.AudioChannelCount; i++) {
    AudioClip *clip = System.AudioChannels[i].PlayingClip;
    if (clip != null)
        Display("Channel %d has clip %d playing", i, clip.ID);
    else
        Display("Channel %d has no clip on it at the moment", i);
}
```

will display information about clips playing on each audio channel.

*Compatibility:* Supported by **AGS 3.5.0** and later versions.

*See also:* [`Game.AudioClips`](Game#gameaudioclips)

---

### `AudioClip.FileType`

```ags
readonly AudioFileType AudioClip.FileType;
```

Gets the file type of this audio clip. This is useful in conjunction
with the PlayFrom and Seek commands to determine what the position
offset represents.

Example:

```ags
if (aExplosion.FileType == eAudioFileMIDI)
{
    Display("Explosion is a MIDI file!");
}
```

displays a message if aExplosion is a MIDI file

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:* [`AudioChannel.Seek`](AudioChannel#audiochannelseek),
[`AudioChannel.Position`](AudioChannel#audiochannelposition),
[`AudioClip.PlayFrom`](AudioClip#audioclipplayfrom)

---

### `AudioClip.IsAvailable`

*(Formerly known as `IsMusicVoxAvailable`, which is now obsolete)*

```ags
readonly bool AudioClip.IsAvailable;
```

Gets whether this audio clip is available on the player's system.

This will normally be *true*, unless the clip was bundled in the
external AUDIO.VOX file and the player does not have the file on their
system.

You do not normally need to check this property, since the Play command
will silently fail if it cannot find the audio clip to play.

Example:

```ags
if (aExplosion.IsAvailable)
{
    aExplosion.Play();
}
```

checks if the aExplosion audio clip is available, and if so plays it.

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:* [`AudioClip.Play`](AudioClip#audioclipplay)

---

### `AudioClip.ScriptName`

```ags
readonly String AudioClip.ScriptName
```

Gets the script name of the audio clip, which serves as a unique identifier, as set in the AGS Editor.

This may be useful if you have a pointer to some clip stored in your variable, and want to know what it actually is. Normally you don't need a script name, as you have an automatic global variable for each clip in the game, but sometimes you may want to save its script name as a text either to display it somewhere for testing purposes, or keep as a reference. You may later use [AudioClip.GetByName](AudioClip#audioclipgetbyname) function to retrieve the clip by the previously saved script name.

Example:

```ags
void CustomPlay(this AudioClip*)
{
  this.Play();
  System.Log(eLogInfo, "Playing audio clip: %s", this.ScriptName);
}
```

Playing a clip using the `CustomPlay()` method, like `aExampleClip.CustomPlay()`, will log it's script name for debugging purposes.

*Compatibility:* Supported by **AGS 3.6.1** and later versions.

*See also:* [`AudioClip.GetByName`](AudioClip#audioclipgetbyname),
[`Game.AudioClips`](Game#gameaudioclips)

---

### `AudioClip.Type`

```ags
readonly AudioType AudioClip.Type;
```

Gets the type of this audio clip, as initially set in the editor.

The AudioType allows you to group audio clips into areas such as Sound
and Music.

Example:

```ags
if (aExplosion.Type == eAudioTypeMusic)
{
    Display("Explosion is music!");
}
```

displays a message if the *aExplosion* clip is music.

*Compatibility:* Supported by **AGS 3.2.0** and later versions.

*See also:* [`AudioClip.Play`](AudioClip#audioclipplay),
[`Game.IsAudioPlaying`](Game#gameisaudioplaying)
