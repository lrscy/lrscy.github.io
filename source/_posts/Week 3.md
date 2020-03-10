[toc]

# Week 3

## Policies and Value Functions

### Policy

***Policy*** is a mapping from states to actions. There are two kinds of policies:

#### Deterministic Policy

It can be represented in the form $\pi(s)=a$. Each state will lead to an action but not all actions have corresponding states.

#### Stochastic Policy

It can be formulized as $\pi(a|s)$, which is a probability distribution. Since it is a distribution, it should follows the following rules:

- $\sum_{a \in A(s)}\pi(a|s)=1$
- $\pi(a|s) \ge 0$

### Value Function

***Value function*** is designed to evaluate expected return in the future in a given state.

#### State-value function

The value function of a state $s$ under a policy $\pi$ denoted $v_\pi(s)$, is the expected return when starting in $s$ and following $\pi$ thereafter. For MDPs, we can define $v_\pi$ formally by:
$$
v_\pi(s) \doteq \mathbb{E}_\pi[G_t|S_t=s] = \mathbb{E}_\pi[\sum_{k=0}^\infty{\gamma^kR_{t+k+1}}|S_t=s], \text{for all } s \in S
$$
The value of any terminal state is 0. We call the function $v_\pi(s)$ the ***state-value function for policy $\pi$***.

#### Action-value function

Similarly, we define the value of taking action $a$ in state $s$ under a policy $\pi$, denoted $q_\pi(s, a)$, as the expected return starting from $s$, taking the action $a$, and thereafter following policy $\pi$:
$$
q_\pi(s,a) \doteq \mathbb{E}_\pi[G_t|S_t=s, A_t=a] = \mathbb{E}_\pi[\sum_{k=0}^\infty{\gamma^kR_{t+k+1}}|S_t=s, A_t=a]
$$
We call the function $q_\pi(s,a)$ the ***action-value function for policy $\pi$***.

Value functions are crucial in reinforcement learning. They allow an agent to query the quality of its current situation without waiting to observe the long-term outcome. The benefits is twofold:

- The return is immediately available
- The return may be random due to stochastic in both the policy and environment dynamics

### Bellman Function

Bellman function is used to formalize the connection between the value of $s$ and the value of its possible successor states.

#### State-value Bellman Function

$$
\begin{align}
v_\pi(s) &\doteq \mathbb{E}_\pi[G_t|S_t=s] \\
&= \mathbb{E}_\pi[R_{t+1} + \gamma G_{t+1}|S_t=s] \\
&= \sum_a\pi(a|s)\sum_{s'}\sum_rp(s',r|s,a)[r+\gamma \mathbb{E}[G_{t+1}|S_{t+1}=s']] \\
&= \sum_a\pi(a|s)\sum_{s'}\sum_rp(s',r|s,a)[r+\gamma v_\pi(s')]
\end{align}
$$

#### Action-value Bellman Function

$$
\begin{align}
q_\pi(s,a) &\doteq \mathbb{E}_\pi[G_t|S_t=s,A_t=a] \\
&= \sum_{s'}\sum_rp(s',r|s,a)[r+\gamma E_\pi[G_{t+1}|S_{t+1}=s']] \\
&= \sum_{s'}\sum_rp(s',r|s,a)[r+\gamma \sum_{a'}\pi(a'|s')\mathbb{E}_\pi[G_{t+1}|S_{t+1}=s', A_{t+1}=a']] \\
&= \sum_{s'}\sum_rp(s',r|s,a)[r+\gamma \sum_{a'}\pi(a'|s')q_\pi(s',a')]
\end{align}
$$

#### Summary

- Bellman functions provides us a way to solve the value function by writing a system of linear equations.
- It can be only directly used on small MDPs

## Optimal Policies and Optimal Value Functions

