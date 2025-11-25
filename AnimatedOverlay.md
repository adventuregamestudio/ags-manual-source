## `Animated Overlay` functions and properties

Animated Overlay is an extension of [Overlay](Overlay) object. It's essentially same thing, but with animation controls. Being an Overlay's extension, AnimatedOverlay inherits all of its functions and properties too.

Just like regular Overlays, Animated Overlays may be created as *screen overlays* and *room overlays*. This defines the visual layer they are positioned in, and which other game objects they are visually sorted with.

Animated Overlays have a setting that tells whether their animation pauses when the game is paused. Such setting exists, because overlays have multiple uses, and may be a part of the game's visual interface, which is not supposed to be suspended in pause state.

*Compatibility:* AnimatedOverlay type is supported by **AGS 4.0.0** and later versions.

---

### `AnimatedOverlay.CreateAnimated`

```ags
static AnimagedOverlay* AnimatedOverlay.CreateAnimated(int x, int y, optional int slot, optional bool pauseWithGame)
```

Creates a AnimatedOverlay on the screen layer, optionally using a initial sprite, and optionally configured to pause its animation when the game is paused. By default animated overlay is initialized with sprite 0, and *pauses with the game*.

The screen overlays are positioned in the screen graphic layer, and are visually sorted among other screen overlays and GUI using overlay's [ZOrder](Overlay#overlayzorder) property.

**NOTE:** if the AnimatedOverlay variable goes out of scope, the overlay will be removed. Hence, if you want the overlay to last on-screen outside of the
script function where it was created, the `AnimatedOverlay*` variable declaration needs to be at the top of the script and outside any script
functions.

**NOTE:** if the player goes to a different room all active overlays are removed automatically.

---

### `AnimatedOverlay.CreateRoomAnimated`

```ags
static AnimagedOverlay* AnimatedOverlay.CreateRoomAnimated(int x, int y, optional int slot, optional bool pauseWithGame)
```

Creates a AnimatedOverlay on the room layer, optionally using a initial sprite, and optionally configured to pause its animation when the game is paused. By default animated overlay is initialized with sprite 0, and *pauses with the game*.

The room overlays are positioned in the room graphic layer, and are visually sorted among other room overlays, characters, room objects and walk-behinds using overlay's [ZOrder](Overlay#overlayzorder) property, similar to how other objects in rooms use `Baseline` property.

**NOTE:** if the AnimatedOverlay variable goes out of scope, the overlay will be removed. Hence, if you want the overlay to last on-screen outside of the
script function where it was created, the `AnimatedOverlay*` variable declaration needs to be at the top of the script and outside any script
functions.

**NOTE:** if the player goes to a different room all active overlays are removed automatically.

---

### `AnimatedOverlay.Animate`

```ags
void AnimatedOverlay.Animate(int view, int loop, int delay, optional RepeatStyle,
                        optional BlockingStyle, optional Direction, optional int frame, optional int volume)
```

Animates a AnimatedOverlay by playing the specified view loop on it.

The DELAY specifies extra time in ticks between animation frames (in addition to the individual frame delay).

The *RepeatStyle* parameter sets whether the animation will continuously repeat the cycling through the frames. This can be one of the [RepeatStyle](StandardEnums#repeatstyle) values.

For *blocking* you can pass either `eBlock` (in which case the function will wait for the animation to finish before returning), or `eNoBlock` (in which case the animation will start to play, but your script will continue). The default is `eBlock`.

*Direction* specifies an initial way in which the animation plays. You can either pass `eForwards` (the default) or `eBackwards`.

*Frame* lets you specify the starting frame, which should be one of the chosen loop's frame.
Note that for compatibility reasons if direction is eBackwards the animation actually begins with the previous frame. If you pass frame 0 (the default) then it will begin with the last frame in the loop.

*Volume* lets you specify the relative volume in percents (0-100) of the frame-linked sounds for the duration of this animation. It's 100 by default (which means - unchanged).

---

### `AnimatedOverlay.Animating`

```ags
bool AnimatedOverlay.Animating
```

Tells whether the overlay is currently animating.

---

### `AnimatedOverlay.Frame`

```ags
int AnimatedOverlay.Animating
```

Gets the current frame number during an animation.

---

### `AnimatedOverlay.Loop`

```ags
int AnimatedOverlay.Loop
```

Gets the current loop number during an animation.

---

### `AnimatedOverlay.PauseWithGame`

```ags
bool AnimatedOverlay.PauseWithGame
```

Gets whether the overlay's animation is paused when the game is paused.

---

### `AnimatedOverlay.View`

```ags
int AnimatedOverlay.View
```

Gets the current view number during an animation.
