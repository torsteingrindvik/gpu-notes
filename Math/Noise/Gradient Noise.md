At grid points (i.e. integer coordinates) places vectors pointing in some pseudo-random direction ([[Pseudorandom#2D]]).

A sample point within a grid would dot itself and the random corner gradients.

For example given this:

```
C --- D
|   . |
|     |
A --- B
```

There the small dot is the sample point `p` somewhere in the domain, we have:

```
i = floor(p) // e.g. (2, 50)
f = fract(p) // e.g. (0.8, 0.7)

offset_a = (0., 0.)
offset_b = (1., 0.)
offset_c = (0., 1.)
offset_d = (1., 1.)

// a

// b
gradient_b = hash(i + offset_b)
dot_b = (gradient_b, f - offset_b)

// c

// d
```

a, c, and d are done the same way.

Then, an interpolation is done on the horizontal axis:

* between a and b; call it `i_ab`
* between c and d; call it `i_cd`

Lastly an interpolation between `i_ab` and `i_cd` is done on the vertical axis.

Note that how we do this interpolation has an effect on how good the result looks.

Just using `mix(dot_a, dot_b, x)` means using [[Mix]], which does `(1-x)a + bx`.
This is a linear interpolation which again means the values are interpolated in a "bumpy" way.

A cubic or event quintic interpolation can be used instead, see [[Interpolants]].