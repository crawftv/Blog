---
description: Keeping multiple inheritance simple
---

# Mixins

{% hint style="info" %}
A link a notebook exploring some of the code featured in this post here. [https://colab.research.google.com/drive/1x-JVFigmxWXMqy9cEE8kXSIPYPOIR5wa?usp=sharing](https://colab.research.google.com/drive/1x-JVFigmxWXMqy9cEE8kXSIPYPOIR5wa?usp=sharing)
{% endhint %}

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

Lets examine how the scikit-learn library solves this problem. We start with the dummy models.   
Both classifiers and regressor will need fit, predict, "\_more\_tags", and  score methods.

{% embed url="https://github.com/scikit-learn/scikit-learn/blob/fd237278e895b42abe8d8d09105cbb82dc2cbba7/sklearn/dummy.py\#L23" %}

## The Solution: Mixins

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

The two dummy classes inherit from 3 classes but expose only 1 inherited function, `score`. Each of the 3 super classes have a `_more_tags` method. 

Mixins do not need to be classes in their own right. `MultiOutputMixin` has one method and `ClassifierMixin` and `RegressorMixin` have two. 

### Looking at What the DummyClassifier inherited from each SuperClass

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

Now lets look at the tags inherited from each SuperClass.

```python
from sklearn.dummy import DummyClassifier
x = DummyClassifier()._get_tags() 
y =BaseEstimator()._get_tags()
different_items = {k: x[k] for k in x if (k in y and x[k] != y[k]) or (k not in y)}
different_items
#output
{'_xfail_checks': {'check_methods_subset_invariance': 'fails for the predict method'},
 'multioutput': True,
 'no_validation': True,
 'poor_score': True,
 'requires_y': True}
```

## MRO and Inheriting the Same Thing From Different Mixins.

The tags are inherited from the `_more_tags` method and all the mixins have them. So how do all the tags make it into the class? With a little hack. From [https://github.com/scikit-learn/scikit-learn/blob/94a9f9a0b04703ae98d61a7e9a98a4dcaac548e8/sklearn/base.py\#L339](https://github.com/scikit-learn/scikit-learn/blob/94a9f9a0b04703ae98d61a7e9a98a4dcaac548e8/sklearn/base.py#L339)

```python
#sklearn/base.py
def _get_tags(self):
    collected_tags = {}
    for base_class in reversed(inspect.getmro(self.__class__)):
        if hasattr(base_class, '_more_tags'):
            # need the if because mixins might not have _more_tags
            # but might do redundant work in estimators
            # (i.e. calling more tags on BaseEstimator multiple times)
            more_tags = base_class._more_tags(self)
            collected_tags.update(more_tags)
    return collected_tags
```

What the code above does is go through each SuperClass that was passed. If the class has a `_more_tags` attribute the function adds it to the classes tags. The most important part is the `reversed(inspect.getmro(self.class))` . MRO means "Method Resolution Order". Priority is given left to right when making a class in Python. `inspect.getmro` returns a tuple `(sklearn.dummy.DummyClassifier, sklearn.base.MultiOutputMixin, sklearn.base.ClassifierMixin, sklearn.base.BaseEstimator, object)`

If we updated the tags and iterated through the list without reversing it, we overwrite the tags updated by the Mixins with the BaseEstimator values. Clearly the opposite of what we want.

### How would you solve this problem without the hack above?

You could try adding more mixins or adding attributes. But this would make the code more complex. More mixins would increase the number of little problems inheritance causes \(i.e. order\).   
Adding a separate attribute would make the code more verbose as well. 

## Conclusion

* Mixins are a nice addition to your Object-Oriented repertoire. They make it easy add one or two methods to your class.
* You can simplify inheritance with a little hack to limit code and inheritance 

