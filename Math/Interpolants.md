An iq example here: 

https://www.shadertoy.com/view/4dffRH

## Cubic

`i = f*f*(3.0 - 2.0 * f)`

This is what is used in [[smoothstep]].

![[graphtoy_cubic_interpolant.png]]

If `smoothstep(0.0, 1.0, x)` is enabled on the above, the line will exactly overlay `f2(x)`.

## Quintic

`i = f*f*f*(f*(f*6.0-15.0)+10.0)`

![[graphtoy_quintic_interpolant.png]]

Notice that when the coordinate wraps (as it will in shaders when we use [[fract]]) the derivative is still smooth.

I _think_ this is important. #TODO 
