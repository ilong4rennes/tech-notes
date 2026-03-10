---
tags: [artificial-intelligence]
---

# Markov Decision Processes

## Learning Objectives

1. Describe the definition of Markov Decision Process
2. Compute utility of a reward sequence given discount factor
3. Define policy and optimal policy of an MDP
4. Define state-value and (true) state value of an MDP
5. Define Q-value and (true) Q value of an MDP
6. Derive optimal policy from (true) state value or (true) Q-values
7. Write Bellman Equation for state-value and Q-value for optimal policy and a given policy
8. Describe and implement value iteration algorithm (through Bellman update) for solving MDPs
9. Describe and implement policy iteration algorithm (through policy evaluation and policy improvement) for solving MDPs
10. Understand convergence for value iteration and policy iteration

## Defining MDPs

### Formulation

A **Markov Decision Process (MDP)** is defined by:

1. A set of states $S$. For pacman this could be grid positions.
2. A set of actions $A$. For pacman, this would be North, South, East, and West.
3. A transition function $T(s, a, s')$ which represents the probability of being in state $s'$ after taking action $a$ from state $s$.
4. A reward function $R(s, a, s')$ which represents the reward received from being in state $s$, taking action $a$, and ending up in state $s'$.
5. A start state $s_0 \in S$
6. Potentially one or more terminal states

### Markov Property

The Markov property states that given the present state, the previous and future states are all conditionally independent. In MDPs, actions depend only on the current state. The following two expressions are equivalent for MDPs:

$$P(S_{t+1}=s_{t+1}|A_t=a_t, s_t=s_t, A_{t-1}=a_{t-1}, S_{t-1}=s_{t-1}...A_0=a_0, S_0=s_0)$$

$$P(S_{t+1}=s_{t+1}|A_t=a_t, s_t=s_t)$$

### Policies

A policy, $\pi$, represents a plan. More formally, $\pi$ is a function that maps states to actions ($\pi: S \rightarrow A$). For each state $s \in S$, $\pi(s)$ returns the corresponding action from the plan.

An optimal policy is one that would maximize expected utility if followed.

**Exercise:** How is this different than what expectimax does (hint: does expectimax tell you what to do for every state)?

### Discounting

The idea of discounting stems from the principle that a reward now is better than the same reward later. We weight each value of a state by some factor $\gamma \in [0, 1]$. Restricting $\gamma$ to this range ensures the game doesn't last forever with infinite reward.

More concretely, the reward is worth $\gamma^0 \cdot R$ now, and $\gamma^1 \cdot R$ after one step and $\gamma^2 R$ after two, and so on. This prioritizes immediate rewards over delayed ones. A $\gamma$ closer to 0 creates a smaller horizon and emphasizes immediate rewards, while a $\gamma$ closer to 1 weighs later rewards almost equally to immediate rewards.

## Solving MDPs

Assuming access to all dynamics information about an MDP (the transition and reward functions), we can directly solve for the optimal value function—the expected utility from being in state $s$ when acting optimally—which yields the optimal policy. Two algorithms for this are **value iteration** and **policy iteration**.

These methods are **offline learning** approaches, requiring no actual environmental interaction to derive a value function or policy. In Reinforcement Learning, we'll explore online learning methods.

## Value Iteration

Recall that a state's value, denoted $V(s)$, is the expected utility for starting in that state and acting optimally. The Q-value, denoted $Q(s, a)$, represents the expected utility of starting in state $s$, taking action $a$, and then acting optimally.

Intuitively, the optimal Q-value is defined as:

$$Q^*(s, a) = \sum_{s' \in S}T(s, a, s')(R(s, a, s') + \gamma V^*(s'))$$

where $V^*$ is the optimal value:

$$V^*(s) = \max_{a}Q(s, a)$$

The Q-value is a weighted sum over all reachable states from $s$ by action $a$ of the reward for landing in $s'$ plus the discounted value of $s'$. Weights are transition probabilities, so likelier states contribute more. The optimal value of a state is simply the maximum Q-value over all possible actions.

### Algorithm

Value iteration follows the Bellman update equation:

$$V(s) = \max_{a}\sum_{s' \in S}T(s, a, s')(R(s, a, s') + \gamma V(s'))$$

Each update represents one ply of the expectimax search tree. The algorithm:

1. Initialize all state values to 0
2. Apply the update rule: $$V_{k+1}(s) = \max_{a}\sum_{s' \in S}T(s, a, s')(R(s, a, s') + \gamma V_k(s'))$$ to all states
3. Repeat step 2 until convergence

Complexity for a single iteration is $O(|S|^2|A|)$ because for every state, we check all actions, and for each action, traverse all possible successor states (worst case $|S|$).

This can also be framed in terms of Q-values, yielding Q-value iteration. Follow the same steps but initialize all Q-values to zero and use:

$$Q_{k+1}(s, a) = \sum_{s' \in S}T(s, a, s')(R(s, a, s') + \gamma \max_{a'}Q_k(s', a'))$$

Convert to values at the end via:

$$V^*(s) = \max_{a}Q^*(s, a)$$

### Convergence Criteria

Value iteration converges to the unique optimal values for each state.

**Case 1:** If the expectimax search tree is finite with $M$ levels, value iteration converges, and $V_M$ holds the optimal values.

**Case 2 ($\gamma < 1$):** For any state $s$, $V_k(s)$ and $V_{k+1}(s)$ can be viewed as depth $k+1$ expectimax results in nearly identical search trees. The deepest layer for $V_k$ is zero but for $V_{k+1}$ contains rewards. Extrema for $V_{k+1}$ are $R_{min}$ and $R_{max}$. The difference between deepest layers is $\gamma^k \max |R|$. Since $\gamma < 1$, this difference shrinks each layer and eventually converges.

### Example

Running value iteration on Prince's House with $\gamma = 0.8$:

| Iteration | Kitchen | Living Room | Bedroom |
|-----------|---------|-------------|---------|
| $V_0$     | 0       | 0           | 0       |
| $V_1$     | 1       | 0           | 0       |
| $V_2$     | 1       | 0.475       | 0       |
| $V_3$     | 1       | 0.475       | 0       |

**$V_0$:** All values initialized to 0.

**$V_1$:**
- $V_1(\text{Kitchen}) = \max(1(1+0.8(0)), 1(0+0.8(0))) = \max(1, 0) = 1$
- $V_1(\text{Living Room}) = \max(0.75(-0.5+0.8(0)) + 0.25(1+0.8(0)), 1(0+0.8(0))) = \max(-0.125, 0) = 0$
- $V_1(\text{Bedroom}) = 0$ (terminal state)

**$V_2$:**
- $V_2(\text{Kitchen}) = \max(1(1+0.8(0)), 1(0+0.8(0))) = \max(1, 0) = 1$
- $V_2(\text{Living Room}) = \max(0.75(-0.5+0.8(1)) + 0.25(1+0.8(0)), 1(0+0.8(0))) = \max(0.475, 0) = 0.475$
- $V_2(\text{Bedroom}) = 0$ (terminal state)

**$V_3$:**
- $V_3(\text{Kitchen}) = \max(1(1+0.8(0)), 1(0+0.8(0.475))) = \max(1, 0.38) = 1$
- $V_3(\text{Living Room}) = \max(0.75(-0.5+0.8(1)) + 0.25(1+0.8(0)), 1(0+0.8(0.475))) = \max(0.475, 0.38) = 0.475$
- $V_3(\text{Bedroom}) = 0$ (terminal state)

Values converged, so we stop and return the optimal values from the last iteration.

### Policy Extraction

Value iteration computes optimal values for all states but doesn't specify actions. Policy extraction determines the corresponding policy $\pi_V$ for any given value set:

$$\pi_V(s) = \text{argmax}_a \sum_{s' \in S}T(s, a, s')(R(s, a, s') + \gamma V(s'))$$

We select the action yielding the highest value.

## Policy Iteration

Policy iteration consists of **policy evaluation** and **policy improvement**.

### Policy Evaluation

Policy evaluation calculates values for a fixed policy $\pi_i$ until convergence. The values $V^{\pi_i}(s)$ represent expected total discounted rewards starting in state $s$ and following $\pi_i$.

The recursive relationship (one-step lookahead):

$$V^{\pi_i}(s) = \sum_{s'} T(s, \pi_i(s), s')[R(s, \pi_i(s), s') + \gamma V^{\pi_i}(s')]$$

To calculate values, iterate the equation above until convergence:

$$V_{k+1}^{\pi_i}(s) = \sum_{s'} T(s, \pi_i(s), s')[R(s, \pi_i(s), s') + \gamma V_k^{\pi_i}(s')]$$

Using a fixed policy eliminates the action loop, reducing complexity to $O(S^2)$ per iteration.

Alternatively, solve values as a linear system of equations (optional method).

### Policy Improvement

After policy evaluation with fixed policy $\pi$, improve it by running one ply of the Bellman equations:

$$\pi_{i+1}(s) = \text{argmax}_a \sum_{s'} T(s, a, s') [R(s, a, s') + \gamma V^{\pi_i}(s')]$$

Run policy evaluation on the new policy and repeat until convergence.

### Example

Policy iteration on Prince's House with $\gamma = 0.8$, starting with the base policy: Move in Kitchen and Living Room.

| Iteration | Kitchen | Living Room | Bedroom |
|-----------|---------|-------------|---------|
| $\pi_0$   | Move    | Move        | N/A     |
| $V_0^{\pi_0}$ | 0 | 0 | 0 |
| $V_1^{\pi_0}$ | 0 | 0 | 0 |
| $\pi_1$   | Play    | Move        | N/A     |
| $V_0^{\pi_1}$ | 0 | 0 | 0 |
| $V_1^{\pi_1}$ | 1 | 0 | 0 |
| $V_2^{\pi_1}$ | 1 | 0 | 0 |
| $\pi_2$   | Play    | Play        | N/A     |
| $V_0^{\pi_2}$ | 0 | 0 | 0 |
| $V_1^{\pi_2}$ | 1 | -0.125 | 0 |
| $V_2^{\pi_2}$ | 1 | 0.475 | 0 |
| $V_3^{\pi_2}$ | 1 | 0.475 | 0 |
| $\pi_3$   | Play    | Play        | N/A     |

**POLICY 0** $\pi_0$

Policy evaluation:
- $V_0$: all values initialized to 0
- $V_1$:
  - $V_1(\text{Kitchen}) = 1(0 + 0.8(0)) = 0$
  - $V_1(\text{Living Room}) = 1(0+0.8(0)) = 0$

Values converged; extract new policy.

Policy Improvement (first value for Play, second for Move):
- $\pi_1(\text{Kitchen}) = \text{argmax}(1(1+0.8(0)), 1(0+0.8(0))) = \text{argmax}(1, 0)$ = Play
- $\pi_1(\text{Living Room}) = \text{argmax}(0.75(-0.5+0.8(0)) + 0.25(1+0.8(0)), 1(0+0.8(0))) = \text{argmax}(-0.125, 0)$ = Move

**POLICY 1** $\pi_1$

Policy evaluation:
- $V_1$:
  - $V_1(\text{Kitchen}) = 1(1 + 0.8(0)) = 1$
  - $V_1(\text{Living Room}) = 1(0+0.8(0)) = 0$
- $V_2$:
  - $V_2(\text{Kitchen}) = 1(1 + 0.8(0)) = 1$
  - $V_2(\text{Living Room}) = 1(0+0.8(0)) = 0$

Values converged; extract new policy.

Policy Improvement:
- $\pi_2(\text{Kitchen}) = \text{argmax}(1(1+0.8(0)), 1(0+0.8(0))) = \text{argmax}(1, 0)$ = Play
- $\pi_2(\text{Living Room}) = \text{argmax}(0.75(-0.5+0.8(1)) + 0.25(1+0.8(0)), 1(0+0.8(0))) = \text{argmax}(0.475, 0)$ = Play

**POLICY 2** $\pi_2$

Policy evaluation:
- $V_1$:
  - $V_1(\text{Kitchen}) = 1(1 + 0.8(0)) = 1$
  - $V_1(\text{Living Room}) = 0.75(-0.5+0.8(0)) + 0.25(1+0.8(0)) = -0.125$
- $V_2$:
  - $V_2(\text{Kitchen}) = 1(1 + 0.8(0)) = 1$
  - $V_2(\text{Living Room}) = 0.75(-0.5+0.8(1)) + 0.25(1+0.8(0)) = 0.475$
- $V_3$:
  - $V_3(\text{Kitchen}) = 1(1 + 0.8(0)) = 1$
  - $V_3(\text{Living Room}) = 0.75(-0.5+0.8(1)) + 0.25(1+0.8(0)) = 0.475$

Values converged; extract new policy.

Policy Improvement:
- $\pi_3(\text{Kitchen}) = \text{argmax}(1(1+0.8(0)), 1(0+0.8(0.475))) = \text{argmax}(1, 0.38)$ = Play
- $\pi_3(\text{Living Room}) = \text{argmax}(0.75(-0.5+0.8(1)) + 0.25(1+0.8(0)), 1(0+0.8(0.475))) = \text{argmax}(0.475, 0.38)$ = Play

The policy converged. Prince should always play to maximize treat intake. Note that policy iteration and value iteration produced the same optimal values and policy.

## Summary

Both value iteration and policy iteration are dynamic programming algorithms that compute optimal values for solving MDPs. Value iteration doesn't explicitly track a policy but implicitly recomputes it by taking the maximum over actions. Policy iteration maintains an explicit policy and performs multiple passes to improve it. By considering only one action per state, each iteration requires less time. However, policy iteration requires several passes of policy evaluation and improvement.
