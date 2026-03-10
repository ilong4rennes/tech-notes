# Probability Reference Sheet

## Learning Objectives

This reference sheet provides foundational probability concepts used throughout the course.

## Probability Notation

**Discrete random variables** are denoted by capital letters (A, B, C) representing all possible disjoint outcomes.

**Outcomes** are represented by lowercase letters, indicating specific values variables can take (e.g., +b for variable B, or a₁, a₂, a₃ for variable A).

**Variables for values** (lowercase letters like a) represent single outcomes rather than complete random variables.

### Example
$$P(+b, C) = \sum_{a \in \{a_1, a_2, a_3\}} P(a, +b, C)$$

## Basic Rules

### Definition of Conditional Probability
$$P(X \mid Y) = \frac{P(X,Y)}{P(Y)}$$

### Product Rule
$$P(X, Y) = P(X \mid Y) P(Y) = P(Y \mid X) P(X)$$

$$P(X_1,X_2, X_3) = P(X_1, X_2 \mid X_3)P(X_3) = P(X_1 \mid X_2,X_3)P(X_2, X_3)$$

### Bayes' Theorem
$$P(Y \mid X) = \frac{P(X \mid Y)P(Y)}{P(X)}$$

### Normalization
$$P(Y \mid X) = \frac{P(X, Y)}{P(X)} = \frac{P(X , Y)}{\sum_{y} P(X , y)}$$

$$P(Y \mid X) \propto P(X , Y)$$

$$P(Y \mid X) = \alpha P(X , Y) \text{ (note difference between } \propto \text{ and } \alpha \text{)}$$

$$\alpha = \frac{1}{P(X)} = \frac{1}{\sum_{y} P(X , y)}$$

### Chain Rule
$$P(X_1,X_2, X_3) = P(X_1 \mid X_2,X_3)P(X_2, X_3) = P(X_1 \mid X_2,X_3)P(X_2 \mid X_3)P(X_3)$$

$$P(X_1, ..., X_N) = \prod_{n=1}^{N} P(X_n \mid X_1, ..., X_{n-1})$$

### Law of Total Probability
$$P(A) = P(A \mid b_1) P(b_1) + P(A \mid b_2) P(b_2)$$

Where events b₁ and b₂ partition the sample space (disjoint and exhaustive). More generally:

$$P(A) = \sum_b P(A \mid b) P(b) = \sum_b P(A, b)$$

**Note:** All basic probability rules apply when conditioning on sets of random variables or outcomes; conditioned variables must appear in each term.

### Conditioned Example
Bayes' Theorem with conditioning on variables A and B:

$$P(Y \mid X, A, B) = \frac{P(X \mid Y, A, B)P(Y \mid A, B)}{P(X \mid A, B)}$$

## Marginalization

Marginalization uses the law of total probability to sum out variables from a joint distribution, useful when finding probability over a subset of variables.

### Sum Out Single Variable
$$P(X) = \sum_{y}P(X, y)$$

### Sum Out Multiple Variables
$$P(X) = \sum_{z} \sum_{y} P(X, y, z)$$

### Conditional Distributions
Works when summing out variables not conditioned upon (left of the ∣):

$$P(A \mid C, d) = \sum_{b} P(A, b \mid C, d)$$

**Does NOT work** when summing over conditioned variables (right of the ∣):

$$P(A, b \mid C) \neq \sum_{d} P(A, b \mid C, d)$$

## Independence

### Independent Variables (X ⊥⊥ Y)
If X and Y are independent:

- $P(X,Y) = P(X)P(Y)$
- $P(X) = P(X \mid Y)$
- $P(Y) = P(Y \mid X)$

### Conditionally Independent Given Z (X ⊥⊥ Y ∣ Z)
If X and Y are conditionally independent given Z:

- $P(X,Y \mid Z) = P(X \mid Z)P(Y \mid Z)$
- $P(X \mid Y,Z) = P(X \mid Z)$
- $P(Y \mid X,Z) = P(Y \mid Z)$

## Answering a Query from a CPT

### Given
- P(B ∣ A)
- P(A)

### Query
- P(A ∣ b)

### Solution Steps

**Step 1:** Construct joint distribution using product rule or chain rule

$$P(B,A) = P(B \mid A)P(A)$$

**Step 2:** Answer query from joint distribution

Using conditional probability definition:
$$P(A \mid b) = \frac{P(b,A)}{P(b)}$$

Using law of total probability:
$$P(A \mid b) = \frac{P(b,A)}{\sum_{a}P(b,a)}$$

## Probability Tables

When using capital letters (e.g., P(A, B)), the notation refers to all outcome combinations of discrete random variables — a complete probability table rather than single values. This applies to conditional probabilities (e.g., P(A, B ∣ C)) and mixed notation (e.g., P(A, b ∣ C, d) contains all combinations for random variables A and C while b and d are fixed).

### Important Note about CPTs

A probability table sums to one when:

1. Exactly one specific combination of outcomes is conditioned upon, AND
2. All possible combinations of remaining random variables are considered

Equivalently:

1. No capital letters appear on the right-hand side of ∣, AND
2. Only capital letters appear on the left-hand side
