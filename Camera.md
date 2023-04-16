# Background

In [[Ray marching]] shaders we often set up a camera to serve as the point from which rays originate.

The scene and its [[Signed Distance Functions]] to objects is defined in world coordinates, for example we might have a [[Sphere]] and a [[Box]] close to the origin.
Setting up a camera allows us to move around the point where rays originate (as mentioned) which again allows us to change what we observe.

## Defining the camera

Typically a camera can be defined by:

1. The point in world space where it is located (the origin)
2. The point in world space the camera is looking at (the target)
3. An up direction in the world

1 is needed because we naturally need to know where the camera is.

2 is needed in order to know which way is forward: We use a normalized vector from the camera to this point and call it forward (i.e. the direction the camera is looking).

But why use a point? Why not just set the camera forward to be e.g. `vec3(0.0, 0.0, 1.0)` for example?
This is because using a point allows us to move the camera position around and have it still look at the target point.
The target point is often more interesting than the direction itself.

3 is needed to lock-in the camera's orientation. Because:
Imagine if we only had 1 and 2- origin and target. The target is a picnic scene in a forest.
However, the scene is upside down. Or slightly harder to imagine: The scene is sideways.
Is this a problem? Technically not at all, the camera is where it is supposed to be, and it is looking towards the target, but we did not get what we wanted: A nice picnic.

The same happens in real life if you take a picture with your analog camera rotated sideways: You are standing at the origin of your choice (1), you are taking a picture of a picnic (2), but the camera is rotated such that its local definition of up is "unusual".

The solution then is to write down which way up is.
A common choice is `vec3(0.0, 1.0, 0.0)`: up is positive Y-axis.

## Calculating the rest

When we know position, target, and world up, we can finish the camera's coordinate system.
We will use the [[cross]] product to do this.

First we have:
`forward = normalize(-origin + target)`

We can then create:

`right = cross(forward, world_up)` which is the camera's right direction.

We then need the camera's up direction. Why is this needed when we already have defined a world up? Are they not the same?

No- not always.

Imagine a pole vaulter, but the athlete just looks like a capsule. The athlete defines the `origin`, and the end of the pole the `target`.
The world-up is towards the sky.
The athlete-up is towards the sky too, up through the capsule's vertical axis.

When the athlete is running forwards, the world up and the camera is the same.

When the athlete has stuck the pole to the ground and is pivoting, the world up stays the same, but the capsule's vertical axis is changing to point more and more towards the horizontal instead.

So:

`up = cross(right, forward)`.

# Shader use

Now we have:

* `origin` (point)
* `forward` (vector, normalized)
* `up` (vector, normalized)
* `right` (vector, normalized)

This is then used to map [[UVs]] to ray directions:

`ray_direction = normalize(uv.x * right + uv.y * up + forward)`

What does the above do?
For each UV coordinate (assume range is -1 to +1 on both axes), create a direction for the ray marcher to move towards.
This will be a rectangular cone.

This is then fed to the ray marcher which then evaluates the scene based on the above: `render(origin, ray_direction)`

