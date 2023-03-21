# 4 Iterative Optimisation & Logistic Regression

## 4.1 Gradient Descent

### 4.1.1 Optimisation Formulations in ML

Training = Fitting = Parameter estimation

Typical formulation

$$
\hat{\theta} \in \argmin_{\theta \in \Theta} L(data, \theta)
$$

- argmin because we want a minimiser not the minimum
  - Note: argmin can return a set (minimiser not always unique!)
- $\Theta$ denotes a model family (including constraints)
- $L$ denotes some objective function to be optimised

### 4.1.2 Log Trick

Instead of optimising $L(\theta)$, try convenient $\log L(\theta)$

We can do this as $\log$ is strictly monotonic function

### 4.1.3 Two Approaches

- Analytic (aka closed form) solution
  - Known only in limited number of cases
  - Use frist-order necessary condition for optimality (Assuming unconstrained, differentiable $L$)
- Approximate iterative solution:
  1. Initialisation: choose starting guess $\theta^{(1)}$, set $i = 1$
  2. Update: $\theta^{(i + 1)} \leftarrow Some Rule[\theta^{(i)}]$, set $i \leftarrow i + 1$
  3. Termination: decide whether to Stop
  4. Go to Step 2
  5. Stop: return $\hat{\theta} \approx \theta^{(i)}$

### 4.1.4 The Gradient

Gradient at $\theta$ defined as $[\frac{\partial L}{\partial \theta_1}, ..., \frac{\partial L}{\partial \theta_p}]^\prime$ evaluated at $\theta$

This means the gradient points to the direction of maximal change of $L(\theta)$ when departing from point $\theta$

Shorthand notation:

- $\nabla L = \left[ \frac{\partial L}{\partial \theta_1}, ..., \frac{\partial L}{\partial \theta_p} \right]^T$ computed at point $\theta$
- Here $\nabla$ is the "nabla" symbol

#### Hessian Matrix

Hessian matrix at $\theta: \nabla L_{ij} = \frac{\partial^2 L}{\partial \theta_i \partial \theta_j}$

### 4.1.5 Gradient Descent and SGD

1. Choose $\theta^{(1)}$ and some $T$
2. For $i$ from $1$ to $T^*$: $\theta^{(i + 1)} = \theta^{(i)} - \eta \nabla L (\theta^{(i)})$
3. Return $\hat{\theta} \approx \theta^{(i)}$

Note: $\eta$ dynamically updated per step

Stochastic gradient descent: two loops

- Outer for loop: each loop (called epoch) sweeps through all training data
- Within each epoch, randomly shuffle training data; then for loop: do gradient steps only on batches of data. Batch size might be $1$ or few

### 4.1.6 Convex Objective Functions

"Bowl shaped" functions

Informally: if line segment between any two points on graph of function lies above or on graph

Formally:

$$
f: D \to \mathbb{R} \text{ is convex if } \\
\forall a, b \in D, t \in [0, 1]: f(ta + (1 - t)b) \leq tf(a) + (1 - t)f(b)
$$

Strictly convex if inequality is strict

Gradient descent on (strictly) convex function gurarnteed to find a (unique) global minimum!

## 4.2 Newton-Raphson

### 4.2.1 Newton-Raphson: Derivation (1D)

Critical points of $L(\theta) =$ Zero-crossing of $L^\prime(\theta)$

Consider case of scalar $\theta$. Starting at given/random $\theta_0$, iteratively:

1. Fit tangent line to $L^\prime(\theta)$ at $\theta_t$
2. Need to find $\theta_{t + 1} = \theta_t + \Delta$ using linear approximation's zero crossing
3. Tagent line given by derivative: rise/run = $- L^{\prime\prime}(\theta_t) = L^\prime(\theta_t)/\Delta$
4. Therefore iterate is $\theta_{t + 1} = \theta_t - L^\prime(\theta_t)/L^{\prime\prime}(\theta_t)$

### 4.2.2 General Case

Newton-Raphson summary

- Finds $L^\prime(\theta)$ zero-crossings
- By successive linear approximations to $L^\prime(\theta)$
- Linear approximations involve derivative of $L^\prime(\theta)$, i.e. $L^{\prime\prime}(\theta)$

Vector-valued $\theta$:

How to fix scalar $\theta_{t + 1} = \theta_{t} - L^\prime(\theta_t)/L^{\prime\prime}(\theta_t)$

- $L^\prime(\theta)$ is $\nabla L(\theta)$
- $L^{\prime\prime}(\theta)$ is $\nabla_2 L(\theta)$
- Matrix division is matrix inversion

General case: $\theta_{t + 1} = \theta_t - (\nabla_2L(\theta_t))^{-1}\nabla L(\theta_t)$

- Pro: May converge faster; fitting a quadratic with curvature information
- Con: Sometimes computationally expensive, unless approximating Hessian

## 4.3 Logistic Regression Model

### 4.3.1 Binary Classification

Given BMI does a patient have type 2 diabetes?

Above is a supervised binary classification

### 4.3.2 Linear Regression

Square loss, points far from boundary have loss squared - even if thery're confidently correct!

Such "outliers" will "pull at" the linear regression

Overall, the least-square criterion looks unnatural in this setting

### 4.3.3 Logistic Regression Model

Probabilistic approach to classification

- $\mathbb{P}(Y = 1 | x) = f(x) = ?$

Problem: the probability needs to be between $0$ and $1$

Logistic function $f(s) = \frac{1}{1 + \exp{(-s)}}$

Logistic regression model

$$
\mathbb{P}(Y = 1 | x) = \frac{1}{1 + \exp(- x^\prime w)}
$$

### 4.3.4 Logistic Regression Linear

Classification rule:

if $(\mathbb{P}(Y = 1 | x) > \frac{1}{2})$ then class "1", else class "0"

Decision boundary is the set of $x$ such that:

$$
\frac{1}{1 + \exp{(-x^\prime w)}} = \frac{1}{2}
$$

### 4.3.5 Effect of Parameter Vector

Vector $w$ is perpendicular to the decision boundary

- That is, $w$ is a normal to the decision boundary
- Note: in this illustration we assume $w_0 = 0$ for simplicity

### 4.3.6 Linear v.s. Logistic Probabilistic Models

Linear regression assumes a Normal distribution with a fixed variance and mean given by linear model

$$
p(y|x) = Normal(x^\prime w, \sigma^2)
$$

Logistic regression assumes a Bernoulli distribution with parameter given by logistic transform of linear model

$$
p(y | x) = Bernoulli(logistic(x^\prime w))
$$

Recall that Bernoulli distribution is defined as

$$
p(1) = \theta \text{ and } p(0) = 1 - \theta \text{ for } \theta \in [0, 1]
$$

Equivalently $p(y) = \theta^y(1 - \theta)^{(1 - y)}$ for $y \in \{0, 1\}$

### 4.3.7 Training as Max-Likelihood Estimation

Assuming independence, probability of data

$$
p(y_1, ..., y_n|x_1, ..., x_n) = \prod^n_{i = 1}p(y_i|x_i)
$$

Assuming Bernoulli distribution we have

$$
p(y_i | x_i) = (\theta(x_i))^{y_i}(1 - \theta(x_i))^{1 - y_i}
$$

where $\theta(x_i) = \frac{1}{1 + \exp{(-x_i^\prime w)}}$

Training: maximise this expression w.r.t. weights $w$

### 4.3.8 Apply Log Trick, Simplify

$$
\begin{align}
  & \log \left( \prod^n_{i = 1} p(y_i | x_i) \right) \notag \\
  = & \sum^n_{i = 1} \log{p(y_i | x_i)} \notag \\
  = & \sum^n_{i = 1} \log{\left( (\theta(x_i))^{y_i} (1 - \theta(x_i))^{1 - y_i} \right)} \notag \\
  = & \sum^n_{i = 1} \left( y_i \log(\theta(x_i)) + (1 - y_i) \log(1 - \theta(x_i)) \right) \notag \\
  = & \sum^n_{i = 1}((y_i - 1) x_i^\prime w - \log(1 + \exp(- x_i^\prime w))) \notag
\end{align}
$$

## 4.4 Logistic Regression: Decision-Theoretic View

### 4.4.1 Cross Entropy

Cross entropy is an information-theoretic method for comparing two distributions

Cross entropy is a measure fo a divergence between reference distribution $g_{ref}(a)$ and estimated distribution $g_{est}(a)$. For distributions:

$$
H(g_{ref}, g_{est}) = - \sum_{a \in A} g_{ref}(a) \log g_{est}(a)
$$

A is support of the distributions, e.g., $A = \{0, 1\}$

### 4.4.2 Training as Cross-Entropy Minimisation

Consider log-likelihood for a single data point

$$
\log p(y_i | x_i) = y_i \log (\theta(x_i)) + (1 - y_i)\log (1 - \theta(x_i))
$$

Cross entropy $H(g_{ref}, g_{est}) = - \sum_a g_{ref}(a) \log g_{est}(a)$

- If reference (true) distribution is
  $$
  g_{ref} (1) = y_i \text{ and } g_{ref}(0) = 1 - y_i
  $$
- With logistic regression estimating this distribution as
  $$
  g_{est}(1) = \theta(x_i) \text{ and } g_{est}(0) = 1 - \theta(x_i)
  $$

## 4.5 Training Logistic Regression: the IRLS Algorithm

### 4.5.1 Iterative Optimisation

Training logistic regression: $w$ maximising log-likelihood $L(w)$ or cross-entropy loss

Bad news: No closed form solution

Good news: Problem is strictly convex, if no irrelevant features $\implies$ convergence!

Look ahead: regularisation for irrelevant features

How does gradient descent work?

- $\mu(z) = \frac{1}{1 + \exp(-z)}$ then $\frac{d \mu}{d z} = \mu(z)(1 - \mu(z))$
- Then $\nabla L(w) = \sum^m_{i = 1} (y_i - \mu(x_i))x_i = X^T(y - \mu)$, stacking instances in $X$, labels in $y, \mu(x_i)$ in $\mu$

### 4.5.2 Iteratively-Reweighted Least Squares

Instead of GD, let's apply Newton-Raphson: IRLS algorithm

$$
\nabla L(w) = X^T(y - \mu)
$$

Differentiate again for Hessian:

$$
\nabla_2 L(w) = -\sum_i \frac{d\mu}{dz_i}x_ix_i^T \\
= - \sum_i \mu(X_i) (1 - \mu(x_i))x_ix_i^T \\
= -X^TMX
$$

where $M_{ii} = \mu_i (1 - \mu_i)$ otherwise $0$

Newton-Raphson then says (now with $M_t, \mu_t$ dependence on $w_t$)

$$
\begin{align}
  w_{t + 1} & = w_t - (\nabla_2 L)^{-1} \nabla L \notag \\
  & = w_t + (X^TM_tX)^{-1}X^T(y - \mu_t) \notag \\
  & = (X^TM_tX)^{-1}[X^TM_tXw_t + X^T(y - \mu_t)] \notag \\
  & = (X^TM_tX)^{-1}X^TM_tb_t \notag
\end{align}
$$

where $b_t = Xw_t + M_t^{-1}(y - \mu_t)$

Each IRLS iteration solves a least squares problem weighted by $M_t$, which are reweighted iteratively!

### 4.5.3 IRLS Intuition: Putting Labels on Linear Scale

- The $y$ are not on linear scale.
- The $b_t$ are a "linearised" approximation to these: the $b_t$ equation matches a linear approx. to $\mu_t^{-1}(y)$
- Linear regression on new labels!

### 4.5.4 IRLS Intuition: Equalising Label Variance

- In linear regression, each $y_i$ has equal variance $\sigma^2$
- Our $y_i$ are Bernoulli, variance: $\mu_t (x_i) [1 - \mu_t(x_i)]$
- Our reweighting standardises, dividing by variances!!
