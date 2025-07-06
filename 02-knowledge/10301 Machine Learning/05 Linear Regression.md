# Big Picture

- Dataset: $D = \{ (x^i, y^i) \}^{n} _ {i=1}, x^i \in \mathbb{R}^{M}, y^{(i)} \in \mathbb{R}$
- Goal: Predict real-valued outputs
- Objective function: $$ MSE = \frac{1}{N} \sum_{i=1}^{N} e_i =  \frac{1}{N} \sum_{i=1}^{N} (y^{(i)}-(w^T\cdot x^{(i)}+b^{(i)}))^2 $$
- Notation:
	- $x' = [1, x_1, ..., x_M]^T$
	- $\theta = [b, w_1, ..., w_M]^T$  
	- $h_{\theta} (x') = \theta ^{T} x'$ 
- Key Idea: Given objective function $J(\theta): \mathbb{R}^M \rightarrow \mathbb{R}$, find $\hat{\theta} = \arg\min_{\theta \in \mathbb{R}} J(\theta)$   

# Examples
## KNN Regression

Algorithm 1: k=1 Nearest Neighbor Regression 
- Train: store all (x, y) pairs 
- Predict: pick the nearest x in training data and return its y

Algorithm 2: k = 2 NN Weighted Regression
- Train: store all (x, y) pairs 
- Predict: pick the nearest two instances $x^{(n1)}$ and $x^{(n2)}$ in training data and return the weighted average of their y values
## Decision Tree Regression

- Decision tree for classification / regression
- During learning, choose the attribute that minimizes an appropriate splitting criterion (e.g. mean squared error, mean absolute error)

# Optimization

## Unconstrained Optimization

- Def: try minimize (or maximize) a function with no constraints on the inputs to the function
- Given objective function $J(\theta): \mathbb{R}^M \rightarrow \mathbb{R}$, find $\hat{\theta} = \arg\min_{\theta \in \mathbb{R}} J(\theta)$   
- Linear Regression as Function Approximation:
![[Screen Shot 2024-02-19 at 05.00.05.png]]
### Method 1: Gradient Descent

- Algorithm:
	- 1. Choose *initial point* $\vec{\theta _0}$
	- 2. Repeat $t=1, 2, ...$
		- a) Compute gradient $$\vec{g}=\nabla J(\vec{\theta } )=\begin{bmatrix}
 \frac{\mathrm{d} J(\vec{\theta } )}{\mathrm{d} \theta _1} \\
 \frac{\mathrm{d} J(\vec{\theta } )}{\mathrm{d} \theta _2}\\
 ...\\
\frac{\mathrm{d} J(\vec{\theta } )}{\mathrm{d} \theta _M}
\end{bmatrix}$$
		- b) *Step size* $\gamma_t$ 
		- c) Update $\vec{\theta} \leftarrow \vec{\theta} - \gamma_t \cdot \vec{g}$ 
	- 3. Return $\vec{\theta}$ if *stopping criteria* reached
- Remarks:
	- Initial point
		- randomly 
		- all zeros
	- Step size
		- fixed value 0.1
		- set a schedule $\gamma_t=\frac{\gamma _0}{(t-1)\gamma_0+1}$
		- line search
	- Stopping criteria
		- $\vec{g}\approx 0$
		- $||\nabla J(\vec{\theta } )||_2 < \epsilon$ for $\epsilon = 10^{-8}$

![[Screen Shot 2024-02-19 at 05.44.41.png]]
![[Screen Shot 2024-02-19 at 05.44.52.png]]
![[Screen Shot 2024-02-19 at 05.45.02.png]]

#### Convexity
- A function $f : \mathbb{R}^D \rightarrow \mathbb{R}$  is convex if $\forall x^{(1)} \in \mathbb{R}^D, x^{(2)} \in \mathbb{R}^D$ and $0 \leq c \leq 1$, $(cx^{(1)} + (1 - c)x^{(2)}) \leq cf(x^{(1)}) + (1 - c)f(x^{(2)})$ 
- Gradient descent is a local optimization algorithm – it will converge to a local minimum (if it converges) Works great if the objective function is convex! Not ideal if the objective function is non-convex.

### Method 2: Closed Form
- Idea: find the critical points of the objective function, specifically the ones where $\nabla J(\theta) = 0$ (the vector of all zeros), and check if any of them are local minima
- Notation
	- design matrix / target vector  ![[Screen Shot 2024-03-26 at 10.19.56.png]]
- Minimizing MSE
	- ![[Screen Shot 2024-03-26 at 10.20.44.png]]
- $\hat{\theta} = (X^TX)^{-1}X^Ty$

Questions
1. Is $X^TX$ invertible? 
	1. When N ≫ D + 1, $X^TX$ is (almost always) full rank and therefore, invertible! 
	2. If $X^TX$ is not invertible (occurs when one of the features is a linear combination of the others), then there are infinitely many solutions 
2. If so, how computationally expensive is inverting $X^TX$? 
	1. $X^TX \in \mathbb{R}^{D+1\times D+1}$  so inverting $X^TX$ takes $O(D^3)$ time… 
		1. Computing $X^TX$ takes $O(ND^2)$ time 
	2. Can use gradient descent to (potentially) speed things up when N and D are large!

### Method 3: SGD
