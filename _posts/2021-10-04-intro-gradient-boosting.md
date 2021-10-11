---
title: 'Machine Learning Notes: Introduction to gradient boosting'
date: 2021-10-03
permalink: /posts/2021/10/ml_gradient_boosting/
tags:
  - machine-learning
  - gradient-boosting
---

This post introduces gradient boosting. A nice introduction can be found at <a href="https://machinelearningmastery.com/gentle-introduction-gradient-boosting-algorithm-machine-learning/">A Gentle Introduction to the Gradient Boosting Algorithm for Machine Learning</a>.


Introduction to Gradient boosting
======

According to Wikipedia, _gradient boosting is a machine learning technique for regression, classification and other tasks, which produces a prediction model in the form of an ensemble of weak prediction models, typically decision trees.[...]  It builds the model in a stage-wise fashion like other boosting methods do, and it generalizes them by allowing optimization of an arbitrary differentiable loss function._ [1]. 

So, to start with, let's see what boosting is?

What is boosting?
------

Any machine learning model that is slightly better than chance is called a weak learner e.g. logistic regression model whose predictions are a bit better than 50%. 
In contrast, a strong learner is any machine learning algorithm that can be tuned to achieve arbitrarily good performance.

Boosting, is a methodology whereby we convert an ensemble of weak learners into a strong learner. 
Thus, boosting is not a specific learning algorithm. Instead, it is a concept that can be applied to a set of models. 

Now that we have an idea of what boosting is, let's see how boosting works.

How boosting works?
------ 

Boosting works iteratively. A collection of weak learners is trained on the training set. Predictions by each weak learner are used for the
overall model prediction. However, each weak learner's prediction is weighted according to its performance.

References
======
1. <a href="https://en.wikipedia.org/wiki/Gradient_boosting">Gradient boosting</a>
