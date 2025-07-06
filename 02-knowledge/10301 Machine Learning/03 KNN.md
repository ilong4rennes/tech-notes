# Algorithm Pseudocode
```
def set_hyperparameters(k, d):
	store k (# of hyperparams)
	store d(·,·)

def train(D):
	store D

def h(x'):
	Let S = the set of k points in D nearest to x' according to distance function d(u, v) 
	Let v = majority_vote(S) 
	return v
```

# Decision Boundaries

- Decision rule / Hypothesis $h: \mathbb{X} \rightarrow \mathbb{Y}$, $h: \mathbb{R}^M \rightarrow \{+, - \}$
- Decision boundaries Ex: 2D Binary classifier: Linear / quadratic

**How to handle ties?**
- Consider another point
- Remove farthest of k points 
- Weight votes by distance 
- Consider another distance metric

# Evaluation / Computational Efficiency

**1NN classifier:**
- requires no training
- always zero training error (A data point is always its own nearest neighbor)

Suppose N training examples, M features, the computational complexity of 1NN model is:

| Task | Naive | k-d Tree |
| ---- | ---- | ---- |
| Train | O(1) | ~O(MN logN) |
| Predict | O(MN) | ~O($2^M \log(N)$) on avg |
- Problem: fast for small M, very slow for large M
- In practice: use stochastic approximations
## Theoretical Guarantees

Let h(x) be a Nearest Neighbor (k=1) binary classifier. As the number of training examples N goes to infinity… $$error_{true} (h) < 2 \times Bayes \space Error \space Rate$$
Bayes error rate: "the best you could possibly do"

# Hyper-parameters
## Distance function

- Distance function: $d: \mathbb{R}^{M} \times \mathbb{R}^{M} \rightarrow \mathbb{R}$
- Euclidean distance:
$$
d(u, v) = \sqrt{\sum_{m=1}^{M}(u_m - v_m)^2 } 
$$
## Setting $k$ 

Theorem: If $k$ is some function of N s.t. $k(N)\rightarrow \infty$ and $\frac{k(N)}{N}\rightarrow 0$ as $N\rightarrow \infty$, then true error of a kNN model $\rightarrow$ Bayes error rate
# Inductive Bias

1. Similar points should have similar labels.
2. All dimensions are created equally.
# Pros and Cons

- Pros:
	- Intuitive / explainable
	- No training / retraining
	- Provably near-optimal in terms of true error rate
- Cons:
	- Computationally expensive, large runtime
		- Always needs to store all data: $O(ND)$
		- Finding the k closest points in D dimensions: $O(ND+N\log(k))$
	- Affected by feature scale
