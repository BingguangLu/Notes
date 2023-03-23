# 8 Support Vector Machines

## 8.1. Maximum-Margin Classifier: Motivation

### 8.1.1 Beginning: Linear SVMs

In the first part, we will consider a basic setup of SVMs, something called linear hard-margin SVM.

Keep in mind: SVMs are more powerful than they initially appear

For now, we model the data as linearly separable, i.e., there exists a hyperplane perfectly separating the classes

### 8.1.2 SVM is a Linear Binary Classifier

Predict class A is $s \geq 0$

Predict class B if $s < 0$

where $s = b + \sum^m_{i = 1} x_i w_i$

SVM is a linear classifier: $s$ is a linear function of inputs, and the separating boundary is linear

### 8.1.3 Choosing Separation Boundary

An SVM is a linear binary classifier: choosing parameters means choosing a separating boundary (hyperplane)

Provided the dataset is linearly separable, classifiers like logistic regression, naive Bayes will find boundaries that separate classes perfectly. Many such boundaries exist (infinite!)

### 8.1.4 Aiming for the Safest Boundary

Intuitively, the most reliable boundary would be the one that is between the classes and as far away from both classes as possible

SVM objective captures this observation

SVMs aim to find the separation boundary that maximises the margin between the classes

### 8.1.5 Maximum-Margin Classifier

An SVM is a linear binary classifier. SVM training aims to find the separating boundary that maximises margin

For this reason, SVMs a.k.a maximum-margin classifiers

The training data is fixed, so the margin is defined by the location and orientation of the separating boundary

Our next step is to formalise our objective by expressing margin width as a function of parameters (and data)

## 6.2 Maximum-Margin Classifier: Derivation

### 6.2.1 Margin Width

While the margin can be thought as the space between two dashed lines, it is more convenient to define margin width as the distance between the separating boundary and the nearest data point(s)

Point(s) on margin boundaries called support vectors

We want to maximise the distance to support vectors

### 6.2.2 Distance From Point to Hyperplane

Consider an arbitrary point $x$ (from either of the classes, and not necessarily the closest one to the boundary), and let $x_p$ denote the projection of $x$ onto the separating boundary

Now, let $r$ be a vector $x_p - x$. Note that $r$ is perpendicular to the boundary, and also that $||r||$ is the required distance

The separating boundary is defined by parameters $w$ and $b$

From our linear algebra slides, recall that $w$ is a vector normal (perpendicular) to the boundary

Distance is

$$
||r|| = - \frac{w^\prime x + b}{||w||}
$$
or more generally
$$
||r|| = \pm \frac{w^\prime x + b}{||w||}
$$

### 6.2.3 Encoding the Side Using Labels

Training data is a collection $\{x_i, y_i\}, i = 1, ..., n$, where each $x_i$ is an $m$-dimensional instance and $y_i$ is the corresponding binary label encoded as $-1$ or $1$

Given a perfect separation boundary, $y_i$ will encode the side of the boundary each $x_i$ is on

Thus the distance from the $i$-th point to a perfect boundary can be encoded as

$$
||r_i|| = \frac{y_i(w^\prime x_i + b)}{||w||}
$$

### 6.2.4 Maximum Margin Objective

The distance from the $i$-th point to a perfect boundary can be encoded as $||r_i|| = \frac{y_i(w^\prime x_i + b)}{||w||}$

The margin width is the distance to the closest point

Thus SVMs aim to maximise $(\min_{i = 1, ..., n} \frac{y_i(w^\prime X_i + b)}{||w||})$ as a function of $w$ and $b$

### 6.2.5 Non-Unique Representation

A separating boundary (e.g., a line in 2D) us a set of points that satisfy $w^\prime x + b = 0$ for some given $w$ and $b$

However, the same set of points will also satisfy $\tilde{w}^\prime x + \tilde{b} = 0$, with $\tilde{w} = \alpha w$ and $\tilde{b} = \alpha b$, where $\alpha > 0$ is arbitrary

The same boundary, and essentially the same classifier can be expressed with infinitely many parameter combinations - that diverge!

### 6.2.6 Constraining the Objective for Uniqueness

SVMs aim to maximise $(\min_{i = 1, ..., n} \frac{y_i(w^\prime x_i + b)}{||w||})$

Introduce (arbitrary) extra requirement $\frac{y_i^*(w^\prime x_i^* + b)}{||w||} = \frac{1}{||w||}$

- $i^*$ denotes index of a closest example to boundary

Instead of maximising margin, can minimise norm of $w$

Ensure classifier makes no errors: constrain $y_i(w^\prime x_i + b) \geq 1$

### 6.2.7 Hard-Margin SVM Objective

We now have a major result: SVMs aim to find

$$
\argmin_{w, b} ||w|| \\
s.t. y_i(w^\prime x_i + b) \geq 1 \text{ for } i = 1, ..., n
$$

Note1: parameter $b$ is optimised indirectly by influencing constraints

Note2: all points are enforced to be on or outside the margin

Therefore, this version of SVM is called hard-margin SVM

## 6.3 SVM Objective as Regularised Loss

1. Choose/design a model
2. Choose/design loss function
3. Find parameter values that minimise discrepancy on training data

### 6.3.1 SVM as Regularised ERM

Recall ridge regression objective

$$
\text{minimise } (\sum^n_{i = 1}(y_i - w^\prime x_i)^2 + \lambda ||w||^2)
$$

Hard-margin SVM objective

$$
\argmin_{w, b} ||w|| \\
s.t. y_i(w^\prime x_i + b) \geq 1 \text{ for } i = 1, ..., n
$$

The constraints can be interpreted as loss

$$
l_\infty = \begin{cases}
    0 \text{ if } 1 - y_i(w^\prime x_i + b) \leq 0 \\
    \infty \text{ if } 1 - y_i(w^\prime x_i + b) > 0
\end{cases}
$$

### 6.3.2 Hard Margin SVM Loss

The constraints can be interpreted as loss
$$
l_\infty = \begin{cases}
    0 \text{ if } 1 - y_i(w^\prime x_i + b) \leq 0 \\
    \infty \text{ if } 1 - y_i(w^\prime x_i + b) > 0
\end{cases}
$$

In other words, for each point:

- If it's on the right side of the boundary and at least $\frac{1}{||w||}$ units aways from the boundary, we're OK, the loss is $0$

If the point is on the wrong side, or too close to the boundary, we immediately give infinite loss thus prohibiting such a solution altogether

## 6.4 Soft-Margin SVMs

### 6.4.1 When Data is not Linearly Separable

Hard-margin loss is too stringent (hard!)

Real data is unlikely to be linearly separable

If the data is not separable, hard-margin SVMs are in trouble

SVMs offer $3$ approaches to address this problem:

1. Still use hard-margin SVM, but transform the data
2. Relax the constraints
3. The combination of $1$ and $2$

### 6.4.2 Soft-Margin SVM

Relax constraints to allow points to be insider the margin or even on the wrong side of the boundary

However, we penalise boundaries by the extent of "violation"

### 6.4.3 Hinge Loss: Soft-Margin SVM Loss

Hard-margin SVM loss

$$
l_\infty = \begin{cases}
    0 \text{ if } 1 - y_i(w^\prime x_i + b) \leq 0 \\
    \infty \text{ if } 1 - y_i(w^\prime x_i + b) > 0
\end{cases}
$$

Soft-margin SVM loss (hinge loss)

$$
l_h = \begin{cases}
    0 \text{ if } 1 - y_i(w^\prime x_i + b) \leq 0 \\
    1 - y_i(w^\prime x_i + b) = 1 - y\hat{y} \text{ if otherwise}
\end{cases}
$$

### 6.4.4 Soft-Margin SVM Objective

Soft-margin SVM objective

$$
\argmin_{w, b}(\sum^n_{i = 1} l_h(x_i, y_i, w, b) + \lambda ||w||^2)
$$

- Reminiscent of ridge regression
- Hinge loss $l_h = \max(0, 1 - y_i(w^\prime x_i + b))$

We are going to re-formulate this objective to make it more amenable to analysis

### 6.4.5 Re-Formulating Soft-Margin Objective

Introduce slack variables as an upper bound on loss

$$
\xi_i \geq l_h = \max(0, 1 - y_i(w^\prime x_i + b))
$$
or equivalently $\xi_i \geq 1 - y_i (w^\prime x_i + b)$ and $\xi_i \geq 0$

Re-write the soft-margin SVM objective as:

$$
\argmin_{w, b, \xi}(\frac{1}{2}||w||^2 + C \sum^n_{i = 1} \xi_i)\\
s.t. \xi_i \geq 1 - y_i(w^\prime x_i + b) \text{ for } i = 1, ..., n\\
\xi_i \geq 0 \text{ for } i = 1, ..., n
$$

### 6.4.6 Side-by-Side: Two Variations of SVM

Hard-margin SVM objective:

$$
\argmin_{w, b} \frac{1}{2}||w||^2\\
s.t. y_i(w^\prime x_i + b) \geq 1 \text{ for } i = 1, ..., n
$$

Soft-margin SVM objective:

$$
\argmin_{w, b, \xi}(\frac{1}{2}||w||^2 + C \sum^n_{i = 1} \xi_i)\\
s.t. \xi_i \geq 1 - y_i(w^\prime x_i + b) \text{ for } i = 1, ..., n\\
\xi_i \geq 0 \text{ for } i = 1, ..., n
$$

In the second case, the constraints are relaxed ("softened") by allowing violations by $\xi_i$. Hence the name "soft margin"
