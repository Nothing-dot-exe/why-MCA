# Master Study Notes: Deep Learning & Neural Networks
## Course Code: 22MCA411 / Professional Elective | Visvesvaraya Technological University (VTU)
### Department of MCA | BKIT College, Bhalki

---

## 📑 Syllabus Architecture & Module Breakdown

* **Module 1**: Neural Network Foundations & Backpropagation (Perceptron, Multi-Layer Perceptron, Activation Functions - Sigmoid, ReLU, Leaky ReLU, Softmax, Cross-Entropy Loss, Gradient Calculus, Vanishing/Exploding Gradients).
* **Module 2**: Optimization & Regularization Strategies (Stochastic Gradient Descent - SGD, Momentum, RMSprop, Adam Optimizer, L1/L2 Weight Decay, Dropout, Batch Normalization, Data Augmentation).
* **Module 3**: Convolutional Neural Networks - CNNs (Convolution Operation, Stride, Padding - Valid vs Same, Pooling Layers, Receptive Field, Architectures: AlexNet, VGG-16, ResNet Residual Skip Connections).
* **Module 4**: Sequence Models & Recurrent Architectures (Recurrent Neural Networks - RNN, Backpropagation Through Time - BPTT, Long Short-Term Memory - LSTM Architecture & Gate Equations, Gated Recurrent Units - GRU).
* **Module 5**: Attention Mechanism, Transformers & Generative Models (Self-Attention Mechanism, Scaled Dot-Product Attention $Q, K, V$, Multi-Head Attention, Positional Encoding, Transformer Encoder-Decoder, Generative Adversarial Networks - GANs).

---

# MODULE 1: NEURAL NETWORK FOUNDATIONS & CALCULUS

## 1.1 Activation Functions Comparison
| Activation | Mathematical Formula | Range | Gradient $\sigma'(z)$ | Key Characteristic / Limitation |
| :--- | :--- | :--- | :--- | :--- |
| **Sigmoid** | $\sigma(z) = \frac{1}{1 + e^{-z}}$ | $(0, 1)$ | $\sigma(z)(1 - \sigma(z))$ | Saturates at extremes; causes vanishing gradient. |
| **Tanh** | $\tanh(z) = \frac{e^z - e^{-z}}{e^z + e^{-z}}$ | $(-1, 1)$| $1 - \tanh^2(z)$ | Zero-centered, but still suffers from saturation. |
| **ReLU** | $\max(0, z)$ | $[0, \infty)$ | $1$ if $z > 0$ else $0$ | Computationally trivial; solves vanishing gradient. Dying ReLU risk. |
| **Leaky ReLU** | $\max(\alpha z, z), \alpha=0.01$ | $(-\infty, \infty)$ | $1$ if $z > 0$ else $\alpha$ | Prevents dying neurons by providing small slope for negative inputs. |
| **Softmax** | $\frac{e^{z_i}}{\sum_{j} e^{z_j}}$ | $(0, 1)$ | $\text{Output sums to } 1.0$ | Multi-class probability distribution output layer. |

## 1.2 Backpropagation Chain Rule Calculus
For a network with weight $W^{[l]}$, bias $b^{[l]}$, pre-activation $Z^{[l]} = W^{[l]} A^{[l-1]} + b^{[l]}$, and activation $A^{[l]} = g(Z^{[l]})$:
$$\frac{\partial \mathcal{L}}{\partial W^{[l]}} = \frac{\partial \mathcal{L}}{\partial A^{[l]}} \cdot \frac{\partial A^{[l]}}{\partial Z^{[l]}} \cdot \frac{\partial Z^{[l]}}{\partial W^{[l]}} = dZ^{[l]} \cdot (A^{[l-1]})^T$$
Where $dZ^{[l]} = dA^{[l]} \odot g'(Z^{[l]})$.

---

# MODULE 2: MODERN OPTIMIZATION & REGULARIZATION

## 2.1 The Adam Optimizer (Adaptive Moment Estimation)
Combines the benefits of **Momentum** (exponential moving average of past gradients) and **RMSprop** (adaptive learning rates based on squared gradients):
1. Compute gradient: $g_t = \nabla_\theta \mathcal{L}(\theta_t)$
2. Update biased first moment (Mean): $m_t = \beta_1 m_{t-1} + (1 - \beta_1) g_t$
3. Update biased second moment (Uncentered Variance): $v_t = \beta_2 v_{t-1} + (1 - \beta_2) g_t^2$
4. Compute bias-corrected moments:
   $$\hat{m}_t = \frac{m_t}{1 - \beta_1^t}, \quad \hat{v}_t = \frac{v_t}{1 - \beta_2^t}$$
5. Update parameters:
   $$\theta_{t+1} = \theta_t - \frac{\alpha}{\sqrt{\hat{v}_t} + \epsilon} \hat{m}_t$$

---

# MODULE 3: CONVOLUTIONAL NEURAL NETWORKS (CNNs)

## 3.1 Convolution Spatial Dimension Formula
Given an input volume of size $W \times H$, filter size $F \times F$, padding $P$, and stride $S$:
$$W_{\text{out}} = \left\lfloor \frac{W - F + 2P}{S} \right\rfloor + 1, \quad H_{\text{out}} = \left\lfloor \frac{H - F + 2P}{S} \right\rfloor + 1$$
* **Valid Padding ($P = 0$)**: No padding; output shrinks.
* **Same Padding ($P = \frac{F - 1}{2}$)**: Output spatial dimensions match input when $S = 1$.

## 3.2 ResNet & Residual Skip Connections
* **Degradation Problem**: As neural networks become deeper (e.g., >20 layers), accuracy saturates and then rapidly degrades due to vanishing gradients during backpropagation.
* **Residual Block**:
  $$\mathcal{H}(x) = \mathcal{F}(x) + x$$
  Instead of forcing layers to directly fit the underlying mapping $\mathcal{H}(x)$, the network learns the residual $\mathcal{F}(x) = \mathcal{H}(x) - x$. The identity shortcut $x$ allows gradients to flow directly back through the network without attenuation:
  $$\frac{\partial \mathcal{L}}{\partial x} = \frac{\partial \mathcal{L}}{\partial \mathcal{H}} \left( \frac{\partial \mathcal{F}}{\partial x} + 1 \right)$$

---

# MODULE 4: RECURRENT NETWORKS & LSTM

## 4.1 LSTM Architecture & Gate Equations
An LSTM solves the vanishing gradient problem in vanilla RNNs through an internal **Cell State ($C_t$)** governed by three multiplicative gates:
```
1. Forget Gate (ft):   ft = σ(Wf · [ht-1, xt] + bf)   -> Decides what info to discard from cell state
2. Input Gate (it):    it = σ(Wi · [ht-1, xt] + bi)   -> Decides which new values to update
3. Candidate State:    C~t = tanh(Wc · [ht-1, xt] + bc)
4. Update Cell State:  Ct = ft ⊙ Ct-1 + it ⊙ C~t      -> Linear combination (preserves long gradients)
5. Output Gate (ot):   ot = σ(Wo · [ht-1, xt] + bo)   -> Decides what parts of cell state to output
6. Hidden State (ht):  ht = ot ⊙ tanh(Ct)
```

---

# MODULE 5: TRANSFORMERS & GENERATIVE ADVERSARIAL NETWORKS

## 5.1 Scaled Dot-Product Attention (Vaswani et al.)
$$\text{Attention}(Q, K, V) = \text{softmax}\left( \frac{QK^T}{\sqrt{d_k}} \right) V$$
* $Q$ (Query), $K$ (Key), $V$ (Value) are linear projections of input embeddings.
* $\frac{1}{\sqrt{d_k}}$: Scaling factor that prevents dot products from growing excessively large for high dimensions, which would push the softmax function into regions with near-zero gradients.

## 5.2 Generative Adversarial Networks (GANs)
Two networks trained simultaneously in a zero-sum minimax game:
$$\min_G \max_D V(D, G) = \mathbb{E}_{x \sim p_{\text{data}}(x)}[\log D(x)] + \mathbb{E}_{z \sim p_z(z)}[\log (1 - D(G(z)))]$$
* **Generator $G(z)$**: Takes random Gaussian noise $z$ and maps it to realistic synthetic data to fool the Discriminator.
* **Discriminator $D(x)$**: Binary classifier that predicts the probability that a sample came from genuine training data rather than $G$.

---

# 🎯 High-Yield VTU Exam Solved Questions (10-Mark Model Answers)

### Question 1: CNN Output Feature Map Calculation (10 Marks)
**An input image of size $224 \times 224 \times 3$ is passed through a convolutional layer with 64 filters of size $7 \times 7$, stride $S = 2$, and padding $P = 3$.**
1. Compute the output feature map spatial dimensions $(W_{\text{out}}, H_{\text{out}}, D_{\text{out}})$.
2. Calculate the total number of trainable parameters in this convolutional layer (including biases).

**Solution:**
1. **Output Spatial Dimensions**:
   $$W_{\text{out}} = \frac{W - F + 2P}{S} + 1 = \frac{224 - 7 + 2(3)}{2} + 1 = \frac{224 - 7 + 6}{2} + 1 = \frac{223}{2} + 1 = 111.5 \to 112$$
   $$H_{\text{out}} = 112$$
   $$D_{\text{out}} = \text{Number of filters} = 64$$
   $$\mathbf{\text{Output Volume} = 112 \times 112 \times 64}$$
2. **Trainable Parameters Calculation**:
   * Each filter has dimensions: $F \times F \times D_{\text{in}} = 7 \times 7 \times 3 = 147$ weights.
   * Adding 1 bias term per filter: $147 + 1 = 148$ parameters per filter.
   * Total for 64 filters:
     $$\text{Total Parameters} = 64 \times 148 = \mathbf{9,472 \text{ parameters}}$$

---

### Question 2: ResNet Residual Architecture vs. Plain Deep Networks (10 Marks)
* Explain why deep neural networks suffer from the vanishing gradient problem and demonstrate mathematically how ResNet's skip connection bypasses this bottleneck.

**Model Answer:**
1. **Vanishing Gradient in Plain Networks**:
   * During backpropagation, the gradient of the loss with respect to early weights is computed via repeated matrix multiplications of layer Jacobians:
     $$\frac{\partial \mathcal{L}}{\partial W_1} = \frac{\partial \mathcal{L}}{\partial a_L} \prod_{k=2}^L \left( W_k^T \cdot \text{diag}(\sigma'(z_k)) \right) \frac{\partial z_1}{\partial W_1}$$
   * If activation function derivatives $\sigma'(z) \le 0.25$ (like Sigmoid), multiplying $L$ numbers smaller than 1 causes the gradient to decay exponentially towards zero as $L \to \infty$. The weights in early layers receive virtually zero updates, freezing learning.
2. **Mathematical Proof of ResNet Solution**:
   * In a residual unit, output is $x_{l+1} = x_l + \mathcal{F}(x_l, \mathcal{W}_l)$.
   * By recursion, for any deeper unit $L$ and shallower unit $l$:
     $$x_L = x_l + \sum_{i=l}^{L-1} \mathcal{F}(x_i, \mathcal{W}_i)$$
   * Computing the gradient of loss $\mathcal{L}$ with respect to $x_l$:
     $$\frac{\partial \mathcal{L}}{\partial x_l} = \frac{\partial \mathcal{L}}{\partial x_L} \frac{\partial x_L}{\partial x_l} = \frac{\partial \mathcal{L}}{\partial x_L} \left( 1 + \frac{\partial}{\partial x_l} \sum_{i=l}^{L-1} \mathcal{F}(x_i, \mathcal{W}_i) \right)$$
   * The term $+1$ guarantees that the gradient $\frac{\partial \mathcal{L}}{\partial x_L}$ can flow directly back to $x_l$ unconditionally, even if the derivative of the residual terms $\frac{\partial \mathcal{F}}{\partial x_l}$ approaches zero. This completely prevents gradient vanishing regardless of depth.
