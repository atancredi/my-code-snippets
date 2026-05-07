# CSS clamp


The `clamp()` function takes three parameters:
`clamp(minimum_size, fluid_preferred_size, maximum_size)`

* **Minimum Size:** The font size on mobile devices (e.g., `1rem` or 16px).
* **Maximum Size:** The font size on desktop devices (e.g., `1.5rem` or 24px).
* **Fluid Preferred Size:** A combination of a fixed unit (`rem`) and a dynamic viewport unit (`vw`) that tells the browser *how fast* to scale between the min and max sizes based on your screen breakpoints.

### how to choose sizes

To get that perfect fluid transition between your mobile breakpoint (e.g., 320px) and your desktop breakpoint (e.g., 1200px), we use the point-slope form of a linear equation. 

Here is the formula used to calculate the fluid preferred size:

1.  **Calculate the Slope ($m$):**
    $m = \frac{maxFontSize - minFontSize}{maxViewport - minViewport}$

2.  **Calculate the Y-Axis Intersection ($b$):**
    $b = minFontSize - (minViewport \cdot m)$

3.  **Construct the Fluid Value:**
    Convert the slope to a viewport width percentage ($m \cdot 100$) and combine it with the intersection (converted to `rem`). 
    $Fluid Size = b [rem] + (m \cdot 100) [vw]$

### Practical Implementation
```css
/* Global tokens, calculated for a viewport range of 320px to 1200px */
:root {
  --text-sm: clamp(0.8rem, 0.73rem + 0.35vw, 1rem);
  --text-base: clamp(1rem, 0.91rem + 0.45vw, 1.25rem);
  --text-lg: clamp(1.25rem, 1.14rem + 0.55vw, 1.56rem);
  --text-xl: clamp(1.56rem, 1.4rem + 0.8vw, 2rem);
  --text-2xl: clamp(1.95rem, 1.7rem + 1.25vw, 2.65rem);
}
```
