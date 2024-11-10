## Clustering
- Goal: split an unlabeled data set into groups or clusters of “similar” data points
- Given a desired number of clusters, $K$, return a partition of the data set into $K$ groups or clusters, ${C_1, ..., C_k}$ , that optimize some objective function 
1. What objective function should we optimize? 
2. How can we perform optimization in this setting?

## Model
- assume K clusters
- use Euclidean distance
- parameters: 
	- cluster center: $\{\mu_1, ..., \mu_k \}$
	- cluster assignment: $\{z^{(1)}, ..., z^{(N)}\}$
- objective function: minimize $\sum_{n=1}^{N} \| x^{(n)} - \mu_{z^{(n)}} \|^2$ 
- optimization: block coordinate descent

## Block Coordinate Descent
**Coordinate Descent**
- To minimize some objective $\hat{\theta} = \arg\min J(\theta)$, iteratively pick one variable and minimize the objective w.r.t. just that variable, keeping all others fixed.

**Block Coordinate Descent**
- Goal: minimize some objective $\hat{\alpha}, \hat{\beta} = \arg\min J(\alpha, \beta)$ 
- Idea: iteratively pick one block of variables, and minimize the objective w.r.t. that block, keeping the others fixed. 
	- Ideally, blocks should be the largest possible set of variables that can be efficiently optimized simultaneously

## $K$-means Algorithm
**Optimization of the $K$-Means objective**
$$\hat{\mu}_1, \ldots, \hat{\mu}_K, z^{(1)}, \ldots, z^{(N)} = \arg\min \sum_{n=1}^{N} \| x^{(n)} - \mu_{z^{(n)}} \|_2
$$
If $\mu_1, ..., \mu_k$ are fixed, $$z^{(n)} = \arg\min_{z \in \{1, \ldots, K\}} \| x^{(n)} - \mu_z \|_2$$
If $z^{(1)}, ..., z^{(N)}$ are fixed, $$\hat{\mu}_k = \frac{1}{N_k} \sum_{n:z^{(n)} = k} x^{(n)} \quad \text{where } N_k \text{ is the number of data points in cluster } k$$
**$K$-Means Algorithm**
- Input: $\mathcal{D} = \{ (x^{(n)}) \}_{n=1}^{N}, K$
1. Initialize cluster centers $\{\mu_1, ..., \mu_k \}$
2. While NOT CONVERGED
	1. Assign each data point to the cluster with the nearest cluster center: $z^{(n)}$
	2. Recompute the cluster centers: $\hat{\mu}_k$ 
- Output: $\{\mu_1, ..., \mu_k \}$, $\{z^{(1)}, ..., z^{(N)}\}$

**Setting $K$**
- Idea: choose the value of $K$ that minimizes the objective function

**Initializing $K$-means**
1. Random
	- Common choice: choose K data points at random to be the initial cluster centers (Lloyd’s method)
	- Lloyd’s method converges to a local minimum and that local minimum can be arbitrarily bad (relative to the optimal clusters) This is because the K-means objective is non-convex! 
	- Intuition: want initial cluster centers to be far apart from one another
2. K-means++
	1. Choose the first cluster center randomly from the data points. 
	2. For each other data point x, compute D(x) , the distance between x and the nearest cluster center. 
	3. Select the next cluster center proportional to $D(x)^2$. 
	4. Repeat 2 and 3 K − 1 times. 
	- K-means++ achieves a O(logK) approximation to the optimal clustering in expectation 
Both Lloyd’s method and K-means++ can benefit from multiple random restarts.
