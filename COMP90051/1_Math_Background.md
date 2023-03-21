# 1 Math Background

## 1.1 Probability Theorey

### 1.1.1 Basic of Probability Theorey

A probability space

- Set $\Omega$ of possible outcomes
- Set $F$ of events (subsets of outcomes)
- Probability measure $\mathbb{P}: F \to \mathbb{R}$

### 1.1.2 Axioms of Probability

1. $F$ contains all of:
   - $\Omega$;
   - all complements $\Omega/f, f \in F$;
   - the union of any countable set of events in $F$.
2. $\mathbb{P}(f) \geq 0$ for every event $f \in F$.
3. $\mathbb{P}(\bigcup_f f) = \sum_f \mathbb{P}(f)$ for all countable sets of pairwise disjoint events.
4. $\mathbb{P}(\Omega) = 1$

### 1.1.3 Random Variables

A random variable $X$ is a numeric function of outcome $X(\omega) \in \mathbb{R}$.

$\mathbb{P}(X \in A)$ denotes the probability of the outcome being such that $X$ falls in the range $A$.

#### Discrete v.s. Continuous Distributions

- Discrete distributions
  - Govern r.v. taking discrete values
  - Described by probability mass function $p(x)$ which is $\mathbb{P}(X = x)$
  - $\mathbb{P}(X \leq x) = \sum^x_{a = - \infty} p(a)$
  - E.g. Bernoulli, Binomial, Multinomial, Poisson
- Countnuous distributions
  - Govern real-valued r.v.
  - Cannot talk about PMF but rather probability density function $p(x)$
  - $\mathbb{P}(X \leq x) = \int^x_{-\infty} p(a)da$
  - E.g. Uniform, Norma, Laplace, Gamma, Beta, Dirichlet

### 1.1.4 Expectation

Expectation $\mathbb{E}[X]$ is the r.v. "average" value

- Discrete: $\mathbb{E}[X] = \sum_x x \mathbb{P}(X = x)$
- Continous: $\mathbb{E}[X] = \int_x xp(x)dx$

Properties:

- Linear
- Monotone

Variance:

- $Var(X) = \mathbb{E}[(X - \mathbb{E}[X])^2]$

### 1.1.5 Multivariate Distributions

- Specify joint distribution over multiple variables
- Probabilities are computed as in univariate case, we now just have repeated summations or repeated integrals.
  - Discrete: $\mathbb{P}(X, Y \in A) = \sum_{(x, y) \in A} p(x, y)$
  - Continuous: $\mathbb{P}(X, Y \in A) = \int_A p(x, y)dxdy$

### 1.1.6 Independence and Conditioning

#### Independence

$X, Y$ are independent if:

- $\mathbb{P}(X \in A, Y \in B) = \mathbb{P}(X \in A)\mathbb{P}(Y \in B)$
- Similarly for densities: $p_{X, Y}(x, y) = p_X(x)p_Y(y)$
- Intuitively: knowing value of $Y$ reveals nothing about $X$
- Algebraically: the joint on $X, Y$ factorises!

#### Conditioning

- $\mathbb{P}(A|B) = \frac{\mathbb{P}(A \cap B)}{\mathbb{P}(B)}$
- Similarly for densities: $p(y|x) = \frac{p(x, y)}{p(x)}$
- Intuitively: probability event $A$ will occur given we know event $B$ has occurred
- $X, Y$ independent equiv to $\mathbb{P}(Y = y|X = x) = \mathbb{P}(Y = y)$

#### Bayes' Theorem

In terms of events $A, B$

- $\mathbb{P}(A \cap B) = P(A|B)P(B) = P(B|A)P(A)$
- $\mathbb{P}(A|B) = \frac{\mathbb{P}(B|A)\mathbb{P}(A)}{\mathbb{P}(B)}$

## 1.2 Vectors

### 1.2.1 Dot Product

#### Algebraic Definition

Given two $m$-dimensional vectors $u$ and $v$, their dot product is $u \cdot v = u^\prime v = \sum^m_{i = 1} u_i v_i$

#### Geometric Definition

Given two $m$-dimensional Euclidean vectors $u$ and $v$, their dot product is $u \cdot v = u^\prime v = ||u|| ||v|| \cos{\theta}$

- $||u||, ||v||$ are $L_2$ norms for $u, v$
- $\theta$ is the angle between the vectors

##### Properties

- If the two vectors are orthogonal then $u^\prime v = 0$
- If the two vectors are parallel then $u^\prime v = ||u||||v||$, if they are anti-parallel then $u^\prime v = - ||u||||v||$
- $u^\prime u= ||u||^2$, so $||u|| = \sqrt{u_1^2 + ... + u_m^2}$ defines the Euclidean vector length

### 1.2.2 Hyperplanes and Normal Vectors

A hyperplane defined by parameters $w$ and $b$ is a set of points $x$ that satisfy $x^\prime w + b = 0$

In 2D, a hyperplane is a line: a line is a set of points that satisfy $w_1x_1 + w_2x_2 + b = 0$

### 1.2.3 Normal Vector

A normal vector for a hyperplane is a vector perpendicular to that hyperplane.

### 1.2.4 Hyperplanes and Normal Vectors

Consider a hyperplane defined by parameters $w$ and $b$. Note that $w$ is itself a vector.

Lemma: Vector $w$ is normal to the hyperplane.

### 1.2.5 $L_1$ and $L_2$ Norms

- $L_2$ norm:
  $$
  ||a|| = ||a||_2 = \sqrt{a_1^2 + ... + a_n^2}
  $$
- $L_1$ norm:
  $$
  ||a|| = ||a||_1 = |a_1| + ... + |a_n|
  $$

## 1.3 Vector Spaces and Bases

### 1.3.1 Linear Combinations

A linear combination of vectors $v_1, ... v_k \in V$ some vector space, is a new vector $\sum^k_{i = 1} a_i v_i$ for some scalars $a_1, ..., a_k$

### 1.3.2 Independence

A set of vectors $\{v_1, ..., v_k\} \subseteq V$ is called linearly dependent if one element $v_j$ can be written as a linear combination of the other elements.

A set that isn't linearly dependent is linearly independent.

### 1.3.3 Spans

The span of vectors $v_1, ..., v_k \in V$ is the set of all obtainable linear combinations (ranging over all scalar coefficients) of the vectors

### 1.3.4 Bases

A set of vectors $B = \{v_1, ..., v_k\} \subseteq V$ is called a basis for a vector subspace $V^\prime \subseteq V$ if

1. The set $B$ is linearly independent; and
2. Every $v \in V^\prime$ is a linear combination of the set $B$.

An orthonormal basis is a basis in which each

1. Pair of basis vectors are orthogonal (zero dot prod); and
2. Basis vector has nrom equal to 1.

## 1.4 Matrices

### 1.4.1 Basic

- A rectangular array, often denoted by upper-case, with two indicies first for row, second for column
- Square matrix has equal dimensions (numbers of rows and columns)
- Matrix transpose $A^\prime$ or $A^T$ of $m$ by $n$ matrix $A$ is an $n$ by $m$ matrix with entries $A^\prime_{ij} = A_{ji}$
- A square matrix $A$ with $A = A^T$ is called symmetric
- The suqre identity matrix $I$ has $1$ on the diagonal, $0$ off-diagonal
- Matrix inverse $A^{-1}$ of square matrix $A$ (if it exists) satisfies $A^{-1}A = I$

### 1.4.2 Matrix Eigenspectrum

Scalar, vector pair $(\lambda, v)$ are called an eigenvalue-eigenvector pair of a square matrix $A$ if $Av = \lambda v$

- Intuition: matrix $A$ doesn't rotate $V$ it just stretches it
- Intuition: the eigenvalue represents stretching factor

In general eigenvalues may be zero, negative or even complex (imaginary) - we'll only encounter reals

### 1.4.3 Spectra of Common Matrices

Eigenvalues of symmetric matric matrices are always real (no imaginary component)

A matrix with linear dependent columns has some zero eigenvalues (called rank deficient) $\implies$ no matrix inverse exists

### 1.4.4 Positive (Semi)definite Matrices

A symmetric square matrix $A$ is called positive semidefinite if for all vectors $v$ we have $v^\prime A v \geq 0$

- Then $A$ has non-negative eigenvalues

Further, if $v^\prime A v > 0$ holds as a strict inequality then $A$ is called positive definite

- Then $A$ has (strictly) positive eigenvalues

## 1.5 Sequences and Limits

### 1.5.1 Infinite Sequences

- Written like $x_1, x_2, ...$ or $\{x_i\}_{i \in \mathbb{N}}$
- Formally: a function from the positive (from 1) or non-negative (from 0) integers
- Index set: subscript set e.g. $\mathbb{N}$
- Sequences allow us to reason about test error when training data grows indefinitely, or training error (or a stopping criterion) when training runs arbitrarily long

### 1.5.2 Limits and Convergence

- A sequence $\{x_i\}_{i \in \mathbb{N}}$ converges if its elements become and remain arbitrarily close to a fixed limit point $L$
- Formally: $x_i \to L$ if, for all $\epsilon > 0$, there exists $N \in \mathbb{N}$ such that for all $n \geq N$ we have $||x_n - L|| < \epsilon$

## 1.6 Supremum

### Upper bound

$u \in \mathbb{R}^+$ of set $S$ has: $u \geq x$ for all $x \in S$

If $u$ is no bigger than any other upper bound of $S$ then it's called a least upper bound or supremum of $S$, written as $\sup{(S)}$.

### Infimum

The greatest lower bound or infimum is generalisation of the minimum.

## 1.7 Stochastic Convergence

### 1.7.1 Why Simple Limits Aren't Enough

- Consider running your favourite learner on varying numbers of $n$ training examples giving classifier $c_n$
- If your learner minimises training error, you 'd wish its test error wasn't much bigger than its training error
- If $R_n = err_{test}(x_n) - err_{train}(c_n)$, you'd wish for $R_n \to 0$ as this would mean eventually tiny test error
- But both training data and test data are random!
- Even if $R_n \to 0$ usually happens, it won't always!!

### 1.7.2 Stochastic Convergence

- A sequence $\{X_n\}$ of random variables (CDFs $F_n$) converges in distribution to random variable $X$ (CDF $F$) if $F_n(x) \to F(x)$ for all constants $x$
- A sequence $\{X_n\}$ of random variables converges in probability to random variable $X$ if for all $\epsilon > 0: \mathbb{P}(|X_n - X| > \epsilon) \to 0$
- A seqence $\{X_n\}$ of random variables converges almost surely to random variable $X$ if: $\mathbb{P}(X_n \to X) = 1$
- Chain of implications:
  almost sure $\implies$ in probability $\implies$ in distribution
