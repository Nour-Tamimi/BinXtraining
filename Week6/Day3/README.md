# Neural Network Training Loop — Mid-Sprint Notebook

## Overview

This notebook covers the core mechanics of how a neural network learns: the
training loop, gradient descent, the learning rate, and backpropagation.
It includes a hands-on experiment comparing three learning rates, and is
prepared for the mid-sprint Mentor Code & Notebook Review.

## Contents

### 1. The Four-Step Training Loop

A markdown cell describing the loop that repeats over the data:

1. **Forward Propagation** — the network produces a prediction. Random (or
   current) weights and biases are applied layer by layer: a dot product
   followed by an activation function, until the output layer produces the
   final prediction.
2. **Loss function** — compares the prediction to the true value to measure
   how wrong it is.
3. **Backpropagation** — using the chain rule, computes how much each weight
   contributed to the loss.
4. **Update** — gradient descent adjusts the weights and biases in the
   direction that reduces the loss.

These four steps repeat for many epochs until the loss is minimized.

### 2. Learning Rate Experiment

A tiny network (1 input → 4 hidden neurons with tanh → 1 output) is trained
on a toy regression problem (`y = x²`) at three learning rates, starting
from identical initial weights for a fair comparison:

| Learning rate | Result |
