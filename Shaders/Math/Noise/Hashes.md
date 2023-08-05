# Theory

The best resource I have found is

> Hash Functions for GPU Rendering, by Mark Jarzynski, Marc Olano

here: https://jcgt.org/published/0009/03/02/paper.pdf

Has a related ShaderToy: https://www.shadertoy.com/view/XlGcRh (by the paper author).

This paper does statistical testing on the hashes to measure quality, as well as performance.

## The TLDR

The so called `pcg3d` is 3->3, but can be used both as 3->1 or 1->3 because _one_ input dimension change leads to change in _all_ the output dimensions.

3->1 is ok by just ignoring two outputs.
1->3 is ok by just having the missing input dimensions constant.

### pc3d GLSL code

```glsl
// from ShaderToy linked above
uvec3 pcg3d(uvec3 v) {

    v = v * 1664525u + 1013904223u;

    v.x += v.y*v.z;
    v.y += v.z*v.x;
    v.z += v.x*v.y;

    v ^= v >> 16u;

    v.x += v.y*v.z;
    v.y += v.z*v.x;
    v.z += v.x*v.y;

    return v;
}

```

### When to use something else

Even though pc3d isn't slow, it isn't the fastest.
If we require fewer dimensions or can sacrifice quality, we can go for something else (see the paper).

Or if we need multi-byte inputs or something like 4->4 (then pcg4d instead!).


## Thing learned

_Note that the best thing is to just use a function which already supports the dimensionality we are going for._

### Increasing input dimensions

If we have a 1->1 dimension hash, but we want to relate a point in space so 3->1, linear combinations of the point coordinates might be a bad idea:

> hash(aX + bY + cZ + d)

This might lead to repetition patterns.
It is fast though, so if quality is no big deal, this is ok.

Alternative:

> hash(aX + hash(bY + hash(cZ)))

which has higher quality but costs more.

### Increasing output dimensions

If we have a 1->1 for example, we can get 1->3 by doing

> (hash(X + a), hash(X + b), hash(X + c))

with some numbers, e.g. primes.

# Found in the wild

## 11

One `f32` in, one `f32` out.

Can be as simple as:

`fract(sin(id * a) * b)` (used in [[Rainy Bridge]], [[Kevin Roast - waves5]]) where `a` and `b` are large and random.
Range is then in 0..1.


## 21

Seen in [[RetroVision (Revision 2023)]].
Interestingly enough just re-uses the x coordinate to make it a [[Hashes#31]].

## 31

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


## 13

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

