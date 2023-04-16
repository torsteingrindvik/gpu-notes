See _Infinite Repition_ here: https://iquilezles.org/articles/distfunctions/

`vec3 q = mod(p + 0.5 * c, c) - 0.5 * c`.

Examples:
![[graphtoy_repetition.png]]

So `c` tells us how many units between repetitions.
The offset inside the `mod` makes it so that input `0.0` is centered on the slope.
In the same way, the last term makes it centered vertically too.


[[Rainy Bridge]] example use:

`z = (fract(p.z / c - 0.5) - 0.5) * c`

Note that this form is seemingly equivalent to the one above, but does _not_ use modulo.

#TODO It seems to repeat in "camera perspective space"?

