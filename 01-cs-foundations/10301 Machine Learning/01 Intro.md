# ML

## Problem Formulation

- Task (T)
- Performance (P)
- Experience (E)

## Notation

- data / feature / label
- training / testing / training error / test error rate

## Routine

1. training
2. testing
3. evaluation

## Core Concepts

![[Screen Shot 2024-01-22 at 11.15.55.png]]
![[Screen Shot 2024-01-22 at 11.15.12.png]]
![[Screen Shot 2024-01-22 at 11.12.59.png]]
# Supervised ML
## Routine
1. training (input: labeled training dataset; output: a classifier)
2. testing (input: classifier + test dataset; output: predictions for test dataset)
3. evaluation (input: predicted test dataset labels; output: error rate)

## Notation
- Feature space, $X$
- Label space, $Y$
- (Unknown) Target function: $c^*: X \rightarrow Y$
- Training dataset: $D = \{(x^{(1)}, c^*(x^{(1)}) = y^{(1)}), ..., (x^{(N)}, y^{(N)}) \}$
	- Example: $(x^{(n)}, y^{(n)})=({x_1}^{(n)}, ..., {x_D}^{(n)}, y^{n})$
	- N = # of data points (rows)
	- D = # of features (cols)
- Hypothesis space: $H$
- Goal: find a classifier, $h \in H$, that best approximates $c^*$
## Evaluation
### Loss Function, $l: Y \times Y \rightarrow \mathbb{R}$
- for regression: $l(y, \hat y) = (y-\hat y)^2$
- for classification: $l(y, \hat y) = \mathbb{1} (y \neq \hat y)$
### Error
- Training error rate = $err(h, D_{train})$
- Test error rate = $err(h, D_{test})$
- True error rate = $err(h)$ = the error rate of $h$ on all possible examples

## Example Supervised ML Models

1. Majority vote classifier
```
def train(D_train):
	store v = mode(y^1, ..., y^n)

def h(x'):
	return v

def predict(D_test):
	for (x^n, y^n) in D_test:
		\hat y^n = h(x^n)
```

2. Memorizer
```
def train(D_train):
	store D_train

def h(x'):
	if (x^n, y^n) in D_train st. x^n = x':
		return y^n
	else:
		return mode(y^1, ..., y^N)
```
