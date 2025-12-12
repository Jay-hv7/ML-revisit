# Principal Component Analysis (PCA) - Mathematical Explanation

## A Simple Example

Let's say we have data about students with two features:
- **Study Hours per week** (x₁)
- **Test Score** (x₂)

Here's our dataset (5 students):

| Student | Study Hours (x₁) | Test Score (x₂) |
|---------|------------------|-----------------|
| A       | 2                | 60              |
| B       | 4                | 70              |
| C       | 6                | 80              |
| D       | 8                | 90              |
| E       | 10               | 100             |

**Intuition**: We can see that study hours and test scores are highly correlated - more study hours generally mean higher scores. PCA will help us find the main "direction" of this relationship.

---

## Step 1: Organize Data into a Matrix

```
X = [2   60 ]
    [4   70 ]
    [6   80 ]
    [8   90 ]
    [10  100]
```

**Intuition**: Each row is one student, each column is one feature. This is our starting point.

---

## Step 2: Center the Data (Mean Normalization)

Calculate the mean of each feature:
- Mean of Study Hours = (2+4+6+8+10)/5 = **6**
- Mean of Test Score = (60+70+80+90+100)/5 = **80**

Subtract the mean from each data point:

```
X_centered = [-4  -20]
             [-2  -10]
             [0    0 ]
             [2   10 ]
             [4   20 ]
```

**Intuition**: We're moving the data so its center is at the origin (0,0). This is crucial because PCA finds directions from the center of the data. Think of it like adjusting your reference point to the "middle" of all students.

---

## Step 3: Calculate the Covariance Matrix

The covariance matrix tells us how features vary together:

```
Cov(X) = (1/(n-1)) × X_centered^T × X_centered
```

Where n = 5 (number of samples)

```
Cov(X) = (1/4) × [(-4)² + (-2)² + 0² + 2² + 4²        (-4)(-20) + (-2)(-10) + 0×0 + 2×10 + 4×20]
                  [(-4)(-20) + (-2)(-10) + 0×0 + 2×10 + 4×20   (-20)² + (-10)² + 0² + 10² + 20²  ]

Cov(X) = (1/4) × [40   200]
                  [200  1000]

Cov(X) = [10   50 ]
         [50   250]
```

**Intuition**: 
- **Diagonal elements** (10, 250): Variance of each feature. Test scores vary much more than study hours.
- **Off-diagonal elements** (50): Covariance between features. Positive value means they increase together.
- This matrix is symmetric, which is always true for covariance matrices.

---

## Step 4: Calculate Eigenvalues and Eigenvectors

We need to solve: **Cov(X) × v = λv**

Where:
- v = eigenvector (direction)
- λ = eigenvalue (importance)

For our covariance matrix [10, 50; 50, 250], we get:

**Eigenvalues:**
- λ₁ = 258.08 (principal eigenvalue)
- λ₂ = 1.92

**Eigenvectors:**
- v₁ = [0.196, 0.981]ᵀ  (1st principal component direction)
- v₂ = [-0.981, 0.196]ᵀ (2nd principal component direction)

**Intuition**: 
- **Eigenvalues** tell us how much variance is captured in each direction. λ₁ captures most of the variance (258.08 out of 260 total).
- **Eigenvectors** tell us the directions. The 1st eigenvector [0.196, 0.981] points mostly in the "test score" direction with a slight contribution from "study hours". This makes sense because test scores vary more and are correlated with study hours.
- Think of eigenvectors as new axes rotated to align with the data's natural spread.

---

## Step 5: Project Data onto Principal Components

Multiply centered data by eigenvectors:

```
PC₁ = X_centered × v₁ = [-4  -20] × [0.196]  = [-20.424]
                         [-2  -10]   [0.981]    [-10.202]
                         [0    0 ]              [0      ]
                         [2   10 ]              [10.202 ]
                         [4   20 ]              [20.424 ]

PC₂ = X_centered × v₂ = [-4  -20] × [-0.981] = [3.140 ]
                         [-2  -10]   [0.196]    [0.004 ]
                         [0    0 ]              [0     ]
                         [2   10 ]              [-0.004]
                         [4   20 ]              [-3.140]
```

**Intuition**: 
- We've rotated our coordinate system! 
- **PC₁ values** (-20.424 to 20.424) spread students along the main direction of variation. Student A is at -20.424 (low performer), Student E is at +20.424 (high performer).
- **PC₂ values** (very small, around ±3) capture tiny remaining variations perpendicular to the main trend.
- We can now represent each student with just PC₁ if we want, losing very little information!

---

## Step 6: Explained Variance

Calculate how much variance each component explains:

```
Variance Explained by PC₁ = λ₁/(λ₁+λ₂) = 258.08/260 = 99.26%
Variance Explained by PC₂ = λ₂/(λ₁+λ₂) = 1.92/260 = 0.74%
```

**Intuition**: 
- PC₁ captures 99.26% of the variance in the data! 
- We can reduce from 2 dimensions to 1 dimension and keep 99.26% of the information.
- PC₂ only adds 0.74% more information, so we could safely discard it.

---

## Dimensionality Reduction

If we keep only PC₁, our new dataset is:

| Student | PC₁     |
|---------|---------|
| A       | -20.424 |
| B       | -10.202 |
| C       | 0       |
| D       | 10.202  |
| E       | 20.424  |

**We've gone from 2 features to 1 feature while retaining 99.26% of the variance!**

**Intuition**: This single number (PC₁) essentially represents "overall academic performance" - a combination of study hours and test scores. Instead of tracking both separately, we have one composite measure that captures almost all the useful information.

---

## Key Takeaways

1. **Centering**: Shifts data to origin for proper direction finding
2. **Covariance Matrix**: Captures how features vary together
3. **Eigenvalues**: Tell us the importance of each direction (component)
4. **Eigenvectors**: Tell us the actual directions of principal components
5. **Projection**: Transforms data into the new coordinate system
6. **Dimensionality Reduction**: Keep top k components that explain most variance

**The Big Picture**: PCA found that the relationship between study hours and test scores can be summarized by a single "academic performance" dimension, reducing our 2D problem to 1D while keeping 99% of the information!

---

## When to Use PCA

- When features are correlated (redundant information)
- When you have too many features (curse of dimensionality)
- When you need to visualize high-dimensional data
-  When you want to speed up machine learning algorithms
## When to avoid PCA
-  When feature interpretability is crucial (PC₁ = "0.196×study_hours + 0.981×test_score" is less intuitive)
-  When features are already uncorrelated


### PCA in one line
PCA finds a unit vector (basically direction) on which projection of datapoints have max variance
- largest eigenvector (whose eigen value is large) of the covariance matrix always points into the direction of the largest variance of the data, and second largest eigenvector is always orthogonal to the largest eigenvector and points into the direction of the second largest spread of the data


#### PCA - Additional 
 - mean is a measure of central tendency (tells us where is the center of data), but it doesn't give us measure of spread of data
 - variance measures the spread of data but it is a single axis metric, it doesn't give us relation between data on different axis 
 - covariance relation between data on different axis (similar to correlation (here we restrict value between -1 and +1))

- Matrices are used to make transformation on the input vectors
- Eigen vectors after linear transformation doesn't change the direction (magnitude may change )
- Amount by which Eigen vector magnitude changes is known as eigen values

A.V =$\lambda$.V  (Here matrix A is applied on vector V, which gets transformed by a scaling factor $\lambda$ ..no change in direction (ofcourse if magnitude is negative final vector will be in opposite direction))
 ### useful tools/websites
 https://www.geogebra.org/m/YCZa8TAH
 https://www.visiondummy.com/2014/03/eigenvalues-eigenvectors/
 