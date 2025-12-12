# Statistical Formulas for PCA

This document contains all the essential statistical formulas used in Principal Component Analysis.

---

##  Mean (Average)

The mean represents the central tendency of data.

### Population Mean

$$\mu = \frac{1}{N} \sum_{i=1}^{N} x_i$$

### Sample Mean

$$\bar{x} = \frac{1}{n} \sum_{i=1}^{n} x_i$$

**Where:**
- $\mu$ = population mean
- $\bar{x}$ = sample mean
- $N$ = population size
- $n$ = sample size
- $x_i$ = individual data point

**Example:**
For data: [2, 4, 6, 8, 10]

$$\bar{x} = \frac{2 + 4 + 6 + 8 + 10}{5} = \frac{30}{5} = 6$$

---

##  Variance

Variance measures how spread out the data is from the mean.

### Population Variance

$$\sigma^2 = \frac{1}{N} \sum_{i=1}^{N} (x_i - \mu)^2$$

### Sample Variance

$$s^2 = \frac{1}{n-1} \sum_{i=1}^{n} (x_i - \bar{x})^2$$

**Where:**
- $\sigma^2$ = population variance
- $s^2$ = sample variance
- $x_i$ = individual data point
- $\mu$ = population mean
- $\bar{x}$ = sample mean
- $n-1$ = Bessel's correction (for unbiased sample variance)

**Example:**
For centered data: [-4, -2, 0, 2, 4]

$$s^2 = \frac{(-4)^2 + (-2)^2 + 0^2 + 2^2 + 4^2}{5-1} = \frac{16 + 4 + 0 + 4 + 16}{4} = \frac{40}{4} = 10$$

---

##  Standard Deviation

Standard deviation is the square root of variance (same units as original data).

### Population Standard Deviation

$$\sigma = \sqrt{\frac{1}{N} \sum_{i=1}^{N} (x_i - \mu)^2}$$

### Sample Standard Deviation

$$s = \sqrt{\frac{1}{n-1} \sum_{i=1}^{n} (x_i - \bar{x})^2}$$

**Where:**
- $\sigma$ = population standard deviation
- $s$ = sample standard deviation

**Example:**
From the variance example above:

$$s = \sqrt{10} \approx 3.16$$

---

##  Covariance

Covariance measures how two variables change together.

### Population Covariance

$$\sigma_{xy} = \frac{1}{N} \sum_{i=1}^{N} (x_i - \mu_x)(y_i - \mu_y)$$

### Sample Covariance

$$\text{Cov}(X, Y) = \frac{1}{n-1} \sum_{i=1}^{n} (x_i - \bar{x})(y_i - \bar{y})$$

**Where:**
- $\sigma_{xy}$ = population covariance
- $\text{Cov}(X, Y)$ = sample covariance
- $x_i, y_i$ = paired data points
- $\mu_x, \mu_y$ = population means
- $\bar{x}, \bar{y}$ = sample means

**Interpretation:**
- **Positive covariance**: Variables increase together
- **Negative covariance**: One increases as the other decreases
- **Zero covariance**: No linear relationship

**Example:**
For centered data:
- $x$: [-4, -2, 0, 2, 4]
- $y$: [-20, -10, 0, 10, 20]

$$\text{Cov}(X, Y) = \frac{(-4)(-20) + (-2)(-10) + (0)(0) + (2)(10) + (4)(20)}{4}$$

$$= \frac{80 + 20 + 0 + 20 + 80}{4} = \frac{200}{4} = 50$$

---

##  Covariance Matrix

For multivariate data with $p$ features, the covariance matrix is a $p \times p$ symmetric matrix.

$$\Sigma = \begin{bmatrix}
\sigma_1^2 & \sigma_{12} & \cdots & \sigma_{1p} \\
\sigma_{21} & \sigma_2^2 & \cdots & \sigma_{2p} \\
\vdots & \vdots & \ddots & \vdots \\
\sigma_{p1} & \sigma_{p2} & \cdots & \sigma_p^2
\end{bmatrix}$$

**Where:**
- Diagonal elements $\sigma_i^2$ = variance of feature $i$
- Off-diagonal elements $\sigma_{ij}$ = covariance between features $i$ and $j$

**Matrix Formula:**

$$\Sigma = \frac{1}{n-1} X^T X$$

Where $X$ is the centered data matrix (each column mean = 0).

**Example (2D):**

$$\Sigma = \begin{bmatrix}
10 & 50 \\
50 & 250
\end{bmatrix}$$

- Variance of feature 1: 10
- Variance of feature 2: 250
- Covariance: 50 (features are positively correlated)

---

##  Correlation Coefficient

Correlation is normalized covariance (always between -1 and 1).

### Pearson Correlation Coefficient

$$r_{xy} = \frac{\text{Cov}(X, Y)}{\sigma_x \sigma_y} = \frac{\sum_{i=1}^{n} (x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum_{i=1}^{n} (x_i - \bar{x})^2} \sqrt{\sum_{i=1}^{n} (y_i - \bar{y})^2}}$$

**Where:**
- $r_{xy}$ = correlation coefficient
- $-1 \leq r_{xy} \leq 1$

**Interpretation:**
- $r = +1$: Perfect positive correlation
- $r = 0$: No linear correlation
- $r = -1$: Perfect negative correlation

**Example:**
Using our previous covariance example:

$$r_{xy} = \frac{50}{\sqrt{10} \times \sqrt{250}} = \frac{50}{\sqrt{2500}} = \frac{50}{50} = 1$$

Perfect positive correlation!

---

##  Matrix Notation for PCA

### Centered Data Matrix

$$X_{\text{centered}} = X - \bar{X}$$

Where $\bar{X}$ is a matrix with each row equal to the mean vector.

### Covariance Matrix (Compact Form)

$$\Sigma = \frac{1}{n-1} X_{\text{centered}}^T X_{\text{centered}}$$

### Eigenvalue Decomposition

$$\Sigma v = \lambda v$$

**Where:**
- $\Sigma$ = covariance matrix
- $v$ = eigenvector (principal component direction)
- $\lambda$ = eigenvalue (variance captured by that component)

### Principal Components

$$PC = X_{\text{centered}} \cdot V$$

**Where:**
- $PC$ = principal components (transformed data)
- $V$ = matrix of eigenvectors (each column is one eigenvector)

---

##  Quick Reference Table

| Statistic | Symbol | Formula | Units |
|-----------|--------|---------|-------|
| Mean | $\bar{x}$ | $\frac{1}{n}\sum x_i$ | Same as data |
| Variance | $s^2$ | $\frac{1}{n-1}\sum(x_i - \bar{x})^2$ | Units² |
| Std Dev | $s$ | $\sqrt{s^2}$ | Same as data |
| Covariance | $\text{Cov}(X,Y)$ | $\frac{1}{n-1}\sum(x_i - \bar{x})(y_i - \bar{y})$ | Unit₁ × Unit₂ |
| Correlation | $r$ | $\frac{\text{Cov}(X,Y)}{s_x s_y}$ | Unitless [-1,1] |

---

##  Why These Matter for PCA

1. **Mean**: Used to center data (make mean = 0)
2. **Variance**: Tells us how much each feature varies
3. **Covariance**: Reveals relationships between features
4. **Covariance Matrix**: Contains all pairwise relationships
5. **Eigenvalues**: Show importance of each principal component
6. **Eigenvectors**: Show direction of each principal component

**PCA finds eigenvectors of the covariance matrix to identify directions of maximum variance!**

---

##  Implementation Note

In practice, most libraries handle these calculations automatically:

```python
import numpy as np

# Mean
mean = np.mean(data, axis=0)

# Variance
variance = np.var(data, axis=0, ddof=1)  # ddof=1 for sample variance

# Standard deviation
std = np.std(data, axis=0, ddof=1)

# Covariance matrix
cov_matrix = np.cov(data.T)  # Transpose to get feature-wise covariance

# Correlation matrix
corr_matrix = np.corrcoef(data.T)
```

---

**Remember**: In PCA, we always work with **centered data** (mean = 0) and use the **covariance matrix** to find principal components!
