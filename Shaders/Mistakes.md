Mostly we mess up.
Let's keep track of things we do wrong in order to learn and look back when we need it.

# Messing up normals

```wgsl
// bad
return normalize(vec3(
	map(p.x + E.xyy).x - map(p.x - E.xyy).x,
	map(p.y + E.yxy).x - map(p.y - E.yxy).x,
	map(p.z + E.yyx).x - map(p.z - E.yyx).x,
));	

// good
return normalize(vec3(
	map(p + E.xyy).x - map(p - E.xyy).x,
	map(p + E.yxy).x - map(p - E.yxy).x,
	map(p + E.yyx).x - map(p - E.yyx).x,
));	
```

I mechanically wrote a normalize function and for some reason I did `p.x`, `p.y`, `p.z` on the first version, which looked _very_ broken as a result.

Bad:
![[normals_bad.png]]

Good:
![[normals_good.png]]
