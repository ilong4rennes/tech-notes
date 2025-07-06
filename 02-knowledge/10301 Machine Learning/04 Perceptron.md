# Linear Decision Boundaries

- $w^T x +b = 0$ defines a hyperplane
	- The vector $w$ is always orthogonal to this hyperplane and always points in the direction where $w^T +b>0$
	- $S_+ = \{ x: w^T x +b >0\}$
	- $S_- = \{ x: w^T x +b <0\}$
- Goal: learn classifiers of form $h(x)= sign(w^T+b)$
- Question: How to learn w and b?

# Online Learning Setup

- **Batch setting**: have access to the entire training dataset at once
- **Online setting**: data points arrive gradually over time and we learn continuously

- For t = 1, 2, 3, ...
	- 1. Receive unlabeled data point, $x^{(t)}$
	- 2. Predict label, $\hat{y} = h_{w, b} (x^{(t)})$
	- 3. Observe true label, $y^{(t)}$
	- 4. Pay a penalty if mistake, $\hat{y} \neq y^{(t)}$
	- 5. Update parameters, w and b
- Goal: minimize the number of mistakes made

# Online Perceptron Learning Algorithm

1. Initialize $w = [0, 0, ..., 0]$, $b=0$
2. For t = 1, 2, 3, ...
	- 1. Receive unlabeled data point, $x^{(t)}$
	- 2. Predict label, $\hat{y} = sign(w^Tx+b)$
	- 3. Observe true label, $y^{(t)}$
	- 4. Pay a penalty if mistake, $\hat{y} \neq y^{(t)}$
		- $w \leftarrow w+y^{(t)}x^{(t)}$
		- $b \leftarrow b+y^{(t)}$
	- 5. Update parameters, w and b

Notational Hack: $\theta = [b, w_1, w_2, ..., w_D] \rightarrow \theta ^T x' = w^T x+b$
1. Initialize $\theta = [0, 0, ..., 0]$
2. For t = 1, 2, 3, ...
	- 1. Receive unlabeled data point, $x^{(t)}$
	- 2. Predict label, $\hat{y} = sign(\theta^T x')$
	- 3. Observe true label, $y^{(t)}$
	- 4. Pay a penalty if mistake, $\hat{y} \neq y^{(t)}$
		- $\theta \leftarrow \theta+y^{(t)}x'^{(t)}$
	- 5. Update parameters, $\theta$

# Inductive Bias

The true decision boundary is linear, so the more recent mistakes are more important to correct. 

# Batch Perceptron Learning Algorithm

- Input: $D = \{ (x^{(1)}, y^{(1)}), (x^{(2)}, y^{(2)}), ..., (x^{(N)}, y^{(N)})\}$
- Initialize $\theta = [0, 0, ..., 0]$
- while NOT CONVERGED
	- for $t\in \{ 1, ..., N\}$
		- Predict label of $x'^{(t)}$, $\hat{y} = sign(\theta^T x')$
		- Observe true label, $y^{(t)}$
		-  if mistake, $\hat{y} \neq y^{(t)}$
			- $\theta \leftarrow \theta+y^{(t)}x'^{(t)}$
# Perceptron Mistake Bound

Definitions:
- A dataset $D$ is *linearly separable* if exists a linear decision boundary that perfectly classifies the data points in $D$
- The *margin*, $\gamma$, of a dataset $D$ is the greatest possible distance between a linear separator and the closest data point in $D$ to that linear separator

Theorem: if the data points seen by the Perceptron Learning Algorithm (online and batch) 
1. lie in a ball of radius $R$ (centered around the origin) 
2. have a margin of $\gamma$ 
then the algorithm makes at most $(R/\gamma)^2$ mistakes.  

Key Takeaway: if $D_{train}$ is linearly separable, the batch Perceptron Learning Algorithm will converge (i.e., stop making mistakes on $D_{train}$ / achieve 0 training error) in a finite number of steps!

Compute the Margin:
![[Screen Shot 2024-02-19 at 04.30.05.png]]