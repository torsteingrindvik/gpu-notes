The UV coordinates in a game engine might be mapped to geometry in a complex way.

For our purposes we just think of UVs as mapped to a rectangle that fits our screen.

The UVs are often made to go from -1 to +1 across both the horizontal and vertical directions in order to have (0, 0) in the middle of the screen.

Sometimes the UVs are modified in such a way that the end result is decoupled from the aspect ratio of the screen.