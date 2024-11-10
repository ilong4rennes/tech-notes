# Bagging
## Random Forests
- Combines the prediction of many diverse decision trees to reduce their variability
- If B independent random variables $x^{(1)}, x^{(2)}, \ldots, x^{(B)}$ all have variance $\sigma^2$, then the variance of $\frac{1}{B} \sum_{b=1}^{B} x^{(b)}$ is $\frac{\sigma^2}{B}$. 
- Random forests = sample bagging + feature bagging 
				= bootstrap aggregating + split-feature randomization

## Aggregating
- How can we combine multiple decision trees, $\{t_1, t_2, ..., t_B\}$ , to arrive at a single prediction? 
- Regression - average the predictions: $\bar{t}(x) = \frac{1}{B} \sum_{b=1}^{B} t_b(x)$
- Classification - plurality (or majority) vote; for binary labels encoded as −1, +1: $\bar{t}(x) = \text{sign}(\frac{1}{B} \sum_{b=1}^{B} t_b(x))$

## Bootstrapping
- Insight: one way of generating different decision trees is by changing the training data set 
- Issue: often, we only have one fixed set of training data 
- Idea: resample the data multiple times with replacement
	- Each bootstrapped sample has the same number of data points as the original data set 
	- Duplicated points cause different decision trees to focus on different parts of the input space

## Split-feature randomization
- Issue: decision trees trained on bootstrapped samples still behave similarly 
- Idea: in addition to sampling the data points (i.e., the rows), also sample the features (i.e., the columns) 
- Each time a split is being considered, limit the possible features to a randomly sampled subset

## Random Forests Bagging Algorithm
- Input: $D = \left\{ \left( x^{(n)}, y^{(n)} \right) \right\}_{n=1}^N, B, \rho$ 
	- $B$ = number of trees in the ensemble
	- $\rho$ = number of features to resample for split-feature randomization
- For b = 1, 2, … , B
	- Create a dataset, $D_b$, by sampling $N$ points from the original training data $D$ with replacement 
	- Learn a decision tree, $t_b$, using $D_b$ and the ID3 algorithm with split-feature randomization, sampling $\rho$ features for each split 
- Output: $\bar{t} = f(t_1, \ldots, t_B)$, the aggregated hypothesis

## Setting hyper-parameters $B$ and $\rho$?

### Hyper-parameter optimization & Validation sets
![[Screen Shot 2024-04-28 at 00.24.37.png]]
We are going to do something similar to this in random forest. 
### Out-of-bag error
- For each training point, $x^{(n)}$, there are some decision trees which $x^{(n)}$ was not used to train (roughly $B/e$ trees or 37%) 
	- Let these be $t_{(-n)} = \left\{ t^{(-n)}_1, t^{(-n)}_2, \ldots, t^{(-n)}_{N_{-n}} \right\}$
- Compute an aggregated prediction for each $x^{(n)}$ using the trees in $\bar{t}^{(-n)}$ , $\bar{t}^{(-n)}(x^{(n)})$
- Compute the out-of-bag (OOB) error, e.g., for classification $$E_{\text{OOB}} = \frac{1}{N} \sum_{n=1}^N 1\left(\bar{t}^{(-n)}(x^{(n)}) \neq y^{(n)}\right)$$
- $E_{OOB}$ can be used for hyper-parameter optimization!
![[Screen Shot 2024-04-28 at 00.49.53.png]]
### Feature importance
- Some of the interpretability of decision trees gets lost when switching to random forests 
- Random forests allow for the computation of “feature importance”, a way of ranking features based on how useful they are at predicting the target 
- Initialize each feature’s importance to zero 
- Each time a feature is chosen to be split on, add the reduction in entropy (weighted by the number of data points in the split) to its importance

# Boosting
## Weighted Majority Algorithm
- Given: pool A of binary classifiers (that you know nothing about) 
- Data: stream of examples (i.e. online learning setting) 
- Goal: design a new learner that uses the predictions of the pool to make new predictions 
- Algorithm: 
	- Initially weight all classifiers equally 
	- Receive a training example and predict the (weighted) majority vote of the classifiers in the pool 
	- Down-weight classifiers that contribute to a mistake by a factor of β
![[Screen Shot 2024-04-28 at 03.45.43.png]]
![[Screen Shot 2024-04-28 at 03.49.20.png]]
## AdaBoost

### Comparison 
- Weighted Majority Algorithm: assumes the classifiers are learned ahead of time, only learns weight for each classifier
- AdaBoost: simultaneously learns: the classifiers themselves, and the weights for each classifier

### Definition
- Weak learner: one that returns a hypothesis that is not much better than random guessing 
- Strong learner: one that returns a hypothesis of arbitrarily low error
- AdaBoost answers the following question: Does that exist an efficient learning algorithm that can combine weak learners to obtain a strong learner?

### Algorithm
![[Screen Shot 2024-04-28 at 03.53.28.png]]

### Theory

#### (Training) Mistake Bound
![[Screen Shot 2024-04-28 at 06.25.05.png]]
#### Generalization Error
![[Screen Shot 2024-04-28 at 06.25.21.png]]
The more classifiers you bring in, the looser the bound is getting. Theoretically it sounds like an overfitting problem. But empirically, it actually works the other way. Instead of going back up after some rounds, the test error actually converges to some pretty good values. ![[Screen Shot 2024-04-28 at 06.25.34.png]]

## Bagging VS. Boosting

- Bagging tends to be most useful when your classifiers exhibit high variance from one training sample to the next 
- Boosting tends to be most useful when your classifiers exhibit high bias (i.e. are very simple)

### Bias-Variance Tradeoff
![[Screen Shot 2024-04-28 at 06.27.03.png]]
