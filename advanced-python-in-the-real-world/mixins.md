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



