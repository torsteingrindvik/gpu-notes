https://iquilezles.org/articles/fog/

#TODO  try this

We can use [[Simple]] fog and be OK.
But we can also do more.

# Sun direction influence

If instead of a static fog color we do (assume normalized directions):

`sunAmount = max(dot(rayDir, sunDir), 0.0)`
`fogColor = mix(fog_gray, sun_yellow, pow(sunAmount, n))`

See [[dot]], [[pow]].

Now we have a sense of how aligned we are with the sun, and we transition our fog color into a more sun-like color when the alignment gets closer to 1.0.

# Independent scene colors and fog colors

Same article. See [[Atmospheric Scattering]].

The example is:

`extinction = vec3(exp(-d * be.x), exp(-d * be.y), exp(-d * be.z)`
`inscattering = vec3(exp(-d * bi.x), exp(-d * bi.y), exp(-d * bi.z)`

`color = scene * (1.0 - extinction) + fog * inscattering`.

With some sliders we could then experiment with 

# Non-constant density

Last part of the iq article. Now things get extra cool

