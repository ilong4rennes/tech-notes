## Bayes Optimal Classifier

![[Screen Shot 2024-03-26 at 22.16.52.png]]

## Mini-batch Stochastic Gradient Descent for Neural Networks

![[Screen Shot 2024-03-26 at 22.19.03.png]]

## Feature Transformations

- Non-linear --> Linear Models
### General $Q^{th}$-order Transforms

- $\phi_{2,3}([x_1, x_2]) = [x_1, x_2, x_1^2, x_1 x_2, x_2^2, x_1^3, x_1^2 x_2, x_1 x_2^2, x_2^3]$
	- 2 - input size
	- 3 - polynomial order
- $\phi_{2,Q}$ maps a 2-dimensional input to a $\frac{Q(Q+3)}{2}$-dimensional output

### Tradeoffs

![[Screen Shot 2024-03-26 at 22.33.37.png]]

## Regularization

- prevent overfitting
- constrained optimization

### Hard Constraints

- $\mathcal{H}_{10} = 10^{th}$-order polynomials$$\begin{align*}
\phi_{1,10}(x) &= [x, x^2, x^3, x^4, x^5, x^6, x^7, x^8, x^9, x^{10}] \\
& \quad
\end{align*}
$$
Given $$
\mathbf{X} = \begin{bmatrix}
1 \\
1 \\
\vdots \\
1 \\
\end{bmatrix}
\begin{bmatrix}
\phi_{1,10}(x^{(1)}) \\
\phi_{1,10}(x^{(2)}) \\
\vdots \\
\phi_{1,10}(x^{(N)})
\end{bmatrix}
\text{ and }
\mathbf{y} = \begin{bmatrix}
y^{(1)} \\
y^{(2)} \\
\vdots \\
y^{(N)}
\end{bmatrix}
$$
Find
$$
\boldsymbol{\theta} = [\theta_0, \theta_1, \theta_2, \theta_3, \theta_4, \theta_5, \theta_6, \theta_7, \theta_8, \theta_9, \theta_{10}]
$$
that minimizes
$$
\min_{\boldsymbol{\theta} \in \mathbb{R}^{11}} MSE = (\mathbf{X}\boldsymbol{\theta} - \mathbf{y})^T(\mathbf{X}\boldsymbol{\theta} - \mathbf{y})
$$
subject to
$$
\theta_3 = \theta_4 = \theta_5 = \theta_6 = \theta_7 = \theta_8 = \theta_9 = \theta_{10} = 0
$$
### Soft Constraints

- More generally, $\phi$ can be any nonlinear transformation, e.g., exp, log, sin, sqrt, etc...
Given: 
$$X = \begin{bmatrix}
1 & \phi_1(x^{(1)}) & \cdots & \phi_m(x^{(1)}) \\
1 & \phi_1(x^{(2)}) & \cdots & \phi_m(x^{(2)}) \\
\vdots & \vdots & \ddots & \vdots \\
1 & \phi_1(x^{(N)}) & \cdots & \phi_m(x^{(N)})
\end{bmatrix}
\text{ and } y = \begin{bmatrix}
y^{(1)} \\
y^{(2)} \\
\vdots \\
y^{(N)}
\end{bmatrix},
$$
Find $\theta$ that minimizes
$$
\min_{\theta} \text{ MSE} = (X\theta - y)^T(X\theta - y) \\
$$
Subject to constraint: (Constrained Optimization)
$$\|\theta\|_2^2 = \theta^T\theta = \sum_{d=0}^{D} \theta_d^2 \leq C
$$
![[Pasted image 20240327190757.png]]

### Ridge Regression

![[Screen Shot 2024-03-27 at 19.10.37.png]]

Minimizing $\mathcal{L}_{\text{AUG}}(\theta) = \mathcal{L}_{D}(\theta) + \lambda_{C} r(\theta)$, as $\lambda_C$ increases, the minimum of $\mathcal{L}_{\text{AUG}}$ moves towards the minimum of r.

### Best Practices

- Should we regularize the bias/intercept parameter, $\theta_0$? 
	- No! Regularizers typically avoid penalizing this term so that our classifiers can adapt to shifts in the y values 
- Is feature scale a concern with regularization? 
	- Yes! Features at dramatically different scales might have vastly different coefficient values When using regularization, it is common to standardize the features first by subtracting the mean and dividing by the standard deviation

### Setting $\lambda$

![[Screen Shot 2024-03-27 at 19.14.51.png]]

### Other regularizers

![[Screen Shot 2024-03-27 at 19.15.10.png]]

![[Screen Shot 2024-03-27 at 19.15.24.png]]
