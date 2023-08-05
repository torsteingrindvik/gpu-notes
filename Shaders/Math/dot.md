# Use cases

## Alignment

We can use the dot product to know if two vectors are pointing in the same direction:

If `dot(a, b) > 0.0`, then they are pointing in the same direction (their angle is no more than 90 degrees).
If negative their angle is larger and thus they point in different directions.

We can also get to know _how much_ they point in the same direction; a sense of the size/amplitude of their "alignmentness".

This is simple as well. A common way to think of it is this:


![[math_insight_dot_product.png]]
https://mathinsight.org/dot_product

The dot product `dot(a, b)` shows the so called _projection of  `a` onto `b`_.
In other words, in the image above if the sun shines directly down (perpendicular) towards `b` , the dot product places `a`'s shadow onto `b`.

But do note that the dot product produces a scalar: The size of `a` scaled by the alignment towards `b`.

From the formula in the image, note that if the angle between the two is zero, you get `cos(0.0) = 1`. Then all we are left with is the size of `a`; it is totally aligned with `b`.

For `cos(pi / 2) = 0.0`, they are perpendicular and thus `a` would give no shadow onto `b`.

Note that the formula normalizes `b` by dividing by its length.
If we did not, the size of `b` would affect the resulting number, which would then change how we would interpret the output value. 
It would then tell us how much the product of _both_ sizes is aligned.