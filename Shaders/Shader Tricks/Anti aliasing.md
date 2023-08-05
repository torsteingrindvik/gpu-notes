
The goal is to eliminate jagged edges.

## Strategy

Render the scene several times.
Offset the screen coordinate around the original point each time.

Add up the colors for each iteration.

In the end, divide by the number of iterations.
Now we have smoothed out each pixel by its neighbours.

## Study: [[Happy Jumping]]

The pseudocode is this:

```glsl
for(m = 0; m < AA; m++)
for(n = 0; n < AA; n++)
{
	// 1.
	o = vec2(m, n) / AA - 0.5

	// 2.
	p = (-iResolution.xy + 2.0 * (fragCoord + o)) / iResolution.y
}
```

Step 1 creates an offset.

For example let's say AA = 2:

| m   | n   | o                |
| --- | --- | ---------------- |
| 0   | 0   | vec2(-0.5, -0.5) |
| 1   | 0   | vec2(0.0, -0.5)  |
| 2   | 0   | vec2(0.5, -0.5)  |
| 0   | 1   | vec2(-0.5, 0.0)  |
| 1   | 1   | vec2(0.0, 0.0)   |
| 2   | 1   | vec2(0.5, 0.0)   |
| 0   | 2   | vec2(-0.5, 0.5)  |
| 1   | 2   | vec2(0.0, 0.5)   |
| 2   | 2   | vec2(0.5, 0.5)                 |

So here we can see that this spans a 3x3 grid over the two coordinates.
Note that this grid has a width (and height) totalling one whole pixel.


Step 2 does the normal trick of normalizing the [[UVs]] to the (-1, 1) range, but offsets the pixel in such a way that we end up sampling at fragment's coordinate center, and the eight pixels around it as well.