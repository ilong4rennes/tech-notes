# Hidden Markov Models and Particle Filtering

## Learning Objectives

1. Work through multiple iterations of particle filtering
2. Implement the Forward-Backward Algorithm for HMMs
3. Implement particle filtering for various Bayesian Networks
4. Apply smoothing to HMM queries for each time step

## Hidden Markov Models

### Motivation

Hidden Markov Models describe time or spatial series data for situations that change dynamically, such as speech recognition. These models examine states at different time points while acknowledging that not all states are observable — only proxies called evidence can be observed. For instance, tracking an object's movement may only reveal its general region rather than exact position.

### Definition

- **X_t**: Hidden (unobservable) state variables at time t
- **E_t**: Observable state variables at time t

HMMs comprise three components:

1. P(X₀): Initial Distribution
2. P(X_t | X_{t-1}): Transition (Dynamics) Model
3. P(E_t | X_t): Sensor (Observation) Model

The joint probability is expressed as:

$$P(X_0, E_1, X_1, ..., E_t, X_t) = P(X_0)\prod_t P(X_t | X_{t-1})P(E_t | X_t)$$

**Example**: Determining daily rainfall by observing whether a friend carries an umbrella. Here, X_t represents rain on day t, and E_t indicates umbrella presence.

HMMs use single discrete random variables. Multiple state variables combine into one using tuples to maintain the framework's structure for simple matrix implementation.

## Inference with HMMs

### Filtering

Filtering determines the current belief state: "what is the current state given all past and present evidence?" The query form is P(X_t | e_{1:t}).

### Prediction

Prediction estimates future states given current evidence: P(X_{t+k} | e_{1:t}), where k > 0.

### Smoothing

Smoothing calculates posterior distributions over past states with all available evidence: P(X_k | e_{1:t}), where 1 ≤ k < t. This enables better estimates of historical states and supports learning tasks.

### Explanation and Most Likely Explanation

This infers entire sequences rather than single states. Given observations through time t, find the sequence most likely to produce them: argmax_{X_{1:t}} P(X_{1:t} | e_{1:t}). Applications include speech recognition.

## Filtering Algorithms

### Forward Algorithm

The forward algorithm answers filtering queries using recursive expansion:

$$P(X_t | e_{1:t}) = \alpha P(e_t | X_t) \sum_{x_{t-1}} P(x_{t-1} | e_{1:t-1})P(X_t | x_{t-1})$$

Key insight: The term P(x_{t-1} | e_{1:t-1}) enables recursion. Computational cost is O(|X|²) where |X| is the number of states.

Filtering is a two-step process:
- **Prediction**: $\sum_{x_{t-1}} P(x_{t-1} | e_{1:t-1})P(X_t | x_{t-1}) = P(X_t | e_{1:t-1})$
- **Update**: Multiply prediction by $P(e_t | X_t)$

#### Forward Algorithm Example

Given uniform initial distribution P(X₀ = 0) = P(X₀ = 1) = 0.5:

| X_t | X_{t-1} | P(X_t \| X_{t-1}) |
|-----|---------|-------------------|
| 1   | 0       | 0.3               |
| 1   | 1       | 0.7               |
| 0   | 0       | 0.7               |
| 0   | 1       | 0.3               |

| X_t | E_t | P(E_t \| X_t) |
|-----|-----|---------------|
| 1   | 0   | 0.1           |
| 1   | 1   | 0.9           |
| 0   | 0   | 0.8           |
| 0   | 1   | 0.2           |

On day 1 with E₁ = 1 (umbrella observed):

**Prediction step:**
$$P(X_1=1) = 0.3 \times 0.5 + 0.7 \times 0.5 = 0.5$$

**Update step:**
$$P(X_1 = 1 | E_1 = 1) = \alpha P(E_1=1 | X_1=1)P(X_1=1) = \alpha \times 0.9 \times 0.5 = \alpha \times 0.45$$

Normalize by computing P(X₁ = 0 | E₁ = 1) similarly and dividing by their sum.

### Backwards Algorithm

After forward filtering, use backward passes for smoothing queries P(X_k | e_{1:t}) where 1 ≤ k < t:

$$P(X_k | e_{1:t}) = \alpha P(e_{k+1:t} | X_k) P(X_k | e_{1:k})$$

The backwards message is:

$$P(e_{k+1:t} | X_k) = \sum_{x_{k+1}}P(x_{k+1} | X_k)P(e_{k+1} | x_{k+1}) P(e_{k+2:t}| x_{k+1})$$

### Particle Filtering Algorithm

Particle filtering approximates probability distributions through likelihood-weighted sampling. Represent P(X_t) as N particles/samples:

$$\hat{P}(X_t=x) = \frac{\text{number of particles with value } x}{N}$$

Where N << |domain(X)|, so many values have $\hat{P}(x) = 0$.

**Algorithm Steps** (starting with belief $\hat{P}(X_t)$):

1. **Propagate Forward**
   - For each particle x_t, draw samples for X_{t+1} from P(X_{t+1} | x_t)

2. **Observe (Weight samples based on evidence)**
   - Construct $\hat{P}(X, e_{t+1})$ for each possible x:
     - Weight: w = P(e_{t+1} | x)
     - Compute: $\hat{P}(x, e_{t+1}) = \hat{P}(x)P(e_{t+1} | x)$
   - Normalize: $\hat{P}(X | e_{t+1}) = \frac{\hat{P}(X, e_{t+1})}{\sum_x \hat{P}(x, e_{t+1})}$

3. **Resample**
   - Resample N times from the updated distribution $\hat{P}(X | e_{t+1})$

**Note**: With N=1 particle, weighting or resampling is unnecessary unless weight equals zero. Higher N values increase accuracy.
