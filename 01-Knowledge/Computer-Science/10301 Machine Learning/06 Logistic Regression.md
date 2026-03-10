# Probabilistic Learning
## Probabilistic Learning

- (Unknown) Target distribution, $y \sim p^*(Y|x)$
- Distribution, $p(Y|x)$
- Goal: find a distribution, $p$, that best approximates $p^*$

## Log-likelihood

- Given N iid samples $\mathcal{D} = \{x^{(1)}, \ldots, x^{(N)}\}$ of a random variable X,
	- X is discrete, log-likelihood of D is: $$\ell(\theta) = \log \prod_{n=1}^{N} p(x^{(n)}|\theta) = \sum_{n=1}^{N} \log p(x^{(n)}|\theta)$$
	- X is continuous, log-likelihood of D is: $$\ell(\theta) = \log \prod_{n=1}^{N} f(x^{(n)}|\theta) = \sum_{n=1}^{N} \log f(x^{(n)}|\theta)$$

## Maximum Likelihood Estimation

- Intuition: assign as much of the (finite) probability mass to the observed data at the expense of unobserved data

## Exponential Distribution MLE

- pdf of exponential distribution: $f(x|\lambda) = \lambda e^{-\lambda x}$
- likelihood: $L(\lambda) = \prod_{n=1}^{N} f(x^{(n)}|\lambda) = \prod_{n=1}^{N} \lambda e^{-\lambda x^{(n)}}$ 
- log-likelihood: $\ell(\lambda) = \sum_{n=1}^{N} \log f(x^{(n)}|\lambda) = \sum_{n=1}^{N} \log \lambda e^{-\lambda x^{(n)}}= N(\log \lambda) - \sum_{n=1}^{N} \lambda x^{(n)}$ 
 $$\frac{\partial \ell}{\partial \lambda} = \frac{N}{\lambda} - \sum_{n=1}^{N} x^{(n)}=0$$
$$\hat{\lambda} = \frac{N}{\sum_{n=1}^{N} x^{(n)}}$$
## Building a Probabilistic Classifier

- decision rule: $\hat{y} = \underset{y}{\mathrm{argmax}} \ P(Y = y|x')$
- Idea: model $P(Y|x)$ as some parametric function of $x$

# Logistic Regression

## 1. Model

- Logistic function: $\sigma(z) = \frac{1}{1 + e^{-z}}$ 
- Model: $$p(y|x,\theta) = \begin{cases} 
\sigma(\theta^T x) & \text{if } y = 1 \\
1 - \sigma(\theta^T x) & \text{if } y = 0
\end{cases}$$
- Decision boundary: $$\hat{y} = 
\begin{cases} 
1 & \text{if } p(y=1|x,\theta) \geq \frac{1}{2} \\
0 & \text{otherwise} 
\end{cases}
$$
## 2. Objective: Minimizing Negative Conditional Log-likelihood 

$J(\theta) = - \frac{1}{N} \sum_{i=1}^{N} \log p(y^{(i)} | x^{(i)}, \theta)$
## 3. Derivatives
$$\frac{\partial J^{(i)}}{\partial \theta_m} = \frac{\partial}{\partial \theta_m} \left( -\log p(y^{(i)} | x^{(i)}, \theta) \right) \\
= - \left( y^{(i)} - \sigma(\theta^T x^{(i)}) \right) x_m^{(i)}$$
## 4. Gradients
$$\nabla J^{(i)}(\theta) = - \left( y^{(i)} - \sigma(\theta^T x^{(i)}) \right) x^{(i)} $$
$$\nabla J(\theta) = \frac{1}{N} \sum_{i=1}^{N} \nabla J^{(i)}$$
![[Pasted image 20240326180117.png]]

# 5. Optimization
## Gradient Descent

![[Screen Shot 2024-03-26 at 18.05.50.png]]
## Stochastic Gradient Descent (SGD)

![[Screen Shot 2024-03-26 at 18.06.48.png]]
![[Screen Shot 2024-03-26 at 15.16.49.png]]

## Stochastic Gradient Descent vs. Gradient Descent

- An epoch is a single pass through the entire training dataset 
	- Gradient descent updates the parameters once per epoch 
	- SGD updates the parameters N times per epoch
- Theoretical![[Screen Shot 2024-03-26 at 18.07.43.png]]

# 6. Predictions