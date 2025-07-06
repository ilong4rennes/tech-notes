# Convolutional Neural Networks (CNNs)
## Convolution (卷积层)
- 目的：降维、提取底层特征
- Input$\times$Kernel = Output
- Different convolutions extract different types of low-level “features” from an image
	- Identity / Blurring / Horizontal, vertical edge detector / Sharpening
- Hyper-parameters of Kernel:
	- Padding
		- Problem: 边缘只计算一次，中间多次计算，output丢失边缘特征
		- Solution: padding your input image with zeros
	- Stride: Kernel滑动步长
	- Filter size: size of the window or matrix that slides over the input data

## Pooling (池化层)
- 目的：防止过拟合、减小数据维度
- **Method 1: Max Pooling** $y_{ij} = \max(x_{ij}, x_{i,j+1}, x_{i+1,j}, x_{i+1,j+1})$ ![[Screen Shot 2024-04-28 at 21.49.33.png]]
- **Method 2: Avg Pooling**![[Screen Shot 2024-04-28 at 21.49.11.png]]
## CNN
### Recipe for ML
1. Given training data: $\{ (x_i, y_i) \}_{i=1}^N$
2. Choose:
	- Decision Function: $\hat{y} = f_\theta (x_i)$
	- Loss Function: $\ell(\hat{y}, y_i) \in \mathbb{R}$
3. Define goal: $\theta^* = \arg\min_{\theta} \sum_{i=1}^N \ell(f_\theta(x_i), y_i)$
4. Train with SGD (take small steps opposite the gradient): $\theta^{(t+1)} = \theta^{(t)} - \eta_t \nabla \ell(f_\theta(x_i), y_i)$ 

- CNNs provide another form of decision function
- CNN key idea: Treat convolution matrix (kernel) as parameters and learn them!

### Training CNNs
- Q: Now that we have the CNN as a decision function, how do we compute the gradient? 
- A: Backpropagation of course!

### SGD for CNNs
![[Screen Shot 2024-04-28 at 22.33.11.png]]
# Recurrent Neural Networks (RNNs)
## Word Embeddings
- Key Idea: 
	- represent each word in your vocabulary as a vector W
	- store as a V x D matrix where: V = number of words in vocab. D = vector’s dimension 
- Modeling: 
	- define a model in which the vectors are parameters 
	- each copy of the word uses the same parameter vector 
	- train model so that similar words have high cosine similarity

## Definition
![[Screen Shot 2024-04-29 at 01.20.36.png]]
- If T=1, then we have a standard feed-forward neural net with one hidden layer, which requires fixed size inputs/outputs 
- By contrast, an RNN can handle arbitrary length inputs/outputs because T can vary from example to example 
- The key idea is that we reuse the same parameters at every timestep, always building off of the previous hidden state

## Bidirectional RNN
![[Screen Shot 2024-04-29 at 01.32.29.png]]
## Deep RNNs
![[Screen Shot 2024-04-29 at 01.37.32.png]]

## Deep Bidirectional RNNs
![[Screen Shot 2024-04-29 at 01.38.51.png]]

## Long Short-Term Memory (LSTM) 
- Motivation: 
	- Vanishing gradient problem for Standard RNNs
	- LSTM units have a rich internal structure 
	- The various “gates” determine the propagation of information and can choose to “remember” or “forget” information
![[Screen Shot 2024-04-29 at 01.42.18.png]]
![[Screen Shot 2024-04-29 at 01.42.41.png]]
$y_t = W_{yh} h_t + b_y$

## Deep Bidirectional LSTM (DBLSTM)
![[Screen Shot 2024-04-29 at 01.43.50.png]]
- Q: Why not just use LSTMs for everything? 
- A: Everyone did, for a time. But… 
1. They still have difficulty with long-range dependencies 
2. Their computation is inherently serial, so can’t be easily parallelized on a GPU 
3. Even though they (mostly) solve the vanishing gradient problem, they can still suffer from exploding gradients

# Human Language Technologies
## n-Gram Language Model
- Goal: Generate realistic looking sentences in a human language 
- Key Idea: condition on the last n-1 words to sample the nth word
- n-Gram Model (n=2) $p(w_1, w_2, \ldots, w_T) = \prod_{t=1}^T p(w_t | w_{t-1})$
- n-Gram Model (n=3) $p(w_1, w_2, \ldots, w_T) = \prod_{t=1}^T p(w_t | w_{t-1}, w_{t-2})$
- Question: How do we learn the probabilities for the n-Gram Model? 
- Answer: 
	- Basic: From data! Just count n-gram frequencies
	- Advanced: 
		1. Treat each probability distribution like a (50k-sided) weighted die 
		2. Pick the die corresponding to p(wt | wt-2, wt-1) 
		3. Roll that die and generate whichever word wt lands face up 4
		4. Repeat

## RNN Language Model
![[Screen Shot 2024-04-29 at 01.57.48.png]]
![[Screen Shot 2024-04-29 at 01.58.01.png]]
### Forward Propagation
![[Screen Shot 2024-04-29 at 02.25.39.png]]
![[Screen Shot 2024-04-29 at 02.25.55.png]]

## Transformer LM
![[Screen Shot 2024-04-29 at 22.14.03.png]]

- attention: how much weight a specific word should pay to each of the other words in my sentence. Take those scores, pass them into a softmax function so the weights sum up to 1.
- Scaled Dot-Product Attention
1. **Queries, Keys, and Values**: Think of every word in a sentence as coming with three sticky notes attached to it: a query (a question about the word), a key (a label describing the word), and a value (the word's actual meaning or content).
2. **Dot-Product**: The model then takes every word's query and sees how well it matches with all other words' keys (labels). This is like calculating a 'match score' by multiplying them together. If they match well, the score is higher. This is the 'dot-product' part.
3. **Scaling**: But there's a catch: when the numbers get really big, they can mess up the model's ability to learn smoothly. So, we scale the scores down, making them smaller by dividing by a certain factor (usually the square root of the size of the keys). This keeps everything manageable.
4. **Softmax**: After scaling, we apply a softmax function, which is a fancy way of turning the scores into probabilities that add up to one. It's like saying, "Okay, out of all these words, here's how much each one matters in relation to the word we’re focusing on."
5. **Multiply by Values**: Now that we know how much each word matters, we weigh their values by these probabilities. It's like adjusting the volume on different voices based on how important they are to the story you're trying to hear.
6. **Sum Up**: Finally, we add up all these weighted values. The result tells us what to focus on for each word when we’re trying to understand the sentence as a whole.

# Module-based Automatic Differentiation
- Backpropagation is a special case of a more general algorithm called reverse-mode automatic differentiation that can compute the gradient of any differentiable function efficiently!

**Backpropagation**
![[Screen Shot 2024-04-29 at 02.20.32.png]]
![[Screen Shot 2024-04-29 at 02.20.43.png]]

Key Idea: 
- componentize the computation of the neural-network into layers 
- each layer consolidates multiple real-valued nodes in the computation graph (a subset of them) into one vector-valued node (aka. a module)![[Screen Shot 2024-04-29 at 02.22.53.png]]
![[Screen Shot 2024-04-29 at 02.23.21.png]]
![[Screen Shot 2024-04-29 at 02.23.30.png]]
**Module-based AutoDiff (OOP Version)**
![[Screen Shot 2024-04-29 at 02.24.04.png]]
