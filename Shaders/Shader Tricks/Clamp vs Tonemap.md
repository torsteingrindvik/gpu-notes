As discussed here: https://www.shadertoy.com/view/3s3GDn
Thanks to `P_Malin`.

![[clamp_vs_tonemap.png]]

The top uses `col = 1 - exp(-col)` and is the "tonemapping" version.
This is useful when colors have values above the range we can actually use.
Notice the nice smooth gradient.

The bottom image uses clamp instead.
We get a sharp cutoff instead.

Important point: The use of clamp is implicit- since the value is above the usable range (e.g. >1.0 float), it will naturally get clamped.