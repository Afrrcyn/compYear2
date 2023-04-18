[[COMP24112]]

Artificial Neural Network
- Computing systems that are inspired by the biological neural networks that constitute animal brains. It has deviated from biology and does not work like a human brain.

>[!info] Structure
>- Based on a collection of connected units called artificial neurons which loosely model the neurons in a biological brain: 
>- ![[Pasted image 20230320081417.png|400]]

##### Single Neuron

So let's look at a Single Neuron Model:
- Multiple inputs $[x_1, x_2,\ ...,\ x_d]$ and output $y$

![[Pasted image 20230320081635.png|500]]

![[Pasted image 20230320081652.png|200]]

There are many types of activation functions...

Identity:

![[Pasted image 20230320081949.png]]
![[Pasted image 20230320082011.png]]


Threshold:

![[Pasted image 20230320082027.png]]
![[Pasted image 20230320082115.png]]

Sigmoid:

![[Pasted image 20230320082045.png]]
![[Pasted image 20230320082135.png]]
> Note that in Sigmoid function, it returns a 0 if no signal is sent, whereas in Tanh it returns a -1

Rectified Linear Unit:

![[Pasted image 20230320082101.png]]
![[Pasted image 20230320082145.png]]

One neuron is very similar to a multi-input, single output model.

##### Single Layer Perception
A single layer perception has one input layer and one output layer. So this is what our single neuron from earlier looks like:

![[Pasted image 20230320083040.png|300]]

And now this is what our multi-neuron looks like:

![[Pasted image 20230320083108.png|400]]

A SLP (Single layer perception) is similar to a multi-input multi-output linear model.

We can add hidden layers now!
- The presence of hidden layers allows to formulate more complicated functions that are non-linear
- Each hidden node finds a partial solution to the problem and is combined in the next layer...

![[Pasted image 20230320083417.png|500]]

Multilayer Perception
- A multilayer perception (MLP) is also called a feedforward artificial neural network, it consists of at least three layers of nodes; input layer, hidden (at least one) and output layerd.

![[Pasted image 20230320084208.png|500]]

![[Pasted image 20230320084224.png|300]]

![[Pasted image 20230320084449.png|600]]

9 input neurons and 2 output neurons and within our 1 hidden layer we have 4 neurons...

Let's look at a MLP Mapping Function
- MLP with 1 input and 1 output, sigmoid activation is used in the output layer.
- Sigmoid activation in all hidden layers
- Random Weights:

![[Pasted image 20230320085007.png|500]]

here's another example with the same setup bar the fact that Relu activation is used in the hidden layers.

![[Pasted image 20230320085224.png|600]]


### Training Neural Networks

it's the process of finding the optimal setting of the neural network weights.

##### Hebbian Learning Rule

![[Pasted image 20230320093948.png|500]]
> Synapse: Connection between the two neurons

![[Pasted image 20230320094240.png|500]]

if $x_i$ and $z_j$ are both activated (both positive) then the change in weight is positive and so the weight increases, otherwise the weight could stay the same or even decrease if either is negative. Used more often in biologically inspired networks.

![[Pasted image 20230320094931.png|600]]![[Pasted image 20230320095652.png|600]]![[Pasted image 20230320095922.png|600]]
We're updating the weights with each sample!

![[Pasted image 20230320100121.png|600]]

AND we can do this with the final sample as well but the graph basically does not change and you can see the graph is the above image separates the different classes out now.

Note this is the first update, and on further iterations of updates we change up the order of the samples picked first for updating. And it follows the exact same rules...

##### Gradient Based Training
A commonly used approach
- treat $z = \psi (x,\ W_{NN})$ as the new feature to be used as the input of linear predictor
- A loss function is minimised to find the optimal neural network weights $W_{NN}$ together with the weights of the linear predictor $W_P$
- training methods: Stochastic Gradient Descent, Mini-batch Gradient Descent
- More accurate and stable than Hebbain Learning rules in practise. 

![[Pasted image 20230320101729.png|500]]

This is the training pipeline:

![[Pasted image 20230320101858.png|500]]

###### Case Study: Perception Algorithm
training a single neuron model by minimising the perception criterion using stochastic gradient descent

![[Pasted image 20230320102142.png|300]]

![[Pasted image 20230320102237.png|500]]

So let's look at the Perception Algo:

![[Pasted image 20230320102358.png|600]]

We update using a misclassified sample in each iteration only! That's the major difference here from Hebbian. So why do we need to follow this rule?

![[Pasted image 20230320102716.png|500]]

[I think above it's multiplied by -1, simply due to the fact if it's misclassified then it's negative and so we multiply to make it positive and then that's our objective function that we have to minimise.]

![[Pasted image 20230320103103.png|400]]

So the perception algorithm invented by the psychologist in 1958 is equivalent (sorta) to the method done by Stochastic gradient descent.

###### Example

![[Pasted image 20230320103309.png|500]]
> So the sign comes from the true label if there exists a misclassification

### Backpropagation

![[Pasted image 20230320104921.png|500]]

![[Pasted image 20230320105334.png|400]]
Quick Note:
Regularisation helps to reduce overfitting - we have seen this before, the regularisation term helps to distract the Objective Function a little bit.

So now let's visualise backpropagation:

![[Pasted image 20230320105648.png|500]]

So how do we calculate the loss function?

![[Pasted image 20230320111310.png|500]]

Each layer passes it's features to the next layer to calculate the new feature.

![[Pasted image 20230320111630.png|500]]

![[Pasted image 20230320112618.png|500]]

![[Pasted image 20230320112856.png|600]]

![[Pasted image 20230320113141.png|400]]

![[Pasted image 20230320113256.png|400]]

![[Pasted image 20230320113404.png|400]]
