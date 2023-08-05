
# Intuition

If we imagine vectors `a` and `b` are two strong beams, we can glue a floor onto them.
Thus we have a plane of sorts (assume the beams are not in the same direction or of zero length).

The `cross` product gives the perpendicular vector of this floor: The direction a vase would point standing on this floor.

But the floor has two sides: Which way is up? This is decided by the right hand rule.

# Most important usage

We likely want a direction only, so:

`normalize(cross(a, b))`.
