# How to add multiple plots to the same figure

```python
import matplotlib.pyplot as plt
import numpy as np

# Sample data
x = np.linspace(0, 10, 100)
y1 = np.sin(x)         # First curve
y2 = np.cos(x)         # Second curve

# Create subplots: 1 row, 2 columns (horizontal layout)
fig, axes = plt.subplots(1, 2, figsize=(12, 4))

# Plot the first curve on the left subplot
axes[0].plot(x, y1, label='sin(x)', color='blue')
axes[0].set_title('Sine Curve')
axes[0].set_xlabel('x')
axes[0].set_ylabel('y')
axes[0].legend()
axes[0].grid(True)

# Plot the second curve on the right subplot
axes[1].plot(x, y2, label='cos(x)', color='green')
axes[1].set_title('Cosine Curve')
axes[1].set_xlabel('x')
axes[1].set_ylabel('y')
axes[1].legend()
axes[1].grid(True)

# Adjust layout
plt.tight_layout()
plt.show()
```

## the same but with common axis

```python
import matplotlib.pyplot as plt
import numpy as np

# Sample data
x = np.linspace(0, 10, 100)
y1 = np.sin(x)
y2 = np.cos(x)

# Create subplots: 1 row, 2 columns
fig, axes = plt.subplots(1, 2, figsize=(12, 4), sharex=True, sharey=True)

# Plot the first curve
axes[0].plot(x, y1, label='sin(x)', color='blue')
axes[0].set_title('Sine Curve')
axes[0].set_ylabel('y')  # Only show y-label on left plot
axes[0].legend()
axes[0].grid(True)

# Plot the second curve
axes[1].plot(x, y2, label='cos(x)', color='green')
axes[1].set_title('Cosine Curve')
axes[1].set_ylabel('')  # Hide y-label on the right
axes[1].legend()
axes[1].grid(True)

# Set common x-axis label
fig.supxlabel('x', fontsize=12)

# Adjust layout
plt.tight_layout()
plt.subplots_adjust(bottom=0.15)  # Add space for xlabel

plt.show()
```

## add space between rows and cols


You can use plt.subplots_adjust to change the spacing between the subplots.

call signature:

```python
subplots_adjust(left=None, bottom=None, right=None, top=None, wspace=None, hspace=None)
```
The parameter meanings (and suggested defaults) are:

```python
left  = 0.125  # the left side of the subplots of the figure
right = 0.9    # the right side of the subplots of the figure
bottom = 0.1   # the bottom of the subplots of the figure
top = 0.9      # the top of the subplots of the figure
wspace = 0.2   # the amount of width reserved for blank space between subplots
hspace = 0.2   # the amount of height reserved for white space between subplots
```
The actual defaults are controlled by the rc file

[source](https://stackoverflow.com/questions/6541123/improve-subplot-size-spacing-with-many-subplots)