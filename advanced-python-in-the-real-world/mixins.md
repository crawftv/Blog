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

### Problem

We are writing a machine learning library for creating models. There are two types of problems: regressions and classifications. 

Regressions return a single numerical output for each instance of a data. A common example is using features of a house to predict its price.   
Classifications make a prediction for a label for each instance of a data. A common example is predicting handwritten digits.

We need to make a lot of models so we should set up some sort of inheritance ensure consistency. 

Lets examine how the scikit-learn library solves this problem. We start with the dummy models.   
Both classifiers and regressor will need fit, predict, "\_more\_tags", and  score methods.

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
         return {
            'poor_score': True, 'no_validation': True,
            '_xfail_checks': {
                'check_methods_subset_invariance':
                'fails for the predict method'
            }
        }
    def score(self, X, y, sample_weight=None):
        return super().score(X, y, sample_weight)
    
class DummyRegressor(MultiOutputMixin, RegressorMixin, BaseEstimator):
    def __init__(self, *, strategy="mean", constant=None, quantile=None):
    def fit(self, X, y, sample_weight=None):
    def predict(self, X, return_std=False):
    def _more_tags(self):
    def score(self, X, y, sample_weight=None):
        return super().score(X, y, sample_weight)
```

We see that both classes inherit from 3 other classes and share 2: MultiOutputMixin & BaseEstimator. 

Lets look at each parent class the dummy models have in common.

```python
class MultiOutputMixin:
    """Mixin to mark estimators that support multioutput."""
    def _more_tags(self):
        return {'multioutput': True}
class BaseEstimator:
    @classmethod
    def _get_param_names(cls):
    def get_params(self, deep=True):
    def set_params(self, **params):
    def _more_tags(self):
        return _DEFAULT_TAGS
    def __repr__(self, N_CHAR_MAX=700):
    # The BaseEstimator contains a lot of methods
    # They are all internal methods and none are
    # supposed to be exposed by the api.
```

Now lets look at the unique Mixins

```python
class ClassifierMixin:
    def score(self, X, y, sample_weight=None):
        from .metrics import accuracy_score
        return accuracy_score(y, self.predict(X), sample_weight=sample_weight)

    def _more_tags(self):
        return {'requires_y': True}

class RegressorMixin:
    def score(self, X, y, sample_weight=None):
        from .metrics import r2_score
        y_pred = self.predict(X)
        return r2_score(y, y_pred, sample_weight=sample_weight)

    def _more_tags(self):
        return {'requires_y': True}
```

### Summary of the Code and What We Can Learn

The two dummy classes inherit from 3 classes but expose only 1 inherited function, `score`. Each of the 3 super classes have a `_more_tags` method. 

Mixins do not need to be classes in their own right. `MultiOutputMixin` has one method and `ClassifierMixin` and `RegressorMixin` have two. 

#### Looking at What the DummyClassifier inherited from each SuperClass

| index | \_estimator\_type | classifier |
| :--- | :--- | :--- |
| 1 | `_get_param_names` | BaseEstimator.\_get\_param\_names |
| 2 | `_get_tags` | BaseEstimator.\_get\_tags |
| 3 | `_more_tags` | DummyClassifier.\_more\_tags |
| 4 | `fit` | DummyClassifier.fit |
| 5 | `get_params` | BaseEstimator.get\_params |
| 6 | `outputs_2d_` | object |
| 7 | `predict` | DummyClassifier.predict |
| 8 | `predict_log_proba` | DummyClassifier.predict\_log\_proba |
| 9 | `predict_proba` | DummyClassifier.predict\_proba |
| 10 | `score` | DummyClassifier.score |
| 11 | `set_params` | BaseEstimator.set\_params |

Interestingly, we don't see anything that is inherited from the Mixins. We  would expect the `score` method to be inherited, but we also know that the DummyClassifier uses the `super.score` method to create it for the class explicitly and not silently inherit. 

Order of the SuperClasses is important. If the Mixins did not come before the BaseEstimator, the tag overwriting function would not work properly and tags would be missing. 

{% hint style="info" %}
Professional software engineers are capable of mixing the order of these up to. At least one  SuperClass order bug was discovered in 2019\([https://github.com/scikit-learn/scikit-learn/pull/14884](https://github.com/scikit-learn/scikit-learn/pull/14884)\). Fixing these errors was still a work in progress in the middle of 2020\([https://github.com/scikit-learn/scikit-learn/pull/17847](https://github.com/scikit-learn/scikit-learn/pull/17847)\).
{% endhint %}

