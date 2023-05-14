k-NN is thought to be the ==simplest of all machine learning algorithms== and dates back to roughly 965-1040, with early academic works on k-NN starting in the 1960s.

### k-NN Classification
- Example Data: $\{x_i,y_i\}^n _{i=1}$
- Feature Vector: $x_i \in \mathbb{R}^d$
- Class Label: $y_i \in \{1,\ 2,\ \ldots,\ c\}$

We're trying to classify a given point to one of several classes. So let's look at how this works:

Given a set of training samples in the form $$\{\text{features}, \text{class label}\}$$ Each sample corresponds to a data point in the feature space.

Let's say we have measured a new object and want to see which class it's to be added into. Depending on what values are measured e.g (Petal Length, Height, ...) We can place our new object within the feature space.

- Once new point $x_{te}$ is placed in the feature space
- For all training data nodes $x_{tr}$ :
	- Measure the distance $(x_{te},\ x_{tr})$
- Sort the distances
- select the $k$ nearest nodes.
- Assign the class to node $x_{te}$ given by the most common class in the $k$ nearest nodes - this is ==known as majority voting==.

![[Pasted image 20230206081500.png|400]]

### k-NN Regression
- Example Data: $\{x_i,y_i\}^n _{i=1}$
- Feature Vector: $x_i \in \mathbb{R}^d$
- Class Label: $y_i \in \mathbb{R}^k$

Given a set of training samples in the form $$\{\text{features}, \text{class label}\}$$ Each sample corresponds to a data point in the feature space. (Basically the same idea as last time...)

- So let's say we have a new point $x_{te}$
- Again, for all nodes in the training data, $x_{tr}$ :
	- Let's measure distance($x_{te},\ x_{tr}$)
- Sort all nodes based on distance
- Select only the $k$ nearest nodes $x_{NN1}, x_{NN2},\ \ldots,\ x_{NNk}$
- We can then calculate $y_{te}$ ==by averaging out the output== of the nearest $k$ nodes:$$y_{te} = \frac{y_{NN1} + y_{NN2} + \ldots + y_{NNk}}{k}$$ So, we're averaging the output as opposed to using majority voting.

### Instance-based Learning
- Many algorithms are developed to predict outputs based on the similarity (or distances) of the query to it's nearest neighbour(s) in the training set.
- Represented by the algorithm: $k$-NN
- Aspects to consider:
	1. How do we compute the distance?
	2. How do we choose the number of neighbours $k$
	3. And, how do we infer the output from the neighbouring points.


### How do we Calculate Distance?

Well the most commonly used distance formula is Euclidean Distance:

>Given two $d$-dimensional data points: $$p = [p_1, p_2, \ldots, p_d],\ \ q = [q_1,q_2,\ldots,q_d]$$ Then: $$d(p, q) = \sqrt{(p_1 - q_1)^2 + \ldots + (p_d - q_d)^2}$$ You can visualise this in a 2D space quite easily:
>
>![[Pasted image 20230206092558.png|300]]

There's also the Minkowski Distance:
>$$d(p,q) = \big[ |p_1 - q_1|^t + \ldots + |p_d - q_d|^t \big]^\frac{1}{t}$$ Which is the same as: $$\bigg[ \sum^d _{i=1} |p_i - q_i|^t \bigg]^\frac{1}{t} $$

This is more so a ==generalisation== of Euclidean distance, as Euclidean Distance is where $t = 2$ and Manhattan (==city block==) Distance (another type of distance algorithm) is where $t= 1$ and below is an image that shows the difference between the 2 formulas:

![[Pasted image 20230206093812.png|200]]


We can also use a similarity measure! The $k$-Nearest Neighbours have the highest similarity values.

There are two formulas we can use: (Similarity formula attempt to capture co-occurence patterns)

==Inner Product== 
>$$s_{inner} (p,q) = \sum^d _{i=1}\ p_i q_i = p^Tq$$
(bigger is better!!)
==Cosine==
> $$S_{cos} (p,q) = \frac{ \sum^d _{i = 1} p_i q_i}{\sqrt{\sum^d _{i=1} p^2 _i} \sqrt{\sum_{i=1} ^d q^2 _i}} = \frac{p^Tq}{||p||_2||q||_2}$$


We can convert cosine to distance as well:
>$$d(p,q) = 1 - s_{cos} (p,q)$$

Nice example to understand:

![[Pasted image 20230206095530.png|300]]

### Effect of training samples:
- Having a small number of training samples leads to ==insufficient information==.
- A large number of training samples leads to more information but ends up taking ==more memory and time== (distance calculation and sorting)
- A Noisy training sample has inaccurate predictions (often outliers cause noise, and training on outliers causes overfitting)

### Effect of the number $k$
- $k$ is known as a hyper parameter and thus the process of determining the neighbour number $k$ is called hyper-parameter selection or model selection.

>[!info]- Hyper-parameter?
>Refers to a quantity that is fixed during training - and therefore not determined by training, but it determines the structure of the model.

- It's not a good idea to set $k$ as an even number for binary classification - as we have to pick the most common class within the $k$ nearest neighbours but what if you have equal number of each class occurring in the $k$ nearest neighbours?
- I think as a generalisation, for $n$ classes, $k$ should not be a multiple of $n$ for classification.

- Having a small $k$, we may model and take into account noise...
- Having a larger $k$, neighbours will include too many samples from other classes, which can affect the prediction.

### Neighbour Search Algorithm

- Fast Computation of nearest neighbours is an active research area in machine learning
- The Naïve approach would be to brute force compute distances between all pairs and samples and sort it..
- There exist tree-based methods:
	- basic idea is that knowing A is very distant from B and C is very close to B, we can conclude that C is far from A, without having to explicitly calculate the distance - this reduces the number of distance calculations.
	- Varieties of this include: K-D trees and Ball trees

>[!note]- Some things to note..
>- No explicit training - we're simply measuring and storing pieces of data...
>- Thus $k$-NN is referred to as non-parametric method as no parameters need to be optimised (remember this is different compared to a hyperparameter.)
