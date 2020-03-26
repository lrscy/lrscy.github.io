---
title: The K-Armed Bandit Problem Notes
date: 2020-03-03 00:12:51
tags: [Coursera, Reinforcement Learning, Notes]
categories: [Reinforcement Learning]
mathjax: true

---

**Fundamentals of Reinforcement Learning Week1 Notes**

<!-- more -->

First of all, there are some notations need to be determined.

| Notation    | Explanation                                                  |
| ----------- | ------------------------------------------------------------ |
| $k$         | number of actions (arms)                                     |
| $t$         | discrete time step or play number                            |
| $q\_\*{(a)}$    | true value (expected reward) of action $a$                   |
| $Q\_t{(a)}$    | estimate at time $t$ of $q\_\*{(a)}$                             |
| $N\_t{(a)}$    | number of times action $a$ has been selected up prior to time $t$ |
| $H\_t{(a)}$    | learned preference for selecting action $a$ at time $t$      |
| $\pi\_t{(a)}$  | probability of selecting action $a$ at time $t$              |
| $\bar{R}\_t$ | estimate at time $t$ of the expected reward given $\pi\_t$    |

# Definition

Consider the following learning problem. You are faced repeatedly with a choice among k different options, or actions. After each choice you receive a numerical reward chosen from a stationary probability distribution that depends on the action you selected. Your objective is to maximize the expected total reward over some time period, for example, over 1000 action selections, or time steps.

This is the original form of the k-armed bandit problem, so named by analogy to a slot machine, or "one-armed bandit", except that it has k levers instead of one. Each action selection is like a play of one of the slot machine’s levers, and the rewards are the payoffs for hitting the jackpot. Through repeated action selections you are to maximize your winnings by concentrating your actions on the best levers.

In short, we need to calculate the following formula:

$$
\begin{align}
q\_\*(a) &\doteq \mathbb{E}[R\_t|A\_t=a]\ \forall{a}\in\{1,\dots,k\} \\\\
&=\sum\_{r}{p(r|a)r}
\end{align} \tag{1}
$$

To explore the best result, there are two types of actions, ***exploitation*** and ***exploration***.
- ***Exploitation*** means selecting the greedy action so that you can gain high reward.
- ***Exploration*** means selecting the nongreedy action in which you may improve your estimate value.

# Method

## Action-value Method

This is the most simple and straight one. We calculate **value** of action $a$ using following formula:

$$
Q\_t(a)=\frac{sum\ of\ rewards\ when\ a\ taken\ prior\ to\ t}{number\ of\ times\ a\ taken\ prior\ to\ t}=\frac{\sum\_{i=1}^{t-1}{R\_i \cdot \mathbb{I}\_{A\_i=a}}}{\sum\_{i=1}^{t-1}{\mathbb{I}\_{A\_i=a}}}=\frac{\sum\_{i=1}^{t-1}{R\_i}}{t-1}
\tag{2}
$$

When $n \rightarrow \infty$, $Q\_t(a)$ converges to $q\_\*(a)$. We call this the *sample-average*. When $t=1$, we may set $Q\_1(a)=0$. Then, the **selection** rule is greedy selection, as the following:

$$A\_t\doteq \mathop{\arg\max}\_{a}Q\_t(a) \tag{3}$$

For those equal valued $Q\_t(a)$, we just arbitrarily select one. This step is called ***exploiting***. To ***explore*** other actions which may improve the result, a simple **strategy** called *$\epsilon-greedy$* is used.

- For each action, with a small probability $\epsilon$, we select to ***explore***, to randomly select an action among all actions. With probability $1-\epsilon$, we do ***exploitation*** normally.

Say short, the test result shows that *$\epsilon-greedy$* method outperformed than pure greedy method, when $ \epsilon=0$.

## Incremental Implementation

At time $t$, we select one action $n$-th time. The estimate value of the $n$-th action is:

$$ Q\_n\doteq\frac{R\_1+R\_2+\cdots+R\_{n-1}}{n-1} \tag{4}$$

We can store the sum of $R$, add each new reward on it, and then calculate the current value.

$$ Q\_{n+1}=\frac{sum\_{1}^{n-1}+R\_n}{n} \tag{5}$$

We only need to store $sum$ and $n$. Also we can see it in an iteration format.

$$
\begin{align}
Q\_{n+1}&=\frac{1}{n}\sum\_{i=1}^{n}{R\_i}\\\\
&=\frac{1}{n}(R\_n+\sum\_{i=1}^{n-1}{R\_i})\\\\
&=\frac{1}{n}(R\_n+(n-1)\frac{1}{n-1}\sum\_{i=1}^{n-1}{R\_i})\\\\
&=\frac{1}{n}(R\_n+(n-1)Q\_n)\\\\
&=\frac{1}{n}(R\_n+nQ\_n-Q\_n)\\\\
&=Q\_n+\frac{1}{n}[R\_n-Q\_n]
\end{align}
\tag{6}
$$

To **generalize** it, the form is:

$$ NewEstimate \leftarrow OldEstimate+StepSize[Target-OldEstimate] \tag{7}$$

The $[Target-OldEstimate]$ is an *error* in the estimation. And $StepSize \in [0,1]$. It has a sense of similarity with the gradient descent.

Finally, a simple bandit algorithm is as the following:

- Initialize, for a = 1 to k:
  - $Q(a)\leftarrow 0$
  - $N(a)\leftarrow 0$
- Loop:
  - $ A=
    \begin{cases}
    \arg\max\_{a}Q(a) & & with\ probability\ 1-\epsilon \\\\
    a\ random\ action & & with\ probability\ \epsilon
    \end{cases} $
  - $R\leftarrow bandit(A)$
  - $N(A) \leftarrow N(A)+1$
  - $Q(A) \leftarrow Q(A)+\frac{1}{N(A)}[R-Q(A)]$

## Tracking a Nonstationary Problem

In reinforcement learning, a common situation is that we give more weight to recent rewards than to long-past rewards. When we assign $StepSize$ an value $\alpha \in (0,1]$. The formula $(6)$ can be transformed to the following:

$$
\begin{align}
Q\_{n+1}&=Q\_n+\alpha[R\_n-Q\_n] \\\\
&=\alpha R\_n+(1-\alpha)Q\_n \\\\
&=\alpha R\_n+(1-\alpha)[\alpha R\_{n-1}+(1-\alpha)Q\_{n-1}] \\\\
&=\cdots \\\\
&=(1-\alpha)^nQ\_1+\sum\_{i=1}^{n}{\alpha(1-\alpha)^{n-i}R\_i}
\end{align}
\tag{8}
$$

We call it a weighted average because the sum of the weights is $(1-\alpha)^n+\alpha(1-\alpha)^{n-i}=1$, which is also called *exponential recency-weighted average*.

Let's use $\alpha\_n(a)$ denoting the step-size parameter, which is a list of sequence with no length limitation. The following is the condition:

$$ \sum\_{n=1}^\infty{\alpha\_n(a)}=\infty, \sum\_{n=1}^\infty{\alpha\_n^2(a)} < \infty \tag{9}$$

The first condition is required to guarantee that the steps are large enough to eventually overcome any initial conditions or random fluctuations. The second condition guarantees that eventually the steps become small enough to assure convergence.

**Note**:

- *Sample-average*, $\alpha\_n(a)=\frac{1}{n}$, meets all conditions. If $\alpha\_n(a)=\alpha$, a constant, it will not meet the second condition and will never completely converge but continue to vary in response to the most recently received rewards.
- The sequence of step-size parameters that meets formular $(9)$ conditions are often very slow. It is often used in theoretical work but not in applications and empirical research.

# Optimization

## Optimistic Initial Values

In above formula $(8)$, the selection of $Q\_1$ can affect the converge speed and accuracy. We can set $Q\_1(a)=+5$, which is wildly optimistic initial value. With such value, each reward of the following value will be "unsatisfied", so that it will go to ***explore***. We call the technique *optimistic initial values*.

The technique is quite effective on stationary problems, but not on nonstationary problems. The reason is that it always start from the beginning. If the task changes, it needs to learn from the start and do ***exploration***. Also, we don't know the maximal reward.

## Upper-Confidence-Bound Action Selection

In the former formulas, we all discuss about how to get a better $Q$, ***exploitation***, value but ignore how to ***explore*** more efficiently. One effective way of doing this is to select actions according to:

$$ A\_t \doteq \arg\max\_a[Q\_t(a)+c\sqrt\frac{\ln{t}}{N\_t(a)}] \tag{10}$$

$N\_t(a)$ denotes the number of times that action a has been selected prior to time t. $c>0$ controls the degree of ***exploration***. If $N\_t(a)=0$, then a is considered to be a maximizing action. This is called *upper confidence bound* (UCB).

The experiment results shows that it performs better than $\epsilon$-greedy on the problem. However, there are several reasons why it is usually not pratical in other reinforcement learning.

- It is more complex than $\epsilon$-greedy method.
- It is hard to deal with large state spaces.

In short, it is specific designed for the k-armed bandit problem.
