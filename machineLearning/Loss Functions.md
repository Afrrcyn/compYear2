[[COMP24112]]

## Regression

Let's revisit the Data + Model Strategy in the context of supervised learning.

![[Pasted image 20230227080311.png|400]]

We have our training data $D_{tr}$, We utilise it to make a prediction function. And we do that by optimising the parameters $\theta$ and returns the output for query $x$.

For parameters $\theta$ some need to to be optimised and others are hyper-parameters and needed to be selected.

After training, we have our final model.

![[Pasted image 20230227080509.png|300]]

The process of using a trained model on unseen data (query or testing data) is called inference.

Loss functions are essential in training and are computed using the training data - it helps to decide how good our model parameters are. (other names include error, cost, objective function)

When training we need to pick a loss function $O(\theta)$ then:
- Minimise it, if $O$ evaluates how bad the function is or
- Maximise it, if $O$ evaluates how good the model is.

Supervised Learning $O(\theta, D_{tr})$

Losses for Training Regression Models

![[Pasted image 20230227080837.png|400]]

### Non-Probabilistic.
- Recall RMSE? -  we can use this for a loss function.
- Some regression losses are simplified versions of RSME using training sample.
- The prediction for each training sample is computed by $\hat y_i = f(\theta, x_i)$

![[Pasted image 20230227081039.png]]

Where $N$ is the number of samples.

Why do we have $1/2$ for the Sum of squares error loss? It simply helps with the derivative, as it will cancel out a $2$ allowing for a simpler gradient.

![[Pasted image 20230227081311.png|500]]
![[Pasted image 20230227081337.png|500]]
![[Pasted image 20230227081425.png|500]]

So to minimise this regression error loss enables you to find the best red line to have the shortest blue distances on average.

![[Pasted image 20230227081517.png|500]]

##### Linear Least Squares (LLS)
- To train a linear model by minimising the sum of the squares error:

![[Pasted image 20230227081702.png|300]]

Regularised LLS
- A regularisation term can be added to the error function, for instance in the single output case we have:

![[Pasted image 20230227081824.png|600]]

We add a term, so for a linear term it's the transpose of the weight vector multiplied by the weight vector. It helps to prevent overfitting to the training data. however when $\lambda$ is to large it leads to under-fitting. SO $\lambda$ is the hyper-parameter.

![[Pasted image 20230227082158.png|700]]
Over-fitting: fits too closely to a particular set of data and may therefore fail to fit new data.

Under-fitting: cannot capture the underlying trend of the training data.

Prevent over-fitting e.g $O_A(w)$ gives less emphasis to the sum of squares error of the training data. 

### Probabilistic
Likelihood: Given the observed data, it is the conditional probability assumed for the observed data given some parameter values.

$$\text{Likelihood}(\theta, data) = p \times (data | \theta)$$
Log Likelihood - takes the natural logarithm of the likelihood.

Likelihood Maximisation, so we can maximise it based on the training samples. And when we formulate the objective function we assume independence between samples.

![[Pasted image 20230227083153.png|400]]

Example with Linear Regression
>![[Pasted image 20230227083401.png|500]]
>Assume Gaussian Distribution with predicted output $w^T \tilde x$ as the mean and $\sigma$ as the standard deviation. 
>
>For the Log Likelihood, the first two terms are constants, and the third time is actually the sum of squares error function... In this case MLE is equivalent to the sum of squares minimisation.

## Classification

![[Pasted image 20230227083839.png|400]]

##### Non-Probabilistic

##### Sum of Squares

You can train a discriminant function by minimising the sum of squares lose, this results in the least square approach for classification.

![[Pasted image 20230227084025.png|500]]

The above is only for binary classification.

For multi-class a similar idea is used:

![[Pasted image 20230227084232.png|500]]

It's for labels $\text{class } A$ and $\text{NOT class } A$, based on this approach we can then make the discriminant function.

Iris Example
> ![[Pasted image 20230227084446.png|500]]


A more general approach that discards the $+1$ and $-1$: 

![[Pasted image 20230227084643.png|500]]

 Here we simply do $a/b$ label coding. Note that $T$ is usually the average of $a$ and $b$.

In theory you can choose any $a$ and $b$ however some data may be sensitive to $a$ and $b$ and so the standard is to use $+1$ and $-1$ or $1$ and $0$. Where we commonly use the average of $a$ and $b$ to determine the threshold $T$ however we could also treat this as a hyperparameter.


##### Hinge Loss

![[Pasted image 20230227085023.png|500]]

We usually use $+1/-1$ label coding over $N$ training samples. We want everything to be zero, so we want $y_i f$ to be greater than 1. When it's greater than one, our cost function returns 0. (known as zero error)

If $y_i f$ is smaller than 1 but $(1 - y_i f)$ is still positive, it's not too bad.. (known as small error)

if $y_i f$ is negative, then we get a large error...

So we want the sign of $y$ and $f$ to be the same, either both negative or both positive.

![[Pasted image 20230227085834.png|500]]

Above is the hinge law and here we can apply regularisation and this new equation gives us a famous machine learning model - support vector machine learning.

Example:

![[Pasted image 20230227090005.png|500]]

### Probabilistic
- Cross entropy loss based on class posterior $$p(\text{class }k\ | \ x)$$
Cross Entropy measures distance between probability distributions. It's discrete version be used to examine the distance between predicted class probabilities (posterior) and the true class probabilities.

![[Pasted image 20230227090725.png|500]]

where $p$ is the true class probability and the $q$ is the predicted class probability.


##### Cross Entropy Loss

![[Pasted image 20230227090849.png|500]]

We apply $0/1$ label coding.

For Multi-class:

![[Pasted image 20230227091115.png|500]]

We store into a $N \times C$ matrix, for each element it's either equal to $1$ or $0$ for $y_{ik}$ where $k$ is the class.

Different models give you different ways to formulate $p(c_k|x)$ 

Logistic Regression - linear classification model trained using cross-entropy loss.

Classification Losses based on Likelihood

One way to train the model is to maximise the likelihood (or log likelihood) function.

![[Pasted image 20230227094721.png|500]]

![[Pasted image 20230227095104.png|500]]

When we do binary classification, assume probability of the predicted class given some parameter it follows the Bernoulli distribution. (Very similar to Binomial Distribution Equation)

For multi-class classification we use categorical distribution - almost like flipping a dice.

![[Pasted image 20230227112528.png|500]]![[Pasted image 20230227113059.png|500]]

Example:
![[Pasted image 20230227113144.png|500]]

Yes because the softmax and sigmoid gives us an output that is between $0$ and $1$. And all outputs sum to $1$!

![[Pasted image 20230227114711.png|500]]

Example:

![[Pasted image 20230227114836.png|500]]

![[Pasted image 20230227115520.png|500]]

