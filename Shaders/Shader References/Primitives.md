https://www.shadertoy.com/view/Xds3zN

The one reference for [[Signed Distance Functions]].

# Lighting

![[primitives_iq.png]]

Even though the scene is meant to display 3D SDFs, the lighting is still very impressive (as expected from iq).

It has 

* Ambient occlusion
* Reflections
	* See the brownish reflection on the floor of nearby SDFs
	* See e.g. the left side sphere material which reflects nearby SDFs
* Soft shadows
	* Very nice fade as the shadows get longer
* A mix of glossy and matte objects


Let's see step by step how it's applied.

## Background
```glsl
vec3 col = vec3(0.7, 0.7, 0.9) - max(rd.y,0.0)*0.3;
```
![[primitives_background.png]]
Interestingly it seems the ray direction Y coordinate is always negative, so `max(rd.y, 0.0)` is always 0.

Conclusion: The background is a white/gray-ish color with a blue lean.

## Material

![[primitives_material_colors.png]]

```glsl
col = 0.2 + 0.2*sin( m*2.0 + vec3(0.0,1.0,2.0) );
```

Here, a base material color is assigned based on `m` which is the raycast object id.
Notice the range of each color channel is from 0.0 to 0.4, which means we don't get a hugely saturated result.

Each channel gets a phase offset via the `vec3`. If not the color channels would be in sync, and that leaves only shades of gray:

![[primitives_material_colors_sync.png]]

## Plane checkers
#TODO https://iquilezles.org/articles/checkerfiltering/

## Occlusion
![[primitives_ao.png]]

Next we add some [[Regular Ambient Occlusion]].

It has a bias based on the normal:

```glsl
float ao = ...

return ao * (0.5 + 0.5 * normal.y)
```

What is the effect of this?
Let's remove that and see what happens:

![[primitives_ao_no_bias.png]]

Now we have "pure" AO which only has the signal made by looking around the scene via the normal.

If we keep the bias, each object gets an inherent color dampening effect as normals turn downwards.

Here is colors and AO without bias:
![[primitives_ao_colors_no_bias.png]]

And here with bias:
![[primitives_ao_colors.png]]

This normals bias provides a lot of depth.
Compare for example the blue triangle where an edge is invisible without the bias.
The green/beige cylinder too becomes very flat without.l

## Sun

### Soft shadows

[[Soft Shadows]]:
![[primitives_ss.png]]

### Diffuse

The soft shadows are multiplied by a [[dot]] product between the normals and the light direction, giving us:
![[primitives_ss_hmm.png]]

### Specular

Here is a color-less exaggerated version:

![[primitives_ss_spe.png]]


## Sky

Two signals here, `dif` and `spe`.

### Dif 
Likely means "diffuse".

```glsl
dif = sqrt(clamp(0.5 + 0.5*n.y, 0.0, 1.0));
dif *= occ;
```

Without the occlusion factor:
![[Pasted image 20230507222724.png]]
This signal is based on alignment towards the sky.
The more the normals align with the sky, the stronger the signal.


With the occlusion factor:

![[Pasted image 20230507222737.png]]

### Spe
Likely means "specular".
![[Pasted image 20230507223017.png]]

Quite the signal!

### Reflective shadows

This is the `calcSoftshadow(pos, ref, 0.02, 2.5)` part.

![[Pasted image 20230507223311.png]]
What does it do?
Calculating [[Soft Shadows]] is done in the normal way: Start at some point and accumulate min distances along a direction; if the accumulation is small enough it will be a hard or soft shadow (based on how much was accumulated).

Normally we go along the SDF normal- now we use the `rd, nor` reflected vector instead.
How do we interpret that?

* Start from the camera.
* Send a ray.
* Find the direction the ray reflects via the normal of the hit surface.
* Start accumulating along this direction: Lots of small SDFs (or a clean hit) means shadows.


## Back

This one adds some lighting to one side, the back is like this:
![[primitives_back_back.png]]

The front is this:
![[primitives_back_col.png]]

If we isolate the signal:
![[primitives_back_bw.png]]

How is it set up to only produce a signal on one side?

This is done via a `clamp(dot(n, normalize(vec3(0.5, 0.0, 0.6))), 0.0, 1.0)` factor, which tells us to only keep anything aligned in the direction of the `vec3`. 

Also, slightly hard to see, there is a factor `clamp(1.0 - pos.y, 0.0, 1.0)` which says that negative or zero world coordinates should be kept as-is, but higher up (towards p.y = 1.0 and above) should be darkened.

Therefore the last image notice the front row SDF objects have brighter parts close to the floor, _except_ that ambient occlusion is also added which hides this effect _very_ close to the floor.

## Sss

Finally a [[Fresnel]] is added:

![[primitives_fresnel.png]]

Here it is exaggerated. 
Occlusion is done to this effect too as usual.

The fresnel is computed by:

```glsl
f = pow(clamp(1.0 + dot(n, rd), 0.0, 1.0), 2.0)
```

So when the camera is sending rays straight towards the normals, the effect cancels out.

If we inspect `rd.x` via 

```glsl
pow(abs(rd.x, 2.))
```

we get:
![[primitives_fresnel_rd_x_abs.png]]

so then we understand the horizontal fresnel effect.

_Actually_... I think the easiest way to understand this fresnel computation is to realize that `1.0 + dot(n, rd)` is only negative when `rd` is pointing opposite from `rd`.
Which means that when `n, rd` align _and_ when they are perpendicular will the signal be kept.

If we think of `rd` as a sort of cone sent out from the camera, this means the fresnel signal will be kept on the backside (which we will never see) and fade when the normals start turning towards the cone.

## Mix