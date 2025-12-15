## Matplotlib rcParams

used to control styling and confguration of matplotlib plots
```python
%matplotlib inline

import matplotlib
import matplotlib.pyplot as plt

```

### Change DPI of the figure
```python
matplotlib.rcParams['figure.dpi'] = 300
```

### Set font
```python
# buggy - untested
font = {'family' : 'normal',
        'weight' : 'bold',
        'size'   : 22}
matplotlib.rc('font', **font)

# PROPER WAY
SMALL_SIZE = 8
MEDIUM_SIZE = 10
BIGGER_SIZE = 12

plt.rc('font', size=SMALL_SIZE)          # controls default text sizes
plt.rc('axes', titlesize=SMALL_SIZE)     # fontsize of the axes title
plt.rc('axes', labelsize=MEDIUM_SIZE)    # fontsize of the x and y labels
plt.rc('xtick', labelsize=SMALL_SIZE)    # fontsize of the tick labels
plt.rc('ytick', labelsize=SMALL_SIZE)    # fontsize of the tick labels
plt.rc('legend', fontsize=SMALL_SIZE)    # legend fontsize
plt.rc('figure', titlesize=BIGGER_SIZE)  # fontsize of the figure title

```

### Tight layout by default
```python
# defaults to plt.tight_layout()
matplotlib.rcParams["figure.autolayout"] = True
```
