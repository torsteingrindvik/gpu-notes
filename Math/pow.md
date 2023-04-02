When using in the 0..1 range, `pow(x, y)`:

* Is just `x` when `y = 1.0`
* Gives more resolution in the bottom half if `y > 1.0` 
* Gives more resolution in the upper half if `y < 1.0`


# Use cases

One use case is to shape [[Mix]] between two values.
`mix(a, b, pow(thing, n)`.
This was used in [[Density, elevation based]] fog.
`thing` there is how aligned the viewer and the sun was ([[dot]]).

![[graphtoy_pow.png]]

Above we see some values for `pow(thing, n)` for the common use case that `thing` is in the 0..1 range.

If used with `mix`, we can control how much we favor one factor over the other.
For example for `n = 0.1` above, we will more quickly use `b`.

In the other end, for `n = 8.0`, the mix factor is close to `0.0` for longer, which favors `a`.
Towards the end, a quick rise towards `b` happens. This can be used to quickly transition into intense colors for lights, for example.
