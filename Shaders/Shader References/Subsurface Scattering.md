https://www.shadertoy.com/view/4dsGRl

Very pretty!
And ~300 LoC.

# Breakdown

## Function `Isosurface`

This is the SDF rendering.

If we replace this whole thing with e.g. `length(p) - r` ([[Sphere]]) it still works, so no need to focus too hard on this.

## Function `Shading`

### Inputs

* `pos` the current position (of the SDF hit)
* `norm` the normal at this position
* `visibility` 
  * subsurface off: just the dot of the normal and the light direction, or
  * subsurface on: the `SubsurfaceTrace` function; seems to take on the warm color of the subsurface effect
* `rd` the ray direction


### Body

`l` is strange; it uses `0.` as the  [[Mix]] value and therefore only uses `visibility` in the product.
So `l` is in effect the light colour times the visbility.

`fl` is `fillLightColour` scaled between 0 and 1 based on how much the normal is aligned with `fillLightDir`.

`fresnel` is based on `dot(view, norm)`. This term normally makes areas close to edges dark, because those areas tend to have normals that face away from us (the viewer).
Since it is inverted by `1.0 - dot(view, norm)` we instead get a highlight.
The highlight is pushed closer to edges via [[pow]]- remember that the higher the last param given to pow, the longer a signal stays off, then ramps up towards the end (i.e. edges, or surfaces facing away from the viewer).

Then we have `fresnel = mix(0.01, 1.0, min(1.0, fresnel))`.
I think this says is essentially a clamp of the signal between 0.01 and 1.0, ensuring that we always have at least some signal.
We get similar effects by simpy adding 0.01 instead.

The final returned color is two main terms:

1.  `albedo * (l + fl) * (1.0 - fresnel)`
2.  `visibility * specular * 32.0 * lightColour`

The first term is active when the fresnel isn't.

The second term adds [[Specular]] highlights. 

The `fresnel` term is returned in the alpha channel.

## Function `SkyColour`

The "hide cracks" seems to do the opposite for me, so let's ignore that.

The cube map is sampled by the ray direction.
Strange: No matter how we scale `rd` (0.00001 or 10000.0) we get the same background.
But if we negate it or multiply by zero that is not the case.

There is an interesting faking of [[HDR]].

`col = 1.0 / (1.2 - col) - 1.0/1.2`.
The effect seems to be to saturate the colors a lot.

## Functions `ReadKey` and `Noise`

Former is input handling, latter is unused, so let's move on.

## Function `Trace`

Seems to be a relatively normal [[Ray marching]] algorithm.
Uses `continue` instead of `break` - bug?

## Function `SubsurfaceTrace`

Is called when ray marching hits.

Becomes the `visibility` parameter to the `Shading` function.

### Inputs

Takes `ro`, which is the ray tracer hit position, but advanced a unit into the ray direction (so, within the surface?).

Takes `rd` which is given as `lightDir`- the same light used for specular highlights.



### Body