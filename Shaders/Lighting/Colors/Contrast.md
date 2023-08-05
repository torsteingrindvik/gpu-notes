
# Examples found

In [[Woods]] we see:

```
col = col * 0.7 + 0.3 * col * col * (3.0 - 2.0 * col)
```

For the extremes, we can see that if a color is 0 it will still be 0, and if it's 1 it will still be 1.

![[graphtoy_contrast.png]]

If we look at the last part, we recognize it as [[Interpolants#Cubic]].

Note that if we only used that, darker colors would be pushed to be even darker, and bright colors would be pushed to be even brighter.
This is the orange curve.

The green curve is the unaltered color.

The yellow is what the example uses- mostly linear but with some extra contrast.

If we apply no contrast:
![[woods_contrast_min.png]]

If we apply max contrast (no linear part):
![[woods_contrast_max.png]]