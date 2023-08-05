The SDF of a sphere from a point `p` is `length(p) - radius`.

Why?

Because `length(p)` answers how far we are from the coordinate system's origin when standing at `p`. This is always zero or positive.
If we then subtract something, the result is negative for results less than what we subtracted.
Since all points further away than what we subtracted is now positive and points closer are negative, this is the radius.