## `MaskPathfinder` functions and properties

MaskPathfinder struct represents a pathfinder that works with walkable areas created using a 8-bit mask bitmap, similar to how it's done in rooms. Such pathfinder will work in the provided mask's coordinates, not screen or room coordinates. Whether these coordinates have any relation to the real room or not - is entirely your decision. MaskPathfinder lets you to both create additional walkable "layers" for the game rooms, or a completely separate imaginary space.

MaskPathfinder is a sub-type of a [Pathfinder](Pathfinder) struct, and inherits all of its functions and properties. It also adds few of its own.

*Compatibility:* The MaskPathfinder struct is supported by **AGS 4.0.0** and later versions.

*See also:*
[Pathfinder](Pathfinder),
[Room.PathFinder](Room#roompathfinder)

### `MaskPathfinder.Create`

```ags
static MaskPathfinder* MaskPathfinder.Create(int mask_sprite)
```

Creates a new MaskPathfinder, initialized with the given 8-bit mask sprite. (If the sprite is not 8-bit, then the pathfinder will not work correctly.)
The created pathfinder will work in the mask's coordinates (not screen or room coordinates).

**IMPORTANT:** When you import sprites in AGS by default they are converted to the game's color depth. If you need to import a 8-bit sprite keeping it 8-bit, then you have to choose "As 8-bit image" in the import options.<br>
If you create this mask using [`DynamicSprite.Create`](DynamicSprite#dynamicspritecreate), then make sure to pass proper "color format" parameter.

Example:

```ags
MaskPathfinder* pathfinder = MaskPathfinder.Create(1000);
Point* path[] = pathfinder.FindPath(20, 30, 200, 160);
```

This creates a new MaskPathfinder object from a sprite 1000, and then finds a path leading from point 20,30 to 200,160.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.

---

### `MaskPathfinder.SetMask`

```ags
MaskPathfinder.SetMask(int mask_sprite)
```

Assigns a new mask to this MaskPathfinder. This has to be a 8-bit sprite to work correctly. The new mask does not have to be the same size as a previous one.

*Compatibility:* Supported by **AGS 4.0.0** and later versions.