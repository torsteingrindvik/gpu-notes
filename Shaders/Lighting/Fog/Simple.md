[[Rainy Bridge]] uses simple fog, of the form:

`fog = 1.0 - pow(clamp(distance / a, 0.0, 1.0), c)`

Where `distance` is distance in terms of how far we have moved into the scene ([[Ray marching]]). 



This value is then used to attenuate colors on objects further into the scene.

![[graphtoy_simple_fog.png]]
See [[pow]].

Here are some examples for values of `c`.
The closer `c` is to 1.0, the more linear the attenuation is.

Examples here use `a = 5`.
Higher `a` values will stretch this shape out further to the right on the x-axis.

![[graphtoy_simple_fog_variant.png]]

Here is an example of `a = 4`, notice that due to `clamp` we get this nicely contained response that goes starts at 1.0 at x=0.0 and goes to 0.0 when x=4.0.

Also if `c > 1.0` we move past the straight line and into being able to push the falloff further towards the top right of the domain.

https://iquilezles.org/articles/fog/

A version of fog is described here as such:

`fogAmount = 1.0 - exp(-distance * b)`.

![[graphtoy_simple_fog_iq_fog_amount.png]]

The idea is similar,  but the variable semantics is now how much fog is there at a certain distance, instead of how much to attenuate.

This amount can then be fed into [[Mix]].