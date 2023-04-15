[[COMP24112]]

# Machine Learning Models I

<aside>
💡 Recall the “Data + Model” Strategy

[           Machine Learning Basics](https://www.notion.so/Machine-Learning-Basics-7860e57d2ca7496991e7b403fa00bd8a)

### Data + Model Strategy

$$
Data(X) \rightarrow Model: f(\theta, X)
$$

Where

$Data: X$ = Collection of experience E (E is for experience)

$Function  : f$ = Function output provides answers / solutions to Task T

$Parameters: \theta$ = Controls model behaviour

$Objective \space function \space O(\theta)$ = Computes a performance P of task T

$Optimisation \space min \space O(\theta)$ = Learning (or training)

> $f$  is also called the prediction function, target function, or just a model
> 

We build our data from the model. Create a function that takes the data as an input and returns answers to a target (task). We need to find the optimal $\theta$ that would help us solve a task

</aside>

In $Model:f(\theta,X)$, we can assume that we have found the best $\theta$ (the best model behaviour, idealistic scenario). So our model can be simplified to $Model:f(X)$. So $X$ gives the input and $f(X)$  returns the output / solution

## Classification: Discrimination Function

**Approach I**: Find a mapping function that maps an input to a class

This mapping function is called a discrimination function

Basically, you give an input and the function returns a class

![Input (image 1) gives us a class ‘dog’. Similarly for (image 2) it gives us a class ‘cat’](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled.png)

Input (image 1) gives us a class ‘dog’. Similarly for (image 2) it gives us a class ‘cat’

### Discriminant Function by Thresholding

We do this by **binary classification**

1. Construct a real-valued function $f(X)$
2. Convert the output to a class prediction by thresholding 
    
    ![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%201.png)
    
    So return class A if the function returns a value greater than and equals to set value $t$, otherwise return class B
    
3. The real number $t$ can be treated as a hyper-parameter
4. A common setting for $t$ is $t = 0$ (or 0.5)

The set of input variables that give $f(X) = t$  is called a level set of $f$

Following is a curve (contour line) in 2D input space

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%202.png)

Check at the key to the right of the graph, green lines represent when the function would be equals to zero, blue when it’s less than 0 and yellow when it’s greater than 0

### Classification by Probabilistic Inference

In Probabilistic Inference, we do not use $f(X)$, instead we make use of probabilities (conditional probability?)

1. Model the posterior class probability $p(class \ k \ |\ x)$

1. Find the most likely class: 

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%203.png)

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%204.png)

> You put the image into each class that returns you a value. As seen, the image gives us a higher value in ‘dog’ class than ‘wolf’ and ‘cat’, it would be safe for us to assume the image 1 belongs (most likely) to ‘dog’ class based on our prediction output
> 

**Model Posterior Class Probability**

Model the posterior class probability $p(class \ k \ |\ x)$

- Approach II: Directly construct the posterior function ([Logistical Regression](https://www.notion.so/Logistic-Regression-37cd625441e54d30ae184e945d0b9d4b))
- Approach III: Construct the likelihood and prior functions, then compute the posterior function following Bayes’ theorem: ([Naïve Bayes’ Classification](https://www.notion.so/Na-ve-Bayes-Classification-52364d9fe61447d2b2f5138872bb9424))
    
    $$
    p(class \ k \ | \ x) = \frac {p(x \ | \ class \ k)\ p(class \ k)}{p(x)}
    $$
    
    Where:
    
    ![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%205.png)
    

## Non-probabilistic Regression

- Approach I: Find a mapping function to map the input to the output value

In approach I, we simply build our prediction function $f$ that takes an input and return an output $y$

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%206.png)

### Regression by Probabilistic Inference

1. Model the posterior output probability $p(f|x)$. Model a probability distribution, what is the change of getting $f$, given a specific input, $x$. The model would then return us a $p(f|x)$. Then we can utilise that distribution to calculate the expectation conditional mean and use it as a predicted output $(y)$- (check the formula below for conditional mean) and then build classification using the two approaches below
2. Estimate the output by the conditional mean: 

$$
y = E_f [f|x]
$$

- Approach II: Directly model the posterior $p(f|x)$
- Approach III: Construct the likelihood and prior, then compute the posterior by using the Bayes’ Theorem:
    
    $$
    p(f \ | \ x) = \frac {p(x \ | \ f )\ p(f)}{p(x)}
    $$
    
    ![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%207.png)
    

## Example

### Approach I

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%208.png)

Red-line: Real relationship between input and output. Non probabilistic approach would give us a point at $f(x_0)$

### Approach II and III

This approach does not return directly what the output value is, but returns a basic distribution of the possible values of output 

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%209.png)

> And then we do this for multiple points …
> 

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2010.png)

So everytime we have a new point, your estimate is a distribution along the relationship line between input and output. The predicted output value is the conditional mean of this distribution

## Summary (Part 1)

For both classification and regression case, we have known probabilistic model that can return you an exact particular output value.

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2011.png)

And when you use probabilistic model, it returns an output with quantified uncertainty

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2012.png)

# Machine Learning Models II

$Data(X) \rightarrow Model: f(\theta, X)$

Parametric Model: A model with parameters to be optimised

Non-parametric Model: A model that does not have any parameter to optimise, e.g. k-NN

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2013.png)

## Linear Model

### Functions and Notations

- Single-output function:
    - There is only one output variable
    - There can be multiple input variables
        
        ![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2014.png)
        
        We call $x_1,...,x_d$ **the $d$ **input features, and
        they represent a data point in a $d$-
        dimensional space.
        
- Multi-output function:
    - There are multiple output variables
    - There can be multiple input variables
        
        ![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2015.png)
        

By storing numbers in a vector, we have:

$$
x = [x_1, x_2, ..., x_d], y = [y_1, y_2,..., y_c]
$$

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2016.png)

**Linear and Quadratic Functions (unga bunga fr):**

[](https://learn-eu-central-1-prod-fleet01-xythos.content.blackboardcdn.com/5f0eeec577cec/14331235?X-Blackboard-S3-Bucket=learn-eu-central-1-prod-fleet01-xythos&X-Blackboard-Expiration=1677013200000&X-Blackboard-Signature=OKHvRoQkGnnwPLxscii7gI9Y0CJox%2FhUTnDfFe2SWNM%3D&X-Blackboard-Client-Id=301771&X-Blackboard-S3-Region=eu-central-1&response-cache-control=private%2C%20max-age%3D21600&response-content-disposition=inline%3B%20filename%2A%3DUTF-8%27%27linear%2520and%2520quadratic%2520functions.pdf&response-content-type=application%2Fpdf&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaDGV1LWNlbnRyYWwtMSJHMEUCICqXcLwP84m0Kp894zZ2mK71CPcqAy9Ct46ER13%2BG9sOAiEAi9nykidKa0O0xF7YFSVOsrSTWD%2FDOXSYKF0rYH2b2jEq3wQIwP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARACGgw2MzU1Njc5MjQxODMiDA5ZXvGSy730%2FV3JaCqzBL7eTUvoJ8DLklRZR640c459gYB9mcHbvKdUQCl02uU0x0kV1CRSdaCJZCa84rIic5qLCKTq2gFoEWNCCcQAs7xUMg6B53q%2FG6v4nofe%2BHiFANkKOJGPIguW4e54pVMh8IgZSz0NMjaejj7W%2BzIv4hcYpk6PbMHdIA1qo9b3EK8%2F840Z8dCLTiUvy3099B9eQ8PuklTMSgXnixge10u9Dr10CF9Z%2Bys2el2M2FYxFxZgXE2xpktz8%2F6EIVsLcCtYIhkULF4N81Wza%2BT0pP4XCqA3yqd4lhXHFw6WcMZLm1F4Sehq%2FjsKtLodxzg2Zmsd1OLqaBdmHZeB4%2BhZGFRvAkjcxiIZKhSmc87V5Pf7ZofCiiVXXcS4uHiUpioyJAdoZWsLXFwBO2%2FfuWEI38Wg9RklT4FDen1aEqHWbMgLTbHmz7b5OnkHyq%2BDqYsetjV4UVRodEEow6Z2dh7qtyKVIXOz8hMrXU0U4Cz945%2BCgydJXiyw%2BpoFiUzjiOcI%2BQnxAK0fbfvEJq2%2BGKVFX3Smt6Ay%2BVqMfvG8a7iwgOqqHAt4d1CTFZqKzpwxMAW7fITHEubmih1vHDBAKOKGIN%2FjYtzMc141TG19xWSBxUd5EaTZW5oer%2BG5MJGCcHbf5UNESutEAVMLjraUAc5pSmQgyusa2SSQwL%2BW8gIIT5ttDK4YtAqjJd5b4bOr6DSO6b5uJchPilRWy4GF60yewJ%2BCm0Fwg5WLNzq66aLEY1stq775Zp0OMMe4058GOqkB9xStHMd5tGz6l3N8cE9Qobplm9z9G1kA3ERMDOl3ftQB4NW2i0aLe0bkbD%2Fc7iwwtoxSQ%2FsAneEU3Bgc96qMDWJrJinz7F0r7QTIDKJX9wwkfwphptf0Bt4iyaji0ksiy%2BEJaV0sW3lAt81mhHVgcsMfHcmEijZUP1ORgteXl3DqjTovOE6U4MU6D4Vwq7G9Tn9m2Zdd8yAKX9IUH3G4MNsPEZ8fM2HEkA%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20230221T150000Z&X-Amz-SignedHeaders=host&X-Amz-Expires=21600&X-Amz-Credential=ASIAZH6WM4PL3ZU4A64C%2F20230221%2Feu-central-1%2Fs3%2Faws4_request&X-Amz-Signature=e7cd617d18ba70e7167e5b32f650a95231cb63c70099784681e9ae1a32793416)

### Using Linear Functions to construct a parametric model

$$
f(x) = w_0 + w_1x_2 + w_2x_2+...+w_dx_d
$$

^ this is a linear function

## Linear Model

### Single Output

We assume that the predicted output is a weighted linear combination of input variables $w_0 + w_1x_2 + w_2x_2+...+w_dx_d$

Where:

$w_0$: Bias Parameter

Training is to find the optimal setting of the coefficients (or called weights):

$w = [w_0, w_1, w_2, ..., w_d]$ (vector form)

Introduce expanded feature vector $x = [1, x_1,x_2,x_3,...x_d]$, we have the matrix representation of the model:

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2017.png)

### Store the Data by Vectors/Matrices

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2018.png)

### Multi Output

Assume each output variable is a linear combination of the input variable

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2019.png)

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2020.png)

## Linear Model for Classification

### Using Discriminant Function

**Binary Classification:** Construct a single-output linear model, then construct the discriminant function by thresholding (t = 0)

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2021.png)

$f$ is a linear function

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2022.png)

If output of function $f$ is greater than 0, then it classifies as class 1, else if it is less than 0, then classifies as class -1

**Multi-Class Classification:** Construct a multi-output linear model, then construct the discriminant function for each class

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2023.png)

We calculate a real value (number) for predicting each output and then we apply a threshold value of 0. If it is greater than the threshold value (0), we say it is from claas $i$, if less than 0 then we say it’s not from class $i$

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2024.png)

### Separating Hyperplane

Setting a linear function to 0 results in a **hyperplane**

In 2D space, $w_1x_1+w_2x_2+2_0 = 0$ is a straight line

In 3D space, $w_1x_1+ w_2x_2+w_3x_3+w_0= 0$  is a plane

### Setting a linear function to 0 (2D space)

Setting a linear function to 0 results in a **hyperplane**

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2025.png)

The hyperplane partitions the space into two parts

![Greater than 0](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2026.png)

Greater than 0

![Smaller than 0](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2027.png)

Smaller than 0

### Setting a linear function to 0 (3D space)

Setting a linear function to zero results in a hyperplane that partitions the space into two **parts**

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2028.png)

### Example I

A linear model in a 2-Dimensional feature space

This creates a separating line f = 0 to divide data points in the space into two groups

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2029.png)

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2030.png)

The black line (corresponding to $f$ = 0) is called the classification boundary, separating boundary, or decision boundary

### Example II

A linear model in a 2-dimensional feature space

It creates a separating line $f$ = 0 to divide data points in the space into two groups

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2031.png)

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2032.png)

Training data would decide what’s the best way to split the space in the graphs

### Classification by Probabilistic Inference

Another way of doing classification using linear function, using a special type of function, either **logistic sigmoid (binary classification)** or **softmax function (multi-class function)** to compute the class posterior from a linear model

<aside>
💡 Class posterior: Refers to the probability of a particular class or label given the observed features or data. It is calculated using Bayes’ theorem. It is written as:

$P(y|x) = \frac{P(x|y)P(y)}{P(x)}$
Where $P(y|x)$  is probability of class y given the input data x
$P(x|y)$ is probability of observing the data x given the class y, 

$P(y)$ is the prior probability of the class y

$P(x)$ is the marginal probability of data x

</aside>

### Logistic Regression

![Note that the exponent to $e$ which is a linear model, can be changed to a neural network](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2033.png)

Note that the exponent to $e$ which is a linear model, can be changed to a neural network

**Binary Classification:** 

- Use a single-output linear model

$f = w_1x_1+w_2x_2+w_0 = w^Tx$

Using the **logistic sigmoid function** to convert a real-valued number to probability

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2034.png)

^ The equation on the graph is the logistic sigmoid function. It takes one input and gives one output. So it would convert all the real value of the number to a number between 0 and 1.

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2035.png)

We can use the converted number to the probability of the class. We plug in $f$ to the logistic regression in the two equations above. So probability that $x$ belonging to one class is gonna be $p(c_1|x)$ and for the other class it would be $1 - p(c_1|x)$ 

**Multi-class Classification:**

1. Use a multi-output linear model

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2036.png)

1. Use the **softmax function** to convert outputs to probabilities

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2037.png)

### **Neural Network:**

A neural network is a type of machine learning model that is inspired by the structure and function of the human brain. It is a collection of interconnected processing nodes, called neurons, that work together to process and analyse complex data. Neural networks are typically used for tasks such as pattern recognition, classification, regression, and image and speech processing.

The basic building block of a neural network is the neuron, which receives input from other neurons or from the input data and processes that input to produce an output signal. The output of a neuron is usually passed on to other neurons in the network, forming a complex network of interconnected processing units. Neural networks typically consist of layers of neurons, with each layer performing a specific type of computation. The input layer receives the initial data, and the output layer produces the final output of the network.

The connections between neurons in a neural network have weights that determine the strength of the connection. During training, the weights are adjusted to minimise the error between the predicted output of the network and the actual output. This process is known as back-propagation, and it allows the neural network to learn from the data and improve its predictions over time.

### Softmax Function

How a softmax function works:

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2038.png)

You give values to it to input, first, it applies exponential functions to each real value to a non-negative number and then you divide each number each positive number with a sum of all these positive number to do a scaling

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2039.png)

### Example of a Softmax Function

![Highest probability is what you would choose](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2040.png)

Highest probability is what you would choose

## Linear Model for Regression

Focus on **single-output** case: 

$f = w_1x_1+w_2x_2+...+w_dx_d = w^Tx$

- **Non-probabilistic approach:** Use the output as the predicted value. In simpler terms, the output of the deterministic function is going to be out predicted value. No ifs and buts
- Probabilistic approach: Need to estimate $p(f|x)$
    - One option is to assume Gaussian distribution:
        
        $p(f|x) = N(w^Tx,\sigma^2)$
        
        Where the mean is the predicted output of this linear model ($w^Tx)$ and the variance is $\sigma^2$
        
    - This is equivalent to adding Gaussian noise with zero mean and variance $\sigma^2$ to the output of a linear model
        
        $y = w^Tx+\epsilon$
        
        where $\epsilon$ is called an error variable, which can be used as a hyper parameter
        

# Machine Learning Models III

![Linear Data Patterns](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2041.png)

Linear Data Patterns

But what if our data points can not just be split between a line? What if they look something like this?

![Non-Linear Data Patterns](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2042.png)

Non-Linear Data Patterns

Here, our data boundary would be within the green data points (the green X), which is not linear (non-linear separating boundary)

## Handle Non-Linear Data Pattern

Each data point is mapped to a new feature space 

Basically we want to create a function that would change the non linear patterns in space to linear patterns, to make them linearly separable

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2043.png)

Create a smart mapping function ($\phi$) which would map the data point into a new space. In the new space, the data sample would be characterized by capital ($D$)

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2044.png)

[Performing nonlinear classification via linear separation in higher dimensional space](https://www.youtube.com/watch?v=9NrALgHFwTo)

## Linear Basis Function Model

One way of handling non linear data patterns is through directly formulating many nonlinear functions $\{ \phi_i (x)  \}^D_{i=1}$. This extracts one new feature for each sample

These non linear functions are called basis functions. Each can be viewed as a feature extractor

Then, apply a linear model to the mapped features:

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2045.png)

### Basis Functions

There are many possible choices of basis functions

- Gaussian basis functions, where $\mu_{ij}$ and $\sigma_i$ are function parameters
    
    ![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2046.png)
    
- Polynomial basis functions where integers $n_{i1}, n_{i2},...n_{id}$ are function parameters
    
    ![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2047.png)
    
    These function parameters can be either treated as hyper parameters to select from candidate options or as model parameters to optimise
    
    <aside>
    💡 Hyper parameters are parameters that are set by a machine learning engineer before the learning process begins. These parameters cannot be learned directly from the data and must be set manually. They are used to control the behaviour of the learning algorithm and influence the quality of the learned model.
    
    Examples of hyperparameters in machine learning include the learning rate, regularization strength, number of hidden layers in a neural network, number of trees in a random forest, kernel function and regularization parameter in a support vector machine, and the number of neighbors in a k-nearest neighbors algorithm.
    
    Choosing the right hyper parameters for a machine learning model is important for achieving good performance. Hyper parameter tuning is the process of finding the best combination of hyper parameters for a given problem, which often involves trying out different values and evaluating their impact on the model's performance. This can be done manually or automatically using techniques such as grid search, random search, and Bayesian optimisation.
    
    </aside>
    
    ### Example I
    
    ![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2048.png)
    
    Apply your thresholding approach and if model output is greater than 0, assign it to class +1, if less than 0 then class -1
    
    ### Example II
    
    ![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2049.png)
    

## Kernel Method

In the original space, the inner product between two points $x_i$ and $x_j$ is

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2050.png)

In the new feature space, the inner product between two points $x_i$ and $x_j$ is 

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2051.png)

Kernel trick avoids to directly define the mapping function $\phi(x)$, but defines the inner product function in the new space using a kernel function:

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2052.png)

### Examples of Kernel Functions

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2053.png)

Many kernels give you a new feature space with infinitely many features

### Express a Function with Kernel

A linear function in the new feature space induced by a kernel:

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2054.png)

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2055.png)

Assume we are given a set of representative points, and those can be our training samples: 

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2056.png)

We can also express the weight vector as a linear combination of these representative points:

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2057.png)

Your prediction function then becomes (oops) :

![Untitled](Machine%20Learning%20Models%207b16869819a54e158af34376e4831aea/Untitled%2058.png)

Applying a polynomial kernel:

[SVM with polynomial kernel visualization](https://www.youtube.com/watch?v=3liCbRZPrZA)

Another space transformation demo using kernel:

[SVM Visualization](https://www.youtube.com/watch?v=ndNE8he7Nnk)


