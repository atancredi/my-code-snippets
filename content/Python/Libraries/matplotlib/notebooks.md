## Matplotlib inline
To have matplotlib inline and showing nicely add this
```python
%matplotlib inline

import matplotlib
import numpy as np
import matplotlib.pyplot as plt

matplotlib.rcParams['figure.dpi'] = 300

# same as plt.tight_layout() instead of show
matplotlib.rcParams["figure.autolayout"] = True
```