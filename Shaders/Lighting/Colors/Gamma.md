


https://learnopengl.com/Advanced-Lighting/Gamma-Correction

Has this: ![[learn_opengl_gamma.png]]

So physical doubling (think amount of photons) does not _look_ like doubling to us.
We want more resolution in the darker region, which we can use [[pow]] for.

Monitors are given colors which have applied this operation, thus giving the upper row.

## Correction

The typical reference value to use is `2.2`.
When monitors are given colors as `pow(color, 2.2)`, we are now in [[sRGB]].

If our goal is to operate on colors in linear space, we can apply the inverse: `pow(color, 1/2.2)`. When the monitor then applies its correction, it nullifies what we did, and we are left in linear space.

Note that this is the _last_ thing we do in a shader.
We assume all our own operations are always in linear space.
If we don't do this at the very end, any subsequent calculation (that assumes linear space) will not work correctly.


## Missing correction

How can we spot missing gamma correction?

https://www.shadertoy.com/view/7tKXWR <-- this has a good quick shader we can try ourselves.

![[learn_opengl_missing_gamma_correction.png]]

Note the general darker feel of the left part, and that dark areas are hard to see details in.

https://blog.johnnovak.net/2016/09/21/what-every-coder-should-know-about-gamma/

This page has lots of examples:
![[john_novak_incorrect_gamma_spheres.png]]
The right is incorrect and an attempt to get the same result as the left.

![[john_novak_incorrect_gamma_opacity.png]]
The wrong image (right side) shows what happens when alpha blending is performed with incorrect gamma.

![[john_novak_incorrect_gamma_gradients.png]]
Here the bottom is gradients in sRGB, wrong.
Top is correct.


## Double correction

![[learn_opengl_gamma_double_correction.png]]

This shows correct on the left, wrong on the right.
The left was the intended result by the artist- but loading this texture and displaying it means it got gamma corrected twice.
Why? Because the artist was already in sRGB when creating the texture.
So when loaded in the shader it is already in sRGB, and then gets corrected _again_.

