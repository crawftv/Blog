---
description: Getting Started with scikit-optimize and Gaussian Processes
---

# Parameter & HyperParameter Tuning with Bayesian Optimization

[**Link to repo with .ipynb notebook for this code**](https://github.com/crawftv/Skopt-hyperparameter-tutorial)**.**  
[**Link to a Google Colab notebook with this code.**](https://colab.research.google.com/drive/1tYXmorCchEtsB12830lo6AhG-4HRZz4q)

This guide will walk the reader through using two scikit-optimize functions to find the optimal model architecture and tune hyperparameters for a “vanilla” neural network. This article uses explores Gaussian Processes and Gradient Boosted Regression. Fortunately we don’t need a deep understanding of these. We just need to know that the search function adjusts based on how each change in the model performs.

Fortunately you don’t need to understand this equation to implement the Gaussian search.![](https://cdn-images-1.medium.com/max/800/1*7c46-ScyLgZq9m24QPl-iA.png)

As is tradition, this tutorial will use the MNIST handwritten digits data set. Let’s start with loading and transforming the data.

### **\(Making the baseline is optional, if you are familiar with this kind of work feel free to skip it and advance to the second part of the guide.\)**

{% embed url="https://gist.github.com/crawftv/dd01cdb4282f572b87f3e4e8ba3f9a44\#file-load\_mnist-py" caption="Code to load and transform MNIST data" %}

{% embed url="https://gist.github.com/crawftv/d99d269cc1e2ff2a74f1a98896958f0b\#file-baseline\_nn-py" caption="Code creating our baseline NN" %}

We get an accuracy of **92.83%** for our naive model.

## **Gaussian Process**

Now Lets get to the Fun Part, HyperParameter Tuning.

> \(If you followed along with the first part of the assignment, I found this part works best if you restart your kernel and skip the code for the baseline NN.\)

We start by importing functions from [sci-kit optimize](https://scikit-optimize.github.io/) and Keras.

{% embed url="https://gist.github.com/crawftv/683f599dd31d5ec02877d0734beccc4f\#file-skopt\_imports-py" %}

Creating our search parameters. “dim\_” short for dimension. Its just a way to label our parameters. We can search across nearly every parameter in a Keras model. This code focuses on:

* Number of Layers
* Number of Nodes per layer
* Learning Rate & Weight Decay for the Adam Optimizer
* activation functions
* batch size

The name feature allows us to use the decorator later. We must also establish default parameters.

{% embed url="https://gist.github.com/crawftv/d1ec2e6c92fa6fee83495cc5ed5bae47\#file-skopt\_named\_dimensions-py" %}

This function will create all the models that will be tested. Importing the Adam optimizer allows us to adjust its learning rate and decay.

{% embed url="https://gist.github.com/crawftv/b11186383af17c79f12dc137155b7e88\#file-skopt\_model\_creation-py" %}

This fitness function looks like a lot, but most of it is just the parameters. We use create\_model to create our model, fit the model, print the accuracy, and delete the model.

{% embed url="https://gist.github.com/crawftv/830d3e48a443df23eec1a249b23adb15\#file-skopt\_fitness-py" %}

 Running these two lines of code can solve a lot of the Tensorflow errors that seem impossible to read. They clear much of the information tensor flow has stored. **Run this code before every hyperparameter or anything that makes a new Keras/Tensorflow model.**

{% embed url="https://gist.github.com/crawftv/fe45de8105880cdab9989820c451ec79\#file-skopt-clear\_session-py" caption="Run this code if you get into trouble with Tensorflow" %}

Finally, we can start our search. This example changes many of the default parameters to give an idea of how much is modifiable. A later code snippet will use simpler parameters. \(And funny enough, your hyperparameter tuner will its own parameters tuned.\) Below is the function that performs the bayesian optimization by way of Gaussian Processes. n\_calls=12 because that is the smallest possible amount to get this function to run. You might need to increase this.

{% embed url="https://gist.github.com/crawftv/d33934ad164d7baf1487c8c59b0dab26\#file-skopt\_gp\_minimize-py" caption="Implementing Gaussian Process search" %}

 Lets see how our search performed. gp\_result.fun returns our best accuracy, which was 97.21%. **What was the range of our search?**

![DataFrame summarizing parameter search](.gitbook/assets/1_twkvx6qh2trnazao0picrg-1.png)

Lets see how our search performed. gp\_result.fun returns our best accuracy, which was 97.21%. **What was the range of our search?** dataframe summarizing parameter search

One last thing for our Gaussian Process model. We need to try it on the test data. So we use our create\_model function to recreate our best model. Then we retrain and evaluate. Our final test accuracy is… **98.09%**.

{% embed url="https://gist.github.com/crawftv/12d83d6b95e002708744a665bbdb2994\#file-skopt-gp-test-py" %}

## **Gradient Boosted Regression Trees**

{% embed url="https://gist.github.com/crawftv/a12f7f84be8f8e3900f681404032ce4a\#file-skopt\_gbrt\_mimize-py" %}

This function uses Gradient Boosting Regression Trees to find the ideal model parameters. Make sure to clear the session and graph before running this. We only need to change this function: replacing gp\_minimze with gbrt\_minimze.

The final result on the test results was **98.13%.**

### **Examining our final models**

We get two very different models but both have great results. Notice the difference in the number of trainable parameters.

### **Gaussian Process Model**

```text
_________________________________________________________________
Layer (type)                 Output Shape              Param #   
=================================================================
dense_3 (Dense)              (None, 7)                 5495      
_________________________________________________________________
layer_dense_1 (Dense)        (None, 4)                 32        
_________________________________________________________________
layer_dense_2 (Dense)        (None, 4)                 20        
_________________________________________________________________
layer_dense_3 (Dense)        (None, 4)                 20        
_________________________________________________________________
layer_dense_4 (Dense)        (None, 4)                 20        
_________________________________________________________________
dense_4 (Dense)              (None, 10)                50        
=================================================================
Total params: 5,637
Trainable params: 5,637
Non-trainable params: 0
_________________________________________________________________
```

### **Gradient Boosted Model**

```text
_________________________________________________________________
Layer (type)                 Output Shape              Param #   
=================================================================
dense_5 (Dense)              (None, 395)               310075    
_________________________________________________________________
layer_dense_1 (Dense)        (None, 16)                6336      
_________________________________________________________________
dense_6 (Dense)              (None, 10)                170       
=================================================================
Total params: 316,581
Trainable params: 316,581
Non-trainable params: 0
```

[Much thanks to this blog post for providing a framework for this code.](https://github.com/Hvass-Labs/TensorFlow-Tutorials/blob/master/19_Hyper-Parameters.ipynb)

\*\*\*\*

