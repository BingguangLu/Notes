# 6 PAC Learning Theorey

Generalisation and Model Complexity

Theory we've seen so far (mostly statistics)

- Asymptotic notions (consistency, efficiency)
- Convergence could be really slow
- Model complexity undefined

Want: finite sample theory; convergence rates, trade-offs

Want: define model complexity and relate it to test error

- Test error can't be measured in real life, but it can be provably bounded!
- Growth function, VC dimension

Want: distribution -independent, learner-independent theory

- A fundamental theory applicable throughout ML
- Unlike bias-variance: distribution dependent, no model complexity

## 6.1 Probably Approximately Correct Learning

### 6.1.1 Standard Setup

Supervised binary classification of

- data in $\mathcal{X}$ into label set $y = \{-1, 1\}$

i.i.d. data $\{(x_i, y_i)\}^m_{i = 1} \sim D$ some fixed unknown distribution

Single test example independent from same $D$ when representing generalisation performance (risk)

Learning from a class of function $\mathcal{F}$ mapping (classifying) $\mathcal{X}$ into $\mathcal{Y}$

What parts depend on the sample of data

- Empirical risk $\hat{R}[f]$ that averages loss over the sample
- $f_m \in \mathcal{F}$ the learned model (it could be same sample or different; theory is actually fully general here)

### 6.1.2 Risk: The Good, Bad, and Ugly

- Excess risk: $R[f_m] - R^*$
- Estimation error: $R[f_m] - R[f^*]$
- Approximation error: $R[f^*] - R^*$

$$
R[f_m] - R^* = (R[f_m] - R[f^*]) + (R[f^*] - R^*)
$$

- Good: what we'd aim for in our class, with infinite data
  - $R[f^*]$ true risk of best in class $f^* \in \argmin_{f \in \mathcal{F}} R[f]$
- Bad: we get what we get and don't upset
  - $R[f_m]$ true risk of learned $f_m \in \argmin_{f \in \mathcal{F}} \hat{R}[f] + C||f||^2$
- Ugly: we usually cannot even hope for perfection
  - $R^* \in inf_f R[f]$ called the Bayes risk; noisy labels makes large

### 6.1.3 Trade-Off: More Intuition

- simple family: may underfit due to approximation error
- complex family: may overfit due to estimation error

### 6.1.4 Bayes Risk

Bayes risk $R^* \in \inf_f R[f]$

- Best risk possible, ever; but can be large
- Depends on distribution and loss function

Bayes classifier achieves Bayes risk

### 6.1.5 $R[f_m]$

Since we don't know data distribution, we need to bound generalisation to be small

- Bound by test error $\hat{R}[f_m] = \frac{1}{m} \sum^m_{i = 1} f(X_i, Y_i)$
- Abusing notation: $f(X_i, Y_i) = l(Y_i, f(X_i))

$$
R[f_m] \leq \hat{R}[f_m] + \varepsilon(m, \mathcal{F})
$$

Unlucky training sets, no always-guarantees possible!

With probability $\geq 1 - \delta: R[f_m] \leq \hat{R}[f_m] + \varepsilon(m, \mathcal{F}, \delta)$

Called Probably Approximately Correct (PAC) learning

- $\mathcal{F}$ called PAC learnable if $m = O(ploy(1/\varepsilon, 1/\delta))$ to learn $f_m$ for any $\varepsilon, \delta$

## 6.2 Bounding True Risk of One Function

We need a concentration inequality

- $\hat{R}[f]$ is an unbiased estimate of $R[f]$ for any fixed $f$
- That means on average $\hat{R}[f]$ lands on $R[f]$
- What's the likelihood $1 - \delta$ that $\hat{R}[f]$ lands within $\varepsilon$ of $R[f]$? Or more precisly, what $1 - \delta(m, \varepsilon$ achieves a given $\varepsilon > 0$?
- Intuition: Just bounding CDF of $\hat{R}[f]$, independent of distribution!!

### 6.2.1 Hoeffding's inequality

Many such concentration inequalities; a simplest one...

Theorem: Let $Z_, ..., Z_m, Z$ be i.i.d. random varibles and $h(z) \in [a, b]$ be a bounded function. For all $\varepsilon > 0$:

$$
\begin{align}
    \mathbb{P}\left( |\mathbb{E}[h(Z)] - \frac{1}{m} \sum^m_{i = 1} h(Z_i)| \geq \varepsilon \right) & \leq 2 \exp \left( -\frac{2 m \varepsilon^2}{(b - a)^2} \right) \notag \\
    \mathbb{P}\left(\mathbb{E}[h(Z)] - \frac{1}{m} \sum^m_{i = 1} h(Z_i) \geq \varepsilon \right) & \leq \exp \left( -\frac{2 m \varepsilon^2}{(b - a)^2} \right) \notag
\end{align}
$$

Two-sided case in words: The probability that the empirical average is far from the expectation is small

### 6.2.2 Common Probability 'Tricks'

Inversion:

- For any event $A, \mathbb{P}(\bar{A}) = 1 - \mathbb{P}(A)$
- Application: $\mathbb{P}(X > \varepsilon) \leq \delta$ implies $\mathbb{P}(X \leq \varepsilon) \geq 1 - \delta$

Solving for, in high-probability bounds:

- For given $\varepsilon$ with $\delta(\varepsilon)$ function $\varepsilon : \mathbb{{P}(X > \varepsilon) \leq \delta(\varepsilon)}$
- Given $\delta^\prime$ can write $\varepsilon = \delta^{-1}(\delta^\prime): \mathbb{P}(X > \delta^{-1}(\delta^\prime)) \leq \delta^\prime$
- Let's you specify either parameter
- Sometimes sample size $m$ a variable we can solve for too

### 6.2.3 Et Voila: A Bound on True Risk

$$
R[f] \leq \hat{R}[f] + \sqrt{\frac{\log(1/\delta)}{2m}}
$$
with high probability (w.h.p) $\geq 1 - \delta$

## 6.3 Uniform Deviation Bounds

Our bound doesn't hold for $f = f_m$

- Result says there's set $S$ of good samples for which $R[f] \leq \hat{R}[f] + \sqrt{\frac{\log(1 / \delta)}{2m}}$ and $\mathbb{P}(Z \in S) \geq 1 - \delta$
- But for different functions $f_1, f_2, ...$ we might get very different sets $S_1, S_2, ...$
- $S$ observed may be bad for $f_m$. Learning minimises $\hat{R}[f_m]$, exacerbating this

We could analyse risk of $f_m$ from specific learner

- But repeating for new learners? How to compare learners?
- Note there are ways to do this, and data-dependently

Bound uniform deviations across whole class $\mathcal{F}$

$$
R[f_m] - \hat{R}[f_m] \leq \sum_{f \in \mathcal{F}}(R[f] - \hat{R}[f]) \leq ?
$$

- Worst deviation over an entire class bounds learned risk
- Convenient, but could be much worse than the actual gap for $f_m$

### 6.3.1 Relation to Estimation Error

$$
R[f_m] - R^* = (R[f_m] - R[f^*]) + (R[f^*] - R^*)
$$

Theorem: ERM's estimation error is at most twice the uniform divergence

## 6.4 Error Bound for Finite Function Clsses

### 6.4.1 The Union Bound

If each model $f$ having large risk deviation is a "bad event", we need a tool to bound the probability that any bad event happens. I.e. the union of bad events!

Union bound: for a sequence of events $A_1, A_2, ...$

$$
\mathbb{P}(\cup_iA_i) \leq \sum_i \mathbb{P}(A_i)
$$

### 6.4.2 Bound fior Finite Classes $\mathcal{F}$

A uniform deviation bound over any finite class or distribution

Theorem: COnsider any $\delta > 0$ and finite class $\mathcal{F}$. Then w.h.p at least $1 - \delta$: For all $f \in \mathcal{F}, R[f] \leq \hat{R}[f] + \sqrt{\frac{\log |\mathcal{F}| + \log(1/ \delta)}{2m}}$

### 6.4.3 Discussion

Hoeffding's inequality only uses boundedness of the loss, not the variance of the loss random variables

- Fancier concentration inequalities leverage variance

Uniform deviation is worst-case, ERM on a very large over-paeameterised $\mathcal{F} may approach the worst-case, but learners generally may not

- Custom analysis, data-dependent bounds, PAC-Bayes, etc.

Dependent data?

- Martingale theorey

Union bound is in general loose, as bad is if all the bad events were independent (not necessarily the case even though underlying data modelled as independent); and finite $\mathcal{F}$

- VC theory coming up next!
