---
title: Finite Markov Decision Processess Notes (Cont.)
date: 2020-03-15 00:36:41
tags: [Coursera, Reinforcement Learning, Notes]
categories: [Reinforcement Learning]

---

**Fundamentals of Reinforcement Learning Week3 Notes**

# Policies and Value Functions

## Policy

***Policy*** is a mapping from states to actions. There are two kinds of policies:

### Deterministic Policy

It can be represented in the form $\pi(s)=a$. Each state will lead to an action but not all actions have corresponding states.

### Stochastic Policy

It can be formulized as $\pi(a|s)$, which is a probability distribution. Since it is a distribution, it should follows the following rules:

- $\sum\_{a \in A(s)}\pi(a|s)=1$
- $\pi(a|s) \ge 0$

Notes:

- A policy depends only on the ***current state***.
- It is a restriction ***on the state*** rather than on the agent.

## Value Function

***Value function*** is designed to evaluate expected return in the future in a given state. It aggregates many possible future returns into a single number.

### State-value function

The value function of a state $s$ under a policy $\pi$ denoted $v\_\pi(s)$, is the expected return when starting in $s$ and following $\pi$ thereafter. For MDPs, we can define $v\_\pi$ formally by:

$$
v\_\pi(s) \doteq \mathbb{E}\_\pi[G\_t|S\_t=s] = \mathbb{E}\_\pi[\sum\_{k=0}^\infty{\gamma^kR\_{t+k+1}}|S\_t=s], \text{for all } s \in S \tag{1}
$$

The value of any terminal state is 0. We call the function $v\_\pi(s)$ the ***state-value function for policy $\pi$***.

### Action-value function

Similarly, we define the value of taking action $a$ in state $s$ under a policy $\pi$, denoted $q\_\pi(s, a)$, as the expected return starting from $s$, taking the action $a$, and thereafter following policy $\pi$:

$$
q\_\pi(s,a) \doteq \mathbb{E}\_\pi[G\_t|S\_t=s, A\_t=a] = \mathbb{E}\_\pi[\sum\_{k=0}^\infty{\gamma^kR\_{t+k+1}}|S\_t=s, A\_t=a] \tag{2}
$$

We call the function $q\_\pi(s,a)$ the ***action-value function for policy $\pi$***.

Value functions are crucial in reinforcement learning. They allow an agent to query the quality of its current situation without waiting to observe the long-term outcome. The benefits is twofold:

- The return is immediately available
- The return may be random due to stochastic in both the policy and environment dynamics

## Bellman Function

Bellman function is used to formalize the connection between the value of $s$ and the value of its possible successor states.

### State-value Bellman Function

$$
\begin{align}
v\_\pi(s) &\doteq \mathbb{E}\_\pi[G\_t|S\_t=s] \\\\
&= \mathbb{E}\_\pi[R\_{t+1} + \gamma G\_{t+1}|S\_t=s] \\\\
&= \sum\_a\pi(a|s)\sum\_{s'}\sum\_rp(s',r|s,a)[r+\gamma \mathbb{E}[G\_{t+1}|S\_{t+1}=s']] \\\\
&= \sum\_a\pi(a|s)\sum\_{s'}\sum\_rp(s',r|s,a)[r+\gamma v\_\pi(s')]
\end{align}
\tag{3}
$$

### Action-value Bellman Function

$$
\begin{align}
q\_\pi(s,a) &\doteq \mathbb{E}\_\pi[G\_t|S\_t=s,A\_t=a] \\\\
&= \sum\_{s'}\sum\_rp(s',r|s,a)[r+\gamma E\_\pi[G\_{t+1}|S\_{t+1}=s']] \\\\
&= \sum\_{s'}\sum\_rp(s',r|s,a)[r+\gamma \sum\_{a'}\pi(a'|s')\mathbb{E}\_\pi[G\_{t+1}|S\_{t+1}=s', A\_{t+1}=a']] \\\\
&= \sum\_{s'}\sum\_rp(s',r|s,a)[r+\gamma \sum\_{a'}\pi(a'|s')q\_\pi(s',a')]
\end{align}
\tag{4}
$$

### Summary

- Bellman functions provides us a way to solve the value function by writing a system of linear equations.
- It can be only directly used on small MDPs

# Optimal Policies and Optimal Value Functions

An ***optimal policy*** is defined as the policy with the ***highest possible value function*** in ***all states***. There is at least one exist. It can be concatenated by best parts of multiple policies. Due to the exponential number of possible policies, brute-force searching is not impossible.

We always use $\pi\_\*$ and $q\_\*$ to denote optimal state-value function and optimal action-value function respectively.

## Bellman Optimal Equation for $v\_\*$

$$
v\_\* = v\_{\pi\_\*}(s) \doteq \mathbb{E}\_{\pi\_\*}[G\_t|S\_t=s] = \max\_\pi v\_\pi(s) \text{ for all }s \in S \tag{5}
$$

According to the formula $(3)$,

$$
v\_\*(s) = \sum\_a\pi\_\*(a|s)\sum\_{s'}\sum\_rp(s',r|s,a)[r+\gamma v\_\*(s')] \tag{6}
$$

Since the optimal policy can be made of optimal action in every state, the $\pi\_\*$ would be $1$ for the optimal action, achieve the highest value, and $0$ for else, which means it becomes a deterministic optimal policy. Then, it becomes

$$
v\_\*(s) = \max\_a \sum\_{s'}\sum\_{r}p(s',r|s,a)[r+\gamma v\_\*(s')] \tag{7}
$$

## Bellman Optimal Equation for $q\_\*$

Similarly, we can do it on $q\_\*$.
$$
\begin{align}
q\_\* = q\_{\pi\_\*}(s,a) &\doteq \max\_\pi q\_\pi(s,a) \text{, for all } s \in S \text{ and } a \in A \\\\
q\_\*(s,a) &= \sum\_{s'}\sum\_rp(s',r|s,a)[r+\gamma \sum\_{a'}\pi\_\*(a'|s')q\_\*(s',a')] \\\\
q\_\*(s,a) &= \sum\_{s'}\sum\_rp(s',r|s,a)[r+\gamma \max\_a q\_\*(s',a')]
\end{align}
\tag{8}
$$

Notes:

- Since $v\_\*$ and $q\_\*$ already take into account the reward consequence of all possible future behavior, the greedy method $\max$ here will finally lead the the final optimal result.
- We cannot simply use linear system solver to solve the optimal $\pi\_\*$ because it is the thing we would like to solve.

# Determine an Optimal Policy

For $v\_\*(s)$,

$$
\begin{align}
v\_\*(s) &= \max\_a \sum\_{s'}\sum\_{r}p(s',r|s,a)[r+\gamma v\_\*(s')] \\\\
\pi\_\*(s) &= \arg\max\_a \sum\_{s'}\sum\_{r}p(s',r|s,a)[r+\gamma v\_\*(s')]
\end{align}
\tag{9}
$$

If we have $q\_\*(s,a)$, we can get $\pi\_\*(s)$ easier,
$$
\pi\_\*(s)=\arg\max\_a q\_\*(s,a) \tag{10}
$$
