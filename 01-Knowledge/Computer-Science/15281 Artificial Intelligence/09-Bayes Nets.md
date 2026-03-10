# Bayes Nets

## Learning Objectives

1. Answer any query from a joint distribution.
2. Construct joint distribution from conditional probability tables using chain rule.
3. Construct joint distribution from Bayes net and conditional probability tables.
4. Construct Bayes net given conditional independence assumptions.
5. Identify conditional independence assumptions from real-world problems.
6. Analyze the computational and space complexity inference on joint distribution.
7. Identify independence relationships in a given Bayes net.
8. Describe why Bayes nets are used in practice.
9. Decide which sampling method is the most appropriate for an arbitrary query.

## Bayes Nets: Representation

### Answer Query from Conditional Probability Table (CPT)

**Given:** $P(E \mid Q), P(Q)$

**Query:** $P(Q \mid e)$

**Steps:**

1. Construct joint distribution using product rule or chain rule:
   $$P(E, Q) = P(E \mid Q) P(Q)$$

2. Answer query from joint distribution using conditional probability or marginalization:
   $$P(Q \mid e) = \frac{P(e, Q)}{P(e)} \text{ by conditional probability}$$
   $$= \frac{P(e, Q)}{\sum_{q} P(e, q)}$$

### Problems with Full Joint Distribution Tables

- The joint becomes extraordinarily large unless there are only a few variables
- Difficult to empirically learn or estimate relationships involving many variables at once

### Motivation for Bayes Nets

**Bayes nets** describe complex joint distributions using simple, local distributions (conditional probabilities).

- Bayes nets are a subset of **probabilistic graphical models**
- Variables interact locally
- Local interactions chain together to produce global, indirect interactions

### Graphical Model Notation

**Nodes:** Variables (with domains)
- Can be assigned (observed) or unassigned (unobserved)

**Arcs:** Interactions
- Similar to CSP constraints
- Encode "direct influence" between variables (Note: arrows do not necessarily mean direct causation)
- More formally, they encode conditional dependence between unobserved variables

### Joint Probability Construction

When constructing the joint probability table for a Bayes Net:

$$P(\text{Node}_1,\ldots\text{Node}_n) = \prod_{i=1}^n P(\text{Node}_i | \text{Parents}(\text{Node}_i))$$

This approach stores conditional probability tables instead of the entire joint probability table, saving significant space by representing conditional independence relationships concisely.

### Example: Joint Probability from Bayes Net

For a Bayes net with structure $A \to C \leftarrow B$, $B \to D$, and $C \to E \leftarrow D$:

$$P(A, B, C, D, E) = P(A)P(B)P(C|A,B)P(D|B)P(E|C,D)$$

This is exponentially smaller than the joint probability without independence assumptions.

## Independence

### Conditional Independence and Bayes Ball

**Question:** Are $X$ and $Y$ conditionally independent given evidence variables $Z$?

**Answer:** Yes, if $X$ and $Y$ are **d-separated** by $Z$.

Consider all (undirected) paths from $X$ to $Y$. No active paths indicates independence.

### Active Path Conditions

A path is active if each triple along the path is active:

1. **Causal chain** $X \rightarrow Z \rightarrow Y$ where $Z$ is unobserved (either direction)
2. **Common cause** $X \leftarrow Z \rightarrow Y$ where $Z$ is unobserved
3. **Common effect (v-structure)** $X \rightarrow Z \leftarrow Y$ where $Z$ _or one of its descendants_ is observed

If all paths between $X$ and $Y$ are inactive: $X \perp\!\!\!\perp Y \mid Z_1, \ldots, Z_n$

### Bayes Ball Algorithm

To determine conditional independence:

1. Shade in $Z$
2. Drop a ball at $X$
3. The ball passes through active paths and is blocked by inactive paths (moves either direction on edges)
4. If the ball reaches $Y$, then $X$ and $Y$ are NOT conditionally independent given $Z$

## Inference

**Inference** refers to answering questions (queries) about variables based on known information.

### Example Queries

- What is the probability distribution of event $Q$ based on observed evidence $e$? i.e., $P(Q \mid e)$
- What is the most likely outcome for event $Q$ based on evidence $e$? i.e., $\text{argmax}_q P(q \mid e)$

### Generic Query-Answering Steps

1. Re-express the query solely in terms of joint probability. Apply the chain rule and marginalize (sum over) uninterested variables.
2. Compute the joint probability using conditional probability tables of the Bayes net (joint = product of all CPT tables).

### Example: Enumeration Query

Given a Bayes net with structure $H \to W \to S$ and $W \to G$:

Answer the query $P(+w \mid +s, -g)$:

$$P(+w \mid +s, -g) = \frac{P(+w, +s, -g)}{P(+s, -g)}$$

$$= \frac{\sum_{h} P(h, +w, +s, -g)}{\sum_{h, w} P(h, w, +s, -g)}$$

$$= \frac{\sum_{h} P(h)P(+w \mid h)P(+s \mid +w)P(-g \mid +w)}{\sum_{h, w} P(h) P(w \mid h) P(+s \mid w) P(-g \mid w)}$$

$$= \frac{0.1 \times 0.667 \times 0.4 \times 0.75 + 0.9 \times 0.25 \times 0.4 \times 0.75}{0.1 \times 0.667 \times 0.4 \times 0.75 + 0.9 \times 0.25 \times 0.4 \times 0.75 + 0.1 \times 0.333 \times 0.2 \times 0.5 + 0.9 \times 0.75 \times 0.2 \times 0.5}$$

$$= 0.553$$

### Computing Probability Distributions

When marginalizing is expensive and the query requests a probability distribution, delay denominator calculation until the end.

For query $P(W \mid +s, -g)$, rewrite as $\alpha \times P(W, +s, -g)$, deferring normalization.

After chain rule and marginalization:

| $P(W, +s, -g)$ | Value |
|---|---|
| $P(+w, +s, -g)$ | 0.0875 |
| $P(-w, +s, -g)$ | 0.0708 |

Normalize by dividing by sum (0.1583):

| $P(W \mid +s, -g)$ | Value |
|---|---|
| $P(+w \mid +s, -g)$ | 0.0875 / 0.1583 = 0.553 |
| $P(-w \mid +s, -g)$ | 0.0708 / 0.1583 = 0.448 |

### Variable Elimination

Enumeration is expensive as the number of Bayes net variables increases. **Variable elimination** reduces computations by rearranging terms and summations.

**Algorithm:**

```python
def variable_elimination(query, e, bn):
    q = re-express query in terms of bn.CPTs
    factors = all probability tables in q
    for v in order(bn.variables):
        new_factor = make_factor(v, factors)
        factors.add(new_factor)
    return normalize(product of all factors)

def make_factor(v, factors):
    relevant_factors = [remove all f from factors if f depends on v]
    new_factor = product of all relevant_factors
    return sum_out(relevant_factors, v)
```

### Example: Variable Elimination

Given Bayes net $H \to W \to S$, $W \to G$, find $P(H \mid +s)$:

$$P(H \mid +s) = (\alpha)\sum_{w, g} P(H)P(w \mid H)P(+s \mid w)P(g \mid w)$$

**Initial factors:** $P(H), P(W \mid H), P(+s \mid W), P(G \mid W)$

**Elimination order:** $G, W$

**Eliminating $G$:** Sum out $G$ from $P(G \mid W)$ → $f_1(W) = \sum_g P(g \mid W)$

**Eliminating $W$:** Sum out $W$ from $P(W \mid H) P(+s \mid W) f_1(W)$ → $f_2(H) = \sum_w P(w \mid H) P(+s \mid w) f_1(w)$

**Final answer:** Normalize $P(H) \cdot f_2(H)$

### Variable Elimination Efficiency

The time and space efficiency depends on variable ordering. Intuitively, eliminate variables with fewer connections first to minimize intermediate factor sizes.

## Sampling

With proper variable ordering, variable elimination reduces computational and space complexity significantly. However, worst-case exact inference remains $O(2^n)$ where $n$ is the number of variables.

**Sampling** provides _approximate inference_. These methods assume access to individual probability tables and a randomness source to simulate variable values.

### Prior Sampling

For each variable, use a random generator to pick a value based on its probability table. Repeat sufficiently, then use sample counts to calculate queries.

**Algorithm:**

```python
def prior_sample():
    for i = 1,2,...k:
        Sample x_i from P(X_i | Parents(X_i))
    return (x_1, x_2,...x_k)
```

The probability of sample $x_1, x_2, \ldots, x_n$ is:

$$\prod_{i=1}^n P(x_i \mid \text{Parents}(X_i)) = P(x_1, x_2, \ldots, x_n)$$

### Prior Sampling Example

**Example:** Estimate $P(+h)$ from 10 samples:

| Sample | Count |
|---|---|
| $(-h, +w, +s, +g)$ | 4 |
| $(-h, +w, -s, +g)$ | 3 |
| $(+h, +w, +s, +g)$ | 1 |
| $(+h, +w, -s, +g)$ | 2 |

Estimate: $P(+h) = \frac{3}{10} = 0.3$

This differs from the true distribution (0.1) but converges to the correct value with large sample sizes.

### Rejection Sampling

Identical to prior sampling except all samples inconsistent with observed evidence are **rejected**.

```python
def rejection_sample(evidence instantiation):
    for i = 1,2 ... n:
        Sample x_i from P(X_i | Parents(X_i))
        if x_i not consistent with evidence:
            return None
    return (x_1, x_2,...x_k)
```

Becomes inefficient when evidence is rare, as many samples are discarded.

### Likelihood-Weighted Sampling

Instead of discarding inconsistent samples, _fix_ evidence variables and sample only remaining variables. Each sample receives a weight $w$ — the product of probabilities for evidence variables given their parent values.

```python
def likelihood_weighted_sample(evidence instantiation):
    w = 1.0
    for i = 1,2,...n:
        if(X_i is an evidence variable):
            X_i = observation x_i for X_i
            w = w * P(x_i | Parents(X_i))
        else:
            Sample x_i from P(X_i | Parents(X_i))
    return ((x_1, x_2,...x_n), w)
```

The weight represents the **likelihood** of the evidence for that sample. Multiply sample counts by weights for approximate inferences.

**Important:** Identical samples (same variable values) have identical weights.

### Likelihood-Weighted Sampling Example

Estimate $P(+h \mid +w, +g)$. Fix $W = +w, G = +g$; sample $H, S$.

Each sample has weight $w = \prod_{e_i} P(e_i \mid \text{parents}(e_i))$ for evidence variables.

$$P(+h \mid +w, +g) \approx \frac{c + d}{a + b + c + d}$$

where a, b, c, d are the weighted counts for each sample type.

### Gibbs Sampling

**Gibbs sampling** alternates sampling individual variables of interest, conditioning on all others. This considers both "upstream" and "downstream" variables, improving efficiency over likelihood-weighted sampling.

In likelihood-weighted sampling, weights often become very small because only upstream variables are conditioned on. Gibbs sampling addresses this.

```python
def gibbs_sample(full instantiation x_1...x_n):
    1. Start with an arbitrary instantiation consistent with the evidence
    2. Sample 1 variable at a time, conditioned on all others
    3. Keep repeating for a long time
```

Repeating these steps many times guarantees the resulting sample comes from the correct distribution.
