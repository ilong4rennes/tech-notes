# Dimensionality Reduction

**Learning Representations**
- Dimensionality Reduction Algorithms
	- Powerful (often unsupervised) learning techniques for extracting hidden (potentially lower dimensional) structure from high dimensional datasets.
	- Examples: PCA, Kernel PCA, ICA, CCA, t-SNE, Autoencoders, Matrix Factorization 
	- Useful for: 
		- Visualization 
		- More efficient use of resources (e.g., time, memory, communication) 
		- Statistical: fewer dimensions - better generalization 
		- Noise removal (improving data quality)
![[Screen Shot 2024-04-24 at 22.07.21.png]]
# Random Projection

- Goal: project from M-dimensions down to K-dimensions
- Data: $\mathcal{D} = \left\{ x^{(i)} \right\}_{i=1}^{N} \text{ where } x^{(i)} \in \mathbb{R}^{M}$
- Algorithm:
1. Randomly sample matrix: $V \in \mathbb{R}^{K \times M}$, $V_{km} \sim \text{Gaussian}(0, 1)$
2. Project down: $$\begin{align*}
\underbrace{u^{(i)}}_{K \times 1} &= \underbrace{V}_{K \times M} \underbrace{x^{(i)}}_{M \times 1}
\end{align*}
$$
3. Project up: $$\begin{align*}
\underbrace{\tilde{x}^{(i)}}_{M \times 1} &= \underbrace{V^T}_{M \times K} \underbrace{u^{(i)}}_{K \times 1} 
= V^T(Vx^{(i)}) 
\end{align*}
$$
- Problem: a random projection might give us a poor low dimensional representation of the data

**Johnson-Lindenstrauss Lemma**
- A set of n points in high dimension space can be mapped intro an $O(\log n/\epsilon^2)$- dim space such that distance between any two points changes by only a factor of $1 \pm \epsilon$.  
# Principal Component Analysis (PCA)
## Assumption and Data
- Assumption: the data lies on a low K-dimensional linear subspace 
- Goal: identify the axes of that subspace, and project each point onto hyperplane 
- Algorithm: find the K eigenvectors with largest eigenvalue using classic matrix decomposition tools
- Data: $n$ variables, each variable has $m$ data points $$\mathcal{D} = \left\{ x^{(i)} \right\}_{i=1}^{N}
\quad
x^{(i)} \in \mathbb{R}^{M}
\mathbf{X} = \begin{bmatrix}
(x^{(1)})^T \\
(x^{(2)})^T \\
\vdots \\
(x^{(N)})^T
\end{bmatrix}
$$
	- Assume the data is centered, i.e., the sample mean is zero $\hat{\mu} = \frac{1}{N} \sum_{i=1}^{N} x^{(i)} = 0$
	- OW subtract off the sample mean $\tilde{x}^{(i)} = x^{(i)} - \hat{\mu}, \quad \forall i$ 

## Sample Covariance Matrix
For any two variables $x^{(j)}$ and $x^{(k)}$, $$\Sigma_{jk} = \frac{1}{N} \sum_{i=1}^{N} (x_j^{(i)} - \mu_j)(x_k^{(i)} - \mu_k)
$$
Since the data matrix is centered, we rewrite as: $$\Sigma = \frac{1}{N} \mathbf{X}^T \mathbf{X}$$
## Definition
- Linear Projection: $$\begin{align*}
\underbrace{u^{(i)}}_{K \times 1} &= \underbrace{V}_{K \times M} \underbrace{x^{(i)}}_{M \times 1}
\end{align*}
$$
- Definition: PCA repeatedly chooses a next vector $v_j$ that minimizes the reconstruction error s.t. $v_j$ is orthogonal to $v_1, v_2,..., v_{j-1}$. Vector $v_j$ is called the jth principal component. $$\mathbf{V} = \begin{bmatrix}
V_1^{(i)} \\
V_2^{(i)} \\
\vdots \\
V_K^{(i)}
\end{bmatrix}
=
\begin{bmatrix}
\tilde{V}_1^T x^{(i)} \\
\tilde{V}_2^T x^{(i)} \\
\vdots \\
\tilde{V}_K^T x^{(i)}
\end{bmatrix}
= V \tilde{x}^{(i)}
$$
- Notice: Two vectors a and b are orthogonal if $a^Tb = 0$. 
	- the K-dimensions in PCA are uncorrelated

## Objectives for PCA

### Minimizes reconstruction error 
- Q: What is the first principal component $\mathbf{v}_1$ for PCA?
- Option 1: The vector that minimizes the reconstruction error $$v_1 = \arg\min_{v:\|v\|=1} \frac{1}{N} \sum_{i=1}^{N} \| x^{(i)} - (v^T x^{(i)})v \|^2
$$
- Option 2: The vector that maximizes the variance $$v_1 = \arg\max_{v:\|v\|=1} \frac{1}{N} \sum_{i=1}^{N} (v^T x^{(i)})^2
$$
![[Screen Shot 2024-04-25 at 02.54.17.png]]
- Claim: Minimizing the reconstruction error is equivalent to maximizing the variance. ![[Screen Shot 2024-04-25 at 03.24.20.png]]
- We cannot use gradient descent to find the minimum of PCA optimization problem
	- 1. it's non-convex
	- 2. it's a constrained opt. problem, and gradient descent assumes unconstrained
	- 3. orthogonality constraint

### Consists of the K eigenvectors with largest eigenvalue 
![[Screen Shot 2024-04-25 at 04.22.20.png]]
![[Screen Shot 2024-04-25 at 04.49.29.png]]
Choose the matrix V that either… 
1. minimizes reconstruction error 
2. consists of the K eigenvectors with largest eigenvalue 
The above are equivalent definitions.

# Algorithms for PCA

How to find principal components (eigenvectors)?
- Power iteration (aka. Von Mises iteration) 
	- finds each principal component one at a time in order 
- Singular Value Decomposition (SVD) 
	- finds all the principal components at once 
	- two options: 
		- Option A: run SVD on $X^TX \in \mathbb{R}^{M \times M}$
		- Option B: run SVD on $X \in \mathbb{R}^{N \times M}$ (not obvious why Option B should work…) 
- Stochastic Methods (approximate) 
	- very efficient for high dimensional datasets with lots of points

## SVD
![[Screen Shot 2024-04-25 at 05.01.00.png]]
![[Screen Shot 2024-04-25 at 05.05.24.png]]
![[Screen Shot 2024-04-25 at 05.06.35.png]]![[Screen Shot 2024-04-25 at 05.18.21.png]]
