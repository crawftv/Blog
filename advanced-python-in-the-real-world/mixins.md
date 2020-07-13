---
description: Keeping multiple inheritance simple
---

# Mixins

> A mixin is a special kind of multiple inheritance. There are two main situations where mixins are used:
>
> 1. You want to provide a lot of optional features for a class.
> 2. You want to use one particular feature in a lot of different classes.
>
> [https://stackoverflow.com/a/547714/9132913](https://stackoverflow.com/a/547714/9132913)

## Problem

We are writing a machine learning library for creating models. There are two types of problems: regressions and classifications. 

Regressions return a single numerical output for each instance of a data. A common example is using features of a house to predict its price.   
Classifications make a prediction for a label for each instance of a data. A common example is predicting handwritten digits.

We need to make a lot of models so we should set up some sort of inheritance ensure consistency. 

Both classifiers and regressor will need fit, predict

{% embed url="https://github.com/scikit-learn/scikit-learn/blob/fd237278e895b42abe8d8d09105cbb82dc2cbba7/sklearn/dummy.py\#L23" %}

```python
class DummyClassifier(MultiOutputMixin, ClassifierMixin, BaseEstimator):
    def __init__(self, *, strategy="warn", random_state=None,
                 constant=None):
    def fit(self, X, y, sample_weight=None):
    def predict(self, X):
    def predict_proba(self, X):
    def predict_log_proba(self, X):
    def _more_tags(self):
    def score(self, X, y, sample_weight=None):
    
class DummyRegressor(MultiOutputMixin, RegressorMixin, BaseEstimator):
    def __init__(self, *, strategy="mean", constant=None, quantile=None):
    def fit(self, X, y, sample_weight=None):
    def predict(self, X, return_std=False):
    def _more_tags(self):
    def score(self, X, y, sample_weight=None):
```

