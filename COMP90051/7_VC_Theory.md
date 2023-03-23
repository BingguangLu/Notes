# 7 VC Theory

Countably infinite $\mathcal{F}$

- Hoeffding gave us for a single $f \in \mathcal{F}$
  $$
  \mathbb{P}\left(R[f] - \hat{R}[f] \geq \sqrt{\frac{\log(\frac{1}{\delta(f)})}{2m}}\right) \leq \delta(f)
  $$
  where we're free to choose (varying) $\delta(f)$ in $[0, 1]$
- Union bound "works" (sort of) for this case
  $$
  \mathbb{P}\left(\exist f \in \mathcal{F}, R[f] - \hat{R}[f] \geq \sqrt{\frac{\log(\frac{1}{\delta(f)})}{2m}} \right) \leq \sum_{f \in \mathcal{F}} \delta(f)
  $$
- Choose confidences to sum to constant $\delta$, then this works
- By inversion: w.h.p $1 - \delta$, for all $f$, $R[f] \leq \hat{R}[f] + \sqrt{\frac{\log(\frac{1}{p(f)}) + \log(\frac{1}{\delta})}{2m}}$

But for general case:

- Much of ML has continuous parameters
  - Countably infinite covers only discrete parameters
- Our argument fails!
  - $p(f)$ becomes a density
  - Its zero for all $f$. No divide by zero!
  - Need a new argument!
- Idea introduced by VC theory: intuition
  - Don't focus on whole class $\mathcal{F}$ as if each $f$ is different
  - Focus on differences over sample $Z_1, ..., Z_m$

## 7.1 Growth Function

### 7.1.1 Bad events: Unreasonably worst case?

Bad event $B_i$ for model $f_i$

$$
R[f_i] - \hat{r}[f_i] \geq \varepsilon \text{ with probability } \leq 2\exp(-1m\varepsilon^2)
$$

Union bound: bad events don't overlap??!!

### 7.1.2 Dichotomies and Growth Function

Definition: Given sample $x_1, ..., x_m$ and family $\mathcal{F}$, a dichotomy is a $(f(x_1), ..., f(x_m)) \in \{-1, +1\}^m$ for some $f \in \mathcal{F}$

- Unique dichotomies $\mathcal{F}(x) = \{(f(x_1), ..., f(x_m)): f \in \mathcal{F}\}$, patterns of labels possible with the family
- Even when $\mathcal{F}$ infinite, $|\mathcal{F}(x)| \leq 2^m$
- And also (relevant for $\mathcal{F}$ finite, tiny), $|\mathcal{F}(x)| \leq |\mathcal{F}|$
- Intuition: $|\mathcal{F}(x)|$ might replace $|\mathcal{F}|$ in union bound? How remove $X$?

Definition: The growth function $S_\mathcal{F} (m) = \sup_{x \in D^m}|\mathcal{F}(x)|$ is the max number of label patterns achievable by $\mathcal{F}$ for any $m$ sample.

### 7.1.3 PAC Bound with Growth Function

Theorem: Consider any $\delta > 0$ and any class $\mathcal{F}$. Then w.h.p. at least $1 - \delta$: For all $f \in \mathcal{F}$

$$
R[f] \leq \hat{R}[f] + 2 \sqrt{2\frac{\log F_\mathcal{F}(2m) + \log(4/\delta)}{m}}
$$

Compare to PAC bounds so far:

- A few negligible extra constants
- $|\mathcal{F}|$ has become $S_\mathcal{F}(2m)$
- $S_\mathcal{F}(m) \leq |\mathcal{F}|$, not "worse" than union bound for finite $\mathcal{F}$
- $S_\mathcal{F}(m) \leq 2^m$, very bad for big family with exponential growth function gets $R[f] \leq \hat{R}[f] +$ Big Constant. Even $R[f] \leq \hat{R}[f] + 1$ is meaningless!!

## 7.2 The VC Dimension

### 7.2.1 Vapnik-Chervonenkis Dimension

Definition: The VC dimension $VC(\mathcal{F})$ of a family $\mathcal{F}$ is the largest $m$ such that $S_\mathcal{F}(m) = 2^m$

- Points $x = (x_1, ..., x_m)$ are shattered by $\mathcal{F}$ if $|\mathcal{F}(x)| = 2^m$
- So $VC(\mathcal{F})$ is the size of the largest set shattered by $\mathcal{F}$

### 7.2.2 Sauer-Shelah Lemma

Lemma (Sauer-Shelah): Consider any $\mathcal{F}$ with finite $VC(\mathcal{F}) = k$, any sample size $m$. Then $S_{\mathcal{F}}(m) \leq \sum^k_{i = 0} \binom{m}{i}$

From basic facts of Binomial coefficients

- Bound is $O(m^k)$: finite VC implies eventually polynomial growth!
- For $m \geq k$, it is bounded by $(\frac{em}{k})^k$

Theorem (VC bound): Consider any $\delta > 0$ and any VC-k class $\mathcal{F}$. Then w.h.p. at least $1 - \delta$: For all $f \in \mathcal{F}$

$$
R[f] \leq \hat{R}[f] + 2\sqrt{2 \frac{k\log\frac{2em}{k} + \log \frac{4}{\delta}}{m}}
$$

### 7.2.3 VC Bound Big Picture

Uniform difference between $R[f], \hat{R}[f]$ is $O(\sqrt{\frac{k\log m}{m}})$ down from $\infty$

Limiting complexity of $\mathcal{F}$ leads to better generalisation

VC dim, growth function measure "effective" size of $\mathcal{F}$

VC dim doesn't count functions, but uses geometry of family: projections of family members onto possible samples
