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