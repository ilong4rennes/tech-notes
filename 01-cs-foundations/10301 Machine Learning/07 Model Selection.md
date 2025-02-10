# Terminology

- **Model class**: the set of all possible trees given some hyper-parameter (eg. max depth)
- **Parameter**: structure of a specific tree selected by the learning algorithm
- **Hyper-parameter**: tunable aspects of the model that need to be specified before learning can happen, set outside of the training procedure (max depth / splitting criterion...)
# Parametric vs. Non-parametric Models

**Parametric models** (e.g., decision trees)
- fixed number of parameters
- can discard data after parameters have been learned
- cannot exactly model every target function

**Nonparametric models** (e.g., kNN)
- no parameters learned from training data; can still have hyper-parameters 
- training data needs to be stored to make predictions 
- can recover any target function given enough data
# Model Selection with `Validation Sets

![[Screen Shot 2024-02-18 at 20.45.26.png]]
1. Split $D=D_{train}\cup D_{val}\cup D_{test}$, we have candidate models: $H_1, ..., H_M$
2. Learn classifiers using $D_{train}$
3. Evaluate models using $D_{val}$, choose the one with lowest validation error
4. Learn a new classifier from the best model using $D_{train}\cup D_{val}$
5. Optionally, use $D_{test}$ to estimate true error
# Hyper-parameter Optimization with Validation Sets

- change hyper-parameters = change the hypothesis space

![[Screen Shot 2024-02-19 at 04.35.46.png]]
- Idea: set the hyper-parameters to optimize some performance metric of the model 
- Issue: if we have many hyper-parameters that can all take on lots of different values, we might not be able to test all possible combinations

# K-fold Cross-validation
*How should we partition our dataset?*

- Given D, split D intro K equally sized datasets: D1, ..., Dk
- Use each one as a validation set one:
	- Let $h_{-i}$ be the classifier learned using $D_{-i}=D\text{ other than } D_i$, let $e_i = err(h_{-i}, D_i)$
	- K-fold cross validation error is: $err_{cv_{K}} = \frac{1}{K} \sum_{i=1}^K e_i$ 


![[Screen Shot 2024-02-18 at 20.46.28.png]]