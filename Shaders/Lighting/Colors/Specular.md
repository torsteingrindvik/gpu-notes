Specular highlights.

These are based on the alignment of the ray direction (normally) and the light direction, _not_ the surface normals.

Often the `view` vector is `normalize(-rd)`, so pointing towards the viewer.

Then we see `h = dot(view, light_dir)` to get a measure of how aligned the view vector is to the light direction- the half vector.

This is then used against the scene normals: `s = dot(h, n)`.
Note that this maxes out when the view direction looks like a reflection of the light direction.
The half vector in this situation points in the direction of the normal, and the [[dot]] maxes out when totally aligned.

Now we can start shaping this signal:

`s = pow(max(0., s), specular_hardness)`.