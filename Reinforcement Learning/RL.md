---
title: Reinforcement Learning
output: pdf_document
---

# Reinforcement Learning Notes

## K-Bandit Problem

In the _K-armed_ bandit problem, we have an agent who chooses between _k_ actions and gets a reward based on the chosen action.

##### Value of an action

Value of an action is defined as the expected reward the agent will receive on choosing that action.
$$q(a) \dot{=} E[R_t \vert A_t=a] \forall a \in \{1,...,k\}=\sum_{a}p(r \vert a)r$$
$q(a)$ is not known, so we have to estimate it. The goal of the agent is to maximize the expected reward i.e. $q(a)$,
$$a_{max} = \argmax_{a}q(a)$$

#### Sample-average Method

Sample average of an action is defined as the ratio of sum of rewards when action $a$ was taken before time $t$ to the number of times action $a$ was taken before time $t$.
$$Q_t(a)\dot{=} \frac{\sum_{i=1}^{t-1}R_i}{t-1}$$

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
> $$A_t \dot{=} \argmax_{a}\left[Q_t(a)+c\sqrt{\frac{\ln t}{N_t(a)}}\right],$$

Here $N_t(a)$ = number of times that action a has been selected prior to time $t$, $c$ controls the degree of exploration. If $N_t(a)=0$, then $a$ is considered to be a maximizing action.

##### Gradient Bandit Algorithms

In this algorithm the agent learns a numerical preference for each action $a$, denoted by $H_t(a)$. The larger the preference, the more often that action is taken. In this algorithm only the relative preference os one action over another is important. The action probabilities are determined according to a $soft$-$max$ $distribution$ (Gibbs or Boltzmann distribution) as follows:
$$\text(Pr)\{a_t=a\}\dot{=}\frac{e^{H_t(b)}}{\sum_{b=1}^{k}{e^{H_t(b)}}}=\pi_t(a)$$
Here $\pi_t(a)$ denoted the probability of taking action $a$ at time $t$. Initially all action have same preferences so that they all have equal probability of being selected.

## Markov Decision Processes

MDPs are a classical formalization of sequential decision making, where current actions also influence subsequent situations and through those future rewards. Thus MDPs involve delayed rewards and the need to tradeoff immediate and delayed reward.

In a finite MDP, the sets of states, actions and rewards all have a finite number of elements. In this case, the random variable $R_t$ and $S_t$ have well defined discrete probability distributions dependent only on the preceding state and action.

$$p(s', r \vert s, a) \dot{=} Prob\{S_t=s', R_t=r \vert S_{t-1}=s,A_{t-1}=a\}$$

$$\sum_{s' \in S}\sum_{r \in R}{p(s',r \vert s, a)}=1~~\forall s \in S, a \in A(s)$$

### The Reward Hypothesis

In reinforcement learning, the goal of the agent is formalized in terms of rewards. At each time step the reward is a simple number and the agent's goal is to maximize the total reward. This means maximizing not immediate reward, but cumulative reward. This informal idea is called the $reward$ $hypothesis$:
From _Reinforcement Learning An Introduction_ 2nd edition by _Richard S. Sutton_ and _Andrew G. Barto_

> That all of what we mean by goals and purposes can be well thought of as the maximization of the expected value of the cumulative sum of a received scalar signal (called reward).

#### Episodic and continuous tasks

In episodic tasks the agent-environment interaction breaks into subsequences or as we call them $episodes$, such as a game, or a maze, or any kind of repeated interaction. Each episode starts in a standard state and ends in a different, unique state called the $terminal$ $state$, followed by another episode. Each episode begins independently of the previous episodes. Tasks with episodes of this kind are called $episodic$ $tasks$.
The cumulative reward for episodic tasks can be defined as sum of rewards of each episodes
$$G_t \dot{=} R_{t+1}+R_{t+2}+...+R_T,$$
where T is a final time step.

On the other hand, in many cases the agent-environment interaction does not break into identifiable episodes, but goes on continuously. These kind of tasks are called $continuing$ $tasks$.
The cumulative reward formulation used for episodic tasks can't be used for continuing tasks because the final time step would be $T=\infty$, and hence the total reward could itself be infinite. Thus we use a slightly more complex definition of cumulative reward. Here we use the concept of $discounting$. Here the agent tries to select action so that the sum of the discounted reward it gets over the future is maximized.
$$G_t\dot{=}R_{t+1}+\gamma R_{t+2}+\gamma^2 R_{t+3}+...=\sum_{k=0}^{\infty}{\gamma^k R_{t+k+1}}$$,
where $0 \leq \gamma \leq 1$ is a parameter, called the $discount$ $rate$.
We can also write $G_t$ in terms of $G_{t+1}$
$$G_T=R_{t+1}+\gamma G_{t+1}$$

### Policies and Value Functions

A $policy$ is a mapping from states to probabilities of selecting each possible action. If the agent is following policy $\pi$ at some time $t$, then $\pi(a \vert s)$ is the probability that $A_t=a$ if $S_t=s$.

The $value$ $function$ of a state $s$ under a policy $\pi$, denoted $v_{\pi}(s)$, is the expected return when starting in state $s$ and following policy $\pi$ thereafter. For MDPs, we can define $v_{\pi}$ as
$$v_{\pi}(s)\dot{=}\mathbb{E}_{\pi}[G_t \vert S_t=s]=\mathbb{E}_{\pi}\left[\sum_{k=0}^{\infty}{\gamma^k R_{t+k+1} \bigg\vert S_t=s}\right]\text{ , for all }s \in \mathcal{S}$$
The function $v_{\pi}$ is called the $state$-$value$ $function$ for policy $\pi$.
Similarly the value of taking action $a$ in state $s$ under a policy $\pi$, denoted as $q_{\pi}(s,a)$, as the expected return starting from $s$, taking the action $a$, and thereafter following policy $\pi$:
$$q_{\pi}(s,a)\dot{=}\mathbb{E}_{\pi}[G_t \vert S_t=s, A_t=a]=\mathbb{E}_{\pi}\left[\sum_{k=0}^{\infty}{\gamma^k R_{t+k+1} \bigg\vert S_t=s, A_t=a}\right]\text{ , for all }s \in \mathcal{S}$$

### Bellman Equation Derivation

We can derive a recursive relation of the state-value function, called the $state$-$value$ $bellman$ $equation$.

$$
\begin{aligned}
  v_{\pi}(s) &= \mathbb{E}_{\pi}[G_t \vert S_t=s]\\
  &= \mathbb{E}_{\pi}[R_{t+1} + \gamma G_{t+1} \vert S_t=s]\\
  &= \sum_{a}{\pi(a \vert s)}\sum_{s'}\sum_{r}{p(s', r \vert s, a)\bigg[r+\gamma\mathbb{E}_\pi[G_{t+1} \vert S_{t+1}=s']\bigg]}\\
  &= \sum_{a}{\pi(a \vert s)}\sum_{s'}\sum_{r}{p(s', r \vert s, a)\bigg[r+\gamma v_\pi(s')\bigg]} ~~~ \forall s \in S\\
\end{aligned}
$$

Similarly we can also derive a recursive relation of the action-value function, called the $action$-$value$ $bellman$ $equation$.

$$
\begin{aligned}
  q_{\pi}(s, a) &= \mathbb{E}_{\pi}[G_t \vert S_t=s, A_t=a]\\
  &= \sum_{s'}\sum_{r}{p(s', r \vert s, a)\bigg[r+\gamma\mathbb{E}_\pi[G_{t+1} \vert S_{t+1}=s']\bigg]}\\
  &= \sum_{s'}\sum_{r}{p(s', r \vert s, a)\bigg[r+\gamma v_\pi(s')\bigg]}\\
  &= \sum_{s'}\sum_{r}{p(s', r \vert s, a)\bigg[r+\gamma \sum_{a'}\pi(a' \vert s') q_\pi(s', a') \bigg]}\\
\end{aligned}
$$

### Optimal Policies & Value Functions

For solving a reinforcement learning task we need to find a policy that achieves a lot of reward over the long run. For finite MDPs, we can define an optimal policy in the following way.
A policy $\pi$ is defined to be better than or equal to a policy $\pi'$ if its expected return is greater than or equal to that of $\pi'$ for all states. In other words, $\pi \geq \pi' \iff v_\pi(s) \geq v_{\pi'}(s)~\forall s \in S$
There is always at least one policy that is better than or equal to all other policies. This is an $optimal$ $policy$. However there can be more than one optimal policies, we denote all the optimal policies by $\pi_*$. They share the same state-value function, called the $optimal$ $state$-$value$ $function$, denoted $v_*$, and defined as
$$v_*(s) \dot{=} \max_{\pi}{v_\pi(s)},~\forall s \in S$$
Optimal policies also share the same $optimal$ $action$-$value$ $function$ $q_*$ defined as
$$q_*(s,a) \dot{=} \max_{\pi}{q_\pi(s,a),~\forall s \in S, a \in A(s)}$$
