## Match/case statement
[PEP 622 – Structural Pattern Matching](https://peps.python.org/pep-0622)

In my opinion the biggest improvement over if statements is that they allow for structural pattern matching, as the PEP is named. Where if statements must be provided a truthy or falsey value, match-case statements can match against the shape/structure of an object in a concise way. Consider the following example from the PEP:

```python
def make_point_3d(pt):
    match pt:
        case (x, y):
            return Point3d(x, y, 0)
        case (x, y, z):
            return Point3d(x, y, z)
        case Point2d(x, y):
            return Point3d(x, y, 0)
        case Point3d(_, _, _):
            return pt
        case _:
            raise TypeError("not a point we support")
```

It not only checks the structure of `pt`, but also unpacks the values from it and assigns them to `x`/`y`/`z` as appropriate.