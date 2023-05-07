See https://iquilezles.org/articles/fbm/.

# Musgrave

A type of fBM which has _lacunarity_ and _dimension_ to scale each octave's amplitude.

## Example use: "Procedural Wood texture"
By `dean_the_coder`: https://www.shadertoy.com/view/mdy3R1

Pseudocode:

```
for all octaves:
  sum += a * noise(pos)
  a *= pow(lacunarity, -dimension)
  pos *= lacunarity
```

The noise should be in -1..1 range.
