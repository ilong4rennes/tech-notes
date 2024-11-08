## Learning Theory

### Questions
1. Given a classifier with zero training error, what can we say about true error (aka. generalization error)? (Sample Complexity, Realizable Case) 
2. Given a classifier with low training error, what can we say about true error (aka. generalization error)? (Sample Complexity, Agnostic Case) 
3. Is there a theoretical justification for regularization to avoid overfitting? (Structural Risk Minimization)

### PAC Model (Function Approximation View)
- Given: $x^{(i)} \sim p^*(\cdot) \quad$ $\quad y^{(i)} = c^*(x^{(i)})$
- Learning algo chooses $h\in H$ with lowest training error $\hat{h}_{h \in H}=\arg \min \hat{R}(h)$
- Goal: choose an $h$ with low generalization error $R(h)$
- only concerned with **binary classification**

### Two types of Error
1. True error (expected risk)
$$
R(h) = P_{x \sim p^*(x)}(c^*(x) \neq h(x))
$$
2. Train error (empirical risk)
$$
\begin{align}
\hat{R}(h) &= P_{x \sim S}(c^*(x) \neq h(x)) \\

&= \frac{1}{N} \sum_{i=1}^{N} \mathbb{1}(c^*(x^{(i)}) \neq h(x^{(i)}))\\

&= \frac{1}{N} \sum_{i=1}^{N} \mathbb{1}(y^{(i)} \neq h(x^{(i)}))
\end{align}
$$
- Intuition: Sample size large enough, train error就能接近true error

### Three Hypotheses of Interest
1. true function $y^{(i)} = c^*(x^{(i)}), \forall i$
2. expected risk minimizer $h^* = \underset{h \in \mathcal{H}}{\mathrm{argmin}} \ R(h)$
3. empirical risk minimizer  $\hat{h} = \underset{h \in H}{\mathrm{argmin}} \ \hat{R}(h)$ 

## PAC Learning
### Setup

- PAC = Probably Approximately Correct
- A PAC learner yields a hypothesis $h \in H$ which is 
	- approximately correct $R(h) \approx R(h^*)$ 
	- with high probability $Pr(R(h) \approx R(h^*))\approx 1$
- PAC Criterion: $\forall h \in H, Pr(|R(h)-\hat{R}(h)|<\epsilon)\geq 1-\delta$ 
- Sample Complexity: min number of training examples, $N$, st. satisfies PAC criterion
- Consistent Learner: $h \in H$ is consistent if $\hat{R}(h)=0$

### Four Cases

![[Screen Shot 2024-03-20 at 05.09.21.png]]

- Realizable: $c^* \in H$
- Agnostic: $c^*$ is not necessarily in $H$ 
- Finite H: : H = the set of all decision trees of depth D over binary feature vectors of length M
- Infinite H: H = the set of all linear decision boundaries in M dimensions
### Theorem 1

- $N \propto M$ (# of features) 
- $|H|=3^M$, $\ln(|H|)=M\cdot \ln 3$  

#### Proof

#### Corollary
- Given training dataset S, $|S|=N$, all $h\in H$ with $\hat{R}(h)=0$ have $R(h) \leq \frac{1}{N} \left( \ln(|\mathcal{H}|) + \ln\left(\frac{1}{\delta}\right) \right)$ with $P>1-\delta$. 

### Theorem 2
#### Corollary
- Given training dataset S, $|S|=N$, all $h\in H$ have $R(h) \leq \hat{R}(h) + \sqrt{\frac{1}{2N} \left( \ln(|\mathcal{H}|) + \ln\left(\frac{2}{\delta}\right) \right)}$ with $P>1-\delta$. 

### VC Dimension

- $|H|=$ how many kinds of functions it can model
- Labeling
	- $\mathcal{S} = \{x^{(1)}, \ldots, x^{(N)}\}$
	- The set of labelings induced by H on S is: $\mathcal{H}(\mathcal{S}) = \{ [h(x^{(1)}), \ldots, h(x^{(N)})] \ | \ h \in \mathcal{H} \}$, $H(S)\leq 2^{|S|}$

- Shattering: learning algo can correctly separate all possible arrangements (labelings) of those points into 2 different categories
- H(S) is the set of all labelings induced by H on S
	- If |S| = N, then $|H(S)|\leq 2^N$ 
	- H shatters S if $|H(S)|= 2^N$ 

- VC(H): the size of the largest set S that can be shattered by H. If H can shatter arbitrarily large finite sets, then VC(H) = $\infty$  
- To prove that VC(H)=d, you need to show 
	1. $VC(H)\geq d$, ∃ some set of d data points that H can shatter and 
	2. $VC(H)\leq d$,∄ a set of d + 1 data points that H can shatter

#### Examples

1. **Linear Classifier (e.g., Perceptron):**
    - Model: A straight line dividing the plane.
    - VC Dimension: 3.
2. **Polynomial of degree d:**
    - Model: A curve that can be as complex as a polynomial of degree d.
    - VC Dimension: d + 1
3. **Decision Trees:**
    - Model: A tree structure where each node represents a decision based on some feature.
    - VC Dimension: Depends on the depth and features used, but generally increases with more nodes.

### Theorem 3
![[Screen Shot 2024-03-20 at 08.24.25.png]]
#### Corollary
![[Screen Shot 2024-03-20 at 08.24.17.png]]

### Theorem 4
![[Screen Shot 2024-03-20 at 08.24.38.png]]

#### Corollary
![[Screen Shot 2024-03-20 at 08.24.53.png]]

### Approximation Generalization Tradeoff
![[Screen Shot 2024-03-20 at 08.26.38.png]]
![[Screen Shot 2024-03-20 at 08.26.50.png]]

### Model Selection
![[Screen Shot 2024-03-20 at 08.27.12.png]]
![[Screen Shot 2024-03-20 at 08.27.36.png]]





