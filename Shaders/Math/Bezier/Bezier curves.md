# Theory

https://pomax.github.io/bezierinfo/
^ This seems extensive to say the least.

https://pomax.github.io/bezierinfo/#projections
"Projecting a poitn onto a Bezier curve" should be relevant for us?
https://web.archive.org/web/20140713004709/http://jazzros.blogspot.com/2011/03/projecting-point-on-bezier-curve.html

https://hhoppe.com/ravg.pdf
^-- it seems shaders are often using the code found in this paper.

# SDF example

in [[Snail]], we see 

```glsl
vec4 sdBezier( vec3 a, vec3 b, vec3 c, vec3 p )
{
	vec3 w = normalize( cross( c-b, a-b ) );
	vec3 u = normalize( c-b );
	vec3 v =          ( cross( w, u ) );

	vec2 a2 = vec2( dot(a-b,u), dot(a-b,v) );
	vec2 b2 = vec2( 0.0 );
	vec2 c2 = vec2( dot(c-b,u), dot(c-b,v) );
	vec3 p3 = vec3( dot(p-b,u), dot(p-b,v), dot(p-b,w) );

	vec3 cp = getClosest( a2-p3.xy, b2-p3.xy, c2-p3.xy );

	return vec4( sqrt(dot(cp.xy,cp.xy)+p3.z*p3.z), cp.z, length(cp.xy), p3.z );
}
```