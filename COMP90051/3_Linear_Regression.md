# 3 Linear Regression

## 3.1 Linear Regression via Decision Theory

Adopt a linaer relationship between response $y \in \mathbb{R}$ and an instance with features $x_1, ..., x_m \in \mathbb{R}$

$$
\hat{y} = w_0 + \sum^m_{i = 1} x_iw_i
$$

Where $w_0, ..., w_m \in \mathbb{R}$ denote weights (model parameters)

Trick: add a dummy feature $x_0 = 1$ and use vector notation

$$
\hat{y} = \sum^m_{i = 0} x_iw_i = X^Tw
$$

## 3.2 Linear Regression via Frequentist Probabilistic Model

### 3.2.1 Regression as a probabilistic model

Assume a probabilistic model: $Y = X^Tw + \varepsilon$

- Here $X, Y$ and $\varepsilon$ are r.v.'s
- Variable $\varepsilon$ encodes noise

Next, assume Gaussian noise (indep. of $X$): $\varepsilon \sim \mathcal{N}(0, \sigma^2)$

Recall that $\mathcal{N}(x; \mu, \sigma^2) = \frac{1}{\sqrt{2 \pi \sigma^2}} \exp \left( -\frac{(x - \mu)^2}{2\sigma^2} \right)$

Therefore

$$
p_{w, \sigma^2} (y|x) = \frac{1}{\sqrt{2 \pi \sigma^2}} \exp \left( -\frac{(y - x^Tw)^2}{2 \sigma^2} \right)
$$

### 3.2.2 Parametric Probabilistic Model

Using simplified notation, discriminative model is:

$$
p(y | x) = \frac{1}{\sqrt{2 \pi \sigma^2}} \exp \left( -\frac{(y - x^Tw)^2}{2\sigma^2} \right)
$$

Unknown parameters: $w, \sigma^2$

Given observed data $\{(X_1, Y_1), ..., (X_n, Y_n)\}$, we want to find parameter values that "best" explain the data

Maximum-likelihood estimation: choose parameter values that maximise the probability of observed data.

### 3.2.3 Maximum Likelihood Estimation

Assuming independence of data points, the probability of data is

$$
p(y_1, ..., y_n|x_1, ..., x_n) = \prod^n_{i = 1} p(y_i|x_i)
$$

For $p(y_i|x_i) = \frac{1}{\sqrt{2\pi\sigma^2}}\exp\left(-\frac{(y_i - x_i^Tw)^2}{2\sigma^2}\right)$

Log trick: Instead of maximising this quantity, we can maximise its logarithm

$$
\sum^n_{i = 1} \log p(y_i|x_i) = -\frac{1}{2\sigma^2} \sum^n_{i = 1}(y_i - x_i^Tw)^2 + C
$$

where $C$ doesn't depend on $w$ (as it's a constant)

Under this model, maximising log-likelihood as a function of $w$ is equivalent to minimising the sum of sqaured errors.

### 3.2.4 Method of Least Squares

Training data: $\{(x_1, y_1), ..., (x_n, y_n)\}$

For convenience, place instances in rows (so attributes go in columns), representing training data as an $n \times (m + 1)$ matrix $X$, and $n$ vector $y$

Probabilistic model/ decision rule assumes $y \approx Xw$

To find $w$, minimise the sum of squared errors

$$
L = \sum^n_{i = 1} \left( y_i - \sum^m_{j = 0} X_{ij}w_j \right)^2
$$

Setting derivative to zero and solving for $w$ yields

$$
\hat{w} = (X^TX)^{-1}X^Ty
$$

- This system of equations called the normal equations
- System is well defined only if the inverse exists

### 3.2.5 Bayesian Derivation

Later: Return of linear regression

Fully Bayesian, with a posterior:

- Bayesian linear regression

Bayesian (MAP) point estimate of weight vector:

- Adds a penalty term to sum of squared losses
- Equivalent to $L_2$ "regularisation"
- Called: ridge regression

## 3.3 Basis Expansion

### 3.3.1 Basis Expansion for Linear Regression

Real data is likely to be non-linear

What if we still wanted to use a linear regression?

- Simple, easy to understand, computationally efficient, etc.

How to marry non-linear data to a linear method?

### 3.3.2 Transform the Data

The trick is to transform the data: Map data into another features space, s.t. data is linear in that space

Denote this transformation $\varphi: \mathbb{R}^m \to \mathbb{R}^k$. If $x$ is the original set of features, $\varphi(x)$ denotes new feature set

#### Example 1: Polynomial Regression

Define:

$$
\varphi_1(x) = x \\
\varphi_2(x) = x^2
$$

Next apply linear regression to $\varphi_1, \varphi_2$

$$
y = w_0 + w_1\varphi_1(x) + w_2\varphi_2(x)
$$

and this is quadratic regression

More generally, obtain polynomial regression if the new set of attributes are powers of $x$

Similar idea basis of autoregression for time series

#### Example 2: Linear Classification

Define transformation as

$$
\varphi_i(x) = ||x - z_i||
$$

where $z_i$ some pre-defined constants

### 3.3.3 Radial Basis Functions

A radial basis function is a function of the form $\varphi(x) = \psi(||x - z||)$, where $z$ is a constant

### 3.3.4 Challenges of Basis Expansion

In the above examples, one limitation is that the transformation needs to be defined beforehand

- Need to choose the size of the new feature set
- If using RBFs, need to choose $z_i$

Regarding $z_i$, one can choose uniformaly spaced points, or cluster training data and use cluster centroids

Another popular idea is to use training data $z_i = x_i$

- E.g., $\varphi_i(x) = \psi(||x - x_i||)$
- However, for large datasets, this results in a large number of features

### 3.3.5 Further Directions

One idea is to learn the transformation $\varphi$ from data

- E.g., Artificial Neural Networks

Another powerful extension is the use of the kernel trick

- "Kernelised" methods, e.g., kernelised perceptron

Finally, in sparse kernel machines, training depends only on a few data points

- E.g., SVM
