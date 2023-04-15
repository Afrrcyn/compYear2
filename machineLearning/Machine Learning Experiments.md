>[!warning] Question for lecture
>We talk about, for k-fold CV, when we have a high number for $k$ it often leads to high variance, is this not the issue with Leave One Out then?

How cam we measure classification and regression performance using samples?

Classification Performance Measures:
- Classification Accuracy and Error
- Confusion Matrix
- Precision, Recall, $F_1$ Score and Specificity

Regression Performance Measures
- RMSE (Root Mean Squared Error)
- MAE
- MAPE
- $R^2$ score

### Classification Measures
- Samples:              $\{x_i,\ y_i\}^n _{i=1}$
- Feature Vector:   $x_i \in R^d$
- Class Label:         $y_i \in \{1,\ 2,\ \ldots,\ c\}$

##### Accuracy and Error
==Correct== Sample 1 is truly from Class A and assigned to Class A by a machine learning model

==Error== Sample 1 is truly from Class A but is assigned to Class B by a machine learning model.

$$\text{Accuracy} = \frac{\text{Number of correctly guesses samples}}{\text{Total sample number}}$$
$$\text{Error} = \frac{\text{Number of wrongly guesses samples}}{\text{Total sample number}}$$

> [!question]- Why can't we just use this measure?? Why do we need others? 
> 1. It''s unreliable for assessing unbalanced data
> 
> For Example:
> - Given 95 samples from Class A and 5 samples from Class B
> - A classifier assigns all the samples to class A
> - This have an accuracy of 95%, but it's not a good classifier, it just got lucky with the testing data...
> 
> 2. It may not tell classifier behaviours well
> 
> For Example:
> 
> ![[Pasted image 20230213080120.png|400]]
> - Both have the same accuracy: $60\%$ $\big(\frac{600}{1000} \big)$
> - But they do not behave the same:
> 	- Classifier 1 is better at classifying samples from class B
> 	- Classifier 2 is better at classifying samples from Class A

##### Confusion Matrix
- One type of confusion matrix is the ==error matrix==
- Each row represents samples in a predicted class, while each column represents samples in an actual class (or vice versa)

![[Pasted image 20230213080741.png|400]]

- Now what you can do is, make a a 2x2 confusion matrix for ==each== class.
- Each confusion matrix contains 2 rows and 2 columns.
	- Here we're dealing with false positives, false negatives, true positives and true negatives.

For example, from the table above, for cats:
True Positive: 5 cats
False Negative: 3 cats
False Positive:  2 dogs
True Negative: 17 non-cats  (3 + 1+ 2 + 11) - we don't care about whether they were predicted correctly or not, as long as they weren't predicted to the cat class. 

Using this information we can make a table:
![[Pasted image 20230213081239.png|500]]


##### Precision and Recall:
These assess capability of classifying positive samples.
$$\text{Precision} = \frac{\text{TP}}{\text{Number of Predicted Positives}} = \frac{\text{TP}}{\text{TP+FP}}$$
$$\text{Recall} = \frac{\text{TP}}{\text{Number of Real Positives}} = \frac{\text{TP}}{\text{TP+FN}}$$

>[!note] Recall can be called the TPR (True Positive Rate)

$F_1$ Score is the ==harmonic mean of precision and recall== 
$$F_1 = \frac{2 \times\text{Precision} \times \text{Recall}}{\text{Precision} + \text{Recall}}$$

##### Specifity
Assess the capability of classifying negative samples:
$$\text{Specificty} = \frac{\text{TN}}{\text{Number of Real Negatives}} = \frac{TN}{TN+FP}$$
$$\text{1- Specificty} = 1-\frac{\text{TN}}{\text{Number of Real Negatives}} = \frac{FP}{TN+FP}$$

>[!note] 1-Specificity is called the False Positive Rate (FPR)



### Regression Measures
- Samples:              $\{x_i,\ y_i\}^n _{i=1}$
- Feature Vector:   $x_i \in R^d$
- Class Label:         $y_i \in R^k$

>[!info] Regression Error
> The difference between the real (or observed or ground truth) and the predicted output

- Measure the difference between the real and predicted outputs
- Core Idea: Assess error for each of the $n$ samples and for each of the $m$ output values and average the errors.
- Commonly used regression performance metrics:

==Root Mean Square Error (RMSE)==
$$\text{RMSE} = \sqrt{\frac{1}{nm} \sum^n _{i=1} \sum^m _{j=1} (y_{ij} - \hat y_{ij})^2}$$ \[Look above if you don't know what $m$ and $n$ are...\]

If there are many samples with only one output?
$$\text{RMSE} = \sqrt{\frac{1}{n} \sum^n _{i=1} (y_{i} - \hat y_{i})^2}$$

==Mean Absolute Error (MAE)==
$$\text{MAE} = \frac{1}{nm} \sum^n _{i=1} \sum^m _{j=1} \big| y_{ij} - \hat y_{ij} \big|$$ and again, for samples with a single output?
$$\text{MAE} = \frac{1}{n} \sum^n _{i=1}  \big| y_{i} - \hat y_{i} \big|$$
MAE is very similar to RMSE, except it's not squaring but rather getting the absolute value.

==Mean Absolute Percentage Error (MAPE)==
$$\text{MAPE} = \frac{1}{nm}\sum^n _{i=1} \sum^m _{j =1} \frac{\big|y_{ij} - \hat y_{ij}\big|}{\big|y_{ij}\big|}$$
For a single output case: $$\text{MAPE} = \frac{1}{n}\sum^n _{i=1}  \frac{\big|y_{i} - \hat y_{i}\big|}{\big|y_{i}\big|}$$
==Coefficient of Determination== $(R^2)$
$$R^2 = 1 - \frac{\sum^n _{i=1} (y_i - \hat y_i)^2}{\sum^n _{i=1} (y_i - \bar y)} = 1 - \frac{\text{RMSE}^2}{\sum^n _{i=1} (y_i - \frac{1}{n} \sum^n _{j=1}y_j)}$$
>[!note] Please Note
>The second equation is just showing that the numerator is the $\text{RMSE}$ squared and it's showing that $\bar y$ from the denominator in the first equation is simply the mean of all the $y$ values

### Sample and True Error
- Hypothesis: Prediction made by a trained machine learning model
- ==Sample Error== of a hypothesis ($\text{error}_S$)
	- Error computed by a performance metric using a set of data samples.
- ==True error== of a hypothesis ($\text{error}_D$)
	- For a classification, it is the probability that a ==single sample is misclassified==, where the sample is randomly drawn from a distribution.
	- For regression case, it is the ==expectation of the error==:
 $$\text{Sample Error} = \sum^n _{i=1}(y_i - f(\bf x_i))^2$$ where $y_i$ is the expected.
$$\text{True Error} = E_{p(\bf x, y)} \big[ (y - f(\bf x))^2 \big]$$ Calculate the difference between the expected and calculated, and then calculate the expected from that.

>[!info] Note that:
>In most cases, it's very hard (or not possible) to compute the true error.
>We can always compute the sample error, and for infinite samples, the sample error converges to the true error. And thus insufficient samples may not approximate true errors well.

##### Limited Data
Limited data leads to ==bias== and ==variance== issues, this is due to the gap between the sample and true error.

Bias Issues
- Accuracy of the training samples can be poor estimator of the accuracy of unseen examples.
- It can provide optimistically biased estimate of the hypothesis over future unseen samples.
- To deal with bias, it's better to choose a new set of test examples independent of the training examples.

Variance Issues
- Accuracy of a new set of test samples can still vary from the true accuracy, depending on the makeup of a particular set of test samples..
- Smaller set of test samples can result in higher variance.

so the question remains: ==Given a set of finite samples, how do we train and evaluate a machine learning model?==

### Holdout Method:
- Split your dataset into a training and test set
	- Train your model using the training set (who knew?)
	- Estimate your model error using the test 
 
![[Pasted image 20230213114051.png|400]]

Drawbacks:
- If the dataset is small, we may not be able to set aside a portion of the dataset for testing.
- The holdout estimate of error rate can be misleading if we happen to get an unfortunate split leading to sample error != true error

### Random Subsampling
- Perform K data splits of the entire dataset
	- Each split randomly selects a fixed number of samples for testing and uses the remaining samples for training.
	- For each data split, the classifier is trained from scratch using the training samples and it's error rate is estimated with the testing samples (denoted by $E_i$ for the $i$-th split)

![[Pasted image 20230213114907.png|600]]
[Note that, each experiment refers to the same data, just randomly selects different (hopefully) testing data]

The Final Error Estimate is computed by: $$E = \frac{1}{k} \sum^k _{i=1} E_i$$
Because it's random, you may select bad random samples...

### K-fold Cross Validation
- Divide the entire dataset into $K$ partitions
	- for each of the K experiments, use ($k-1$) partitions for training and the remaining one for estimating th error rate $E_i$

![[Pasted image 20230213115452.png|500]]

The final error estimate is computed by: $$E = \frac{1}{k} \sum^k _{i=1} E_i$$
The advantage is that, all examples are used for both training and testing.

- Each sample is used a testing samples only once, but used as the training samples $k-1$ times.
- All the test sets are independent but here is an overlapping between training sets.
- Low number of $k$ results in insufficient training-testing trails.
- High number of $k$ results in small testing set with potentially high variance.

### Leave One Out
- Special case of $k$-fold CV
	- We have a total of N samples
	- LOO is an N-fold cross validation
Suitable for small sample set.

![[Pasted image 20230213120614.png|500]]


### Bootstrap
- Bootstrapping is based on sampling with replacement
- Repeat the process $k$ times:
	- Randomly select (with replacement) $M$ samples and use these for training
	- The remaining samples that were not selected are for testing, the number of testing samples can be changed over repeats.

![[Pasted image 20230213124632.png|500]]

The final error estimate is computed by: $$E = \frac{1}{k} \sum^k _{i=1} E_i$$
### Quick Review

A complete machine learning experiment includes:
1. Model Training
2. Model Evaluation
3. Model Selection: Select a best-model among different options (also known as hyper-parameter selection)
 - Do not train, assess and select hyper-parameter strategy.
- You need to split the data with an appropriate strategy, ... hold-out, k-fold CV, LOO, bootstrapping...

>[!example] Example: Hyper-parameter selection
>- Different hyper-parameter settings correspond to different model options
>- An example hyper-parameter selection strategy: Holdout for testing with random subsampling embedded for model selection:
>
>![[Pasted image 20230213131549.png|500]]

>[!example] Experiment: Handwriting Digit Recognition
>- We have a handwriting image, includes actual images and labels for the digits 0 to 9, with 500 digits per class.
>- The task is to predict what digit class an input image belongs to.
>
>![[Pasted image 20230213132245.png|350]]
>![[Pasted image 20230213132732.png|600]]


### Bias-Variance Decomposition
- Take regression as an example
- Expected squared error for a new test sample: $$E \big[ (y - f(\bf x))^2 \big]$$
- With some Calculation it has: ![[Pasted image 20230213133557.png|600]]
Bias Error: $$(y - E[f])^2$$
> Bias error measures how much the averaged prediction is close to the true value.

Variance Error: $$E\big[(f - E[f])^2\big]$$
> Variance error measures how much the prediction varies among different realisations of the model.

![[Pasted image 20230213134400.png]]

Overfitting often leads to  a low-bias error but high variance error, an over-complex model that is sensitive to small fluctuations in the training set.
Under-fitting: high bias error, low variance error, an excessively simple  model.

### Hypothesis
- refers to a prediction made by a trained machine learning model
- Often we talk about the context where a hypothesis refers to a model trained on a sample set. $$\text{Hypothesis A(T): the model A trained on sample set T}$$
For example:
- Hypothesis 1: A 5-NN classifier trained on sample set A
- Hypothesis 2: A 3-NN classifier trained on sample set B
- Hypothesis 3: A 6-NN classifier trained on sample set C


### Model Evaluation
- We want to know the True Error of a model $A$, so the Expectation of the true error of the hypothesis $A(T)$ with randomly drawn training set $T$ $$E_{r \subset D} \big\{error_D(A(T))\big\}$$
Hypothesis Evaluation: 
- So we train our model using training data $T$ and we estimate the true error using a new test data $E$  $$error_D(A(T))$$
Model Evaluation:
- Approximate the expectation by running multiple training testing trials and average the test error rates. $$E_{r \subset D} \big\{error_D(A(T))\big\}$$
- This motivates evaluation methods like random subsampling, k-fold CV, LOO, Bootstrap, based on multiple trials of training and testing.

### Model Comparison
- Often we are interested in evaluating and comparing ML models both their approaches and algorithms
	- For example, comparing 5-NN, 10-NN and 1-NN for classification.
- We can use the formula below to check the difference between performance of difference models: $$E_{r \subset D} \big\{ error_D(A(T)) - error_D(B(T))\big \}$$ We compare the two hypotheses first.


This raises a very important question, given a trained model we usually use a new set of samples to estimate it's performance. 
==Question==: How good of an estimate of the true error is provided by the sample error? We can answer this using the confidence interval.

### Confidence Interval
- You have computed the sample classification error, using a set of $n$ samples.
- Confidence interval tells you:
> with $p$ probability, the true error lies in the interval of: $$error_D \in [error_S - \alpha, error_S + \alpha]$$

We wish to have a ==small== $\alpha$ ==for a more precise estimate== and a ==larger $p$ for a high confidence==.

So, how do we compute $\alpha$ given the choice $p$

Very smart scientists have provided us with an equation :POGGIES: $$a = z_p \sqrt{\frac{error_S(1-error_S)}{n}}$$![[Pasted image 20230213185311.png|600]]
> With probability $p$, the true error lies in the interval of $$error_D \in [error_S - \alpha, error_S + \alpha]$$![[Pasted image 20230213185517.png|500]]

This confidence interval is an approximate, it works well for over 30 samples and with sample error not too close to 1 or 0.

### Comparing Two Hypotheses
***Classifier A:*** error rate computed using a set of $n_1$ samples, denoted by $error_{S1}(A)$
***Classifier B:*** error rate computed using a set of $n_2$ samples, denoted by $error_{S2}(B)$
***Fact***: $error_{S1}(A) - error_{S2}(B) > 0$

> Note they are trained on different sample sets.

>[!question] Question?
>Given that classifier A has a higher sample error than classifier B $$error_{S1}(A) - error_{S2}(B) > 0$$
>What is the probability C that classifier A has a higher true error than classifier B?


### Z-test
Given that classifier A has a higher sample error than classifier B, what is the probability C that classifier A has a higher true error than classifier B, we can use a Z-test for this.

1. Compute the quantity $z_p$ where $$\begin{align} z_p = \frac{d}{\sigma} \text{ where} \\ d = \big| error_{S1}(A) - error_{S2}(B) \big| \\ \sigma = \sqrt{\frac{error_{S1}(A)\big[1- error_{S1}(A)\big]}{n_1} + \frac{error_{S2}(B)\big[1-error_{S2}(B)\big]}{n_@}}\end{align}$$
2. Lookup the table below to get the confidence value p

![[Pasted image 20230213185311.png|600]]

3. Compute the final probability by: $$C = 1 - \frac{(1-p)}{2}$$
Z-Tests:
- It can only compare two hypotheses at a time
- Hypotheses can be tested on different sets of samples
- The approximation works well for test sets containing over 30 samples



