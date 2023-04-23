https://www.shadertoy.com/view/DdVSzR

My main motivation here was the procedural wood which I think looks great.

# Function `matWood`

Let's try understanding this.

We need:

* `fbmDistorted`
* `musgraveFbm`
* `waveFbmX`
* `remap01`
* And [[Mix]].

## Function `fbmDistorted`

This [[fBM]] returns values in the 0..1 range at some point `p` where a user can provide octaves, roughness, distortion.

It displaces `p` via a function `n31` three times by  `randomPos` , then distorts that, before calling `fbm` with the new `p`.

### Function `n31`

This links to another place: https://www.shadertoy.com/view/lstGRB
This seems to be [[Value Noise#3D]].


### Function `randomPos`

Takes in a seed, uses this to essentially create a [[Hashes#31]] for each coordinate of `p`.
Then we head into `fbm` proper.

### Function `fbm`

Returns range 0..1 noise over the amount of octaves given.
Amplitude decreases per octave, while `p` doubles.


## Function `musgraveFbm`

See [[fBM#Musgrave]].

## Function `remap01`

Remaps the input to the 0..1 range via the limits given by the input parameters.