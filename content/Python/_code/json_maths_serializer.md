# Numpy stuff to JSON

```python
from json import JSONEncoder
import numpy as np
class MathEncoder(JSONEncoder):
    def default(self, o):
        if isinstance(o, np.ndarray):
            return o.tolist()
        if isinstance(o, np.float32) or isinstance(o, np.float64):
            return float(o)
        return o.__dict__
```