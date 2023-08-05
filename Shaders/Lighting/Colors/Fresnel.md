
![[fresnel_ground_up.png]]
Image from http://kylehalladay.com/blog/tutorial/2014/02/18/Fresnel-Shaders-From-The-Ground-Up.html.


# A basic fresnel
From https://www.ronja-tutorials.com/post/012-fresnel/
we have:

```
// would max out when looking straight at normals
fr = dot(-rd, n)

// now instead is large at grazing angles
fr = 1. - fr

// push the effect outwards
fr = pow(fr, 10.)

// add on top of what we already have
col += fr * fr_col
```