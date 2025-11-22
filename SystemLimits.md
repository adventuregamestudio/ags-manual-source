## System limits

This section tells you the maximums for various parts of the system. If
you have been wondering "How many rooms can I have?" or something
similar, chances are this section will answer it.

### System restrictions

| Item | 3.6.0 | 3.6.2 | 3.6.3 |
|-|-|-|-|
| Audio channels | 16 | 16 | 16 |
| Background frames per room | 5 | 5 | 5 |
| Characters | unlimited | unlimited  | unlimited |
| Cursors | unlimited | unlimited | unlimited |
| Custom properties | unlimited | unlimited | unlimited |
| Dialog options per topic | 30 | 30 | 30 |
| Dialog topics | unlimited | unlimited | unlimited |
| Dynamic array elements | 2+ billion | 2+ billion | 2+ billion |
| Fonts | unlimited | unlimited | unlimited |
| GUI | unlimited | unlimited | unlimited |
| GUI Controls per GUI | unlimited | unlimited | unlimited |
| Hotspots per room | 49 | 49 | 49 |
| Inventory items | 300 | 300 | 300 |
| Overlays (at runtime) | unlimited | unlimited | unlimited |
| Regions per room | 15 | 15 | 15 |
| Rooms, total | 1000 | 1000 | 1000 |
| Rooms, state-saving | 300 | 300 | 300 |
| Room Objects per room | 256 | 256 | 256 |
| Script modules | 126 | 1022 | 1022 |
| Script function arguments | 14 | 14 | unlimited |
| Sprites, imported | 90000 | 90000 | 90000 |
| Sprites, dynamic | 2+ billion | 2+ billion| 2+ billion |
| Timers (at runtime) | 20 | 20 | 20 |
| Views (animation sets) | unlimited  | unlimited  | unlimited  |
| Walk-behinds per room | 15 | 15 | 15 |
| Walkable areas per room | 15 | 15 | 15 |

*See also:* [Constants](Constants)

### Integers and Floats limits 

In AGS integers and floats are 32-bit.
- Integers are signed and can be from -2147483648 to +2147483647 (from `-2^31` to `2^31-1`)
- Floats are [a bit more complicated](https://en.wikipedia.org/wiki/Single-precision_floating-point_format): an [IEEE 754](https://en.wikipedia.org/wiki/IEEE_754) 32-bit base-2 floating-point variable has a maximum value of `(2 − 2^(-23)) × 2^127 ≈ 3.4028235 × 10^38`. 
- A float should then be able to reliably represent integer values between -16777216 and 16777216 (from `-2^24` to `2^24`).

In brief, integers have a strict min and max limit, by exceeding which they "wrap" their values (from positive to negative and vice-versa).
Floats, on other hand, do not have a "hard" limit, instead they begin to loose precision as their integer part reaches high numbers. Occasional "even" values may still get stored correctly, but majority of values between them are not guaranteed to be precise.

### Additional considerations

- You should be able to have up to 15 parameters to a function.

- To ensure the pathfinder always works, your walkable areas should always be at least 3 pixels wide.

- AGS currently allows the call stack to be 50 levels deep, so if you have a recursive function that calls itself more often you'll get the "call stack overflow" error. Additionally, the stack size is set at 4 KB so if the recursive function declares a lot of local variables you could reach the limit that way, too.

- There is a total overall limit on the number of functions that can be exported by all plugins added together, which in theory it would be possible for a single plugin to exceed. It's in the region of a couple of hundred though, so it shouldn't be an issue. It wouldn't be too difficult to increase, if the need arose.

We are working on removing existing limitations in the AGS, so some of the remaining restrictions might be loosened or eliminated in the following updates.
