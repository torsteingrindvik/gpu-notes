Does not exist in WGSL. The goal is to only allow values in the 0..1 range,
so can be implemented by making a define where `sat(x) = clamp(x, 0., 1.)`.
