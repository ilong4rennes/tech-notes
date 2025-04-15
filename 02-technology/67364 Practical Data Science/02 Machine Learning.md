# Recommender Systems

# Random Forests, Ensemble Methods

## Decision Trees

- Used in both regression and classification
- At each node, the tree evaluates a condition (typically based on one feature) and splits the data into two or more groups. 
- The process is repeated recursively until a stopping criterion is met, such as reaching a maximum depth or having minimal node impurity.

**Splitting Criteria**: Methods to determine the best feature and threshold at each node
- Gini impurity
- entropy (information gain)
- variance reduction (for regression)

## Random Forests

- A random forest is an ensemble learning method that builds a “forest” of decision trees and combines their predictions to improve overall performance. 
- The idea is based on the observation that aggregating the predictions of several weak learners (individual decision trees) can yield a more robust, accurate, and generalized model.

**Random Forests = Bootstrap Aggregating + Feature Bagging**
- **Bootstrap Sampling**: Each tree is trained on a bootstrap ***sample***, which is created by sampling with replacement from the original dataset.
	- Each bootstrapped sample has the same number of data points as the original data set 
	- Duplicated points cause different decision trees to focus on different parts of the input space
- **Feature Bagging**: At each split in the tree, don’t look at all the ***features***. Instead, randomly pick a few of them, and then choose the best one among those.

## How to Train a Random Forest

1. Data preparation: Clean data, Split data (into training and validation sets)
2. Create bootstrapped datasets: For each tree, randomly sample from training set to create a unique dataset
3. Build individual decision tree: For each tree, ...
	1. Feature subset selection (feature bagging - randomly select a subset of features)
	2. Splitting criterion
	3. Stopping conditions
4. Aggregation of predictions
	1. For Classification: Use majority voting among all trees
	2. For Regression: Calculate the average of all individual tree predictions
5. Evaluation and tuning 
	1. Cross-Validation
	2. Hyperparameter Tuning: number of trees ($B$), maximum depth, minimum samples per split, number of features at each split

* OOB Error
## Ensemble Methods

- techniques that combine multiple models (often referred to as “weak learners”) to produce a more robust and accurate predictive model

**Types of Ensemble Methods**:
1. **Bagging** (Bootstrap Aggregating)
	- Focus on **reducing variance by averaging or voting across models trained on different bootstrap samples**.
	- Bootstrapping: resample the data multiple times with replacement
	- Aggregating: For Classification / Regression
	- Examples: Random forest
2. **Boosting**
	- Sequentially trains models with a focus on misclassified instances in previous iterations.
	- Examples: AdaBoost, Gradient Boosting, and XGBoost