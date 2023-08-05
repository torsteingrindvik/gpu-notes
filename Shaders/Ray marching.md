Choose a starting point `ro` (ray origin) and a direction `rd` (ray direction).

Define how close we must be in order to decide we hit something, some small value like `eps = 0.001`.

Let's call how far we have traversed into the scene in the ray direction `t`.

If we initialize this to `t = 0.1` (for example) we can get a first evaluation by moving to a point: `p = ro + rd * t`  and getting `d = map(p)`.

If `d <= eps` we are done: `t` is how far we moved, and `d` was the distance at this point to the closest part of the scene.

If not, then `d > eps`, and by definition there is only empty air in at least a sphere of radius `d`.
Why? Because `map` told us the distance to any closest thing is `d`, and if we are standing at `p` and spin around, tracing a path at distance `d`, we have formed a circle (in 2D) or a sphere (in 3D).

Therefore we can safely skip evaluating anything before moving `d` in any direction.
We _could_ evaluate as often as we'd like, but that would be a waste and not give us new information.

So we increase our distance along the direction: `t += d`.

We typically continue this process until we:
1. Are close enough to something (`d <= eps`)
2. Iterate too many times (~500 iterations is common)
3. Traverse too far (`t` gets too large)

The ray marching function typically returns `t`.
Why not just `d`?
Because `t` can be used by the caller to reconstruct _where_ the hit happened: `p_hit = ro + rd * t`.
And this point can be perturbed in order to find the [[Normals]].

Also, any information the `map` function returns might be propagated as well back to the caller.
This might not only be distance `d` but other things such as e.g. `id` to identify _what_ was hit in scene.