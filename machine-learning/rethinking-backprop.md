---
description: and make your own neural net framework.
---

# Rethinking Backprop

### Why another guide?

* Most guides are bad. Take it from me, a guy who has tried them all. The code is unhelpful. Usually its a combination of poor math explanations and code that just barely works. 
* "example" neural nets are the only time anyone tries to use functional programming paradigms, despite trying to make everyone conceptualize the layers as objects.
* This guide does a better job of connecting the math to the code. 
* This code should more closely resemble the code found in top libraries and scales better than most examples.

**Hints for reading.**

This takes a long time to understand. It may takes a lot of rereading and drawing out notes to fully understand backprop.

Backward propagation of errors is easy to understand in the basic form of "it's just passing the error rate through all the layers." But a deeper understanding is much more difficult.  
$$\delta l = \sum ' (z^l)(w^{l+1})...\sum ' (z^{L-1})(w^{L})\sum ' (z^{L})\nabla_a C$$ and $$\delta_j^L = \frac{\partial C}{\partial a_k^L} \frac{\partial a_k^L}{\partial z_J^L}$$   
The above is not as difficult as it looks.

### Definitions

* Cost: this often means the sum of all the error for each prediction.

## Feed forward

Individual Layer

#### Definition

This is for one layer of the neural net. $$a^l = \sigma (w^l a^{l-1} +b^l)$$

#### Math Components

* $$a^l$$is the activation vector of the current layer
* z is the sum of multiplying the weights of the nodes times the input, and adding the bias 
* $$ z = \sum w^l \cdot a^{l-1} + b^l $$
* $$a^{l-1}$$ is the activation vector from the previous layer
* $$w^l$$ is the weight vector of the current layer. 
* $$b^l$$ is the bias vector of the layer
* $$\sigma(z) = \frac{1}{1+e^-z}$$ is the sigmoid function
  * The sigma function is used to compress the values into between 0 and 1. 
  * If the values passed to the next layer are too large\(positive or negative\) the net stops working properly.

#### Multiple Layers

This function can be thought of as recursive. For a net with one hidden neuron it looks like this. X is the input data. $$\begin{align} a^l & = \sigma (w^l a^{l-1} +b^l) \\ a^{l-1} & = \sigma (w^{l-1} a^{l-2} + b^{l-1}) \\ a^{l-2} & = \sigma (w^{l-2} X + b^{l-2}) \end{align} \\$$

Putting this all together it would like. $$a^l = \sigma (w^l \sigma (w^{l-1} \sigma (w^{l-2} X + b^{l-2}) + b^{l-1}) +b^l)$$

#### Code components

The code simplified

```python
class InputLayer:
    self.z = input_data
class HiddenLayer:
    def feedforward(self):
        self.S = np.dot(self.upper_layer.z, self.weights) + self.bias
        self.z = 1/(1+np.exp(-self.S))
class OutputLayer:
     def feedforward(self):
         self.S = np.dot(self.upper_layer.z,self.weights) + self.bias
         self.z = 1/(1+np.exp(-self.S))
```

```python
i = InputLayer(X)
l = HiddenLayer(upper_layer = i)
l2 = OutputLayer(upper_layer=l)
l.feedforward()
l2.feedforward()
```

#### Matching the code with the math

```python
self.S = np.dot(self.upper_layer.z, self.weights) + self.bias
self.z = 1/(1+np.exp(-self.S))
```

$$z = w^l \cdot a^{l-1} + b^l$$

## Backward Propagation of Errors

Backprop is just using the sum of all the error from the function to update the weights of the different layers.

#### Components

* $$\delta^L $$ is the output error, the true values minus the predicted values.
* $$\partial $$ designates the partial derivative of a function
* $$z$$ & $$a$$ are the outputs and the activations of those outputs.

#### Starting with the last layer and some simplified math.

$$\begin{align}\delta^L &= \nabla_a C ⊙ \sigma ' (z^L) \\ &= \frac{\partial C}{\partial z^L} \end{align}$$ 

Using the chain rule, this equation is reshaped as $$\delta^L =\frac{\partial C}{\partial a^L} \frac{\partial a^L}{\partial z^L}$$

We know what $$a^L$$ is, $$\sigma (z^L)$$, that is $$a^L$$ is the activation of $$z^L$$. So the derivative of the activation function function applied to the layer, with respect to the layer is:   
$$\delta^L = \frac{\partial C}{\delta a^L} \sigma '(z^L)$$

#### Connecting for all the layers.

The error for a hidden layer is: $$\delta^{L-1} = \frac {\partial C}{\partial z^{L-1}}$$ _\(there is more going on in here than even I can follow right now. But I take the mathematicians word.\)_  
Chain ruling it leaves:

 $$\begin{align} \delta^{L-1} &= \sum \frac{\partial C}{\partial z^L} \frac{\partial z^L}{z^{L-1}} \\ &= \sum \frac{\partial z^L}{\partial z^{L-1}} \delta^L \\ &= \sum w^L \delta^L \sigma ' (z^{L-1}) \end{align}$$

So each layer is connected to it's upper layer and the error rate is passed back that way.

#### Gradient Descent

#### Components

* $$\eta $$ is the learning rate
* $$\mu $$ is the number of samples going through the algorithm at the time, i.e. the size of the minibatch or the size of the of the data in the NAND Gate example.

#### Math

Starting with $$\delta^{L-1} = \sum w^L \delta^L \sigma ' (z^{L-1}) $$ as an example layer. The weights of each layer are updated by the learning rate times the error of the layer. $$w^L = w^L - \frac{\eta}{\mu} \sum w^L \delta^L \sigma ' (z^{L-1})$$

## Seeing Backprop with code

#### Notes

In code I use `harp` where $$\nabla$$ would be used in math

```text
class OutputLayer:
    def backprop(self,y):
        self.delta = (self.z - y) * self.activation_function_derivative(self.z)
        #uses harp becuase the word nabla weirds me out. nabla is the ∇
        self.harp_b = self.delta
        self.harp_w = self.upper_layer.z.T @ self.delta
        self.weights = self.weights + (self.learning_rate/self.delta.shape[0]) * self.harp_w
        self.bias = self.bias + (self.learning_rate/self.delta.shape[0]) * self.harp_b
```

#### Output layer error

$$ \delta^L = \nabla_a C ⊙ \sigma ' (z^L) $$  
`self.delta = (self.z - y) * self.activation_function_derivative(self.z)`

#### gradient of weights and biases

$$ \sum w^L \delta^L \sigma ' (z^{L-1}) $$  
`self.harp_w = self.upper_layer.z.T @ self.delta`  
`self.harp_b = self.delta`

#### Updating the weights

$$ w^L = w^L - \frac{\eta}{\mu} \sum w^L \delta^L \sigma ' (z^{L-1})$$  
`self.weights = self.weights + (self.learning_rate/self.delta.shape[0]) * self.harp_w`  
`self.delta.shape[0]` is hacky way of counting the number of samples that goes through the function.

{% embed url="https://gist.github.com/crawftv/12d093599b4d2bec94752312435dab42" caption="The full code for the neural net." %}

## Bibliography

[http://neuralnetworksanddeeplearning.com/chap2.html](http://neuralnetworksanddeeplearning.com/chap2.html)

