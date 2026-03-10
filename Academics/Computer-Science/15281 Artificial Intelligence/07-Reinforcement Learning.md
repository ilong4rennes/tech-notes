---
tags: [artificial-intelligence]
---

# Reinforcement Learning

## Learning Objectives

1. Understand the concept of exploration, exploitation, regret
2. Describe the relationships and differences between:
   - Markov Decision Processes (MDP) vs Reinforcement Learning (RL)
   - Model-based vs Model-free RL
   - Temporal-Difference Value Learning (TD Value Learning) vs Q-Learning
   - Passive vs Active RL
   - Off-policy vs On-policy Learning
   - Exploration vs Exploitation
3. Describe and implement:
   - Temporal difference learning
   - Q-Learning
   - ε-Greedy algorithm
   - Approximate Q-learning (Feature-based)
4. Derive weight update for Approximate Q-learning

## Definitions

### MDP vs RL

MDPs describe how we formulate the environment, while RL methods derive useful information (like state values or optimal policies) about the MDP. Algorithms like value and policy iteration can be considered RL, though they are "offline" methods.

**Online learning:** Learning by taking actions and observing outcomes in an environment.

**Offline learning:** Learning from environment dynamics information alone without needing to act in the environment. This requires access to the MDP's transition and reward functions.

### Passive vs Active Learning

Within online learning:

**Passive learning:** The policy π we follow while exploring is fixed. The goal is generally to evaluate states or our policy.

**Active learning:** The policy we follow changes over time.

### Model-based vs Model-free Learning

**Model-based learning:** We first try to model the environment by estimating transition and reward functions. This can accompany offline learning methods.

**Model-free learning:** We learn evaluations/policies without modeling the environment.

### On-policy vs Off-policy Learning

**On-policy learning:** The policy we're evaluating/improving is the current policy we're following.

**Off-policy learning:** The policy we're improving/evaluating differs from the one we follow (e.g., the optimal policy π*).

### Primary Algorithms

The two main online learning algorithms are **temporal difference (TD) learning** and **Q-learning**.

---

## TD Learning

TD learning estimates the utility of states from action-outcome information. Given an initial estimate V₀(s) and an observed sample (s, a, s', R(s, a, s')), we update our utility estimate using a weighted average:

$$V_{i + 1}(s) = (1 - \alpha)V_i(s) + \alpha (R(s, a, s') + \gamma V_i(s'))$$

Where α is the learning rate. As α increases, we give more weight to new observations.

Rearranged, this becomes:

$$V_{i + 1}(s) = V_i(s) + \alpha ((R(s, a, s') + \gamma V_i(s')) - V_i(s))$$

This updates the utility estimate using the **difference** between states, scaled by α.

### TD Learning Example

Consider four states: Kitchen, Bedroom, Living Room, and Laundry Room.

**Observed transitions:**
- (Kitchen, MOVE, Laundry, 2)
- (LivingRoom, MOVE, Kitchen, 6)
- (Bedroom, PLAY, LivingRoom, 1)
- (Kitchen, MOVE, Laundry, 2)
- (LivingRoom, PLAY, Laundry, 4)

With γ=1 and α=0.6:

$$V^π(s) ← 0.4 V^π(s) + 0.6 [R(s, π(s), s') + V^π(s')]$$

**Final values after all observations:**

| Transitions | Kitchen | LivingRoom | Bedroom | Laundry |
|---|---|---|---|---|
| (initial) | 0 | 0 | 0 | 0 |
| (K, MOVE, L, 2) | 1.2 | 0 | 0 | 0 |
| (LR, MOVE, K, 6) | 1.2 | 4.32 | 0 | 0 |
| (B, PLAY, LR, 1) | 1.2 | 4.32 | 3.19 | 0 |
| (K, MOVE, L, 2) | 1.68 | 4.32 | 3.19 | 0 |
| (LR, PLAY, L, 4) | 1.68 | 4.13 | 3.19 | 0 |

**Calculation after first observation (Kitchen, MOVE, Laundry, 2):**

$$V^π(Kitchen) ← 0.4(0) + 0.6[2 + 0] = 1.2$$

**Calculation after second observation (LivingRoom, MOVE, Kitchen, 6):**

$$V^π(LivingRoom) ← 0.4(0) + 0.6[6 + 1.2] = 4.32$$

**Calculation after third observation (Bedroom, PLAY, LivingRoom, 1):**

$$V^π(Bedroom) ← 0.4(0) + 0.6[1 + 4.32] = 3.19$$

**Calculation after fourth observation (Kitchen, MOVE, Laundry, 2):**

$$V^π(Kitchen) ← 0.4(1.2) + 0.6[2 + 0] = 1.68$$

**Calculation after fifth observation (LivingRoom, PLAY, Laundry, 4):**

$$V^π(LivingRoom) ← 0.4(4.32) + 0.6[4 + 0] = 4.13$$

---

## Q-Learning

Q-learning models Q-values, the expected utility of taking a particular action at a particular state. The optimal policy is characterized by:

$$π^*(s) = \text{argmax}_a Q(s, a) \quad \forall s$$

The Q-learning update rule is:

$$Q_{i + 1}(s, a) = (1 - \alpha)Q_i(s, a) + \alpha (R(s, a, s') + \gamma \max_{a'}Q(s', a'))$$

In difference form:

$$Q_{i + 1}(s, a) = Q_i(s, a) + \alpha (R(s, a, s') + \gamma \max_{a'}Q(s', a') - Q(s, a))$$

### Q-Learning Example

With α=0.6 and γ=1, using the same transitions as the TD example:

$$Q(s, a) = (0.4)Q(s, a) + 0.6 [R(s, a, s') + \max_{a'}Q(s', a')]$$

**Final results:**

| Transitions | (K, MOVE) | (LR, MOVE) | (B, PLAY) | (LR, PLAY) |
|---|---|---|---|---|
| (initial) | 0 | 0 | 0 | 0 |
| (K, MOVE, L, 2) | 1.2 | 0 | 0 | 0 |
| (LR, MOVE, K, 6) | 1.2 | 4.32 | 0 | 0 |
| (B, PLAY, LR, 1) | 1.2 | 4.32 | 3.19 | 0 |
| (K, MOVE, L, 2) | 1.68 | 4.32 | 3.19 | 0 |
| (LR, PLAY, L, 4) | 1.68 | 4.32 | 3.19 | 2.4 |

**Calculation after fifth observation (LivingRoom, PLAY, Laundry, 4):**

$$Q(LR, PLAY) = 0.4(0) + 0.6[4 + 0] = 2.4$$

### Count Exploration Q-Learning

An alternative to ε-greedy Q-learning that augments Q-values with an exploration term:

$$f(u, n) = u + \frac{k}{n+1}$$

Where u is the original Q-value, k is a hyperparameter, and n is the number of visits to the (state, action) pair.

The modified Q-update rule:

$$Q'(s,a) = Q'(s,a) + \alpha (r + \gamma \max_{a'}f(Q'(s', a'), N(s', a')) - Q'(s, a))$$

This incentivizes exploration of less-visited states. Advantage: can outperform ε-greedy in scenarios with long action sequences that have low initial rewards but high final rewards. Disadvantage: requires choosing hyperparameter k.

### Exploration vs Exploitation

A fundamental question in RL is balancing exploration versus exploitation — exploring new or less-seen states versus exploiting known good actions.

One solution is **ε-greedy exploration:** take a random action with probability ε, and follow the policy with probability 1 - ε.

**Regret**: the difference between the sum of rewards actually received and the sum of rewards from following an optimal policy. More exploration may accumulate higher regret early but ensures learning the optimal policy long-term.

---

## Approximate Q-Methods

Traditional algorithms only converge reliably if all states are visited enough times. For large or infinite state spaces, approximate learning methods generalize states by mapping them to feature sets and learning functions to approximate state values based on these features.

### Approximate Q-Learning

States are generalized by mapping them to sets of features, and Q-values are calculated based on features rather than individual states.

Features are mappings from states to real numbers, sometimes normalized between 0 and 1. For example, in Pacman, features might include the ratio of remaining food pellets or minimum distance from ghosts.

A simple approach combines features through a linear value function using a dot product:

$$Q_w(s, a) = w_1f_1(s, a) + w_2f_2(s,a) + ... + w_nf_n(s,a)$$

Where weight w_i represents how valuable the agent considers feature f_i to be.

Instead of updating Q-values, we update the weights. The ideal weights minimize error between our estimate and the utility predicted by a sample:

$$w^*_1, \ldots, w^*_n = \text{argmin}_{w_1, \ldots, w_n}\text{Error}(\text{sample}, Q_w(s, a))$$

Where Error(x, y) = ½(x - y)², the least-squares error function. We minimize this by moving each weight opposite the direction of increasing error:

$$w_i ← w_i - \alpha\frac{\partial \text{Error}}{\partial w_i}$$

Since $f_i(s, a) = \frac{\partial Q_w(s, a)}{\partial w_i}$ and $-[\text{sample} - Q_w(s, a)] = \frac{\partial \text{Error}}{\partial Q_w(s, a)}$, by the chain rule:

$$\frac{\partial \text{Error}}{\partial w_i} = \frac{\partial \text{Error}}{\partial Q_w(s, a)} \cdot \frac{\partial Q_w(s, a)}{\partial w_i}$$

Substituting and recalling $\text{sample} = R(s, a, s') + \gamma \max_{a'} Q(s', a')$, the weight update rule for linear approximate Q-functions:

$$w_i ← w_i + \alpha [R(s, a, s') + \gamma \max_{a'} Q(s', a') - Q(s, a)]f_i(s, a)$$

### Approximate Q-Learning Example

Prince uses Approximate Q-learning with two features f₁ and f₂ with weights w₁ and w₂.

The approximation function is:

$$Q_w(s,a) = w_1f_1(s,a) + w_2f_2(s,a)$$

**Given:**
- f₁(Bedroom, PLAY) = 3, w₁ = 1
- f₂(Bedroom, PLAY) = 1, w₂ = 2
- γ = 0.5, α = 0.5

**Calculate initial Q-value for (Bedroom, PLAY):**

$$Q_w(\text{Bedroom, PLAY}) = 1(3) + 2(1) = 5$$

**Observe transition (Bedroom, PLAY, LivingRoom, 2) with max_{a'}Q_w(LivingRoom, a') = 8:**

$$Q_{\text{sample}}(\text{Bedroom, PLAY}) = 2 + 0.5(8) = 6$$

**Update weights:**

$$w_1 = 1 + 0.5[6 - 5](3) = 2.5$$

$$w_2 = 2 + 0.5[6 - 5](1) = 2.5$$

These updated weights are used for future state calculations.
