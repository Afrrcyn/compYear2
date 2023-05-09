[[COMP24112]]

![[Pasted image 20230306075643.png|500]]
Revisit the pipeline 
- the first step is the prediction function - we need to choose the method as well - probabilistic and non-probabilistic.
- Now we have to find the best values for $\theta_1,\ ...\ \theta_m$, thus we construct a loss function.
- Finally today - we will look at optimising the loss function. Solving the optimisation problem

### Optimality Condition

![[Pasted image 20230306080113.png|500]]

This makes sense, thinking of when we have $\frac{\text d y}{\text d x}$, thus the solution must satisfy the partial derivative.
The vector of all the fractions is called a ==gradient vector== 

Let's look at a case study:

![[Pasted image 20230306080325.png|500]]

For a single-output Linear Model, we have a set of training samples up to $N$, and get a predicted output for each training sample.
Finally compute the sum-of-squares error loss.

For Multi-Output:

![[Pasted image 20230306080529.png|500]]

Note here the output and weight vectors are now 2d matrices compared to previously being a column vector.


![[Pasted image 20230306080631.png|500]]

So if we can get the perfect optimisation when $\hat X w = y$ which makes sense when you look at the loss function in the two images above. (might not be the best thing to perfectly optimise to the training data tho..)

When there are more training samples than input features ($N > d$) the system is overdetermined - so we have too much information...

We use normal equations instead of linear equations:

![[Pasted image 20230306080957.png|500]]

[To know how to derive check the notes on linear and quadratic equations]

So rewriting the formula, the gradient contains the difference between the predicted output and the true value for the $i$-th term, thus the gradient contains the error term.

![[Pasted image 20230306081517.png|500]]

Apply the optimality condition - setting it to zero. This is only possible because $\hat X^T \hat X$ is invertible. And when $N > d$

And similarly we can do this for multi-output:

![[Pasted image 20230306081658.png|500]]

![[Pasted image 20230306081716.png|500]]

What happens if $N < d$, thus the system is underdetermined.. Not enough training samples to determine how to set weight vector and thus we have higher freedom..

Thus there exists infinite many solutions and we want to use the one with a minimum normal - set a constraint to help us determine an answer.

We want the minimum such that $||W||_z$ and $||W||_F$ are minimised (not sure what these are...)

![[Pasted image 20230306082050.png|500]]

Regardless of the sample and feature size, we can find the solution using the pseudoinverse.


![[Pasted image 20230306082811.png|500]]


![[Pasted image 20230306083005.png|500]]

We can use regularisation to improve our model... So we change the function we are optimising to avoid overfitting...

Given the new function we can use the same method to derive $W$. $\lambda$ here is the regularisation term and it's also a hyperparameter which we will need to find.

![[Pasted image 20230306083200.png|500]]

Helps to distract the linear model to avoid overfitting.. Keep in mind it's a hyperparameter - we do not optimise using the training data.

A good way is to split the data into 3 parts - training, validation and testing...
- training used for model parameters
- validation - try multiple values for $\lambda$ and compare the performance...

### Example
[Regular Linear Least Square Solution]

![[Pasted image 20230306083550.png|500]]

Remember $X..$ (with the funny symbol) is the PseudoInverse

![[Pasted image 20230306083707.png|600]]

What is the difference between $\hat X$ and $X$, $\hat X$ has an extra column filled with 1's for the first weight which is not multiplied by a variable. (A constant...)

![[Pasted image 20230306083856.png|600]]

This figure shows the model. Against testing and new samples.

![[Pasted image 20230306083959.png|500]]

![[Pasted image 20230306084228.png|500]]

![[Pasted image 20230306084246.png|500]]

This is our testing data... Now let's test a new sample:

![[Pasted image 20230306084315.png|500]]


Third Example:

![[Pasted image 20230306084409.png|500]]

This shows how the power of the input variable affects the model.

![[Pasted image 20230306084640.png|500]]

[Red are new data points], for complex models we get overfitting... especially outside the range of the original testing data. And on the other side the linear model is underfitting.

![[Pasted image 20230306084810.png|500]]

What if we don't have the validation set?

![[Pasted image 20230306085026.png|400]]

PROBLEM:

![[Pasted image 20230306085339.png|500]]

We're setting the partial derivative to zero here, setting the gradient to zero is giving us a set of linear equations however when the model becomes very complex, these equations become non-linear..

Classic Iterative Approach:

![[Pasted image 20230306085500.png|500]]

How do we decide the change here? We want to make sure that every time we change the current guess, we would want the change() function to give us an optimisation function that returns a smaller value.

![[Pasted image 20230306085736.png|500]]

We want to se the change to the opposite of the gradient as the gradient points us to the maximum, so we use the opposite. $\eta$ is our learning rate - how much do we change every change - $\eta$ determines this?

![[Pasted image 20230306090029.png|500]]

Let's look at an example:

![[Pasted image 20230306090113.png|500]]

The gradient is simply $2(x-1)$ as there's only one variable so it's easy to determine...

Let's look at how the learning rate affects our outcome..

![[Pasted image 20230306090323.png|400]]![[Pasted image 20230306092802.png|400]]![[Pasted image 20230306092902.png|400]]
See how the value of $\eta$ will determine whether we find the minimum - however $\eta$ will determine how many iterations we have to perform to find the minimum.

![[Pasted image 20230306093315.png|400]]

![[Pasted image 20230306094803.png|500]]

![[Pasted image 20230306095020.png|600]]

Stochastic simply uses one training sample as opposed to all training samples...

Mini-batch gradient descent -  uses a smaller sample set.

![[Pasted image 20230306095726.png|500]]

