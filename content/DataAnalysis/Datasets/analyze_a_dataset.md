# how to analyze a dataset


## On distributions

### Mean and Standard Deviation
\<put formulas>


### Skewness
Tells how symmetrically distributed is the data.


- Positive skewness means the right tail is bigger than the left, so $mean > median > mode$. 
- Zero skewness means a symmetric distribution, where the mean, median and mode are all at the center point
- Negative skewness means the left tail is bigger than the right, so $mean < median < mode$

$$
skewness=\frac{3(mean-median)}{Stdev}
$$

#### Pearson Skewness
$$
skewness = \frac{n}{(n-1)(n-2)}\sum_{i=1}^n{(\frac{x_i-mean}{Stdev})^3}
$$

Where $n$ is the length of the distribution.

Is it recommended to consider a transformation on highly skewed data.

### Kurtosis
Tells how peaked or flat the distribution is.

A high kurtosis means the data has lots of outliers, so it has heavier tails and sharp peakedness at the center.

$$
kurtosis=\frac{1}{n}\sum_{i=1}^n{(\frac{x_i-mean}{Stdev})^4}
$$

Where $n$ is the length of the dataset and $x_i$ is the individual data point.

$exc.kurtosis = kurtosis-3$

- A distribution with $kurtosis=3$ and $exc.kurtosis=0$ is a perfect normal distribution.
- A distribution with $kurtosis>3$ and $exc.kurtosis>0$ has a sharp peak and heavy tails
- A distribution with $kurtosis<3$ and $exc.kurtosis<0$ has flat peak and light tails.


### Range (min/max)

### NOTE
The distribution can be roughly visualized using the 'Kernel Density Estimate (KDE)' plot, available through Seaborn.

This is particularly useful for visualizing skewness and kurtosis.

```
import seaborn as sns
import matplotlib.pyplot as plt
sns.kdeplot(distribution)
plt.show()
```