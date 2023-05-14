[[COMP24112]]

- Refers to a technique for learning using neural networks.
- It's considered as a kind of representation learning - learning features to characterises your objects.

![[Pasted image 20230501104928.png|300]]

You can see how many more hidden layers it has! Hence, Deep Learning...

The traditional ML strategy:

![[Pasted image 20230501105330.png|600]]

And the Deep Learning:

![[Pasted image 20230501105547.png|600]]

We refer to the neural network as a representation Generator, and our prediction model as the predictor. 

So some examples of End-to-end systems:

![[Pasted image 20230501105728.png|500]]

To build these end-to-end systems:

- Convolutional neural networks (CNN) is used to automatically learn a good feature vector for an image form it's pixels
- Recurrent neural network (RNN) is a particularly useful for learning from sequential data. Each neuron can use it's internal memory to maintain information about the previous input. This makes it suitable for processing natural languages, speech and music, ...

##### Attention Mechanism

This is how Neural Networks operate:
- Black box decision - a decision supported by model parameters and mathematical operations that are hard to understand.

But maybe we wanna know what's happening under the hood?

![[Pasted image 20230501110436.png|500]]

Why are these two similar? Why did our NN say they are?

- Attention mechanism - a type of effective mathematical diagrams based on weighted sum, which uses weight functions to locate salient information.

So we can embed an attention mechanism to a neural network would help identify the contributing parts to final decision making.


##### Tips for Deep NN

- Pre-train: Consider to use pre-trained neural networks if your data shares some similar structure to existing training data that has been used to train some well-known neural networks
- Batch Normalisation: remove the mean and scale by standard deviation for outputs of hidden layers.
- Regularisation

![[Pasted image 20230501111600.png|600]]


- Skip Connection: a type of network connection proposed by ResNet

![[Pasted image 20230501110803.png|400]]


Allows for a link from different layers ^


### CNN

This is a Multi-Layered Perceptual:

![[Pasted image 20230501112408.png|500]]

Here each neuron is connected to all the previous neurons in the last layer, so we can call this fully connected.

Now for CNN: - 2D/3D neurons, the CNN supports layers that have neurons arranged in 2 dimensions or 3 dimensions

There are several types of layers:
- Convolutional
- Pooling
- Fully Connected (This occur at the end, like in MLP (multi-layer perceptuan (?)))

![[Pasted image 20230501112741.png|400]]

It's training is based on backpropagation and stochastic gradient descent.

##### Convolutional Layer
- Each neuron inside a layer is connected only to a small region of the previous layer, called a receptive field.
- The output of a neuron is a number computed from the output of those neurons from the corresponding local region and the weights of the convolutional filter: $y = \text{Activation}(w^Tx+b)$ 

![[Pasted image 20230501113336.png|450]]

Neuron in layer $h$ is only connected to a small portion of the previous layer $h-1$ and this is called local connectivity!

- ==Weight Sharing==: One same filter of the size of the local region slides over all spatial locations. - so basically when we calculate the output for the single neuron in layer $h$ all the neurons in that same layer should use the same weights.

![[Pasted image 20230501114152.png|600]]

![[Pasted image 20230501114241.png|600]]

Now slide the filter over all the spatial locations. 
- (Move to the right by 1)
- (Now jump a row and start at the left side...)
- (Move to the right by 1)

This creates a (7 - 2) by (7 - 2) layer.


The sliding process results in an activation map of size: $$\bigg ( \frac{N -F}{S} + 1 \bigg ) \times \bigg ( \frac{N -F}{S} + 1 \bigg )$$ where $N$ is the size of the input and $F$ is the size of the filter, and $S$ is the stride - so how much we move the filter by each time. $S$ should be chosen such that $(N-F)/S$ is an integer!

#### 3D Layer - Convolutional Layer
![[Pasted image 20230511015344.png]]

#### Further about Convolutional Layer
![[Pasted image 20230511015617.png]]

We can also connect multiple convolutional layers. With more convolutional layers, the activation side becomes smaller as we move on 
![[Pasted image 20230511015737.png]]

### Pooling Layer
![[Pasted image 20230511015929.png]]
This example just returning the maximum value from a stride

### Fully Connected Layer
![[Pasted image 20230511020058.png]]
Flats out the 3D array into a 1D array (yeah?)

### CNN Architecture
![[Pasted image 20230511020301.png]]



