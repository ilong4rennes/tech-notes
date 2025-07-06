# RL
## Learning Paradigms

- Supervised Learning$$
\mathcal{D} = \left\{ \left( x^{(n)}, y^{(n)} \right) \right\}_{n=1}^{N}
$$
	- Regression: $y^{(n)} \in \mathbb{R}$
	- Classification: $y^{(n)} \in \{1, \ldots, C\}$
- Reinforcement Learning$$
\mathcal{D} = \left\{ \left( s^{(n)}, a^{(n)}, r^{(n)} \right) \right\}_{n=1}^{N}
$$
## Components

### From Environment (MDP)
- State space, $S$
- Action space, $A$
- Reward function
	- Stochastic, $p(r|s,a)$
	- Deterministic, $R: S\times A \rightarrow \mathbb{R}$
- Transition function
	- Stochastic, $p(s'|s,a)$
	- Deterministic, $\delta: S \times A \rightarrow S$
### From Model
- Policy, $\pi: S\rightarrow A$
	- Specifies an action to take in every state
- Value function, $V^\pi: S\rightarrow \mathbb{R}$ 
	- Measures the expected total payoff of starting in some state s and executing policy $\pi$

## Markov Decision Process (MDP)

1. Start in some initial state $s_0$
2. For time step $t$
	1. Agent observes state $s_t$
	2. Agent takes action $a_t=\pi(s_t)$
	3. Agent receives reward $r_t \sim p(r|s_t, a_t)$
	4. Agent transitions to state $s_{t+1}\sim p(s'|s_t, a_t)$
3. Total reward is $\sum_{t=0}^\infty \gamma^t r_t$, $\gamma$ = discount factor ($0\leq\gamma<1$)

- MDPs make the Markov assumption: the reward and next state only depend on the current state and action.

## Key Challenges

- The algorithm has to gather its own training data  
- The outcome of taking some action is often stochastic or unknown until after the fact  
- Decisions can have a delayed effect on future outcomes (exploration-exploitation tradeoff)

## Objective Function

- Goal: Find a policy $\pi^* = \underset{\pi}{\text{argmax}}\ V^{\pi}(s) \ \forall s \in S$ for choosing "good" actions that maximize: $$\mathbb{E}[\text{total reward}] = \mathbb{E} \left[ \sum_{t=0}^{\infty} \gamma^t r_t \right]$$
- The above is called the "infinite horizon expected future discounted reward"
### Deterministic Transitions & Deterministic Rewards

$$
\begin{align*}
	V^{\pi}(s) &= \text{discounted total reward of starting in state } s \text{ and executing policy } \pi \text{ forever}  \\
    &= R(s, \pi(s)) + \gamma R(s_1 = \delta(s, \pi(s)), \pi(s_1)) \\
    &\phantom{=} + \gamma^2 R(s_2 = \delta(s_1, \pi(s_1)), \pi(s_2)) + \cdots \\
    &= R(s,\pi(s)) + \sum_{t=1}^{\infty} \gamma^t R(s_t = \delta(s_{t-1}, \pi(s_{t-1})), \pi(s_t)) \text{ for some } 0 \leq \gamma < 1 \\
    \end{align*}
$$
### Stochastic Transitions & Deterministic Rewards

$$
\begin{align}
V^{\pi}(s) &= \mathbb{E}\left[ \text{discounted total reward of starting in state } s \text{ and executing policy } \pi \text{ forever} \right] \\
&= \mathbb{E}_{p(s'|s,a)} \left[ R(s, \pi(s)) + \gamma R(s_1, \pi(s_1)) + \gamma^2 R(s_2, \pi(s_2)) + \ldots \right] \\
&\approx R(s, \pi(s)) + \sum_{t=1}^{\infty} \gamma^t \mathbb{E}_{p(s'|s,a)} [ R(s_t, \pi(s_t)) ] \text{ for some } 0 \leq \gamma < 1
\end{align}
$$
### Bellman Equations
- recursive def of $$V^{\pi}(s) = R(s, \pi(s)) + \gamma \sum_{s_1 \in S} p(s_1 | s, \pi(s))V^{\pi}(s_1)$$
#### Optimal value function

$$

\begin{align}
V^{\pi}(s) &= \mathbb{E}[\text{discounted total reward of starting in state } s \text{ and executing policy } \pi \text{ forever}] \\

&= \mathbb{E}[R(s_0, \pi(s_0)) + \gamma R(s_1, \pi(s_1)) + \gamma^2 R(s_2, \pi(s_2)) + \cdots | s_0 = s] \\

&= R(s, \pi(s)) + \gamma \mathbb{E}[R(s_1, \pi(s_1)) + \gamma R(s_2, \pi(s_2)) + \cdots | s_0 = s] \\

&= R(s, \pi(s)) + \gamma \sum_{s_1 \in S} p(s_1 | s, \pi(s))(R(s_1, \pi(s_1)) 

+ \gamma \mathbb{E}[R(s_2, \pi(s_2)) + \cdots | s_1]) \\
\end{align}

$$
$$
V^*(s) = V^{\pi^*}(s) = \max_{a \in A} R(s,a) + \gamma \sum_{s' \in S} p(s'|s,a) V^*(s')
$$
- system of $|s|$ equations and $|s|$ variables
#### Optimal policy
- desired output of RL algorithm

Given $V^*$,$R(s,a)$, $p(s'|s,a)$, $\gamma$,    $$
\pi^*(s) = \underset{a \in A}{\mathrm{argmax}} \left[ R(s,a) + \gamma \sum_{s' \in S} p(s'|s,a) V^*(s') \right]$$
- $R(s,a)$ = Immediate Reward
- $\gamma \sum_{s' \in S} p(s'|s,a) V^*(s')$ = (Discounted) Future Reward

# Value & Policy Iteration (Dynamic Programming)

## Fixed Point Iteration

![[Screen Shot 2024-04-23 at 11.39.21.png]]

![[Screen Shot 2024-04-23 at 13.46.47.png]]
## Value Iteration
### Synchronous Value Iteration
- without $Q(s,a)$ table
![[Screen Shot 2024-04-17 at 10.03.56.png]]
### Asynchronous Value Iteration
- with $Q(s,a)$ table
![[Screen Shot 2024-04-17 at 10.03.45.png]]
- $Q(s,a)=R(s, a) + \gamma \sum_{s' \in S} p(s'|s, a) V(s')$ 

### Value Iteration Theory
1. $V$ converges to $V^*$, if each state is visited infinitely often
	- holds for both asynchronous and synchronous updates
2. If $\max_{s} | V^{t+1}(s) - V^t(s) | < \epsilon$, then $\max_{s} | V^{t+1}(s) - V^*(s) | < \frac{2 \epsilon \gamma}{1 - \gamma}, \ \forall s$
	- provides reasonable stopping criterion for value iteration
3. Greedy policy will be optimal in a finite number of steps (even if not converged to optimal value function)
## Policy Iteration

### Algorithm
![[Screen Shot 2024-04-23 at 16.27.04.png]]
- #. of possible policies $\leq |A|^{|S|}$ 
### Policy Iteration Theory
1. Finite possible policies for a finite sized state and action space
2. Number of iterations to converge is bounded
	- Value iteration: $O(|A||S|^2)$ per iteration
	- Policy iteration: $O(|A||S|^2+|S|^3)$ per iteration
# Q-Learning (Temporal Difference Learning)

## Q-Learning Motivation and $Q^*(s,a)$

### Non-deterministic version

**Motivation**
Q: What if we don’t know the reward function $R(s,a)$ / transition probabilities $p(s'|s,a)$?
A: Then value iteration and policy iteration don’t work!

**Definition**
Let $Q^*(s,a)$ be the (true) expected discounted future reward of taking action $a$ in state $s$ and following optimal policy $\pi^*$
$$
V^*(s) = \max_a Q^*(s, a)
$$
$$
Q^*(s, a) = R(s, a) + \gamma \sum_{s' \in S} p(s' | s, a) V^*(s')
$$
$$
\mathcal{Q}^*(s, a) = R(s, a) + \gamma \sum_{s' \in S} p(s' | s, a) \left[ \max_{a'} Q^*(s', a') \right]
$$

**Key Insight**: If we can learn $Q^*$, we can define $\pi^*$ without knowing $R(s,a)$ / $p(s'|s,a)$$$\pi^*(s) = \underset{a \in A}{\mathrm{argmax}} \ Q^*(s, a)
$$
### Deterministic version
![[Screen Shot 2024-04-18 at 11.51.53.png]]

## Q-Learning Algorithm

### Deterministic transition

![[Screen Shot 2024-04-18 at 11.52.35.png]]

### Non-deterministic transition
![[Screen Shot 2024-04-18 at 11.54.58.png]]
![[Screen Shot 2024-04-18 at 12.06.32.png]]
### Q-Learning Convergence
- Q converges to Q* with probability 1.0, assuming… 
	1. each is visited infinitely often 
	2. 0 ≤ $\gamma$ < 1 
	3. rewards are bounded $|R(s,a)| < \beta$, for all 
	4. initial Q values are finite 
	5. Learning rate $\alpha_t$ follows some schedule s.t. $\sum_{t=0}^{\infty} \alpha_t = \infty$ and $\sum_{t=0}^{\infty} \alpha_t^2 = 0$, e.g., $\alpha_t = \frac{1}{t+1}$
- Q-Learning is exploration insensitive 
	- visiting the states in any order will work assuming point 1 is satisfied 
- May take many iterations to converge in practice
- Reordering experiences: some algorithms are faster thus better than others
- We need to retrain our RL agent every time we change our state space, but whether state space changes from one setting to another is determined by design of state representation

## Deep Q-Learning
- Question: state space S too large to represent with a table
- Key Idea:
	1.  Use a neural network $Q(s,a;\theta)$ to approximate $Q^*(s,a)$
	2. Learn $\theta$ via SGD with training examples $<s_t, a_t, r_t, s_{t+1}>$
### Model
- Represent states using some feature vector $\vec{s}_t \in \mathbb{R}^M$, eg. $\vec{s}_t = \begin{bmatrix} 1 & 0 & \dots & 1 \end{bmatrix}^T$

1. Define a neural network![[Screen Shot 2024-04-24 at 14.16.42.png]]
2. Define a bunch of linear regressors (technically still neural networks), one for each action (let $K=|A|$) $$Q(\vec{\bar{s}}, a_k; \boldsymbol{\theta}) = \boldsymbol{\theta}_k^T \vec{\bar{s}} \text{ where } \boldsymbol{\theta} = \begin{bmatrix} \boldsymbol{\theta}_1 \\ \boldsymbol{\theta}_2 \\ \vdots \\ \boldsymbol{\theta}_K \end{bmatrix} \in \mathbb{R}^{K \times M} \\ $$

- Goal: $K \times M \ll |S| \rightarrow \text{computational tractability!}$
- Gradients are easy:  $$\nabla_{\boldsymbol{\theta}_j} Q(\vec{\bar{s}}, a_k; \boldsymbol{\theta}) = 
\begin{cases} 
\vec{0} & \text{if } j \neq k \\
\vec{\bar{s}} & \text{if } j = k 
\end{cases}$$
### Loss Function
![[Screen Shot 2024-04-24 at 14.43.28.png]]
### Algorithm
![[Screen Shot 2024-04-24 at 14.44.14.png]]
**Experience Replay** 
![[Screen Shot 2024-04-24 at 14.44.45.png]]