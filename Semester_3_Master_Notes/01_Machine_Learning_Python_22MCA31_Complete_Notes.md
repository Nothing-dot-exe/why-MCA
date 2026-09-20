# Master Study Notes: Machine Learning & Data Analytics using Python
## Course Code: 22MCA31 / MMC201 | Visvesvaraya Technological University (VTU)
### Department of MCA | BKIT College, Bhalki

---

## 📑 Syllabus Architecture & Module Breakdown

* **Module 1**: Python Data Science Ecosystem (NumPy, Pandas, Matplotlib), Machine Learning Categories (Supervised, Unsupervised, Reinforcement), Bias-Variance Tradeoff, Cross-Validation.
* **Module 2**: Supervised Learning (Linear Regression, Gradient Descent Optimization, Logistic Regression, Cost Functions, Confusion Matrix, Precision, Recall, F1-Score, ROC-AUC).
* **Module 3**: Tree-Based Models & SVM (Decision Trees, Entropy, Information Gain, Gini Impurity, Random Forests, AdaBoost, Support Vector Machines, Kernel Trick).
* **Module 4**: Unsupervised Learning (K-Means Clustering, Elbow Method, Hierarchical Agglomerative Clustering, Principal Component Analysis - PCA for Dimensionality Reduction).
* **Module 5**: Neural Networks & Generative AI (Perceptron, Multi-Layer Perceptron, Backpropagation, CNN, RNN/LSTM, Transformer Architecture, Self-Attention & LLMs).

---

# MODULE 1: PYTHON DATA SCIENCE & ML TAXONOMY

## 1.1 NumPy & Pandas Quick Syntax
* **NumPy Vectorization**: Avoids slow Python `for` loops by performing parallel C-level array operations:
  ```python
  import numpy as np
  X = np.array([[1, 2], [3, 4]])
  y = np.dot(X, np.array([0.5, 1.5])) # Matrix-vector dot product
  ```
* **Pandas Data Cleaning**:
  ```python
  import pandas as pd
  df = pd.read_csv('vtu_dataset.csv')
  df.fillna(df.median(numeric_only=True), inplace=True) # Impute missing values
  df = pd.get_dummies(df, drop_first=True) # One-Hot Encoding
  ```

---

## 1.2 Machine Learning Taxonomy & The Bias-Variance Tradeoff
* **Supervised Learning**: Model trained on labeled input-output pairs $(X, y)$ (e.g., Regression, Classification).
* **Unsupervised Learning**: Model identifies hidden patterns, groupings, or clusters from unlabeled data $X$ (e.g., K-Means, PCA).
* **Reinforcement Learning**: Agent learns optimal decision policy through reward and penalty feedback in an environment.

### Bias vs. Variance Tradeoff:
* **High Bias (Underfitting)**: Model is overly simplistic (e.g., fitting a linear line to a quadratic curve). Fails to capture underlying trends; high error on both training and test data.
* **High Variance (Overfitting)**: Model is overly complex; memorizes noise in training data. Very low training error, but fails to generalize to test data.
* **Goal**: Minimize **Total Expected Error** = $\text{Bias}^2 + \text{Variance} + \text{Irreducible Noise } \sigma^2$.

---

# MODULE 2: REGRESSION & CLASSIFICATION

## 2.1 Linear Regression & Gradient Descent
* **Hypothesis**: $\hat{y} = \theta_0 + \theta_1 x_1 + \dots + \theta_n x_n = \theta^T X$.
* **Mean Squared Error (MSE) Cost Function**:
  $$J(\theta) = \frac{1}{2m} \sum_{i=1}^m \left( h_\theta(x^{(i)}) - y^{(i)} \right)^2$$
* **Batch Gradient Descent Parameter Update**:
  $$\theta_j := \theta_j - \alpha \frac{\partial J(\theta)}{\partial \theta_j} = \theta_j - \alpha \frac{1}{m} \sum_{i=1}^m \left( h_\theta(x^{(i)}) - y^{(i)} \right) x_j^{(i)}$$
  Where $\alpha$ is the **Learning Rate**.

---

## 2.2 Logistic Regression & Evaluation Metrics
* **Sigmoid Activation Function**: Maps any real-valued number into a probability $[0, 1]$:
  $$\sigma(z) = \frac{1}{1 + e^{-z}}, \quad \text{where } z = \theta^T X$$
* **Log-Loss (Binary Cross-Entropy) Cost Function**:
  $$J(\theta) = -\frac{1}{m} \sum_{i=1}^m \Big[ y^{(i)} \log(\hat{y}^{(i)}) + (1 - y^{(i)}) \log(1 - \hat{y}^{(i)}) \Big]$$

### Model Evaluation Metrics (Confusion Matrix):
* **Accuracy**: $\frac{TP + TN}{TP + TN + FP + FN}$
* **Precision**: $\frac{TP}{TP + FP}$ *(Crucial when False Positives are expensive, e.g., Spam detection)*
* **Recall (Sensitivity)**: $\frac{TP}{TP + FN}$ *(Crucial when False Negatives are dangerous, e.g., Cancer diagnosis)*
* **F1-Score**: Harmonic mean of Precision and Recall:
  $$\text{F1} = 2 \times \frac{\text{Precision} \times \text{Recall}}{\text{Precision} + \text{Recall}}$$

---

# MODULE 3: DECISION TREES & ENSEMBLE METHODS

## 3.1 Decision Tree Mathematics
* **Entropy ($H(S)$)**: Measure of impurity/randomness in dataset $S$:
  $$H(S) = -\sum_{i=1}^c p_i \log_2(p_i)$$
* **Information Gain ($IG(S, A)$)**: Reduction in entropy after splitting on attribute $A$:
  $$IG(S, A) = H(S) - \sum_{v \in \text{Values}(A)} \frac{|S_v|}{|S|} H(S_v)$$
* **Gini Impurity (CART Algorithm)**:
  $$\text{Gini}(S) = 1 - \sum_{i=1}^c p_i^2$$

---

## 3.2 Random Forests & Boosting
* **Random Forest (Bagging)**: Constructs an ensemble of $B$ decision trees trained on bootstrap samples with random feature subsets. Predictions aggregated via Majority Voting (Classification) or Mean (Regression).
* **Boosting (AdaBoost & XGBoost)**: Sequential training where each successive tree focuses on correcting the residual errors of prior trees.

---

# MODULE 4: UNSUPERVISED LEARNING & PCA

## 4.1 K-Means Clustering Algorithm
1. Randomly initialize $k$ cluster centroids $\{\mu_1, \dots, \mu_k\}$.
2. **Assignment Step**: Assign each data point $x^{(i)}$ to its closest centroid:
   $$c^{(i)} = \arg\min_j ||x^{(i)} - \mu_j||^2$$
3. **Update Step**: Recompute centroids as the mean of all assigned points:
   $$\mu_j = \frac{1}{|S_j|} \sum_{i \in S_j} x^{(i)}$$
4. Repeat Steps 2 and 3 until centroids converge.
* **Elbow Method**: Plots Within-Cluster Sum of Squares (WCSS) vs. $k$; selects $k$ at the inflection point ("elbow").

---

## 4.2 Principal Component Analysis (PCA)
Technique for unsupervised linear dimensionality reduction:
1. Standardize features to zero mean and unit variance.
2. Compute the **Covariance Matrix** $\Sigma = \frac{1}{m} X^T X$.
3. Compute the **Eigenvalues ($\lambda_i$)** and **Eigenvectors ($v_i$)** of $\Sigma$.
4. Sort eigenvectors in descending order of eigenvalues; select top $k$ eigenvectors to form projection matrix $W$.
5. Transform original data: $X_{\text{reduced}} = X \cdot W$.

---

# MODULE 5: NEURAL NETWORKS & GENERATIVE AI

## 5.1 Backpropagation & Gradient Calculation
Using the chain rule of calculus, the gradient of the loss function $\mathcal{L}$ with respect to weight $w_{jk}$ is:
$$\frac{\partial \mathcal{L}}{\partial w_{jk}} = \frac{\partial \mathcal{L}}{\partial \hat{y}} \cdot \frac{\partial \hat{y}}{\partial z_k} \cdot \frac{\partial z_k}{\partial w_{jk}}$$

---

## 5.2 The Transformer Architecture & Self-Attention
Introduced in "Attention Is All You Need" (Vaswani et al., 2017). Replaced recurrent neural networks by processing entire sentences in parallel.

### Scaled Dot-Product Attention Formula:
$$\text{Attention}(Q, K, V) = \text{softmax}\left( \frac{Q K^T}{\sqrt{d_k}} \right) V$$
Where:
* $Q$ = Query matrix, $K$ = Key matrix, $V$ = Value matrix.
* $d_k$ = Dimension of key vectors (scaling factor prevents vanishing gradients in softmax).

---

# 🎯 Model Exam Questions with Step-by-Step Solutions

### Q1. [Module 3 - 10 Marks]
**A training dataset of 14 instances has 9 "Yes" and 5 "No" labels for loan approval. A candidate split feature 'Credit Rating' divides the data into: High (4 Yes, 0 No) and Low (5 Yes, 5 No). Calculate:**
1. Entropy of the parent dataset $H(S)$.
2. Entropy of both subsets $H(S_{\text{High}})$ and $H(S_{\text{Low}})$.
3. Information Gain $IG(S, \text{Credit Rating})$.

**Solution:**
1. **Parent Entropy $H(S)$**:
   $$p_+ = \frac{9}{14} \approx 0.643, \quad p_- = \frac{5}{14} \approx 0.357$$
   $$H(S) = -\left( \frac{9}{14} \log_2 \frac{9}{14} + \frac{5}{14} \log_2 \frac{5}{14} \right)$$
   $$H(S) = -(0.643 \times (-0.637) + 0.357 \times (-1.485)) = 0.410 + 0.530 = \mathbf{0.940\text{ bits}}$$

2. **Subset Entropies**:
   * For **High** ($4$ Yes, $0$ No): Pure node $\implies H(S_{\text{High}}) = \mathbf{0\text{ bits}}$.
   * For **Low** ($5$ Yes, $5$ No): Perfectly balanced $\implies H(S_{\text{Low}}) = -\left( \frac{5}{10} \log_2 \frac{1}{2} + \frac{5}{10} \log_2 \frac{1}{2} \right) = \mathbf{1.000\text{ bit}}$.

3. **Information Gain**:
   $$IG(S, \text{Credit Rating}) = H(S) - \left( \frac{|S_{\text{High}}|}{|S|} H(S_{\text{High}}) + \frac{|S_{\text{Low}}|}{|S|} H(S_{\text{Low}}) \right)$$
   $$IG = 0.940 - \left( \frac{4}{14} \times 0 + \frac{10}{14} \times 1.000 \right)$$
   $$IG = 0.940 - 0.714 = \mathbf{0.226\text{ bits}}$$
