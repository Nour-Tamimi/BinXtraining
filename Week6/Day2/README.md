# Deep Learning Fundamentals: Activation Functions & Forward Pass

This document summarizes the core theoretical concepts and architecture rules learned today while designing a custom neural network.

##  Key Concept Summaries

### 1. Visualizing Activation Functions
Activation functions introduce non-linearity, allowing a neural network to learn complex patterns instead of acting like a simple linear calculator:
*   **ReLU (Rectified Linear Unit)**: Sets all negative inputs to zero and leaves positive inputs unchanged. It is the standard choice for hidden layers due to its computational speed.
*   **Sigmoid**: Squeezes any input into a range between `0` and `1`. It represents probabilities, making it the perfect choice for the final layer in binary classification tasks.
*   **Tanh (Hyperbolic Tangent)**: Squeezes inputs between `-1` and `1`. It maps data symmetrically around zero, often helping hidden layers converge faster than Sigmoid.

### 2. Output and Loss Alignment
For the project (Phase 3), the network's final layer and evaluation criteria must align perfectly with the problem type:
*   **Binary Classification**: Requires a **Sigmoid** activation function at the output and a **Binary Cross-Entropy Loss** function.
*   **Multi-class Classification**: Requires a **Softmax** activation function at the output and a **Categorical Cross-Entropy Loss** function.
*   **Regression (Numeric Prediction)**: Requires a **Linear** output (no activation) and **Mean Squared Error (MSE) Loss**.

---

## 🛠️ The Architecture Rule: Data Flow & Alignment

A common structural mistake during manual implementation is connecting every single neuron directly to the raw input features. 

In a true multi-layer neural network, information must flow sequentially like an assembly line:
[Raw Inputs] ──> [Hidden Layer Neurons] ──> [ReLU Activation] ──> [Hidden Outputs] ──> [Output Neuron] ──> [Sigmoid]