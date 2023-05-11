[[COMP24112]]

(Support Vector Machines)

In a 2D space we use a straight line to separate data and in a 3D space we use a plane to do the same. And when there are more than 3 Dimensions we call it a hyperplane.

You can calculate the distance between any arbitrary point in the space and the hyperplane.. $$r = \frac{\text{w}^T \text{x} + b}{\sqrt{\sum^d _{i=1} w^2 _i} } $$ where the equation of the hyperplane is: $$\text{w}^T\text{x} + b = 0$$ and $r$ is the distance from a point $\text{x}$, and where $r$ is negative or positive depends on which side of the hyperplane $\text{x}$ is on...

![[Pasted image 20230417085609.png|500]]

And so the distance from the origin to the line is :$$\frac{b}{||\text{w}||_2}$$ (We do not have $w^Tx$ here because from the origin that equals to 0 so we are just left with $b$)
 where $$||\text{w}||_2 = \sqrt{\sum^d _{i=1} w^2 _i}$$
$b$: Bias parameter

##### Hyperplanes
Now let's focus on 2 hyperplanes, say: $$\cases{\text{w}^T\text{x}+b = 1 \\ \text{w}^T\text{x} + b = -1}$$
Geometrically the distance between the two planes is $$\frac{2}{||\text{w}||_2}$$

![[Pasted image 20230417090128.png|500]]

We can very clearly use the above example to separate binary classes for a classification problem.

$$\begin{cases}\text{w}^T\text{x}+b \geq 1 &  \text{if } y = 1\\ \text{w}^T\text{x} + b \leq -1 & \text{if } y = -1 \end{cases}$$ where $y = 1$ is one class and $y = -1$ is another class

And the region bounded by these 2 hyperplanes is called the separation "margin" given by: $$p = \frac{2}{||\text{w}||_2}$$

![[Pasted image 20230417091639.png|500]]

So the two hyperplanes actually prevent training data points from falling into the margin...

So what is the core idea behind SVM - simply to find an optimal hyperplane to separate the two classes of data points with the ==widest margin==.

>[!note] We are after the widest separation margin!

So how do we actually find the hyperplane with the widest margin?

##### Hard-Margin SVM
- finds an optimal hyperplane to fully separate the two classes of data points with the widest margin, by solving the following constrained optimisation problem.

$$\text{min}_{\text{w},b}\ \ \  \frac{1}{2}\text{w}^T\text{w}$$ such that: $$y_i(\text{w}^T\text{x}_i + b) \geq 1 \ \ \  \ \forall i \in \{1,\ ...,\ N\}$$ ^25b9cf

Where did we get the first equation from, asking us to minimise [[#^25b9cf|this]] 

##### Support Vectors

Support vectors are training points that satisfy: $$y_i (\text{w}^T\text{x}_i + b) = 1$$ These points are the most difficult to classify and are very important for the location of the optimal hyperplane - as they decide the location of the optimal hyperplane - they *support* the location.

![[Pasted image 20230417110926.png|500]]

Let's go back to the Hard-Margin SVM - the process of solving the following constrained optimisation problem.

$$\text{min}_{\text{w},b}\ \ \  \frac{1}{2}\text{w}^T\text{w}$$ such that: $$y_i(\text{w}^T\text{x}_i + b) \geq 1 \ \ \  \ \forall i \in \{1,\ ...,\ N\}$$
The above problem is solved by solving a dual problem as shown below.

![[Pasted image 20230417111326.png|500]]

We do not need to know how to derive the dual problem! We just know that we have derived this problem and it's easier to solve the dual problem and once solved also solves the other problem. this problem is known as Quadratic Programming

![[Pasted image 20230417111739.png|500]]


So we can use a hard-margin SVM for linearly separable data patterns such as this one:

![[Pasted image 20230417112323.png|300]]

However, what if the data is non-separable?

![[Pasted image 20230417112348.png|300]]

hard-margin will struggle with this and so we use ==soft-margin==

##### Soft-Margin SVM
- We have slack variables $\varepsilon_i \geq 0$ where ($i = 1,\ 2,\ ...,\ N$) each of which measures the deviation of the i-th point from the ideal situation, 
- So we relax the hard margin SVM constraints: $$\begin{cases}\text{w}^T\text{x}+b \geq 1 - \varepsilon_i &  \text{if } y = 1 \\ \text{w}^T\text{x} + b \leq -1 + \varepsilon_i & \text{if } y = -1 \end{cases}$$
We don't push all the points to stay out of the margin anymore

![[Pasted image 20230417113510.png|600]]

In addition to maximising the margin - as before we also need to keep all slack variables $\varepsilon_i$ as small as possible to minimise the classification errors.

![[Pasted image 20230417114548.png|500]]
Note the difference here and the hard margin? We use a regularisation variable in soft margin whereas in hard-margin it was just $\lambda \ge 0$ (so lambda had to be positive)

But, there's another way to relax the hard margin, we can instead use hinge-loss:

![[Pasted image 20230417120504.png|500]]

So now we can modify our definition of a support vector, training points that satisfy: $$y_i(\text{w}^T\text{x}_i + b) \leq 1$$
These points either distribute along one of the two parallel hyperplanes, or fall within the margin or stay 

![[Pasted image 20230417121558.png|500]]

Can we handle non-linear data patterns? We can start with the Dual Problem and change some bits to apply a kernel function:

![[Pasted image 20230417123123.png|500]]

After solving the quadratic problem for SVM, we can compute the optimal values for the multipliers.

![[Pasted image 20230417123418.png|500]]

Linear uses the inner product and non-linear uses a kernel function.

![[Pasted image 20230417123622.png|500]]

What about regression?

![[Pasted image 20230417123853.png|500]]

The problem formulation is very similar and can be solved using a Quadratic Programming problem and can allow for non-linear solutions using kernels.


### ROC analysis
Evaluation method for classification.

Given a binary classification, we vary it's decision threshold.

![[Pasted image 20230417124230.png|300]]

- Your model can be a linear model, an artificial neural network or an Kernel SVM classifier.
- You can evaluate the classification performance without fixing $T$ but observing performance change over a varying value of $T$

Called ROC analysis.

- For each threshold value, we can collect a pair of measurements
	- (1 - specificity, sensitivity (basically recall))
	- or False Positive Rate and True Positive Rate
Sensitivity is recall (or how good your model is at identifying positive samples)
Specificity is (how good your model is at identifying negative samples)

- Receiver Operating Characteristics (ROC) curve is a graphical plot that illustrates pairs of the above collected measures as the decision threshold varies.
- 1 - specificity on the $x$-axis and sensitivity on the $y$-axis
- *or* FPR on the $x$ and TPR on the $y$

![[Pasted image 20230417124942.png|600]]

![[Pasted image 20230417125324.png|500]]

This is a comparison as to how good you are at separating positive and negative classes.

### Comparison of ML models

![[Pasted image 20230417125639.png|500]]

![[Pasted image 20230417125710.png|500]]![[Pasted image 20230417125726.png|500]]![[Pasted image 20230417125742.png|500]]![[Pasted image 20230417125754.png|500]]

### No free lunch theorem
- if we are interested in the generalisation performance, is there any reason to prefer one classifier or learning algo over another?
- If we make no prior assumptions about the nature of the classification task, can we expect any classification method to be superior?

No Lunch Theorem says the answer is no

![[Pasted image 20230417130157.png|700]]

