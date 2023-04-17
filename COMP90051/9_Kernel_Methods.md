# 9 Kernel Methods

## 9.1 Lagrangian Duality for the SVM

### 9.1.1 Soft-Margin SVM Recap

Soft-Margin SVM objective:

$$
\argmin_{w, b, \xi}\left( \frac{1}{2}||w||^2 + C \sum^n_{i = 1}\xi_i \right) \\
\text{s.t. }y_i (w^\prime x_i + b) \geq 1 - \xi_i \text{ for } i = 1, ..., n \\
\xi \geq 0 \text{ for } i = 1, ..., n
$$

while we can optimise the above "primal", often instead work with the dual.

### 9.1.2 Constrained Optimisation

Constrained optimisationL canonical form

$$
\text{minimise } f(x) \\
\text{s.t. }g_i(x) \leq 0, i = 1, ..., n \\
h_j(x) = 0, j = 1, ..., m
$$
Gradient descent doesn't immediately apply

Hard-margin SVM: $\argmin_{w, b} \frac{1}{2}||w||^2$ s.t. $1 - y_i(w^\prime x_i +b) \leq 0$ for $i = 1, ..., n$

Method of Lagrange multipliers:

- Transform to unconstrained optimisation
- Transform primal program to a related dual program, altermate to primal
- Analyse necessary & sufficient conditions for solutions of both programs

### 9.1.3 The Lagrangian and Duality

Introduce auxiliary objective function via auxiliary variables

$$
\mathcal{L}(x, \lambda, \nu) = f(x) + \sum^n_{i = 1}\lambda_ig_i(x) + \sum^m_{j = 1} v_jh_j(x)
$$
- called the Lagrangian function
- New $\lambda$ and $\nu$ are called the Lagrange multpliers or dual variables

Primal program: $\min_x \max_{\lambda \geq 0, \nu}\mathcal{L}(x, \lambda, \nu)$

Dual program: $\max_{\lambda \geq 0, \nu}\min_{x}\mathcal{L}(x, \lambda, \nu)$

Duality theory relates primal/dual:

- Weak duality: dual optimum $\leq$ primal optimum
- For convex programs (inc. SVM!) strong duality: optimal coincide!

### 9.1.4 Karush-Kuhn-Tucker Necessary Conditions

Lagrangian: $\mathcal{L}(x, \lambda, \nu) = f(x) + \sum^n_{i = 1} \lambda_ig_i(x) + \sum^m_{j = 1}\nu_jh_j(x)$

Necessary conditions for optimality of a primal solution

Primal feasibility:

- $g_i(x^*) \leq 0, i = 1, ..., n$
- $h_j(x^*) = 0, j = 1, ..., m$

Dual feasibility: $\lambda_i^* \geq 0$ for $i = 1, ..., n$

Complementary slackness: $\lambda_i^*g_i(x^*) = 0, i = 1, ..., n$

Stationarity: $\triangledown_x \mathcal{L}(x^*, \lambda^*, \nu^*) = 0$

### 9.1.5 KKT Conditions for Hard-Margin SVM

The Lagrangian:

$$
\mathcal{L}(w, b, \lambda) = \frac{1}{2} ||w||^2 - \sum^n_{i = 1}\lambda_i(y_i(w^\prime x_i + b) - 1)
$$

KKT conditions:

- Feasibility: $y_i((w^*)^\prime x_i + b^*) - 1 \geq 0$ for $i = 1, ..., n$
- Feasibility: $\lambda^*_i \geq 0$ for $i = 1, ..., n$
- Complementary slackness: $\lambda_i^*(y_i((w^*)^\prime x_i + b^*) - 1) = 0$
- Stationarity: $\triangledown_{w, b}\mathcal{L}(w^*, b^*, \lambda^*) = 0$

### 9.1.6 Minimise Lagrangian

The Lagrangian:

$$
\mathcal{L}(w, b, \lambda) = \frac{1}{2} ||w||^2 - \sum^n_{i = 1}\lambda_i(y_i(w^\prime x_i + b) - 1)
$$

Stationarity conditions give us more information:

$$
\frac{\partial \mathcal{L}}{\partial b} = \sum^n_{i = 1} \lambda_iy_i = 0 \\
\frac{\partial \mathcal{L}}{\partial w_j} = w_j^* - \sum^n_{i = 1} \lambda_i y_i (x_i)_j = 0
$$

The Lagrangian becomes (with additional constraint, above)

$$
\mathcal{L}(\lambda) = \sum^n_{i = 1}\lambda_i - \frac{1}{2} \sum^n_{i = 1}\sum^n_{j = 1} \lambda_i\lambda_jy_iy_jx^\prime_ix_j
$$

### 9.1.7 Dual Program for Hard-Margin SVM

Having minimised the Lagrangian with respect to primal variables, now maximising w.r.t. dual variables yields the dual program

$$
\argmax_{\lambda} \sum^n_{i = 1} \lambda_i - \frac{1}{2} \sum^n_{i = 1}\sum^n_{j = 1} \lambda_i\lambda_jy_iy_jx^\prime_ix_j \\
\text{s.t. } \lambda_i \geq 0 \text{ and } \sum^n_{i = 1}\lambda_iy_i = 0
$$

Strong duality: Solving dual, solves the primal!!

Like primeal: A so-called quadratic program - off-the-shelf software can solve - more later

Unlike primal:

- Complexity of solution is $O(n^3)$ instead of $O(d^3)$ - more later
- Program depends on dot products of data only - more later on kernels!

### 9.1.8 Making Predictions with Dual Solution

Recovering primal variables:

- Recall from stationarity: $w_j^*$ - $\sum^n_{i = 1} \lambda_i y_i (x_i)_j = 0$
- Complementary slackness: $b^*$ can be recovered from dual solution, nothing for any example $j$ with $\lambda_i^* > 0$, we have $y_j(b^* + \sum^n_{i = 1} \lambda^*_iy_ix_i^\prime x_j) = 1$ (these are the support vectors)

Testing: classify new instance $x$ based on sign of

$$
s = b^* + \sum^n_{i = 1} \lambda_i^*y_ix_i^\prime x
$$

### 9.1.9 Soft-Margin SVM's Dual

Training: find $\lambda$ that solves

$$
\argmax_\lambda \sum^n_{i = 1}\lambda_i - \frac{1}{2} \sum^n_{i = 1}\sum^n_{j = 1} \lambda_i\lambda_jy_iy_jx_i^\prime x_j \\
\text{s.t. } C \geq \lambda_i \geq 0 \text{ and } \sum^n_{i = 1}\lambda_iy_i = 0
$$

Making predictions: same pattern as in as in hard - margin case

### 9.1.10 Training the SVM

The SVM dual problems are quadratic programs, solved in $O(n^3)$, or $O(d^3)$ for the primal.

This can inefficient; specialised solutions exist:

- chunking: original SVM training algorithm exploits fact that many $\lambda$s will be zero (sparsity)
- Sequential minimal optimisation (SMO), an extreme case of chunking. An iterative precedure that analytically optimises randomly chosen pairs of $\lambda$s per iteration

## 9.2 Kernelising the SVM

### 9.2.1 Handling Non-Linear Data with the SVM

Method 1: Soft-margin SVM

Method 2: Feature space transformation

- Map data into a new feature space
- Run hard-margin or soft-margin SVM in new space
- Decision boundary is non-linear in original space

### 9.2.2 Feature Transformation (Basis Expansion)

Consider a binary classification problem

Each example has features $[x_1, x_2]$

Not linearly separable

Now 'add' a feature $x_3 = x_1^2 + x_2^2$

Each point is now $[x_1, x_2, x_1^2 + x_2^2]$

Linearly separable!

### 9.2.3 Naive Workflow

Choose/design a linear model

Choose/design a high-dimensional transformation $\varphi(x)$

- Hoping that after adding a lot of various features some of them will make the data linearly separable

For each training example, and for each new instance compute $\varphi(x)$

Train classifier/Do predictions

Problem: impractical/impossible to compute $\varphi(x)$ for high/infinite-dimensional $\varphi(x)$

### 9.2.4 Hard-Margin SVM's

#### Dual Formulation

Training: finding $\lambda$ that solve:

$$
\argmax_\lambda \sum^n_{i = 1} \lambda_i - \frac{1}{2} \sum^n_{i = 1}\sum^n_{j = 1}\lambda_i\lambda_jy_iy_jx_i^\prime x_j \\
\text{s.t. } \lambda_i \geq 0 \text{ and }\sum^n_{i = 1}\lambda_i y_i = 0
$$

Making predictions: classify instance $x$ as sign of

$$
s = b^* + \sum^n_{i = 1} \lambda_i^* y_i x_i^\prime x
$$

Note: $b^*$ found by solving for it in $y_j(b^* + \sum^n_{i = 1}\lambda^*_iy_ix_i^\prime x_j) = 1$ for any support vector $j$

#### Feature Space

Training: finding $\lambda$ that solve

$$
\argmax_\lambda \sum^n_{i = 1} \lambda_i - \frac{1}{2}\sum^n_{i = 1}\sum^n_{j = 1}\lambda_i\lambda_jy_iy_j\varphi(x_i)^\prime\varphi(x_j) \\
\text{s.t. }\lambda_i \geq 0 \text{ and }\sum^n_{i = 1}\lambda_iy_i = 0
$$

Note: $b^*$ found by solving for it in $y_j(b^* + \sum^n_{i = 1}\lambda_i^*y_i\varphi(x_i)^\prime\varphi(x_j)) = 1$ for support vector $j$

### 9.2.5 Observation: Kernel Representation

Both parameter estiation and computing predictions depend on data only in a form of a dot product

- In original space $u^\prime v = \sum^m_{i = 1} u_iv_i$
- In transformed space $\varphi(u)^\prime \varphi(\nu) = \sum^l_{i = 1} \varphi(u)_i\varphi(\nu)_i$

Kernel is a function that can be expressed as a dot product in some feature space $K(u, v) = \varphi(u)^\prime \varphi(v)$

### 9.2.6 Example

For some $\varphi(x)$'s, kernel is faster to compute directly than first mapping to feature space then taking dot product.

For example, consider two vectors $u = [u_1]$ and $v = [v_1]$ and transformation $\varphi(x) = [x_1^2, \sqrt{2c}x_1, c]$, some $c$

- So $\varphi(u) = [u_1^2, \sqrt{2c}u_1, c]^\prime$ and $\varphi(v) = [v_1^2, \sqrt{2c}v_1, c]^\prime$
- Then $\varphi(u)^\prime\varphi(v) = (u_1^2v_1^2 + 2cu_1v_1 + c^2)$

