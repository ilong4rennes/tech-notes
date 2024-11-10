# Setup
- Items: movies, songs, products, etc. (often many thousands) 
- Users: watchers, listeners, purchasers, etc. (often many millions) 
- Feedback: 5-star ratings, not -clicking ‘next’, purchases, etc.

# Key Assumptions
- Can represent ratings numerically as a user/item matrix 
- Users only rate a small number of items (the matrix is sparse)

Two Types of Recommender Systems:
1. Content Filtering
2. Collaborative Filtering

# Content Filtering
- Example: Pandora.com music recommendations (Music Genome Project)
- Con: Assumes access to side information about items (e.g. properties of a song) 
- Pro: Got a new item to add? No problem, just be sure to include the side information

# Collaborative Filtering
- Example: Netflix movie recommendations 
- Pro: Does not assume access to side information about items (e.g. does not need to know about movie genres) 
- Con: Does not work on new items that have no ratings
- Common insight: personal tastes are correlated 
	- If Alice and Bob both like X and Alice likes Y then Bob is more likely to like Y, especially (perhaps) if Bob knows Alice

Two Types of Collaborative Filtering 
1. Neighborhood Methods 
2. Latent Factor Methods
## Neighborhood Methods
![[Screen Shot 2024-04-28 at 18.51.04.png]]
## Latent Factor Methods
![[Screen Shot 2024-04-28 at 18.51.38.png]]
### Matrix Factorization
- Many different ways of factorizing a matrix 
	1. Unconstrained Matrix Factorization 
	2. Singular Value Decomposition 
	3. Non-negative Matrix Factorization 
- MF is just another example of a common recipe: 1. define a model 2. define an objective function 3. optimize with SGD

#### Low-rank Matrix Factorization
![[Screen Shot 2024-04-28 at 18.53.13.png]]
#### Regression vs. Collaborative Filtering
![[Screen Shot 2024-04-28 at 18.58.16.png]]
#### Unconstrained Matrix Factorization
![[Screen Shot 2024-04-28 at 18.58.49.png]]
![[Screen Shot 2024-04-28 at 19.00.08.png]]

![[Screen Shot 2024-04-28 at 19.00.36.png]]
![[Screen Shot 2024-04-28 at 19.00.49.png]]
