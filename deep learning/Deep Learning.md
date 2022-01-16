# Neural Networks and Deep Learning

## Week 1

### 1. Introduction to Deep Learning

- Supervised Learning
- Structured Data
- Unstructured Data

### 2. Logistic Regression as a Neural Network

#### 2.1 Binary Classification

#### 2.2 Logistic Regression

- outputs

$$
\hat{y} = \sigma(w^Tx+b)
$$
> 其中，$\sigma(z)=\frac{1}{1+e^{-z}}$

- loss function
  $$
  L(\hat{y},y) = -(y\log\hat{y}+(1-y)\log(1-\hat{y}))
  $$
  
- 

>The `loss function` computes the error for a single training example; the `cost function` is the average of the loss functions of the entire training set.

#### 2.3 Gradient Descent(Derivatives)

$J=J/m$

$d\bold{w_{1}}=d\bold{w_{1}}/m$

$d\bold{w_{2}}=d\bold{w_{2}}/m$

$db=db/m$

#### 2.4 Computation Graph

#### 2. 5 Vectorization

## Week 3

### 1. Setting Up your Goal

#### 1.1 Single Number Evaluation Metric

#### 1.2 Satisficing and Optimizing Metric

#### 1.3 Train/Dev/Test Distributions

Choose a dev set and test set to reflect data you expect to get in the future and consider important to do well on.

> Dev set and test set come from the same distribution.

#### 1.4 Size of the Dev and Test Sets

Set your test set to be big enough to give high confidence in the overall performance of you system.

#### 1.5 When to Change Dev/Test Sets and Metrics

### 2. Comparing to Human-level Performance

Improving Model Performance:

+ Human-level  -> Training error(Avoidable bias):
  + Train bigger model
  + Train longer/better optimization algorithms(momentum, RMSprop, Adam)
  + NN architecture/hyperparameters search(RNN, CNN)
+ Training error -> Dev error:
  + More data
  + Regularization(L2, dropout, data argumentation)
  + NN architecture/hyperparameters serach

### 3. Error Analysis

#### 3.1 Cleaning Up Incorrectly Labeled Data

+ Incorrectly labeled examples

#### 3.2 Mismatched Training and Dev/Test Set

+ Training and Testing on Different Distributions
+ Bias and Variance with Mismatched Data Distributions
+ Addressing Data Mismatch

#### 3.3 Learning from Multiple Tasks

+ Transfer Learning
+ Multi-task Learning

#### 3.4 End-to-end Deep Learning



