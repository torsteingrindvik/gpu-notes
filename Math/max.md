Doing `max(a, b)` means returning the bigger number. 

If `a` and `b` are SDF evaluations, the operation gives back `a` but with `b` carved out.

(Experimentally if I already had `a` then I needed to do `max(a, -b)`).

Due note as mentioned in [[Greek Temple]], the normals of the resulting operation will have to be considered.
If you carve out a plane from `a`, then the direction of the normals in the intersection is based on the sign of the plane.

#TODO  show this visually.