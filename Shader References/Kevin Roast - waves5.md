https://github.com/kevinroast/webglshaders/blob/master/waves5.html

Nice looking water and code looks to be simplified well.

# Breakdown


## Function `Hash`

Seems to be a standard 1D->1D hash in range 0..1.
See [[Pseudorandom]].


## Function `Noise`

See [[2D smooth noise]].

#TODO : What's going on with the scaling?

## Function `FAST32_hash_2D`

#TODO: Why is this needed

## Function `Interpolation_C2`

A quintic interpolant.

See [[Interpolants]].


## Function `Perlin2D`

A specific implementation of [[Gradient Noise]].

## Function `FractalNoise`

Does [[fBM]].

Has an interesting idea where the first two octaves use both [[Value Noise]] and [[Gradient Noise]], while later octaves are made more turbulent by using [[Gradient Noise]]'s absolute value.

Also all of these ingredients are offset by time at varying rates, and the input coordinate is rotated [[2D]] each iteration.

## Function `Dist`

A single line, but very interesting!

In [[Signed Distance Functions]] we are used to having some shape and evaluating it to know if we are outside (positive) or inside (negative).

Here, a dot product is used with the world up vector `(0, 1, 0)` and the fractal noise offset in the vertical axis.
#TODO  make some images and figure out how this works.

### Reduced v1

A simpler version is:

```
up = vec3(0., 1., 0.)
query = ?
return dot(query, up)
```

This in general is positive for query vectors that are in the world upper half hemisphere.
So at grazing angles or below that hemisphere, there is no hit.

### Reduced v2 (use `p`)

```
up = vec3(0., 1., 0.)
pv = -vec3(0.) + p
query = vec3(pv, up)
return dot(query, up)
```

`p` is the current position to check. Since this is a point, we "re-interpret" it by starting off at the world origin and pointing to the position; thus vector `pv`.

Now we can tell that in general, points above the `p.y = 0.` plane should not be a hit (dot product is positive).


### Reduced v3 (add bias)

```
up = vec3(0., 1., 0.)
bias = 2.
pv = -vec3(0., bias, 0.) + p
query = vec3(p, up)
return dot(query, up)
```

Now  `p` starts with an offset in the y direction.
This means `pv` isn't pointing upwards until `p.y` > `bias`.
Therefore, we have essentially raised the sea-floor!

## Function `GetNormal`

Standard central differences, see [[Normals]].

## Function `Sky`

Seems to do some [[Specular]] highlights as well as a mixing of colors based on view vertical direction.

## Function `Fog`

Does [[Simple]] fog.

## Function `Shading`

Adds diffuse (based on normals and light dirs),
specular, and fresnel.

Also some reflections.

Lastly distance based attenuation, fog.


## Function `PostEffects`

[[Gamma]] is fixed here.

As well as some interesting settings around [[Saturation]] and [[Contrast]].

Lastly a [[Vignette]] effect is applied.

## Function `GetRay`

A ray is created here in the expected camera based approach; see [[Camera]].

## Function `March`

See [[Ray marching]].
A straight forward implementation returning the position of a hit, as well as a second component indicating a hit or not- which is used to select water or sky.

## Function `main`

Sets up camera, ray origin, ray direction.

Performs ray marching and shades based on hit or miss.
Applies post processing then returns the result.

A slight variation is done if [[Anti aliasing]] is enabled.

### Anti aliasing

If enabled, 16 samples of the scene are taken per frame.

Each sample is taken at an offset from the original fragment coordinate.
The offset is circular and based on an angle given by the current AA sample index.

The angle increments are given as `2*pi / samples`, and each sample increments the angle by this amount, such that by the end a full circle has been sampled.

Each sample accumulates the resulting color.
At the end, the result is divided by the number of samples to counteract this, as expected.