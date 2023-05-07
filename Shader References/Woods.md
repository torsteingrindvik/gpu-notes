By iq: https://www.shadertoy.com/view/XsfGD4

List of stuff we need:

* Hashes. All return range 0..1.
	* 21
	* 13
	* 22
* Noise. Uses a noise texture. Use [[Interpolants#Cubic]]. Is [[Value Noise]].
	* 21
	* 31
* [[fBM]]. Range is ~0..1. Uses the noise above. 4 octaves. Frequency doubles, amplitude halves.
	* 31
	* 21

# Function `texturize`

Takes a 2D sampler, a point, and a normal.

TODO.

# Function `treeBase`

This returns the length to a tree base.

It returns `length(fract(pos * a) - b)`.



# Function `terrain`

## `h` term
Gets some layered noise. The domain given to this noise is spread far out such that it isn't extremely hilly.

## `ar` term

This one is interesting..
![[woods_terrain_math.png]]

This shapes the base of the trees somehow.

We start with the yellow signal, which is part of [[Woods#Function `treeBase`]] before squaring it: `fract(pos) - 0.5`, however that function squares the result and returns it, giving us the green signal.
This signal is only positive.

Using `r = max(0., r - bias)` gives us the blue signal.
This squashes the green signal down, but keeps it clamped.

If we square this signal, we get purple, a smoother version (with lower amplitude).

Lastly we use exp on this signal, which returns simply 1 when the input is close to zero, and tends towards zero itself otherwise.

So we have the imaged pink signal, but in two dimensions.

# Function `grassDistr`

# Function `mushroomAnim`

# Function `map`

# Function `map2`

# Function `calNormal`

# Function `intersect`

# Function `softshadow`

# Function `shade`

# Function `moveLights`

Works on an array of global data.
Each contain the position of a light (which contributes light to the scene) as well as a 4th component which controls the light flicker.

The XZ movement is mostly sinusoidal time based, while the Y component samples the terrain in order to follow it (and stay above it).

There is a part that makes the lights avoid colliding with the trees:

```
// make the lights avoid the trees
vec2 chos = 2.5 + 5.0*floor( pos.xz/5.0);
float r = length( pos.xz - chos);
pos.xz = chos + max( r, 1.5 )*normalize(pos.xz-chos);
```

But I don't understand how it works #TODO 

Then we have the flicker part:

```
lpos[i].w = smoothstep(5.0,10.0,iTime)*(0.85 + 0.15*sin(25.0*iTime+23.1*float(i)));
```

The first part is clever, a [[smoothstep]] between seconds 5 through 10 is makes the lights fade in after a little while, which is a nice effect.

# Function `doFirefly`

Takes in `ro, rd, t`; the ray origin, direction, and hit.
Takes in `lpo`- the light position, and `ra` - the radius.

Creates `lv` which is the view vector from the light toward the viewer.

Then checks if the distance to the light is less than the distance to the scene hit.
This is a visibility check. If the light was further away than the hit, this means something is inbetween, and thus we shouldn't show this light (0 is returned).

## Hit
If there is a hit, this is done:

```
float b = dot(rd,lv); // 1
float c = ll - ra; // 2
h = max(b*b-c,0.0)/ra; // 3
h = h*h*h*h; // 4
```

### Part 1

The first uses [[dot]] on the ray direction versus the view vector.
Note that the view vector is not normalized, but the ray direction is.

So in part 3 when `b*b` happens it is really the view vector magnitude squared multiplied by a cosine squared.

Cosine squared has a pi period and ranges between 0..1.



### Part 2

Here we start off with a negative radius bias via `-ra`.
Fighting this is `ll`, which is the distance to the viewer.

### Part 3

We can think of `b*b-c` as: `b*b + ra - ro_li_dist`.
So we add the radius, then subtract the distance to the light.
We also do `b*b`, not sure yet, #TODO .

### Part 4
This is just shaping.
Multiplying with itself is like using [[pow]] with integer exponents.

# Function `mainImage`

