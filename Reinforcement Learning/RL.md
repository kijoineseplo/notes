---
title: RL Notes
output: pdf_document
---

# Reinforcement Learning Notes

## K-Bandit Problem

In the _K-armed_ bandit problem, we have an agent who chooses between _k_ actions and gets a reward based on the chosen action.

##### Value of an action

Value of an action is defined as the expected reward the agent will receive on choosing that action.
$$q(a) \coloneqq E[R_t \vert A_t=a] \forall a \in \{1,...,k\}=\sum_{a}p(r \vert a)r$$
$q(a)$ is not known, so we have to estimate it. The goal of the agent is to maximize the expected reward i.e. $q(a)$,
$$a_{max} = \argmax_{a}q(a)$$

#### Sample-average Method

Sample average of an action is defined as the ratio of sum of rewards when action $a$ was taken before time $t$ to the number of times action $a$ was taken before time $t$.
$$Q_t(a)\coloneqq \frac{\sum_{i=1}^{t-1}R_i}{t-1}$$

#### Action selection

The agent can choose to exploit its current knowledge and choose the action which has highest current expected reward(called _greedy action_) to get most reward it can get right now.
$$a_g=\argmax_{a}Q_t(a)$$
Otherwise the agent can choose to sacrifice immediate reward and explore the other _non-greedy_ actions to gain more information about them.

The agent can't choose to explore and exploit at the same time, this is one of the fundamental problem in reinforcement learning, the **exploration-exploitation dilemma**.

#### Incremental update rule

$$
\begin{aligned}
Q_{n+1} &= \frac{1}{n} \sum_{i=1}^{n}R_i\\
        &= \frac{R_n+(n-1)Q_n}{n}\\
        &= Q_n+\frac{R_n-Q_n}{n}
\end{aligned}
$$

$$OldEstimate+StepSize[Target-OldEstimate] \rightarrow NewEstimate $$

#### Non-stationary bandit problem

In these problems the distribution of rewards changes with time. Note that the agent is not aware of these changes but would like to adapt to the changes.

One way to tackle this problem is to use constant step size so that the recent rewards have more weightage compared to the older rewards.

$$
\begin{aligned}
Q_{n+1} &= Q_n+\alpha(R_n-Q_n)\\
        &= \alpha R_n + (1-\alpha)Q_n\\
\end{aligned}
$$

Further unrolling this recursion

$$Q_{n+1}=(1-\alpha)^nQ_1+\sum_{i=1}^{n}{\alpha(1-\alpha)^{n-i}R_i}$$

#### Exploration-exploitation tradeoff

- Exploration - Improve agent knowledge for long-term benefit
- Exploitation - Exploit agent knowledge for short-term benefit

How to choose when to explore and when to exploit? How 'bout randomly?

##### Epsilon-Greedy Action Selection

The epsilon-greedy agent will exploit the greedy action with $1-\epsilon$ probability and explore with $\epsilon$ probability.

$$
A_t \leftarrow \begin{cases}
  \argmax Q_t(a) && \text{with probability }1-\epsilon \\
  a \sim Uniform(\{a_1...a_k\}) && \text{with probability } \epsilon
\end{cases}
$$

##### Optimistic initial value

We can change the initial action value $Q_1(a)$ in order to encourage the agent to explore more.

When the initial action value is high, whichever action the agent choose initially will give less reward than the starting estimate hence encouraging the agent to explore other actions.

Optimistic initial values only causes early exploration, this means agents won't continue to explore after some time. This causes problems in non-stationary problems where the reward distribution changes over time. Another potential problem is that we may not always know how to set the optimistic initial values, because we may not know the maximal reward.

##### Upper-Confidence-Bound action selection

Quoting "Reinforcement Learning - An Introduction"

> Exploration is needed because there is always uncertainty about the accuracy of the action-value estimates. The greedy actions are those that look best at present, but some of the other actions may actually be better. $\epsilon$-greedy action selection forces the non-greedy actions to be tried, but indiscriminately, with no preference for those that are nearly greedy or particularly uncertain. It would be better to select among the non-greedy actions according to their potential for actually being optimal, taking into account both how close their estimates are to being maximal and the uncertainties in those estimates. One effective way of doing this is to select actions according to
> $$A_t \coloneqq \argmax_{a}\left[Q_t(a)+c\sqrt{\frac{\ln t}{N_t(a)}}\right],$$

Here $N_t(a)$ = number of times that action a has been selected prior to time $t$, $c$ controls the degree of exploration. If $N_t(a)=0$, then $a$ is considered to be a maximizing action.
