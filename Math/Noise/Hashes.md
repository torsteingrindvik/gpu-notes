# 1D

One `f32` in, one `f32` out.

Can be as simple as:

`fract(sin(id * a) * b)` (used in [[Rainy Bridge]], [[Kevin Roast - waves5]]) where `a` and `b` are large and random.
Range is then in 0..1.


# 21

Seen in [[RetroVision (Revision 2023)]].
Interestingly enough just re-uses the x coordinate to make it a [[Hashes#31]].

# 31

Seen in [[RetroVision (Revision 2023)]].

Does

```
// f1 arbitrary
p = fract(p * f1)

// f2 arbitrary
// p.swizzle is arbitrary e.g. p.xyz, p.yxz, ...
p += dot(p, p.swizzle + f2)

// p.swizzle2 is e.g. p.xy, p.xz, ...
// p.swizzle1 is the one _not_ used by p.swizzle2
// f3 is arbitrary
return fract(dot(p.swizzle2, vec2(f3)) * p.swizzle1)
```


# 13

Seen in [[RetroVision (Revision 2023)]].

Does:

```
seed = vec4(s, 0, 1, 2)
return vec3(
	h21(s.xy),
	h21(s.xz),
	h21(s.xw),
) * a + b
```

So the input value is used to map into a point in 3D space, where each coordinate is given via [[Hashes#21]].

