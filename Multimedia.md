## Multimedia functions

### `IsSpeechVoxAvailable`

```ags
IsSpeechVoxAvailable()
```

Returns whether the SPEECH.VOX file is being used by the game. This
could be useful if you have an optional speech download pack, and you
want to know whether the player has it or not.

Returns 1 if the speech files are available, 0 if not.

Example:

```ags
if (IsSpeechVoxAvailable()==0)
    Display ("You don't have the voice pack");
```

will display a message if the voice pack is not available.

**NOTE:** This function used to be called IsVoxAvailable, but has now
been renamed to avoid confusion.

*See also:* [`AudioClip.IsAvailable`](AudioClip#audioclipisavailable)
[`Game.ChangeSpeechVox`](Game#gamechangespeechvox),
[`Game.SpeechVoxFilename`](Game#gamespeechvoxfilename),
[Voice speech](VoiceSpeech)

---

### `PlayFlic`

```ags
PlayFlic (int flic_number, int options)
```

Plays a FLI or FLC animation. The game checks for FLICx.FLC and
FLICx.FLI (where X is FLIC_NUMBER) and if it finds one, plays it.
The game is paused while the animation plays.

OPTIONS has these meanings:

    0  player can't skip animation
    1  player can press ESC to skip animation
    2  player can press any key or click mouse to skip animation
    +10 (i.e. 10, 11, 12) do not stretch to full-screen, just play at flc size
    +100 do not clear the screen before starting playback

Example:

```ags
PlayFlic(2, 1);
```

will play flic2 and the player will be able to skip the flic by pressing
the ESC key.

*See also:* [`PlayVideo`](Multimedia#playvideo)

---

### `PlaySilentMIDI`

```ags
PlaySilentMIDI (int music_number)
```

This command is obsolete.

Use the AudioClip.Play command and set its Volume property to 0 to
simulate this effect.

*See also:* [`AudioClip.Play`](AudioClip#audioclipplay),
[`AudioChannel.Volume`](AudioChannel#audiochannelvolume)

---

### `PlayVideo`

```ags
PlayVideo(const string filename, VideoSkipStyle, VideoPlayStyle style)
```

Plays a supported video file. The game is paused while the video plays.

**As of 3.6.0** AGS only supports OGG Theora videos (*.ogv). **Prior to 3.6.0** it also supported AVI and MPG, but only on MS Windows, and if necessary codecs are installed on player's system. Please see the notes in the end of this article for more information on this.

*VideoSkipStyle* defines how the player can skip the video:

    eVideoSkipNotAllowed     player can't skip video
    eVideoSkipEscKey         player can press ESC to skip video
    eVideoSkipAnyKey         player can press any key to skip video
    eVideoSkipAnyKeyOrMouse  player can press any key or click mouse to skip

*VideoPlayStyle* flags have these meanings:

    (0) eVideoPlayDefault: default mode; the video is be played at original size, with its own audio; game's audio is muted.
    (+1) eVideoPlayStretchToScreen: the video will be stretched to full screen, with appropriate black borders to maintain its aspect ratio.
    (+10) eVideoPlayNoAudio (i.e. 10, 11): video's own audio will be muted, and game audio will continue playing.
    (+20) eVideoPlayAudioAndDontMuteGame (i.e. 20, 21): both game's and video's audio will be playing.

**As of 3.6.3** AGS provides a VideoPlayStyle enum. **Prior to 3.6.3** it used a simple `int flags` in its place.

**NOTE:** In 256-color games, PlayVideo is not supported. Please use a
FLC/FLI video with the [`PlayFlic`](Multimedia#playflic) command instead.

Example:

```ags
PlayVideo("Intro.ogv", eVideoSkipEscKey, 11);
```

will play the video "Intro.ogv", allowing the player to skip with ESC if
they've seen it before. The video will be stretched to fullscreen, maintaining aspect ration; but it's audio track will be muted, while game sounds will continue to play.

**OGG Theora format**

As of AGS 3.6.0 PlayVideo can only play OGG Theora (*.ogv). The benefit of this format is that no extra codecs or drivers have to be installed on the user's system in order to play them. OGG Theora videos are automatically included into the game package when you compile the game. For that you need to make sure that file has a .OGV extension and is _placed in your game project folder_.

There are multiple tools out there that may convert another video to OGV. For instance, a good converter for Theora is [ffmpeg](https://ffmpeg.org/) ([Guide](https://trac.ffmpeg.org/wiki/TheoraVorbisEncodingGuide)).

**NOTE:** _Prior to 3.6.0_ there has been a mistake in AGS, because of which the OGG videos could be positioned on screen with a slight negative offset, cutting few rows or columns of pixels. If you are still using AGS 3.5.1 or lower, you have to make sure that your video's frame sizes are multipliers of 16 and actual image placed aligned to the center. For example, if you want 320x200 video then increase canvas to 320x208 and place image at offset (0, 4). This is not a problem anymore in 3.6.0 or later versions.

**Other video formats**

In previous versions of AGS (3.5.1 or lower), PlayVideo could also play AVI, MPG and few other formats, but only on MS Windows, because the playback was implemented through the use of the DirectShow interface. Engine also relied on the codecs installed on player's system (for example, if you created your video with the XVid codec, the player will need to have XVid installed to be able to view it).

These types of video were not included into the game package. You have to place them separately near you game's exe for them to work.

**Cross-Platform Support**

OGG Theora: supported everywhere.
AVI / MPG: Windows only.

*Compatibility:*

OGG Theora supported by **AGS 3.1.1** and later versions.<br>
AVI / MPG support was discontinued in **AGS 3.6.0** and not supported until further notice.<br>
Flag +20 (play both game and video's audio) is supported by **AGS 3.6.0** and later versions.

*See also:* [`PlayFlic`](Multimedia#playflic),
[`VideoSkipStyle`](StandardEnums#videoskipstyle)
[`VideoPlayStyle`](StandardEnums#videoplaystyle)

---

### `SetSpeechVolume`

```ags
SetSpeechVolume (int volume)
```

Sets the volume for in-game speech. VOLUME ranges from 0-255, where 255
is the loudest. The default speech volume is 255 so this function can
only be used to reduce the volume.

Example:

```ags
SetSpeechVolume(200);
```

will set the speech volume to 200.

*See also:* [`Game.SetAudioTypeVolume`](Game#gamesetaudiotypevolume), 
[`Game.SetAudioTypeSpeechVolumeDrop`](Game#gamesetaudiotypespeechvolumedrop), 
[`Game.PlayVoiceClip`](Game#gameplayvoiceclip)
