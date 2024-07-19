## `Speech` functions and properties

### `Speech.AnimationStopTimeMargin`

*(Formerly known as `game.close_mouth_end_speech_time`, which is now
obsolete)*

```ags
static int Speech.AnimationStopTimeMargin
```

Gets/sets the time margin at which the character talking animation
should stop before before speech time ends. This property is specified
in **game loops** and is set to 10 by default.

**NOTE:** This property only affects the animation if voice mode is
disabled.

Example:

```ags
Speech.AnimationStopTimeMargin = 40;
```

will stop talking animation 40 game loops (1 second with the default
game speed) before speech time ends.

*See also:*
[`Speech.DisplayPostTimeMs`](Speech#speechdisplayposttimems)

---

### `Speech.CustomPortraitPlacement`

```ags
static bool Speech.CustomPortraitPlacement
```

Enables/disables the custom speech portrait placement. When set to
**true** the character portraits are positioned at screen coordinates
defined by [`Speech.PortraitXOffset`](Speech#speechportraitxoffset)
and [`Speech.PortraitY`](Speech#speechportraity). When set to
**false** the portraits will be automatically aligned again.

**NOTE:** This property has no effect if the Lucas-Arts speech style is
used.

*Compatibility:* Supported by **AGS 3.3.0** and later versions.

*See also:* [`Speech.PortraitXOffset`](Speech#speechportraitxoffset),
[`Speech.PortraitY`](Speech#speechportraity)

---

### `Speech.DisplayPostTimeMs`

```ags
static int Speech.DisplayPostTimeMs
```

Gets/sets the extra time the speech will stay on screen after its base
time runs out. Commonly the time the speech lines and portrait stay on
screen is calculated based on the text length - if the text mode is on,
or voice clip length - if the voice mode is on. This property prolongs
the time the speech text and/or portrait is displayed. This property
does not interfere with speech skipping by key or mouse click: players
will still be able to skip speech any time they want (if appropriate
skip mode is enabled). This property is specified in **milliseconds**
and is set to zero by default.

*Compatibility:* Supported by **AGS 3.3.0** and later versions.

*See also:*
[`Speech.AnimationStopTimeMargin`](Speech#speechanimationstoptimemargin)

---

### `Speech.GlobalSpeechAnimationDelay`

*(Formerly known as `game.talkanim_speed`, which is now obsolete)*

```ags
static int Speech.GlobalSpeechAnimationDelay
```

Gets/sets global speech animation delay which affects every character in
game. This property is specified in **game loops** and is set to 5 by
default.

**NOTE:** This property is ignored if lip sync is enabled.

**NOTE:** The property is only used when the
**Speech.UseGlobalSpeechAnimationDelay** is set to **true**. This
property **cannot** be used if the global speech animation delay is
disabled. In that case, the individual character's animation delay is
used instead.

*See also:*
[`Character.SpeechAnimationDelay`](Character#characterspeechanimationdelay),
[`Speech.UseGlobalSpeechAnimationDelay`](Speech#speechuseglobalspeechanimationdelay)

---

### `Speech.PortraitOverlay`

```ags
static Overlay* Speech.PortraitOverlay
```

Retrieves the portrait overlay of a current blocking speech. It's currently only available in eihter
a `repeatedly_execute_always` or `late_repeatedly_execute_always`. It will return null if no
portrait overlay is currently being shown.

Example:

```ags
#define PORTRAIT_YMIN 5
#define PORTRAIT_YMAX 30
int plast = -1;
int pmove = 1;
void repeatedly_execute_always() {
    if (Speech.PortraitOverlay != null) {
        if (plast >= 0) Speech.PortraitOverlay.Y = plast;
        Speech.PortraitOverlay.Y += pmove;
        if (Speech.PortraitOverlay.Y < PORTRAIT_YMIN) pmove = 1;
        if (Speech.PortraitOverlay.Y > PORTRAIT_YMAX) pmove = -1;
        plast = Speech.PortraitOverlay.Y;
    }
}
```

Waves a speech portrait up and down

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:*
[`Speech.TextOverlay`](Speech#speechtextoverlay)

---

### `Speech.PortraitXOffset`

```ags
static int Speech.PortraitXOffset
```

Gets/sets the character's speech portrait **horizontal** offset relative
to screen side. The actual x coordinate of the portrait is calculated
based on whether portrait is to be displayed at the left or right side
of the screen. This property specifies the distance between the screen
side and respected portrait's border.

**NOTE:** The property is only used when the
**Speech.CustomPortraitPlacement** is set to **true**.

*Compatibility:* Supported by **AGS 3.3.0** and later versions.

*See also:*
[`Speech.CustomPortraitPlacement`](Speech#speechcustomportraitplacement),
[`Speech.PortraitY`](Speech#speechportraity)

---

### `Speech.PortraitY`

```ags
static int Speech.PortraitY
```

Gets/sets the character's speech portrait **y** coordinate on screen.

**NOTE:** The property is only used when the
**Speech.CustomPortraitPlacement** is set to **true**.

*Compatibility:* Supported by **AGS 3.3.0** and later versions.

*See also:*
[`Speech.CustomPortraitPlacement`](Speech#speechcustomportraitplacement),
[`Speech.PortraitXOffset`](Speech#speechportraitxoffset)

---

### `Speech.SkipKey`

*(Formerly known as `game.skip_speech_specific_key`, which is now
obsolete)*

```ags
static eKeyCode Speech.SkipKey
```

Gets/sets special key which can skip speech text. This makes all other
keys ignored when speech is displayed on screen, unless eKeyNone is
assigned, in which case any key can be used again.

**NOTE:** The specified key will only skip speech if the appropriate
speech skip style is enabled.

Example:

```ags
Speech.SkipKey = eKeySpace;
```

will assign the "space" key to skip the speech.

*See also:* [`Speech.SkipStyle`](Speech#speechskipstyle)

---

### `Speech.SkipStyle`

*(Formerly known as `SetSkipSpeech`, which is now obsolete)*

```ags
static SkipSpeechStyle Speech.SkipStyle
```

Gets/sets how the player can skip speech lines.

The accepted values are

    eSkipKeyMouseTime  player can skip text by clicking mouse or pressing key
    eSkipKeyTime       player can skip text by pressing key only, not by clicking mouse
    eSkipTime          player cannot skip text with mouse or keyboard
    eSkipKeyMouse      text does not time-out; player must click mouse or press key each time
    eSkipMouseTime     player can skip text by clicking mouse only, not by pressing key
    eSkipKey           text does not time-out; player can skip text by pressing key only
    eSkipMouse         text does not time-out; player can skip text by clicking mouse only

Example:

```ags
Speech.SkipStyle = eSkipTime;
```

will make the player unable to skip the text by pressing a mouse button
or a key.

*See also:*
[`Game.IgnoreUserInputAfterTextTimeoutMs`](Game#gameignoreuserinputaftertexttimeoutms),
[`Game.TextReadingSpeed`](Game#gametextreadingspeed),
[`Speech.SkipKey`](Speech#speechskipkey)

---

### `Speech.SpeakingCharacter`

```ags
static readonly attribute Character* Speech.SpeakingCharacter
```

Gets the currently speaking Character (only works for blocking speech).
It returns null if no character is speaking in a blocking speech.

Differently from checking any character speaking using `Speech.TextOverlay`,
this also works for a blocking voice only speech, and it tells which character is speaking.

*Compatibility:* Supported by **AGS 3.6.2** and later versions.

*See also:*
[`Dialog.CurrentDialog`](Dialog#dialogcurrentdialog),
[`Speech.TextOverlay`](Speech#speechtextoverlay),
[`Character.Speaking`](Character#characterspeaking)

---

### `Speech.Style`

*(Formerly known as `SetSpeechStyle`, which is now obsolete)*

```ags
static eSpeechStyle Speech.Style
```

Gets/sets the way in which speech text is displayed. This modifies the
setting originally set in the editor. SpeechStyle can be:

    eSpeechLucasarts
      speech text over character's head
    eSpeechSierra
      close-up portrait of character
    eSpeechSierraWithBackground
      close-up portrait + background window for text
    eSpeechFullScreen
      QFG4-style full screen dialog pictures

Example:

```ags
Speech.Style = eSpeechSierra;
```

will change the speech style to a close up portrait of the character.

---

### `Speech.TextAlignment`

*(Formerly known as `game.speech_text_align`, which is now obsolete)*

```ags
static Alignment Speech.TextAlignment
```

Sets how text in LucasArts-style speech is aligned.

The accepted values are

    eAlignLeft
    eAlignCentre
    eAlignRight

The default is eAlignCentre.

Example:

```ags
Speech.TextAlignment = eAlignRight;
```

will align the speech text at the right side.

---

### `Speech.TextOverlay`

```ags
static Overlay* Speech.TextOverlay
```

Retrieves the text overlay of a current blocking speech. It's currently only available in eihter a
`repeatedly_execute_always` or `late_repeatedly_execute_always`. It will return null if no
text overlay is currently being shown.

It allows to detect appearance, removal, and change of the blocking speech.
Additionally, calling Speech.TextOverlay.Remove() will work as a speech interrupt.

Example:

```ags
Overlay* lastSpeech;

void late_repeatedly_execute_always() {
    Overlay* curSpeech = Speech.TextOverlay;
    if (lastSpeech == null && curSpeech != null) {
        // speech has started
    } else if (lastSpeech != null && curSpeech == null) {
        // speech is over
    } else if (lastSpeech != null && curSpeech != lastSpeech) {
        // speech changed to the next line
    }
    lastSpeech = curSpeech;
}
```

Detects when the blocking speech has begun, changed to the next line or is over.

*Compatibility:* Supported by **AGS 3.6.0** and later versions.

*See also:*
[`Speech.PortraitOverlay`](Speech#speechportraitoverlay)

---

### `Speech.UseGlobalSpeechAnimationDelay`

```ags
static bool Speech.UseGlobalSpeechAnimationDelay
```

Gets/sets whether speech animation delay should use global setting, as
opposed to individual character's setting. The actual global delay value
is specified with **Speech.GlobalSpeechAnimationDelay**.

Example:

```ags
Speech.UseGlobalSpeechAnimationDelay = true;
```

will make the game use global speech animation delay.

*Compatibility:* Supported by **AGS 3.3.0** and later versions.

*See also:*
[`Character.SpeechAnimationDelay`](Character#characterspeechanimationdelay),
[`Speech.GlobalSpeechAnimationDelay`](Speech#speechglobalspeechanimationdelay)

---

### `Speech.VoiceMode`

*(Formerly known as `SetVoiceMode`, which is now obsolete)*

```ags
static eVoiceMode Speech.VoiceMode
```

Gets/sets whether voice and/or text captions are used in the game.

Valid values for VoiceMode are:

    eSpeechTextOnly      no voice, text only
    eSpeechVoiceAndText  both voice and text
    eSpeechVoiceOnly     voice only, no text

The default is *eSpeechVoiceAndText* if in-game speech is enabled, and
*eSpeechTextOnly* if it is not. Changing this setting changes the
behavior of all [`Say`](Character#charactersay) and
[`Display`](Globalfunctions_Message#display) commands which have a speech file assigned
to them.

**WARNING:** you should only ever use *eSpeechVoiceOnly* at the player's
request to do so, because there is no guarantee that they even have a
sound card and so may not understand what is going on.

Example:

```ags
if (IsSpeechVoxAvailable()==1)
    Speech.VoiceMode = eSpeechVoiceAndText;
```

will set the voice mode to voice and text if the voice pack is
available.

