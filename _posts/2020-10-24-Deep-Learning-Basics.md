---
layout: post
title: A Gentle Introduction to Deep Learning
full-width: true
image: https://42xtjqm0qj0382ac91ye9exr-wpengine.netdna-ssl.com/wp-content/uploads/2017/05/neuralnet.jpg
---

Recent advances in data collection and computational power have enabled the development and usage of many machine learning techniques. Algorithms are being used to run self-driving cars, translate documents, and even diagnose diseases. Perhaps the most famous of these techniques is deep learning, which uses layers of neural networks to extract information from the provided input. In this post I'll describe how neural networks work and use neural networks to classify images of insects.

# What is a Neural Network?
A neural network, in its simplest form, takes a series of inputs (images, text, numbers), and passes them through a series of functions to map the input data onto the output or prediction. Let's take a look at a basic example to gain some intuition into how they work. 

The image below shows a simple neural network. The inputs are X1, X2, and X3. These inputs are combined together using numbers called weights. Weights determine how much of each input you want to pass on to the next layer. For example, if my weights were: W1: .3, W2: .5, W3: .2, that would mean I want the next layer to be 30% W1, 50% W2, and 20% W3. This summed input is then fed into another function, often a sigmoid, to produce the end prediction Y. 

![neuralnetwork](https://i.stack.imgur.com/xqPAc.png)

This basic building block, called the perceptron, can be stacked repeatedly. These deeper networks can learn more complex relationships between inputs and outputs.

![complexnetwork](https://victorzhou.com/media/nn-series/network.png)
# How do Neural Networks Learn?

# Classifying Insect Images with a Neural Network
