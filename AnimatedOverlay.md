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

Example:

```ags
AnimatedOverlay* rain;

function room_Load()
{
  rain = AnimatedOverlay.CreateAnimated(0, 0);
  rain.Animate(VRAIN, 0, 2, eRepeat);
}
```

will cover the screen with a looping rain animation for as long as the player stays in this room. The variable is declared outside of the function, so that the overlay is not removed when `room_Load` ends.

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

Example:

```ags
AnimatedOverlay* torchFire;

function room_Load()
{
  torchFire = AnimatedOverlay.CreateRoomAnimated(120, 80);
  torchFire.Animate(VFIRE, 0, 4, eRepeat);
}
```

will place a looping fire animation at the room position 120,80, where it is sorted among characters and objects just like any other room element. The variable is declared outside of the function, so that the overlay is not removed when `room_Load` ends.

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

Example:

```ags
function oChimney_Interact(Object *theObject, CursorMode mode)
{
  AnimatedOverlay* puff = AnimatedOverlay.CreateRoomAnimated(150, 40); // just above the chimney
  puff.Animate(VSMOKE, 0, 4, eOnce, eBlock);
}
```

will play a puff of smoke above the chimney once, and wait until it's over. Here the overlay is only needed for the duration of the blocking animation, so it is kept in a local variable, and removed automatically when the function ends.

---

### `AnimatedOverlay.Animating`

```ags
bool AnimatedOverlay.Animating
```

Tells whether the overlay is currently animating.

Example:

```ags
AnimatedOverlay* explosion;

function oBarrel_Interact(Object *theObject, CursorMode mode)
{
  explosion = AnimatedOverlay.CreateRoomAnimated(180, 70); // over the barrel
  explosion.Animate(VEXPLOSION, 0, 3, eOnce, eNoBlock);
}

function room_RepExec()
{
  if (explosion != null && !explosion.Animating)
  {
    explosion.Remove();
    explosion = null;
  }
}
```

will play an explosion without blocking, so that the player keeps control during the animation, and remove the overlay as soon as the animation is over.

---

### `AnimatedOverlay.Frame`

```ags
int AnimatedOverlay.Animating
```

Gets the current frame number during an animation.

Example:

```ags
// make the reflection in the water play the same animation as the fire, in sync
fireReflection.Animate(fire.View, fire.Loop, 4, eRepeat, eNoBlock, eForwards, fire.Frame);
```

will start an animation on the overlay `fireReflection` using the same view and loop that the overlay `fire` is showing, beginning from its current frame. If the fire was started with the same delay of 4, both overlays will animate in sync.

---

### `AnimatedOverlay.Loop`

```ags
int AnimatedOverlay.Loop
```

Gets the current loop number during an animation.

Example:

```ags
// make the reflection in the water play the same animation as the fire, in sync
fireReflection.Animate(fire.View, fire.Loop, 4, eRepeat, eNoBlock, eForwards, fire.Frame);
```

will start an animation on the overlay `fireReflection` using the same view and loop that the overlay `fire` is showing, beginning from its current frame. If the fire was started with the same delay of 4, both overlays will animate in sync.

---

### `AnimatedOverlay.PauseWithGame`

```ags
bool AnimatedOverlay.PauseWithGame
```

Gets whether the overlay's animation is paused when the game is paused.

Example:

```ags
function DescribeOverlay(AnimatedOverlay* ovl)
{
  if (ovl.PauseWithGame)
  {
    System.Log(eLogDebug, "This overlay stops animating while the game is paused.");
  }
  else
  {
    System.Log(eLogDebug, "This overlay keeps animating while the game is paused.");
  }
}
```

will write to the game log whether the given overlay's animation stops when the game is paused. Overlays that are a part of the user interface, such as an animated icon on a pause menu, are usually created with *pauseWithGame* set to false.

---

### `AnimatedOverlay.View`

```ags
int AnimatedOverlay.View
```

Gets the current view number during an animation.

Example:

```ags
// make the reflection in the water play the same animation as the fire, in sync
fireReflection.Animate(fire.View, fire.Loop, 4, eRepeat, eNoBlock, eForwards, fire.Frame);
```

will start an animation on the overlay `fireReflection` using the same view and loop that the overlay `fire` is showing, beginning from its current frame. If the fire was started with the same delay of 4, both overlays will animate in sync.
