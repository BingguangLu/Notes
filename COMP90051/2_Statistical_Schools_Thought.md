# 2 Statistical Schools of Thought

## 2.1 Frequentist Statistics

Abstract problem

- Given: $X_1, X_2, ..., X_n$ drawn i.i.d. from some distribution
- Want to: identify unknown distribution, or a property of it

Parametric approach ("parameter estimation")

- Class fo models $\{p_{\theta}(x): \theta \in \Theta\}$ indexed by parameters $\Theta$ (could be a real number, or vector, or ...)
- Point estimate $\hat{\theta}(X_1, ..., X_n)$ a function (or statistic) of data

### 2.1.1 Estimator Bias

Frequentists seek good behaviour, in ideal conditions:

Bias:

$$
B_\theta (\hat{\theta}) = E_\theta \left[\hat{\theta}(X_1, ..., X_n)\right] - \theta
$$

### 2.1.2 Estimator Variance

Frequentists seek good behaviour, in ideal conditions:

Variance:

$$
Var_\theta (\hat{\theta}) = E_\theta \left[ (\hat{\theta} - E_\theta[\hat{\theta}])^2 \right]
$$

### 2.1.3 Asymptotically Well Behaved

Asymptotic properties often hold:

- Consistency: $\hat{\theta}(X_1, ..., X_n) \to \theta$ in probability
- Asymptotic efficiency: $Var_\theta \left( \hat{\theta}(X_1, ..., X_n)\right)$ converges to the smallest possible variance of any estimator of $\theta$

NB:

Cramer-Rao lower bound:

$$
Var_\theta (\hat{\theta}) \geq \frac{1}{I(\theta)}
$$

where $I(\theta)$ is the Fisher information of $p_\theta$ for any $\hat{\theta}$

### 2.1.4 Maximum-Likelihood Estimation

A general principle for designing estimators

Involves optimisation

$$
\hat{\theta} (x_1, ..., x_n) \in \argmax_{\theta \in \Theta} \prod^n_{i = 1} p_\theta (x_i)a
$$

The idea is: The best estimate is one under which observed data is most likely

#### MLE Algorithm

1. Given data $X_1, ..., X_n$ define probability distribution, $p_\theta$, assumed to have generated the data
2. Express likelihood of data, $\prod^n_{i = 1} p_\theta (X_i)$ (usually its logarithm)
3. Optimise to find best (most likely) parameters $\hat{\theta}$
   1. take partial derivatives of log likelihood wrt $\theta$
   2. set to $0$ and solve (failing that, use gradient descent)

## 2.2 Statistical Decision Theorey

### 2.2.1 Decision Theorey

Act to maximise utility - connected to economics and operations research

#### Decision Rule

$\delta(x) \in A$ an action space

- E.g. Point estimate $\hat{\theta}(x_1, ..., x_n)$
- E.g. Out-of-sample prediction $\hat{Y}_{n + 1} | X_1, Y_1, ..., X_n, Y_n, X_{n + 1}$

#### Loss Function

$l(a, \theta)$ economic cost, error metric

- E.g. square loss of estimate $(\hat{\theta} - \theta)^2$
- E.g. 0-1 loss of classifier predictions $1\left[y \neq \hat{y}\right]$

### 2.2.2 Risk & Empirical Risk Minimisation

In decision theorey, really care about expected loss:

- Risk:
  $$
  R_\theta [\delta] = E_{X \sim \theta} \left[ l(\delta(X), \theta)\right]
  $$
- Want: Choose $\delta$ to minimise $R_\theta[\delta]$

ERM: Use training set $X$ to approximate $p_\theta$

- Minimise empirical risk $\hat{R}_\theta[\delta] = \frac{1}{n}\sum^n_{i = 1} l(\delta(X_i), \theta)$

### 2.2.3 Decision Theorey v.s. Bias-Variance

Note:

- Bias: $B_\theta (\hat{\theta}) = E_\theta \left[ \hat{\theta}(X_1, ..., X_n) \right] - \theta$
- Variance: $Var_\theta (\hat{\theta}) = E_\theta \left[ (\hat{\theta} - E_\theta[\hat{\theta}])^2 \right]$

Here is the Bias-variance decomposition of square-loss risk:

$$
E_\theta \left[ (\theta - \hat{\theta})^2 \right] = [B(\hat{\theta})]^2 + Var_\theta (\hat{\theta})
$$

## 2.3 Extremum Estimators

### 2.3.1 Extremum Estimators

$$
\hat{\theta}_n (X) \in \argmin_{\theta \in \Theta} Q_n(X, \theta)
$$

for any objective $Q_n()$

### 2.3.2 Consistency of Extremum Estimators

Recall consistency: stochastic convergence to 0 bias

Theorem:

$\hat{\theta}_n \to \theta$ in probability if there is a ("limiting") function $Q()$ such that:

1. $Q()$ is uniquely maximised by $\theta$, that is, no other parameters make $Q()$ as large as $Q(\theta)$
2. The parameter family $\Theta$ is "compact" (a generalisation of the familiar "closed" and "bounded" set, like $[0, 1]$)
3. $Q()$ is a continuous function
4. Uniform convergence: $\sup_{\theta \in \Theta}|Q_n(\theta) - Q(\theta)| \to 0$ in probability

### 2.3.3 A Game Changer

Frequentists: estiators that aren't even correct with infinite data (inconsistent), aren't adequate in practice

Cannot proving consistency for every new estimator.

So many estimators are extremum estimators - general guarantees make it much easy to prove

Asumptotic normality:

- Extremum estimators converge to Gaussian in distribution
- Asymptotic efficiency: the variance of that limiting Gaussian

Practical: Confidence intervals - think error bars!!

## 2.4 Bayesian Statistics

Probabilities correspond to beliefs

Parameters

- Modeled as r.v.'s having distributions
- Prior belief in $\theta encoded by prior distribution $P(\theta)$
  - Parameters are modeled like r.v.'s (even if not really random)
  - Thus: data likelihood $P_\theta(X)$ written as conditional $P(X|\theta)$
- Rather than point estimate $\hat{\theta}$, Bayesians update belief $P(\theta)$ with observed data to $P(\theta|X)$ the posterior distribution

### 2.4.1 Tools of Probabilistic Inference

Bayesian probabilistic inference

- Start with prior $\mathbb{P}(\theta)$ and likelihood $\mathbb{P}(X|\theta)$
- Observe data $X = x$
- Update prior to posterior $\mathbb{P}(\theta|X = x)$

Primary tools to obtain the posterior

- Bayes Rule:
  $$
  \mathbb{P}(\theta|X = x) = \frac{\mathbb{P}(X = x|\theta)\mathbb{P}(\theta)}{\mathbb{P}(X = x)}
  $$
- Marginalisation:
  $$
  \mathbb{P}(X = x) = \sum_t \mathbb{P}(X = x, \theta = t)
  $$

### 2.4.2 How Bayesians Make Point Estimates

They don't, unless forced at gunpoint!

- The posterior carries full information.

But, there are common approaches

- Posterior mean: $E_{\theta|X}[\theta] = \int \theta \mathbb{P}(\theta | X)d\theta$
- Posterior mode: $\argmax_\theta \mathbb{P}(\theta | X)$ (max a posteriori or MAP)
- There're Bayesian decision-theoretic interpretations of these

### 2.4.3 MLE in Bayesian Context

MLE formulation: find parameters that best fit data:

$$
\hat{\theta} \in \argmax_\theta \mathbb{P}(X = x|\theta)
$$

Consider the MAP under a Bayesian formulation:

$$
\begin{align}
    \hat{\theta} & \in \argmax_\theta \mathbb{P}(\theta | X = x) \notag \\
    & = \argmax_\theta \frac{\mathbb{P}(X = x | \theta)\mathbb{P}(\theta)}{\mathbb{P}(X = x)} \notag \\
    & = \argmax_\theta \mathbb{P}(X = x|\theta)\mathbb{P}(\theta)
\end{align}
$$

Prior $\mathbb{P}(\theta)$ weights; MLE like uniform

Extremum estimator:

$$
\max \log \mathbb{P}(X = x| \theta) + \log \mathbb{P}(\theta)
$$

## 2.5 Categories of Probabilistic Models

### 2.5.1 Parametric v.s. Non-parametric models

- Paramteric
  - Determined by fixed, finite number of parameters
  - Limited flexibility
  - Efficient statistically and computationally
- Non-Parametric
  - Number of parameters grows with data, potentially infinite
  - More flexible
  - Less efficient

### 2.5.2 Generative v.s. Discriminative Models

- X's are instances, Y's are labels (supervised setting!)
  - Given: i.i.d. data $(X_1, Y_1), ..., (X_n, Y_n)$
  - Find model that can predict $Y$ of new $X$
- Generative approach
  - Model full joint $\mathbb{P}(X, Y)$
- Discriminative approach
  - Model conditional $\mathbb{P}(Y | X)$ only
