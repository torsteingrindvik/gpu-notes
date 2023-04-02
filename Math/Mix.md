https://registry.khronos.org/OpenGL-Refpages/gl4/html/mix.xhtml

Linear interpolation between two values.

`mix(x, y, a)`  _mixes_ `x` and `y` together, and `a` is the answer to the question "how much of each"?

`a = 0.0` uses _only_ `x`, `a = 1.0` uses _only_ `y`, and everything in-between is a weighted mixture.