# Decision Stump

- def: based on a single feature, $x_d$, predict the most common label in $D_{train}$ among all data points that have the same value for $x_d$
- Graphical representation

## Pseudocode
```
def train(D_train):
	pick a feature, x_d
	
	# split D_train according to x_d
	for v in V(x_d), all possible values for x_d:
		D_v = {(x^n, y^n) in D_train | x_d^n = v}
		
	# compute the majority vote in each subset
	for v in V(x_d):
		store hat y_v = mode(labels in Dv)

def h(x'):
	for v in V(x_d):
		if x_d' = v: 
			return hat y_v
```

# Decision Tree
## Transition from decision stump to decision tree
- Which feature to split on: Splitting criterion
- Order of the split: Recursion

## Splitting Criterion
- def: function that measures how useful splitting on a particular feature is for a specified dataset
- optimize the splitting criterion when deciding which feature to split on
- Real-valued Features: 
	- Discretize real-valued features using thresholds (effectively creating new categorial features)
	- Can split on real-valued features multiple times in the same tree
### Training error rate (minimize)
- calculate the training error rate for every feature
- choose the feature that cause the least training error rate

### Gini impurity (minimize) $\rightarrow$ CART algo

### Mutual information (maximize) $\rightarrow$ ID3 algo
#### Entropy
- Entropy of a **random variable**$$
H(X) = -\sum_{v \in V(X)}^{} P(X=v)\log_{2} (P(X=v))
$$

- Entropy of a **set**$$
H(S) = -\sum_{v \in V(S)}^{} \frac{\left | S_v \right | }{\left | S \right | }\log_{2} (\frac{\left | S_v \right | }{\left | S \right | })
$$
	- $H(S)$: how uniform the set is. The higher the entropy, the more impure / mixed-up
	- $S$: a collection of values
	- $V(S)$: the set of unique values in S
	- $S_v$: the collection of elements in S with value v

#### Mutual Information
- Mutual information between two random variables $$\begin{align}
I(Y;X) & = H(Y)-H(Y|X) \\
&=H(Y) - \sum_{v \in V(X)}^{} P(X=v) (H(Y|X=v)) 
\end{align}$$
	
- Mutual information between a feature and the label $$\begin{align}
I(y;x_d) & = H(y)-H(y | x_d) \\
&=H(y) - \sum_{v \in V(x_d)}^{} f_v (H(Y_{x_d = v})) 
\end{align}$$
	- $I(y; x_d)$: how much clarity knowing the feature provides about the label
	- $x_d$: feature; $y$: the set of all labels
	- $V(x_d)$: the set of possible values $x_d$ can take on
	- $f_v$: the fraction of data points where $x_d=v$
	- $Y_{x_d = v}$: the set of all labels where $x_d =v$

## Training Pseudocode (recursively)
```
def train(D_train):
	store root = tree_recurse(D_train)

def tree_recurse(D'):
	q = new node()
	# base case
		if (all labels are the same
		OR all features have already been splitted on
		OR D' contains all the same feature vectors
		OR our tree is at some max depth):
			q.label = mode(labels in D')
	
	# recursion 
	else:
		find the best feature to split on, x_d
		q.split = x_d
		for v in V(x_d), al possible values of x_d:
			D_v = {(x^n, y^n) \in D' | x_d^n = v}
			q.children(v) = tree_recurse(D_v)
	return q
```

## Prediction Pseudocode (iteratively)
```
def h(x'):
	# walk from the root node to a leaf
	curr_node = root
	while true:
		if curr_node is internal (non-leaf):
			check the feature in curr_node, x_d
			go down the branch corresponding to x_d'
		else: # curr_node is a leaf
			return the associated label of curr_node
			
# x' = [x_1', x_2', ..., x_D'] one row of the data 
# x_d = feature (blood pressure)
# x_d' = value that the feature can take on (low/medium/high)
```

## Inductive Bias
- def: principle by which it generalizes to unseen examples
- **inductive bias of the ID3 algorithm**: Greedy search for the smallest tree that achieves low training error with high mutual information features at the top

## Pros & Cons
### Pros
- Interpretable 
- Efficient (computational cost and storage) 
- Can be used for classification and regression tasks 
- Compatible with categorical and real-valued features

### Cons
- Learned greedily: each split only considers the immediate impact on the splitting criterion
	- Not guaranteed to find the smallest (fewest number of splits) tree that achieves a training error rate of 0.  
- Liable to overfit!

## Overfitting & underfitting

**Problem:**
- Overfitting: not having enough inductive bias pushing it to generalize 
	- true error rate > training error rate
- Underfitting: has too much inductive bias

**Solution:**
- Do not split leaves
	- > depth $\delta$
	- < c data points
	- maximum information gain < $\tau$
- Take a majority vote in impure leaves
- Pruning
	- Learn a tree
	- Evaluate (validation dataset) validation error rate with and without the split
	- Greedily remove split that most decreases the validation error rate
	- Stop if no split is removed