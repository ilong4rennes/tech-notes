# A Recipe for Machine Learning

1. Training data:
2. Choose:
	- Decision function: $\hat{y}=f_{\theta}(x_i)$ 
		- Linear regression, logistic regression, neural network
	- Objective function / Loss function: $l(\hat{y}, y_i)\in \mathbb{R}$
		- MSE, Cross entropy
3. Goal: $\theta^{*}=\arg \min_{\theta} \sum_{i=1}^{N}l(f_{\theta}(x_i), y_i)$
4. Train with SGD: $\theta^{(t+1)} = \theta^{(t)} - \eta_t \nabla \ell(f_{\theta}(x_i), y_i)$

# Decision Functions

- Linear regression: $y = h_{\theta}(x) = \sigma(\theta^{T} x)$ where $\sigma(a) = a$
- Logistic regression: $y = h_{\theta}(x) = \sigma(\theta^{T} x)$ where $\sigma(a) = \frac{1}{1 + \exp(-a)}$
- Perceptron: $y = h_{\theta}(x) = \sigma(\theta^{T} x)$ where $\sigma(a) = \text{sign}(a)$
- Neural network: $z=\sigma(u)$, $u=\sum_{i=1}^{M}x_i\cdot w_i$ 
- Components of a Neural network:
	- Neuron
		- input layer: receives the raw data
		- hidden layers: process the data through weights and activation functions
		- output layer: produces the final classification or prediction.
	- Weight ($w_i$)
	- Activation function ($\sigma$): function applied to a node's input to produce an output, which will be used as input for subsequent layers of neural network

# Non-linearity in Neural Network
## Non-linear Activation functions

- Key to neural networks’ ability to model non-linear decision boundaries lies in non-linear activation functions. Without these, a neural network, regardless of its depth, would still behave like a linear model, incapable of solving complex classification tasks.
- Examples:
	- sigmoid
	- tanh(x)
	- ReLU(x)
	- ELU(x)

## 1D Image Recognition

![[Screen Shot 2024-03-13 at 13.27.26.png]]
- Objective function of a neural network is **non-convex**

# Neural Network Design Choices
## Neural Network Architecture

1. # of hidden layers (depth) 
2. # of units per hidden layer (width) 
3. Type of activation function (nonlinearity) 
4. Form of objective function 
5. How to initialize the parameters

## Building wider networks

- #. input features = M
- #. hidden units in a layer = D

The hidden units could learn to be… 
- a selection of the most useful features (D < M)
- nonlinear combinations of the features (D = M)
- a lower dimensional projection of the features (D < M)
- a higher dimensional projection of the features (D > M)
- a copy of the input features 
- a mix of the above (D > M)
## Building deeper networks

Q: How many layers should we use?
- Theoretical answer: For any continuous function g(x), there exists a 1-hidden-layer neural net $h_{\theta}(x)$ s.t. $|h_{\theta}(x) - g(x)| < \varepsilon$ for all x, assuming sigmoid activation functions 
- Empirical answer: After 2006: “Deep networks are easier to train than shallow networks (e.g. 2 or fewer layers) for many problems” Big caveat: You need to know and use the right tricks.

# Forward Propagation

## Two Representation of Neural Networks
### Matrix form
![[Screen Shot 2024-03-13 at 14.49.16.png]]
![[Screen Shot 2024-03-13 at 14.56.54.png]]
### Vector form
![[Screen Shot 2024-03-13 at 14.49.29.png]]

## Forward Propagation for Making Predictions

- Inputs: weights \($\alpha^{(1)}, \ldots, \alpha^{(L)}, \beta$\) and a query data point \(x'\)
- Initialize $z^{(0)} = x'$
- For l = 1, ..., L
	- $u^{(l)} = \alpha^{(l)T} z^{(l-1)}$
	- $z^{(l)} = \sigma(u^{(l)})$
- $\hat{y} = \sigma(\beta^T z^{(L)})$
- Output: the prediction $\hat{y}$

# SGD for Learning

- Input: $D = \{ (x^{(i)}, y^{(i)}) \}_{i=1}^{N}, \gamma$
- Initialize all weights $\alpha^{(1)}, ..., \alpha^{(L)}, \beta$ 
- While TERMINATION CRITERION is not satisfied
	- for $i\in \text{shuffle}(\{1, ..., N\})$
		- $g_\beta = \nabla_\beta J^{(i)} (\alpha^{(1)}, \ldots, \alpha^{(L)}, \beta)$
		- for $l=1, ..., L$
			- $g_{\alpha^{(L)}} = \nabla_{\alpha^{(L)}} J^{(i)} (\alpha^{(1)}, \ldots, \alpha^{(L)}, \beta)$
		- update $\beta = \beta - \gamma g_\beta$
		- for $l=1, ..., L$
			- update $\alpha^{(l)} = \alpha^{(l)} - \gamma g_{\alpha^{(l)}}$
- Output: $\alpha^{(1)}, ..., \alpha^{(L)}, \beta$

# Loss Function $J^{(i)}$
- Regression - squared error (same as linear regression!)
$$
J^{(i)}(\boldsymbol{\theta}) = (\hat{y}_{\theta}(x^{(i)}) - y^{(i)})^2
$$
- Binary classification - cross-entropy loss (same as logistic regression!)
	- Assume $Y\in \{0,1\}$ and $P(Y = 1|x, \boldsymbol{\theta}) = \hat{y}_{\theta}(x)$
$$
\begin{align}
J^{(i)}(\boldsymbol{\theta}) &= -\log P(y^{(i)}|x^{(i)}, \theta)\\
&=-\log P(y=1|x^{(i)},\theta) \cdot P(y=0|x^{(i)},\theta)\\
&=-\log(\hat{y_\theta}(x^{(i)})^{y^{(i)}})((1-\hat{y_\theta}(x^{(i)}))^{1-{y^{(i)}}})\\
&=-y^{(i)}\log(\hat{y_\theta}(x^{(i)})-(1-y^{(i)})\log(1-\hat{y_\theta}(x^{(i)})
\end{align}
$$
- Multi-class classification - cross-entropy loss
	- Label: one-of-C vector, $y=[0,0,1,0,...,0]$
	- Output: vector of length C, $\hat{y_\theta}$ $$P(y[c] = 1 | x, \boldsymbol{\theta}) = \hat{y}_{\theta}(x^{(i)})[c]$$
	- Cross-entropy loss: $$\begin{align}J^{(i)}(\boldsymbol{\theta}) &= -\log P(y^{(i)} | x^{(i)}, \boldsymbol{\theta}) \\ &= - \sum_{c=1}^{C} y^{(i)}[c] \log(\hat{y}_{\theta}(x^{(i)}))[c] \end{align}$$
	- Softmax: 
		- placed as final layer activation function in NN when dealing with multi-class classification
		- transform raw scores into probabilities that sum up to 1 ![[Screen Shot 2024-03-14 at 01.54.36.png]]

# Three Approaches to Differentiation

Given $f:\mathbb{R}^D \rightarrow \mathbb{R}$, compute $\nabla_x f(x) = \frac{\partial f(x)}{\partial x}$
## 1. Finite difference method 
$$\frac{\partial f(x)}{\partial x_i} \approx \frac{f(x + \epsilon d_i) - f(x - \epsilon d_i)}{2\epsilon}$$
where $d_i$ is a one-hot vector with a 1 in the $i^{th}$ position.

- want $\epsilon$ to be small to get a good approximation but floating point issues when $\epsilon$ is too small 
- Getting the full gradient requires computing approximation for each dimension of the input

- Requires the ability to call $f(x)$ 
- Great for checking accuracy of implementations of more complex differentiation methods 
- Computationally expensive for high-dimensional inputs

## 2. Symbolic differentiation 

![[Screen Shot 2024-03-14 at 02.23.48.png]]

- Requires systematic knowledge of derivatives 
- Can be computationally expensive if poorly implemented
## 3. Automatic differentiation (reverse mode) 

1. define some intermediate quantities 
2. draw the computation graph and run the “forward” computation
3. compute partial derivatives, starting from y and working back

![[IMG_27030E57A0CD-1.jpeg]]

- Backprop is efficient b/c of reuse in the forward pass and the backward pass
- Requires systematic knowledge of derivatives and an algorithm for computing $f(x)$
- Computational cost of computing $\frac{\partial f(x)}{\partial x}$ is proportional to the cost of computing $f(x)$

![[Screen Shot 2024-03-14 at 03.34.25.png]]

# Example of Backpropagation

## Case 1: Logistic Regression

![[Screen Shot 2024-03-14 at 03.46.03.png]]

## Case 2: Neural Network

![[Screen Shot 2024-03-14 at 04.04.31.png]]
# Full Cycle of Training Neural Network
## Forward Computation

![[Screen Shot 2024-03-14 at 03.48.29.png]]

## SGD with Backprop

![[Screen Shot 2024-03-14 at 04.03.52.png]]

## Backward