[[COMP24112]]

### What can Machine Learning do?

A machine learning system can be used to:

- Automate a process
- Automate decision making
- Extract knowledge from data
- Predict future event (for e.g. changes in stock prices)

Machine learning is not about writing code to explicitly do the above tasks, but machine learning writes code to make the computer learn from data how to do the above tasks (unclear?)

## Machine Learning in Artificial Intelligence

Machine learning plays a significant role in AI

Fields in AI where Machine Learning could be used:

- Speech recognition
- Speech synthesis
- Natural Language Processing 😩
- Text Mining
- Computer Vision
- Data mining, analysis, engineering
- Robotics

## Machine Learning in Data Science

Data is recorded on real-world phenomenon. The World is driven by data. (cringe)

Humans cannot manually handle data in such scale any more. A machine learning system can learn from data and offer insights. Obviously a machine would be far off better than a human understanding petabytes of data.

What could be done with such big data?

- Prediction: What can we predict about this phenomenon?
- Description: How can we describe/understand this phenomenon in a new way?

- Machine Learning History and Key Historical Events (skippable?)
    
    This is a young subject with pioneer research started around 1950s.
    
    [https://en.wikipedia.org/wiki/Timeline_of_machine_learning](https://en.wikipedia.org/wiki/Timeline_of_machine_learning)
    
    This is a timeline of machine learning, explaining major discoveries, achievements,
    
    milestones and other major events in the field.
    
    Here is another webpage talking about “A History of Machine Learning”.
    
    [https://cloud.withgoogle.com/build/data-analytics/explore-history-machine-learning/](https://cloud.withgoogle.com/build/data-analytics/explore-history-machine-learning/)
    
    ## Key Historical Events
    
    - 1940s, human reasoning / logic first studied as a formal subject within mathematics (Claude Shannon, Kurt Godel et al).
    - 1950s, the Turing Test is proposed: a test for true machine intelligence, expected to be passed by year 2000. Various game-playing programs built.
    - 1956, Dartmouth conference coins the phrase artificial intelligence.
    - 1959, Arthur Samuel wrote a program that learnt to play draughts (checkers if you are American).
    - 1960s, A.I. funding increased (mainly military). Famous quote: Within a generation ... the problem of creating 'artificial intelligence' will substantially be solved."
    - 1970s, A.I. winter. Funding dries up as people realise it is hard. Limited computing power and dead-end frameworks.
    - 1980s, revival through bio-inspired algorithms: neural networks, genetic algorithms. A.I. promises the world – lots of commercial investment – mostly fails. Rule based expert systems used in medical / legal professions.
    - 1990s, AI diverges into separate fields: Machine Learning, Computer Vision, Automated Reasoning, Planning systems, Natural Language processing... Machine Learning begins to overlap with statistics / probability theory.
    - 2000s, ML merging with statistics continues. Other subfields continue in parallel. First commercial-strength applications: Google, Amazon, computer games, route-finding, credit card fraud detection, etc... Tools adopted as standard by other fields e.g. biology.
    - 2010s, deep neural networks have led to significant performance improvement in speech recognition, reinforcement learning, image classification, machine translation, etc.. Yoshua Bengio, Geoffrey Hinton, and Yann LeCun 2018 ACM A.M. Turing Award for their contribution in deep neural network.

## Machine Learning Strategy

<aside>
💡 ‘Data + Model’ Strategy

</aside>

$$
Data \rightarrow Model: f(\theta, X)
$$

Where

$Data: X$ = Collection of experience E (what is E? E is for experience)

$Function  : f$ = Function output provides answers / solutions to Task T

$Parameters: \theta$ = Controls model behavior

$Objective \space function \space O(\theta)$ = Computes a performance P of task T

$Optimisation \space min \space O(\theta)$ = Learning (or training)

We build our data from the model. Create a function that takes the data as an input and returns answers to a target (task). We need to find the optimal $\theta$ that would help us solve a task

## What is Optimisation?

<aside>
💡 Optimization refers to the process of making something (such as a computer program or system) work more efficiently or effectively. In the context of computer programming, optimization usually means making a program run faster or use fewer resources (such as memory or power).

</aside>

- Basics on Optimisation (Blackboard)
    
    [https://learn-eu-central-1-prod-fleet01-xythos.content.blackboardcdn.com/5f0eeec577cec/20505218?X-Blackboard-S3-Bucket=learn-eu-central-1-prod-fleet01-xythos&X-Blackboard-Expiration=1674961200000&X-Blackboard-Signature=5nCgRAvMMN3Jh5R%2FHm0nqIqh8fV08pT%2Fjaiv4ggwGEE%3D&X-Blackboard-Client-Id=301771&X-Blackboard-S3-Region=eu-central-1&response-cache-control=private%2C%20max-age%3D21600&response-content-disposition=inline%3B%20filename%2A%3DUTF-8%27%27optimisation.pdf&response-content-type=application%2Fpdf&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaDGV1LWNlbnRyYWwtMSJHMEUCIC4k4qfHjA1Ae6Rey9XX8zUZT5zf9xqzi%2FH3nFemvsquAiEAwMcT5UAo%2F8zO7rgpH%2BfISnR7ZOuID0ua%2BFnIaeFYOhIqvAUIaBACGgw2MzU1Njc5MjQxODMiDDcETXKHnSSs4sASAyqZBfDHcLKhIAC2emGazh1u%2Fw%2BeOs3rW9dpiQEgbEVwHEMWwFeqetyYJa76Alqh0zOLoaqXdK3Rfpwb%2F4YbZC0hCsRyO8Lcc9JQt9lKaJilqnHdnyq9IjFxCuuTwnHHd0T9hoNYV9RLK75%2FhFTidSuVSLFS%2FrqFIPy2jHeHr2L4IsOokaxxMThrHOy7bO24bNJf4UwRZOrRKSfdPDlrfJGa0RrKhfPbKU%2BR2mIvmSrp9tP7teJjQXm7ISPTH1QQf3X9G0iSlKtdQEDR7gDctdG2wsOxUrZTH8u7km4PHdTMDMVqqmP9yunhbdNKlJPtPkt1GPOqXLnhfEO%2BMBgCocNs7o185kG2ZgK3KmVK8CVczkc94Wqczlrq2HwXbBg2f2OKTGpjalHTAGZxqOX2e%2B724BQHbGhPOby%2FXwCzg1QvSYUibKAEJp412Yr61AUCFAfqcHFtkl1D5QDuf%2B5M%2F2JVj%2FrhTdc8s43HEJ61yvkDugknT4Z979%2FDn%2BMnJ3d39ZFbMiHrr1W3ixeTwjeZ8%2BF9L8PFbdp95lg70W6e1rLiozzGjPRJhI0s1TRFOJa%2F4ebltNc%2B%2F19cuGVf2bDoABGefHJcxIplhMGdsoumfXDq2G8OUN0HBGTMn%2F2OcdG5T0wD%2FK2Eqhj%2FmwBFw1RvlLgzoM%2BHJchdZ6fg16tXuqR3qPM4850azq6j49J6MnI6ylXfpFdkSnELAONUTyLF4ylLjW9dfDhZ1%2FWGfBnYThtXmZz6avQOCi1STFEaesbtlxrUWa03Iwnst4KBlIYRLnSdTKVS4m8xCzE5B3PgMk%2FvedJeY%2B9%2F52G5KDhvTrEH%2BjHEMhZ%2B7lnhStOWJXydkg6Pl%2FSbRXCexPlBRZtHP3g8jy5TmxoNPFGrPZ4PMK3B1p4GOrEB2qew2bUlf%2FH4ZmF8Ht5Csj3do%2FCggYCQOSoaI8NtZxx4e1UxhgEZzLHpEn%2FQSKkpG2aTghcMhOext9%2BbXwJcnvN5o59GLZIxns4KXhedCeTg6bKJWyCK%2BIv4HiNqVBx2jNeJP06UlYAYj5JxVKUgAmHcEyiFad7uD2kDJ3e%2BNJmI9%2BpVz%2FJsXroO3HLqtwvExNrY8DQxfZgERF%2FLhkT7Edd1favVxtntflsby0D%2FIfmw&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20230128T210000Z&X-Amz-SignedHeaders=host&X-Amz-Expires=21600&X-Amz-Credential=ASIAZH6WM4PLXRW2OW6B%2F20230128%2Feu-central-1%2Fs3%2Faws4_request&X-Amz-Signature=24fe0f2f3199a910b166c8b1aeadd316fc67568090c2bfa48db3cdd3a6c676da](https://learn-eu-central-1-prod-fleet01-xythos.content.blackboardcdn.com/5f0eeec577cec/20505218?X-Blackboard-S3-Bucket=learn-eu-central-1-prod-fleet01-xythos&X-Blackboard-Expiration=1674961200000&X-Blackboard-Signature=5nCgRAvMMN3Jh5R%2FHm0nqIqh8fV08pT%2Fjaiv4ggwGEE%3D&X-Blackboard-Client-Id=301771&X-Blackboard-S3-Region=eu-central-1&response-cache-control=private%2C%20max-age%3D21600&response-content-disposition=inline%3B%20filename%2A%3DUTF-8%27%27optimisation.pdf&response-content-type=application%2Fpdf&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaDGV1LWNlbnRyYWwtMSJHMEUCIC4k4qfHjA1Ae6Rey9XX8zUZT5zf9xqzi%2FH3nFemvsquAiEAwMcT5UAo%2F8zO7rgpH%2BfISnR7ZOuID0ua%2BFnIaeFYOhIqvAUIaBACGgw2MzU1Njc5MjQxODMiDDcETXKHnSSs4sASAyqZBfDHcLKhIAC2emGazh1u%2Fw%2BeOs3rW9dpiQEgbEVwHEMWwFeqetyYJa76Alqh0zOLoaqXdK3Rfpwb%2F4YbZC0hCsRyO8Lcc9JQt9lKaJilqnHdnyq9IjFxCuuTwnHHd0T9hoNYV9RLK75%2FhFTidSuVSLFS%2FrqFIPy2jHeHr2L4IsOokaxxMThrHOy7bO24bNJf4UwRZOrRKSfdPDlrfJGa0RrKhfPbKU%2BR2mIvmSrp9tP7teJjQXm7ISPTH1QQf3X9G0iSlKtdQEDR7gDctdG2wsOxUrZTH8u7km4PHdTMDMVqqmP9yunhbdNKlJPtPkt1GPOqXLnhfEO%2BMBgCocNs7o185kG2ZgK3KmVK8CVczkc94Wqczlrq2HwXbBg2f2OKTGpjalHTAGZxqOX2e%2B724BQHbGhPOby%2FXwCzg1QvSYUibKAEJp412Yr61AUCFAfqcHFtkl1D5QDuf%2B5M%2F2JVj%2FrhTdc8s43HEJ61yvkDugknT4Z979%2FDn%2BMnJ3d39ZFbMiHrr1W3ixeTwjeZ8%2BF9L8PFbdp95lg70W6e1rLiozzGjPRJhI0s1TRFOJa%2F4ebltNc%2B%2F19cuGVf2bDoABGefHJcxIplhMGdsoumfXDq2G8OUN0HBGTMn%2F2OcdG5T0wD%2FK2Eqhj%2FmwBFw1RvlLgzoM%2BHJchdZ6fg16tXuqR3qPM4850azq6j49J6MnI6ylXfpFdkSnELAONUTyLF4ylLjW9dfDhZ1%2FWGfBnYThtXmZz6avQOCi1STFEaesbtlxrUWa03Iwnst4KBlIYRLnSdTKVS4m8xCzE5B3PgMk%2FvedJeY%2B9%2F52G5KDhvTrEH%2BjHEMhZ%2B7lnhStOWJXydkg6Pl%2FSbRXCexPlBRZtHP3g8jy5TmxoNPFGrPZ4PMK3B1p4GOrEB2qew2bUlf%2FH4ZmF8Ht5Csj3do%2FCggYCQOSoaI8NtZxx4e1UxhgEZzLHpEn%2FQSKkpG2aTghcMhOext9%2BbXwJcnvN5o59GLZIxns4KXhedCeTg6bKJWyCK%2BIv4HiNqVBx2jNeJP06UlYAYj5JxVKUgAmHcEyiFad7uD2kDJ3e%2BNJmI9%2BpVz%2FJsXroO3HLqtwvExNrY8DQxfZgERF%2FLhkT7Edd1favVxtntflsby0D%2FIfmw&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20230128T210000Z&X-Amz-SignedHeaders=host&X-Amz-Expires=21600&X-Amz-Credential=ASIAZH6WM4PLXRW2OW6B%2F20230128%2Feu-central-1%2Fs3%2Faws4_request&X-Amz-Signature=24fe0f2f3199a910b166c8b1aeadd316fc67568090c2bfa48db3cdd3a6c676da)
    

Mathematical optimisation finds the input that can given the minimum (or maximum) output value of a real-valued function

- Systematically choose the values of the function input from an allowed set
- Compute the output value of the function using the chosen input value

An example of optimisation could be using constraints to limit our input values (constraint optimisation)

### Machine Learning Pipeline

Key stages in building a machine learning system:

1. Model Construction
    - To prepare experience (E) in proper data format
    - To characterise a task (T) by a parametric model
    - To characterise a performance metric (P) by an objective function
2. Training
    - To determine the model through optimising the objective function
3. Evaluation
    - To assess the determined model using a performance metric

Machine learning research builds on optimisation theory, linear algebra, probability theory, etc..

## Example: Wine testing

![Untitled](Machine%20Learning%20Basics%207860e57d2ca7496991e7b403fa00bd8a/Untitled.png)

Task: To recognise the grape type of a given wine sample based on it’s measured chemical quantities

- Collecting wine samples for each grape type
- Characterising each wine sample with 13 chemical features (feature extraction)

![Untitled](Machine%20Learning%20Basics%207860e57d2ca7496991e7b403fa00bd8a/Untitled%201.png)

Design a mathematical model to predict the grape type. The model below is controlled by 14 parameters: $[w_0,w_1, w_2,..., w_{13}]$

![Untitled](Machine%20Learning%20Basics%207860e57d2ca7496991e7b403fa00bd8a/Untitled%202.png)

System training is the process of finding the best model parameters by minimising a loss function

![Untitled](Machine%20Learning%20Basics%207860e57d2ca7496991e7b403fa00bd8a/Untitled%203.png)

<aside>
💡 Loss function: A loss function, is a function that measures the difference between the predicted output of a model and the actual output. The goal of training a machine learning model is to minimize the value of the loss function, so that the predicted output of the model is as close as possible to the actual output.
In simpler terms, a loss function is a measure of how well a model is doing in predicting the output, and the goal is to minimize the loss.

</aside>

Finally, test an unseen bottle of wine

## Machine Learning Ingredients (Summary)

- Data (Experience)
- Model: A piece of code (model function) with some parameters that need to be optimised
- Loss function: The function you use to judge how well the parameters of the model are set. It is also called error function, cost function, objective function
- Training algorithm: The algorithm that optimises the model parameters, using the error function to judge how well a model is performing

Finally, the model with determined parameters is the thing you have to package up and send to a customer

# Machine Learning Basics II

## Supervised Learning

The machine learning task of learning a function that maps an input to an output based on example input-output pairs

- There is a teacher who provides a target output for each data pattern
- A pair of input data pattern and it’s target output is called a training example
- Typical supervised learning tasks include classification and regression, differing from their output type

## Unsupervised Learning

Unsupervised learning is a type of algorithm that learns patterns from untagged data

<aside>
💡 Untagged data, also known as unannotated data, is data that has not been labeled or categorized in any way. It is raw data that has not been processed or organized in any specific way.

</aside>

- There is no explicit ‘teacher’
- The systems form a natural ‘understanding’ of the hidden structure from unlabelled data

Typical unsupervised learning tasks include

- Clustering: Grouping similar data patterns together
- Generative Modelling: Estimate distribution of the observed data patterns
- Unsupervised representation learning: Remove noise, capture data statistics, capture inherent data structures

## Reinforcement Learning

Reinforcement learning is an area of machine learning concerned with how intelligent agents ought to take actions in an environment in order to maximize the notion of cumulative reward (what?)

- There is a teacher who provides feedback on the action of an agent, in terms of reward and punishment
- Helicopter Manoeuvre: Example reward can be following a desired trajectory; and an example punishment can be crashing

- Mini Quiz
    
    Guess which one is Supervised, Unsupervised, or Reinforcement
    
    1. Write a computer program to find out what topics a given set of news articles are talking about and display these topics to inform readers.
    2. Train a computer system to control a power station by rewarding for producing power and penalizing for exceeding safety thresholds.
    3. Map a sentence in English to its Chinese translation by learning from many translation examples.
    
    - Answer
        1. Unsupervised
        2. Reinforcement learning
        3. Supervised

## Classification and Regression

Classification and regression are both supervised learning tasks, but differ in output types

<aside>
💡 In simpler terms, classification is when the output is a category, while regression is when the output is a numerical value.

</aside>

### Classification (Class Output):

- Goal: Identify which categories a data pattern belongs to
- Training data: Observed data patterns and their category memberships (what are category memberships?)

### Regression (Continuous Output):

- Goal: Estimate the relationships among the input and output variables
- Training data: Observed input variables and their corresponding output variables

- Mini-quiz
    
    X: Input, Y: Output ( i think?)
    
    1. Medical diagnosis: x=patient data, y=positive/negative of some pathology
    2. Finance: x=current market conditions and other possible side information, y=tomorrow’s stock market price
    3. Image analysis: x=image pixel features, y=scene/objects contained in image
    4. Robotics: x=control signals sent to motors, y=the 3D location of a robot arm end effector
    - Answers
        1. Classification
        2. Regression
        3. Classification
        4. Regression

## Types of Classification

1. Binary Classification
    
    Classify into one of the two classes
    
2. Multi-class Classification
    
    Classify into one of the many classes
    
3. Multi-label Classification
    
    Classify into some of the many classes, there is no constraint on how many classes a sample can belong to
    
4. Structured Classification
    
    Classify into structured classes, classes can be organised in sequence, tree, lattices, graph
    

## Typical Machine Learning Tasks

![Untitled](Machine%20Learning%20Basics%207860e57d2ca7496991e7b403fa00bd8a/Untitled%204.png)

# Basics on Optimisation

[](https://learn-eu-central-1-prod-fleet01-xythos.content.blackboardcdn.com/5f0eeec577cec/20505218?X-Blackboard-S3-Bucket=learn-eu-central-1-prod-fleet01-xythos&X-Blackboard-Expiration=1674972000000&X-Blackboard-Signature=NLS66vwlruQpx7dRyVD53h%2BIBuMchBBQdTsT4GUXOw8%3D&X-Blackboard-Client-Id=301771&X-Blackboard-S3-Region=eu-central-1&response-cache-control=private%2C%20max-age%3D21600&response-content-disposition=inline%3B%20filename%2A%3DUTF-8%27%27optimisation.pdf&response-content-type=application%2Fpdf&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaDGV1LWNlbnRyYWwtMSJGMEQCIAGEJQdFEJ%2FJwQWQWG6AKIuvg97uLsb0DWRC%2BskalzgPAiAT%2FSMoBgC3QIwW0qwiGI4GW0wgv9Njvx1zhw%2BQgvqseyq%2BBQhqEAIaDDYzNTU2NzkyNDE4MyIMXECMtTuPoziMkjuOKpsFXhJuQE7P4c2jbxs7b8Lm0a4%2FlGrWEAjt3ZF31wNH5D%2FEWNMRSV9KtYEXiTxGPUjTfkH9M0ewyEfrQq7KunNZZ9Yvklm01Dj1IcfrWMkPI7uVrObFp6xn7ceQ8fhZ2KZLGY1DokwgyYrCdNZdd%2B%2BQIKkRzZWjCp8lmPPQpxQ1lJkBDN4ecaxh1wH1hD3USo3jJ8%2Fath12B7R6e9fgDetBSsjh%2FO92WHvuG3kY8GOKCAMOExu13Q9DizsG0dkL6B55qMAcZ5FWh%2FdzMhpyvyF%2FheLtf4knIP9LSgMcj2DveAji9szMH72%2B34J3uId3AAANQk9YQ5dELHDH5BhjTV%2F3G5UwpP0YcOkWF1WH1%2FR5NSEu4NUbSTqL%2B6ehF%2F7vru27Mex1ODHuI9515hDHMkVMqYU2fuN1Z3S1wQPYCPHJ3EfGiEhmDhlUzLI1qC%2BRG1jjiTrygP1N0app%2B61g7jdjinhgIVjbhyJT%2FTivRDPsAUepWpXvvvrp7nsBQmXhQumBPM31XMcWl%2FjAW%2BmB0juX0jAM6gdt15WsWaRxX43srisZQ3mDxo6RKpFiiSkPgTrOCdoMR%2BMo8r9uoxsXqYXGiqsWco6yXCaSrWG6CUFH8GleHTfBRWYoi%2BHjlnrsJyQudVM9TMdK5DEsNippFqZ3wYiBPTWvLwrQhgHvLRuuxpmhHlOa4ncOV8QuABYC1tcnTFEQJqOTeBTJFXZ8swUYoI%2BBqoD0VYtdeV%2F0fNhKzl9hu4KsoDytCr0MQbZ%2FZa%2FvghlyRmzkrCnGuAkHTLQfM0s8M%2F1d5lZRCrQCEuPgNYUBDJYfOk%2Bq6D1QQl9aipeU87CYvP%2BbgWHYiTHfzlYHa8BcwSpM6C8wf2e5NqS9k2c7nKl3LTGCHcvkajCG%2B9aeBjqyAQZo0ds4po7C%2Fm%2BXhbTo00jlEY3T%2FsYi2kxh6T39uWyqdZeO1Ytjo4Dv5bbEtEqFFMNUMkCe%2BAys8qVoA42x4eRGkodLqFvC1%2FAIIMTc98m%2Fb78kb5PtmvZUyh96VfWAcoM5TZWnJfcfqxtE7VHEsNvxNulTQqBSawih0NHA7pNmTTHnVWU3eDDVan4SY2PXZoOW9J3JmOovEa9g0eAnTnCJqeHs1KpSxd2gc97Snf%2FZw8s%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20230129T000000Z&X-Amz-SignedHeaders=host&X-Amz-Expires=21600&X-Amz-Credential=ASIAZH6WM4PLQJXKHIT3%2F20230129%2Feu-central-1%2Fs3%2Faws4_request&X-Amz-Signature=bfbb462bc013fec64afa54822165a8444804d5110a1d462b1429a7614697f7c2)