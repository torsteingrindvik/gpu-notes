# [[Rainy Bridge]]

This one uses uses the time and the vertical coordinate to scroll the rain.

The input point is multiplied by a large factor in order to have lots of rain.
The horizontal axis' id ([[floor]]) is used as an input to a hash (see [[Hashes]]) in order to get a random vertical offset.


```
x = cos(p.x * a)
y = sin(p.y * b) - c
drop = length(vec2(x, y))

return clamp(1.0 - drop, 0.0, 1.0)
```

This creates a droplet which is thin horizontally and long vertically.
It's inverted to have the max value at the center.

Rain is then accumulated over several steps.