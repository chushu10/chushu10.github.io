---
layout: post
title:  "Notes for Week 4 & 5"
date:   2016-05-27
categories: machine-learning
---
这篇文章是我对[Coursera](coursera.org)上的课程[Machine Learning](https://www.coursera.org/learn/machine-learning/)
的总结，第四、五周有关[神经网络](https://en.wikipedia.org/wiki/Artificial_neural_network)的内容。该课程由著名的机器学习专家[Andrew Ng](http://www.andrewng.org/)授课，是非常适合入门机器学习的课程。

# Notes for Week 4 & 5

@(读书笔记)[Machine Learning]

## Neural Networks Model Representation

Representation of a single neuron:

![neuron](https://raw.githubusercontent.com/Choosue/Choosue.github.io/master/img/notes-for-week-4-neuron.png)

In neural networks:

- $ g(z) = \frac{1}{1 + e^{-z}} $ is also called the activation function.
- $ \theta $ is also called the "weights" of the model.
- $ a_i^{(j)} $ = "activation" of unit $ i $ in layer $ j $.
- $ \Theta^{(j)} $ = matrix of weights controlling function mapping from layer $ j $ to layer $ j + 1 $.

Representation of neural network:

![neural network](https://raw.githubusercontent.com/Choosue/Choosue.github.io/master/img/notes-for-week-4-neural-network.png)

where, 
- $ a_1^{(2)} = g(\Theta_{10}^{(1)}x_0 + \Theta_{11}^{(1)}x_1 + \Theta_{12}^{(1)}x_2 + \Theta_{13}^{(1)}x_3) $
- $ a_2^{(2)} = g(\Theta_{20}^{(1)}x_0 + \Theta_{21}^{(1)}x_1 + \Theta_{22}^{(1)}x_2 + \Theta_{23}^{(1)}x_3) $
- $ a_3^{(2)} = g(\Theta_{30}^{(1)}x_0 + \Theta_{31}^{(1)}x_1 + \Theta_{32}^{(1)}x_2 + \Theta_{33}^{(1)}x_3) $
- $ h_{\Theta}(x) = a_1^{(3)} = g(\Theta_{10}^{(2)}a_0^{(2)} + \Theta_{11}^{(2)}a_1^{(2)} + \Theta_{12}^{(2)}a_2^{(2)} + \Theta_{13}^{(2)}a_3^{(2)}) $
- $ a $ is the shorthand of "activation".

If network has $ s_j $ units in layer $ j $, $ s_{j+1} $ units in layer $ j + 1 $, then $ \Theta^{(j)} $ will be of dimension $ s_{j+1} \times (s_j + 1) $

## Cost Function

Logistic regression:

$$ J(\theta) = -\frac{1}{m}[\sum_{i=1}^my^{(i)}\log h_\theta(x^{(i)}) + (1 - y^{(i)})\log (1 - h_\theta(x^{(i)}))] + \frac{\lambda}{2m}\sum_{j=1}^n\theta_j^2 $$

Neural network:($ K $ classifiers)

$$ J(\Theta) = -\frac{1}{m}[\sum_{i=1}^m\sum_{k=1}^Ky_k^{(i)}\log (h_\Theta(x^{(i)}))_k + (1 - y_k^{(i)})\log (1 - (h_\Theta(x^{(i)}))_k)] $$
$$ + \frac{\lambda}{2m}\sum_{l=1}^{L-1}\sum_{i=1}^{s_l}\sum_{j=1}^{s_{l+1}}(\Theta_{ji}^{(l)})^2 $$

$$ h_\Theta(x) \in \mathbb{R}^K, (h_\Theta(x))_i = i^{th}\text{output} $$

If $ K = 1 $, it turns out to be a binary classification problem.

## Forward Propagation

- Steps:
	- $ a^{(1)} = x $
	- $ z^{(2)} = \Theta^{(1)}a^{(1)} $
	- $ a^{(2)} = g(z^{(2)})\space(\text{add}\space a_0^{(2)},g\space \text{is the sigmodi function}) $
	- $ z^{(3)} = \Theta^{(2)}a^{(2)} $
	- $ a^{(3)} = g(z^{(3)})\space(\text{add}\space a_0^{(3)},g\space \text{is the sigmodi function}) $
	- $ z^{(4)} = \Theta^{(3)}a^{(3)} $
	- $ a^{(4)} = h_\Theta(x) = g(z^{(4)}) $

## Back Propagation

Intuition: $ \delta_j^{(l)} $ = "error" of node $ j $ in layer $ l $.(So that $ \delta^{(l)} $ is "errors" of all the nodes in layer $ l $).

- Steps:(for one example $ (x, y) $)
	- $ \delta^{(4)} = a^{(4)} - y $
	- $ \delta^{(3)} = (\Theta^{(3)})^T\delta^{(4)}.*g'(z^{(3)})\space(g'(z^{(3)}) = a^{(3)}.*(1 - a^{(3)})) $
	- $ \delta^{(2)} = (\Theta^{(2)})^T\delta^{(3)}.*g'(z^{(2)})\space(g'(z^{(2)}) = a^{(2)}.*(1 - a^{(2)})) $
	- (There is no $ \delta^{(1)} $)
	- and $ \frac{\partial}{\partial\Theta_{ij}^{(l)}}J(\Theta) = a_j^{(l)}\delta_i^{(l+1)}\space(\lambda = 0) $

- How to compute $ g'(z^{(l)}) $:
	- $ g(z^{(l)}) = \frac{1}{1 + e^{-z^{(l)}}} = a^{(l)} $
	- $ g'(z^{(l)}) = \frac{e^{-z^{(l)}}}{(1 + e^{-z^{(l)}})^2} $
	- $ g'(z^{(l)}) = \frac{1 + e^{-z^{(l)}} - 1}{(1 + e^{-z^{(l)}})^2} $
	- $ g'(z^{(l)}) = \frac{1}{1 + e^{-z^{(l)}}} - \frac{1}{(1 + e^{-z^{(l)}})^2} $
	- $ g'(z^{(l)}) = a^{(l)}.*(1 - a^{(l)})) =  g(z^{(l)}).*(1 - g(z^{(l)}))$

- Steps:(for $ m $ examples training set $ \{(x^{(1)},y^{(1)}), \dots, (x^{(m)},y^{(m)})\} $)
	- Set $ \Delta_{ij}^{(l)} = 0 $ (for all $ l,i,j $).
	- For $ i = 1$ to $ m $
		- Set $ a^{(1)} = x^{(i)} $
		- Perform forward propagation to compute $ a^{(l)} $ for $ l = 2,3,\dots,L $
		- Using $ y^{(i)} $, compute $ \delta^{(L)} = a^{(L)} - y^{(i)} $ (forward propagation)
		- Compute $ \delta^{(L - 1)}, \delta^{(L - 2)}, \dots, \delta^{(2)} $ (back propagation)
		- $ \Delta_{ij}^{(l)} := \Delta_{ij}^{(l)} + a_j^{(l)}\delta_i^{(l+1)} $ (vectorized form: $ \Delta^{(l)} := \Delta^{(l)} + \delta^{(l+1)}(a^{(l)})^T $)
	- $ D_{ij}^{(l)} := \frac{1}{m}\Delta_{ij}^{(l)} + \lambda\Theta_{ij}^{(l)} $ if $ j \neq 0 $
	- $ D_{ij}^{(l)} := \frac{1}{m}\Delta_{ij}^{(l)} $ if $ j = 0 $
	- $ \frac{\partial}{\partial\Theta_{ij}^{(l)}}J(\Theta) = D_{ij}^{(l)} $

## Reshaping Matrix Between Vector

## Gradient Checking

Numerical calculated gradient vs. Neural network trained gradient

Apply small scale of neural network.

## Training a Neural Network

1. Pick a network architecture (connectivity pattern between neurons)
	- No. of input units: Dimension of features $ x^{(i)} $
	- No. of output units: Number of classes
	- Reasonable default: 1 hidden layer, of if >1 hidden layer, have same no. of hidden units in every layer (usually more the better)
2. Apply algorithms
	- Randomly initialize weights
	- Implement forward propagation to get $ h_\Theta(x^{(i)}) $ for any $ x^{(i)} $
	- Implement code to compute cost function $ J(\Theta) $
	- Implement backprop to compute partial derivatives $ \frac{\partial}{\partial\Theta_{jk}^{(l)}}J(\Theta) $
	- Use gradient checking to compare $ \frac{\partial}{\partial\Theta_{jk}^{(l)}}J(\Theta) $ computed using backpropagation vs. using numerical estimate of gradient of $ J(\Theta) $
	- Use gradient descent or advanced optimization method with backpropagation to try to minimize $ J(\Theta) $ as a function of parameters $ \Theta $
3. Gradient checking
4. And so on...



