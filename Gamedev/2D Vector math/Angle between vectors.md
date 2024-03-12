### 2D case

Just like the [dot product](https://en.wikipedia.org/wiki/Dot_product) is proportional to the cosine of the angle, the [determinant](https://en.wikipedia.org/wiki/Determinant) is proportional to its sine. So you can compute the angle like this:

```
dot = x1*x2 + y1*y2      # Dot product between [x1, y1] and [x2, y2]
det = x1*y2 - y1*x2      # Determinant
angle = atan2(det, dot)  # atan2(y, x) or atan2(sin, cos)
```

The orientation of this angle matches that of the coordinate system. In a [left-handed coordinate system](https://en.wikipedia.org/wiki/Cartesian_coordinate_system#In_two_dimensions), i.e. _x_ pointing right and _y_ down as is common for computer graphics, this will mean you get a positive sign for clockwise angles. If the orientation of the coordinate system is mathematical with _y_ up, you get counterclockwise angles as is the convention in mathematics. Changing the order of the inputs will change the sign, so if you are unhappy with the signs just swap the inputs.

### 3D case

In 3D, two arbitrarily placed vectors define their own axis of rotation, perpendicular to both. That axis of rotation does not come with a fixed orientation, which means that you cannot uniquely fix the direction of the angle of rotation either. One common convention is to let angles be always positive, and to orient the axis in such a way that it fits a positive angle. In this case, the dot product of the normalized vectors is enough to compute angles.

```
dot = x1*x2 + y1*y2 + z1*z2    # Between [x1, y1, z1] and [x2, y2, z2]
lenSq1 = x1*x1 + y1*y1 + z1*z1
lenSq2 = x2*x2 + y2*y2 + z2*z2
angle = acos(dot/sqrt(lenSq1 * lenSq2))
```

_Note that some comments and alternate answers advise against the use of `acos` for numeric reasons, in particular if the angles to be measured are small._

### Plane embedded in 3D

One special case is the case where your vectors are not placed arbitrarily, but lie within a plane with a known normal vector _n_. Then the axis of rotation will be in direction _n_ as well, and the orientation of _n_ will fix an orientation for that axis. In this case, you can adapt the 2D computation above, including _n_ into the [determinant](https://en.wikipedia.org/wiki/Determinant) to make its size 3×3.

```
dot = x1*x2 + y1*y2 + z1*z2
det = x1*y2*zn + x2*yn*z1 + xn*y1*z2 - z1*y2*xn - z2*yn*x1 - zn*y1*x2
angle = atan2(det, dot)
```

One condition for this to work is that the normal vector _n_ has unit length. If not, you'll have to normalize it.

#### As triple product

This determinant could also be expressed as the [triple product](https://en.wikipedia.org/wiki/Triple_product#Scalar_triple_product), as [@Excrubulent](https://stackoverflow.com/users/1133479/excrubulent) pointed out in a suggested edit.

```
det = n · (v1 × v2)
```

This might be easier to implement in some APIs, and gives a different perspective on what's going on here: The cross product is proportional to the sine of the angle, and will lie perpendicular to the plane, hence be a multiple of _n_. The dot product will therefore basically measure the length of that vector, but with the correct sign attached to it.

### Range 0 – 360°

Most `atan2` implementations will return an angle from [-π, π] in radians, which is [-180°, 180°] in degrees. If you need positive angles [0, 2π] or [0°, 360°] instead, you can just add 2π to any negative result you get. Or you can avoid the case distinction and use `atan2(-det, -dot) + π` unconditionally. If you are in a rare setup where you need the opposite correction, i.e. `atan2` returns non-negative [0, 2π] and you need signed angles from [-π, π] instead, use `atan2(-det, -dot) - π`. This trick is actually not specific to this question here, but can be applied in most cases where `atan2` gets used. Remember to check whether your `atan2` deals in degrees or radians, and convert between these as needed.