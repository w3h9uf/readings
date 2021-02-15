# Chapter 1

## Machine Learning Systems Categories

### Supervised/Unsupervised Learning

#### Supervised Learning

> In supervised learning, the training set you feed to the algorithm includes the desired solutions, called labels. 
A typical supervised learning task is classification.

Supervised learning algorithms:
- k-Nearest Neighbors
- Linear Regression
- Logistic Regression
- Support Vector Machines (SVMs)
- Decision Trees and Random Forests
- Neural Networks

Examples: _spam filter_

#### Unsupervised Learning

> In unsupervised learning, the training data is unlabeled. The system tries to learn without a teacher.

algorithms
- Clustering
  - K-Means
  - DBSCAN
  - Hierarchical Cluster Analysis (HCA)
- Anomaly detection and novelty detection
  - One-class SVM
  - Isolation Forest
- Visualization and dimensionality reduction
  - Principal Component Analysis (PCA)
  - Kernel PCA
  - Locally Linear Embedding (LLE)
  - t-Distributed Stochastic Neighbor Embedding (t-SNE)
- Association rule learning
  - Apriori
  - Eclat

> A related task is dimensionality reduction, in which the goal is to simplify the data without losing too much information. One way to do this is to merge several correlated features into one. For example, a car’s mileage may be strongly correlated with its age, so the dimensionality reduction algorithm will merge them into one feature that represents the car’s wear and tear. This is called feature extraction.

Examples: _clustering, visualization, anomaly detection, association rule learning_

#### Semisupervised Learning

> Since labeling data is usually time-consuming and costly, you will often have plenty of unlabeled instances, and few labeled instances. Some algorithms can deal with data that’s partially labeled. This is called semisupervised learning


#### Reinforcement Learning

> Reinforcement Learning is a very different beast. The learning system, called an agent in this context, can observe the environment, select and perform actions, and get rewards in return


### Batch and Online learning

#### Batch Learning
> In batch learning, the system is incapable of learning incrementally: it must be trained using all the available data. This will generally take a lot of time and computing resources, so it is typically done offline. First the system is trained, and then it is launched into production and runs without learning anymore; it just applies what it has learned. This is called offline learning.

#### Online Learning
> In online learning, you train the system incrementally by feeding it data instances sequentially, either individually or in small groups called mini-batches. Each learning step is fast and cheap, so the system can learn about new data on the fly, as it arrives

_learning rate_: how fast online learning systems adapt to changing data. High learning rate means rapidly adapt to new data but also quickly forget old data.

> A big challenge with online learning is that if bad data is fed to the system, the system’s performance will gradually decline. If it’s a live system, your clients will notice.


### Instance-Based/Model-Based Learning

#### Instance-Based Learning
> the system learns the examples by heart, then generalizes to new cases by using a similarity measure to compare them to the learned examples

#### Model-Based Learning
> Another way to generalize from a set of examples is to build a model of these examples and then use that model to make predictions. This is called model-based learning

> Model selection consists in choosing the type of model and fully specifying its architecture. Training a model means running an algorithm to find the model parameters that will make it best fit the training data (and hopefully make good predictions on new data).


A typical machine learning project will look like:
1. You studied the data.
2. You selected a model.
3. You trained it on the training data (i.e., the learning algorithm searched for the model parameter values that minimize a cost function).
4. Finally, you applied the model to make predictions on new cases (this is called inference), hoping that this model will generalize well.


## Machine Learning Challenges

- Insufficient quantity of training data
- Nonrepresentative training data
- Poor-Quality Data
- Irrelevant Features
- Overfitting in Training data
- Underfitting the training data

## Testing and Validating

_training set/test set_






