---
comments: true
layout: post
title:  "Coding a Simple Logistic Regression in NumPy"
excerpt: Logistic Regression is a fundamental concept to deep learning. In this blog I'm going to code a logistic regression with gradient descent algorithm from scratch in NumPy.
date:   2018-12-19
---

How many times I got stuck in understanding weight dimensions and dot products and thought I'll code Simple Logistic regression in NumPy and go through the basics. But everytime my Laziness stopped me doing so. But not this time.

Logistic Regression is the one of the most fundamental concept of neural nets. In the 1950s decade there was huge interest among researchers to mimic human brain for artificial intelligence. At that time first Logistic Regression model was implemented with linear activation. It was trained with simple logistic loss function and worked well for linear data but failed substantially for non-linear one - like the very famous **XOR** gate problem. 

Then in around 1980s came the concept of **Gradient Descent** and **non-linear activation**. This gained immense popularity at that time as the model started working for non linear data as well. Then came Backpropagation, ConvNets, RNNs which are now supported by immense computaion power and here we are with such explosion in Deep learning field. But it all started with **Logistic Regression**. 

Logistic Regression is basically used for binary classification. It has two parts - **forward pass** and **backward pass**. 

### 1. Forward Pass: 
In the forward step you feed in multiple inputs, multiply it with corresponding weight vectors, add a bias vector and pass it through non-linear activation function (like *sigmoid*) and you'll get a probability between (0 - 1). The mathematical equation for the same is given as :

<code>out = sigmoid( w1*x1 + w2*x2 + ... + w(n)*x(n) + b )</code>


<code>out = sigmoid( W<sup>T</sup>.X + b)</code>

Here `x` is input data and `out` is the network output. Let's say we have a dataset `(x,y)` where `y`(correct label) correspnds to label for corresponding `x` and we get `out`(predicted label) from the network for the same x. Now we need to know how far is the predicted label from correct label. For that we define a loss function. In this case for keeping things simple let's take mean square loss defined as :

<code>L = 1/2(out-y)<sup>2</sup></code>


### 2. Backward Pass:
In the backward step we need to alter weights so that model starts predicting better than the last time. We know how the model performed on the last iteration by loss function, we just need to somehow encode this information and pass it backwards. We do this by means of `Gradient Descent`.
The loss function is parabolic as clear by the definition. So there exist a local minima for sure at which loss is minimum and model will perform the best. Gradients of any function tells the direction of steepest(maximum) increase. If we calculate the gradient of loss function and take negative of the value, that will give the direction of steepest decrease. But since gradients gives only direction, we multiply it by a constant `(learning rate)`  to get a value that signifies how far should we go in that direction. Add this value to the weights of the network to alter them so as to move one step further down the loss function. We do this recursively until we reach the local minima and that is precisely the **Gradient Descent Algorithm**.

For finding the gradient of loss function we take partail derivate of loss function with respect to weights of the network. 
```
∆L = (∂L(w1), ∂L(w2) ......), where ∂L(w1) = partial derivate of L with respect to w1
```

Let's differentiate loss with repect to w1 :
```
∂L(w1) = (out-y)*out*(1-out)*x1
```
Where the term `out*(1-out)` came because out is sigmoid activated function and derivative of sigmoid function is defined as :
```
∂(sigmoid(x)) = sigmoid(x)*(1-sigmoid(x))
```
In general :

<code>∂L(w<sub>i</sub>) = (out-y)*out*(1-out)*x<sub>i</sub></code>

So we alter weights as :

<code>w<sub>i</sub> = w<sub>i</sub> - learning_rate * ∂L(w<sub>i</sub>)</code>

Now with this much theoretical background we can't most surely code logistic regression and test on different data. Let's make a simple network for AND gate.

### **Implementation:**

The dataset for *AND* gate is :
```
xdata=np.array([[0,0],[0,1],[1,0],[1,1]])
ydata=np.array([[0],[0],[0],[1]])
```
Defining helper functions :
```
def sigmoid(x):
    return 1/(1+np.exp(-x))

def sigmoid_derivative(x):
    return x*(1-x)
```

Defining hyperparameters (parameter not trained by model but specified manually) :
```
input_neuron=2                      # number of inputs
output_neuron=1                     # number of outputs
etah=0.01                           # learning rate
batch_size=1                        # number of example we'll show the network at once
num_epoch=1000                      # how many times we'll loop over the data
```

Define weights and biases:
```
w=np.random.random(size=(input_neuron,))
b=np.random.random(size=(output_neuron,))
```

Main training code :
```
for epoch in range(num_epoch):
    for x,y in zip(xdata,ydata):
    
        out=sigmoid(x.dot(w)+b)                              # forward pass
        dw = (out-y)*sigmoid_derivative(out)                 # calculating derivative

        for i in range(len(w)):                              # backward pass(weights)
            w[i] = w[i] - etah*dw*x[i]                          
        for i in range(len(b)):                              # backward pass(bias)
            b[i] = b[i] - etah*dw                               
```
After running this code for `num_epoch` times, the model will prefectly learn the weights. 

```
print(w,b)
=> [2.64924114 2.6467143 ] [-4.09029157]
```
As you can see when x1 and x2 both will be 1, then only `x1*w1+x2*w2` will  be greater than bias and network will output 1 (hence AND gate).

You can find all the codes I used here, and in addition simple implementation for IRIS dataset as well on my [Github](https://github.com/ashukid/Neural_Networks_using_Numpy)