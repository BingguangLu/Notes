# 5 Regularisation

Major technique & theme, throughout ML

Addresses one or more of the following related problems:

- Avoids ill-conditioning (a computational problem)
- Avoids overfitting (a statistical problem)
- Introduce prior knowledge into modelling

This is achieved by augmenting the objective function

In this lecture: we cover the first two aspects. We will cover more of regularisation throughout the subject

## 5.1 The Problem with Ill-posed Problems

### 5.1.1 Irrelevant Features

#### Example

Linear model on three features, first two same

- $X$ is matrix on $n = 4$ instances (rows)
- Model: $y = w_1x_1 + w_2x_2 + w_3x_3 + w_0$
- First two columns of $X$ identical
- Feature 2 (or 1) is irrelevant

#### Problems 

In example, suppose $[\hat{w}_0, \hat{w}_1, \hat{w}_2, \hat{w}_3]^T$ is "optimal"

For any $\delta$ new $[\hat{w}_0, \hat{w}_1 + \delta, \hat{w}_2 - \delta, \hat{w}_3]^T$ get

- Same predictions!
- Same sum of squared errors!

Problems this highlights

- The solution is not unique
- Lack of interpretability
- Optimising to learn parameters is ill-posed problem

#### Irrelevant Features in General

Extreme case: features complete clones

For linear models, more generally:

- Featrue $X_{\cdot j}$ is irrelevant if
- $X_{\cdot j}$ is a linear combination of other columns
  $$
  X_{\cdot j} = \sum_{l \neq j}\alpha_lX_{\cdot l}
  $$
  for some scalars $\alpha_l$. Also called multicollinearity
- Equivalently: Some eigenvalue of $X^TX$ is zero

Even near-irrelevance/colinearity can be problematic (i.e. small eigenvalues of $X^TX$)

Not just a pathological extreme, easy to happen!

### 5.1.2 The Problem with Lack of Data

#### Example

Extreme example:

- Model has two parameters (slope and intercept)
- Only one data point

### 5.1.3 The Problem with Ill-posed Problems

In both examples, finding the best parameters becomes an ill-posed problem

This means that the problem solution is not defined

- In our case $w_1$ and $w_2$ cannot be uniquely identified

Remember normal equations solution of linear regression:

$$
\hat{w} = (X^TX)^{-1}X^Ty
$$

With irrelevant/multicolinear features, matrix $X^TX$ has no inverse

## 5.2 Regularisation in Linear Models

### 5.2.1 Re-conditioning the Problem

Regularisation: introduce an additional condition into the system

The original problem is to minimise $||y - Xw||^2_2$ 

The regularised problem is to minimise

$$
||y - Xw||^2_2 + \lambda ||w||^2_2 \text{ for } \lambda > 0
$$

The solution is now

$$
\hat{w} = (X^TX + \lambda I)^{-1} X^T y
$$

This formulation is called ridge regression

- Turns the ridge into a deep, singular valley
- Adds $\lambda$ to eigenvalues of $X^TX$: makes invertible

### 5.2.2 Regulariser as a Prior

Without regularisation, parameters found based entirely on the information contained in the training set $X$

- Regulaisation introudces addtional information

Recall our probabilistic model $Y = x^Tw + \varepsilon$

- Here $Y$ and $\varepsilon$ are random variables, where $\varepsilon$ denoates noise

Now suppose that $w$ is also a random variable (denoted as $W$) with a Normal prior distribution

$$
W \sim \mathcal{N}(0, 1/\lambda)
$$

- i.e. we expect small weights and that no one feature dominates
- Is this always appropriate? E.g. data centring and scalling
- We could encode much more elaborate problem knowledge

### 5.2.3 Computing Posterior

The prior is then used to compute the posterior

$$
p(w|X, y) = \frac{p(y|X, w)p(w)}{p(y|X)}
$$

Instead of maximum likelihood (MLE), take maximum a posteriori estimate (MAP)

Apply log trick, so that $\log (posterior) = \log(likelihood) + \log(prior) - \log(marg)$

Arrive at the problem of minimising

$$
||y - Xw||^2_2 + \lambda||w||^2_2
$$

### 5.2.4 Lasso

$||w||_1$

Lassos ($L_1$ regularisation) encourages solutions to sit on the axes

Some of the weightes are set to zero (which means solution is sparse)

## 5.3 Regularisation in Non-Linear Models

To avoid overfitting and underfitting

### 5.3.1 Explicit Model Selection

Try different classes of models. Example, try polynomial models of various degree $d$ (linear, quadratic, cubic, ...)

Use held out validation (cross vaildation) to select the model

1. Split training data into $D_{train}$ and $D_{validate}$ sets
2. For each degree $d$ we have model $f_d$
   1. Train $f_d$ on $D_{train}$
   2. Test $f_d$ on $D_{validate}$
3. Pick degree $\hat{d}$ that gives the best test score
4. Re-train model $f_{\hat{d}}$ using all data

### 5.3.2 Regularisation

Augment the problem:

$$
\hat{\theta} \in \argmin_{\theta \in \Theta}(L(data, \theta) + \lambda R(\theta))
$$

E.g., ridge regression

$$
\hat{w} \in \argmin_{w \in W}||y - Xw||^2_2 + \lambda ||w||^2_2
$$

Note that regulariser $R(\theta)$ does not depend on data

Use held out validation/cross validation to choose $\lambda$

## 5.4 Bias-Variance Trade-Off

### 5.4.1 Assessing Generalisation

Supervised learning: train the model on existing data, then make predictions on new data

Training the model: ERM / minimisation of training error

Generalisation capacity is captured by risk / test error

Model complexity is a major factor that influences the ability of the model to generalise (vague still)

In this section, our aim is to explore error in th context of supervised regression. One way to decompose it.

### 5.4.2 Training Error and Model Complexity

More complex model means training error goes down

Finite number of points means usually can reduce training error to $0$ (is it always possible?)

### 5.4.3 Bias-Variance Decomposition

Squared loss for supervised-regression predictions

$$
l(Y, \hat{f}(X_0)) = (Y - \hat{f}(X_0))^2
$$

Lemma: Bias-variance decomposition

$$
\mathbb{E}[l(Y, \hat{f}(X_0))] = (\mathbb{E}[Y] - \mathbb{E}[\hat{f}])^2 + Var[\hat{f}] + Var[Y]
$$

### 5.4.4 Model Complexity and Variance

- simple model means low variance
- complex model means high variance

- simple model means high bias
- complex model means low bias