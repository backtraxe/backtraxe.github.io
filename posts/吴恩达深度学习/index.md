# 吴恩达深度学习


<!--more-->

## 1 Neural Networks and Deep Learning

### 1.1 Introduction to Deep Learning

![Simple Neural Networks](吴恩达深度学习.assets/屏幕截图 2020-09-18 165126.png)

#### Machine Learning Classification

- **Supervised Learning**: we already know the correct output called label.
- **Unsupervised Learning**: with no label.

#### Data Classification

- **Structued Data**: Table, Records ...
- **Unstructured Data**: Audio, Image, Text ...

### 1.2 Basics of Neural Network programming

#### Math Notations

- $m$: the number of training samples, $m_{train},m_{test}$.
- $n$: the number of features.
- $(x, y)$: all samples.
- $(x^{(i)},y^{(i)})$: one sample, $x^{(i)}$ is a $(n \times 1)$ matrix, $y^{(i)}$ is a number.

$$
x^{(i)}=\begin{bmatrix} x^{(i)}_1 \\ x^{(i)}_2 \\ ... \\ x^{(i)}_{n} \end{bmatrix}
$$

- $X$: training set, is a $(n \times m)$ matrix.

$$
X=[x^{(1)},x^{(2)},...,x^{(i)},...,x^{(m)}]
$$

- $Y$: label set, is a $(1 \times m)$ matrix.

$$
Y=\begin{bmatrix} y^{(1)}, y^{(2)},...,y^{(m)} \end{bmatrix}
$$

#### Binary Classification with Logistic Regression

$\hat y=P(y=1|x), 0 \le \hat y \le 1$

Parameters:

- $w$: weights, a $(n \times 1)$ matrix.
- $b$: threshold (bias), a $(1 \times m)$ matrix with all values are the same.

$$
w=\begin{bmatrix} w_1 \\ w_2 \\ ... \\ w_n \end{bmatrix},
b=\begin{bmatrix} b_0,b_0,...,b_0 \end{bmatrix} \\
\hat y = \sigma(w^Tx+b)
$$

**Another Notation:**

Parameters:

- $\theta$: a $((n+1) \times 1)$ matrix.
- $x^{(i)}$: a $((n+1) \times 1)$ matrix.
- $X$: a $((n+1) \times m)$ matrix.

$$
\theta = \begin{bmatrix} b_0 \\ w_1 \\ w_2 \\ ... \\ w_n \end{bmatrix},
x^{(i)} = \begin{bmatrix} 1 \\ x^{(i)}_1 \\ x^{(i)}_2 \\ ... \\ x^{(i)}_n \end{bmatrix},
X = \begin{bmatrix}
1 & 1 & ... & 1 \\
x^{(1)}_1 & x^{(2)}_1 & ... & x^{(m)}_1 \\
x^{(1)}_2 & x^{(2)}_2 & ... & x^{(m)}_2 \\
... & ... & ... & ... \\
x^{(1)}_n & x^{(2)}_n & ... & x^{(m)}_n
\end{bmatrix}
$$

$$
\begin{aligned}
w^Tx^{(i)}+b_0
&= w_1 \cdot x^{(i)}_1 + w_2 \cdot x^{(i)}_2 + ... + w_n \cdot x^{(i)}_n + b_0 \cdot 1 \\
&= \begin{bmatrix} b_0 \\ w_1 \\ w_2 \\ ... \\ w_n \end{bmatrix}^T
\cdot \begin{bmatrix} 1 \\ x^{(i)}_1 \\ x^{(i)}_2 \\ ... \\ x^{(i)}_n \end{bmatrix} \\
&= \theta^Tx^{(i)} \\

w^Tx+b
&= \begin{bmatrix} w^Tx^{(1)},w^Tx^{(2)},...,w^Tx^{(m)} \end{bmatrix}
+ \begin{bmatrix} b_0,b_0,...,b_0 \end{bmatrix} \\
&= \begin{bmatrix} w^Tx^{(1)}+b_0,w^Tx^{(2)}+b_0,...,w^Tx^{(m)}+b_0 \end{bmatrix} \\
&= \begin{bmatrix} \theta^Tx^{(1)},\theta^Tx^{(2)},...,\theta^Tx^{(m)} \end{bmatrix} \\
&= \theta^Tx
\end{aligned} \\

\hat y = \sigma(\theta^Tx)
$$

**Cost Function:** error of all training examples.

$$
\begin{aligned}
\mathcal J(w,b)
&= \frac{1}{m}\sum_{i=1}^{m}\mathcal L(\hat y^{(i)},y^{(i)}) \\
&= -\frac{1}{m}\sum_{i=1}^{m}[y^{(i)}\log{(\hat y^{(i)})}+(1-y^{(i)})\log{(1-\hat y^{(i)})}]
\end{aligned}
$$

Reduction:

$$
If \ \ y=1: \ P(y|x)=\hat y \\
If \ \ y=0: \ P(y|x)=1-\hat y \\
Suppose \ P(y|x)=\hat y^y(1-\hat y)^{(1-y)} \\
Then \ \log{P(y|x)}=y\log{(\hat y)}+(1-y)\log{(1-\hat y)}
$$

**Loss (error) function:** error of single training example.

$$
\mathcal L(\hat y,y)=-y\log{(\hat y)}-(1-y)\log{(1-\hat y)}
$$

**Gradient Descend:**

$$
w=w-\alpha\frac{\part \mathcal J(w,b)}{\part w} \\
b=b-\alpha\frac{\part \mathcal J(w,b)}{\part b}
$$

```python
def GradientDescend(x, y, learning_rate):
    n_samples, n_features = x.shape


n_samples, n_features = x.shape
J = 0
w = np.zeros([1, n_features])
dw = np.zeros([1, n_features])
b = 0
db = 0
alpha = 0.05

for i in range(m):
    z = np.dot(x, w.T) + b
    y_hat = sigmoid(z)
    J += -y[i] * np.log(y_hat) - (1 - y[i]) * np.log(1 - y_hat)
    dw =
    db =
    w -= alpha * dw
    b -= alpha * db
```

> Initialize weights to zero will make no sense, all weights in same layer will be the same.

### 1.3 One hidden layer Neural Networks

![2-layer Neural Network](吴恩达深度学习.assets/屏幕截图 2020-09-27 103709.png)

- Input Layer: We don't count input layer as an official layer.
- Hidden Layer
- Output Layer

![Calculation in a neuron](吴恩达深度学习.assets/屏幕截图 2020-09-27 103212.png)

#### Activation Function

Activation Function must be `nonlinear` function.

- **Sigmoid**

$$
\begin{aligned}
\sigma(z) &= \frac{1}{1+e^{-z}} \\
\sigma'(z) &= \sigma(z)\cdot(1-\sigma(z))
\end{aligned}
$$

![Sigmoid](吴恩达深度学习.assets/屏幕截图 2020-09-18 171904.png)

```python
def sigmoid(z):
    return 1 / (1 + np.exp(-z))
```
- **Tanh**

$$
\begin{aligned}
\tanh(z) &= \frac{e^z-e^{-z}}{e^z+e^{-z}} \\
(\tanh(z))' &= 1-(\tanh(z))^2
\end{aligned}
$$

```python
def tanh(z):
    return (np.exp(z) - np.exp(-z)) / (np.exp(z) + np.exp(-z))
```

- **ReLU (Rectified Linear Unit)**: Optimal.

$$
g(z)=\max(0,z) \\
\begin{equation}
g'(z)=\begin{cases}
0, & \text{if} \ z \lt 0; \\
1, & \text{if} \ z \ge 0.
\end{cases}
\end{equation}
$$

![ReLU](吴恩达深度学习.assets/屏幕截图 2020-09-28 095206.png)

```python
def relu(z):
    return np.maximum(0, z)
```

- **Leaky ReLU**

$$
g(z)=\max(0.01z,z) \\
\begin{aligned}
g'(z)=\begin{cases}
0.01, & \text{if} \ z \lt 0; \\
1, & \text{if} \ z \ge 0.
\end{cases}
\end{aligned}
$$

![Leaky ReLU](吴恩达深度学习.assets/屏幕截图 2020-09-28 095353.png)

```python
def leaky_relu(z):
    return np.maximum(0.01 * z, z)
```

### 1.4 Deep Neural Networks

Forward propagation:

- Input: $a^{[l-1]}$
- Output: $a^{[l]}$
- Cache: $z^{[l]}, W^{[l]}, b^{[l]}$

Backward propagation:

- Input: $da^{[l]}$
- Output: $da^{[l-1]}, dW^{[l]}, db^{[l]}$

**Parameters vs Hyperparameters**

- **Parameters:** $W,b$

- **Hyperparameters:** $\alpha,iterations,hiddenLayers,hiddenUnits,activationFunctions$

Optimal hyperparameters will change.

## 2 Improving Deep Neural Networks: Hyperparameter tuning, Regularization and Optimization

### 2.1 Setting up your ML application

**Datasets**

- Train sets:
- Dev sets:
- Test sets: evaluation

**Bias and Variance**

- high variance: Dev set error >> Train set error.

  More data/Regularization to solve.

- high bias: Dev set error >> 0 & Train set error > 0.

  Bigger network to solve.

**Regularization**

- L1 Regularization

$$
\mathcal J(w,b)=\frac{1}{m}\sum_{i=1}^m\mathcal L(\hat y^{(i)},y^{(i)})+\frac{\lambda}{2m}\left\| w \right\|_1 \\
\left\| w \right\|_1=\sum_{j=1}^n|w_j|
$$

- L2 Regularization

$$
\mathcal J(w,b)=\frac{1}{m}\sum_{i=1}^m\mathcal L(\hat y^{(i)},y^{(i)})+\frac{\lambda}{2m}\left\| w \right\|_2^2 \\
\left\| w \right\|_2^2=\sum_{j=1}^nw_j^2=W^TW
$$

- $\lambda:$ regularization parameter

**Dropout regularization**

- Inverted dropout

keep-prop: the percentage of keeping neuron.

**Data augmentation**

- revolve
- flip
- zoom
- distortion

**Normalizing inputs**

1. $x=x-\mu=x-\frac{1}{m}\sum_{i=1}^mx^{(i)}$
2. $x=\frac{x}{\sigma^2}=\frac{x}{\frac{1}{m}\sum_{i=1}^m(x^{(i)})^2}$

**Vanishing/Exploding gradients**

**Weight initialization**

### 2.2 Optimization Algorithms

**Mini-batch gradient descent**

**Stochastic gradient descent**

**Exponentially weighted averages**

**Gradient descent with momentum**

$$
\begin{aligned}
v_{dW}&=\beta v_{dW}+(1-\beta)dW \\
v_{db}&=\beta v_{db}+(1-\beta)db \\
W&=W-\alpha v_{dW} \\
b&=b-\alpha v_{db}
\end{aligned}
$$

**RMSprop (Root Mean Square prop)**

$$
\begin{aligned}
S_{dW}&=\beta S_{dW}+(1-\beta)dW^2 \\
S_{db}&=\beta S_{db}+(1-\beta)db^2 \\
W&=W-\alpha \frac{dW}{\sqrt{S_{dW}}} \\
b&=b-\alpha \frac{db}{\sqrt{S_{db}}}
\end{aligned}
$$

**Adam (Adaptive Moment Estimation) Optimization Algorithm**

$$
\begin{aligned}
v_{dW}&=\beta_1 v_{dW}+(1-\beta_1)dW \\
v_{db}&=\beta_1 v_{db}+(1-\beta_1)db \\
S_{dW}&=\beta_2 S_{dW}+(1-\beta_2)dW^2 \\
S_{db}&=\beta_2 S_{db}+(1-\beta_2)db^2 \\
v_{dW}^{corrected}&=\frac{v_{dW}}{1-\beta_1^t} \\
v_{db}^{corrected}&=\frac{v_{db}}{1-\beta_1^t} \\
S_{dW}^{corrected}&=\frac{S_{dW}}{1-\beta_2^t} \\
S_{db}^{corrected}&=\frac{S_{db}}{1-\beta_2^t} \\
W&=W-\alpha\frac{v_{dW}^{corrected}}{\sqrt{S_{dW}^{corrected}}+\epsilon} \\
b&=b-\alpha\frac{v_{db}^{corrected}}{\sqrt{S_{db}^{corrected}}+\epsilon}
\end{aligned}
$$

- $\alpha$: needs to be tune
- $\beta_1$: 0.9
- $\beta_2$: 0.999
- $\epsilon: \ 10^{-8}$

**Learning rate decay**

$$
\begin{aligned}
\alpha&=\frac{1}{1+decay\_rate \times epoch\_number}\alpha_0 \\
\alpha&=0.95^{epoch\_number}\alpha_0 \\
\alpha&=\frac{k}{\sqrt{epoch\_number}}\alpha_0
\end{aligned}
$$

### 2.3 Hyperparameter tuning

**Hyperparameter advices**

- Try random values: don't use a grid.

  We don't know which hyperparameter is more important.

- Coarse to fine search.

**Batch Norm**

$$
\begin{aligned}
\mu&=\frac{1}{m}\sum_iz^{(i)} \\
\sigma^2&=\frac{1}{m}\sum_i(z^{(i)}-\mu)^2 \\
z_{norm}^{(i)}&=\frac{z^{(i)}-\mu}{\sqrt{\sigma^2+\epsilon}} \\
\tilde z^{(i)}&=\gamma \cdot z_{norm}^{(i)}+\beta
\end{aligned}
$$

**Softmax Regression**

$$
\mathcal L(\hat y,y)=-\sum_{i=1}^Cy_i\log\hat y_i
$$

## 3 Structuring your Machine Learning project

**ML strategy**

- Collect more data
- Collect more diverse training set
- Train algorithm longer with gradient descent
- Try Adam instead of gradient descent
- Try bigger/smaller network
- Try dropout
- Add $L_2$ regularization
- Network architecture
  - Activation functions
  - hidden units number

**Orthogonalization**

modifying an instruction or a component of an algorithm will not create or propagate side effects to other components of the system.

1. Fit training set well in cost function

   If not, the use of a bigger neural network or switching to a better optimization algorithm might help.

2. Fit development set well on cost function

   If not, regularization or using bigger training set might help.

3. Fit test set well on cost function

   If not, the use of a bigger development set might help.

4. Performs well in real world

   If not, the development test set is not set correctly or the cost function is not evaluating the right thing.

**Evaluation Metrics**

|           |  Positive   |  Negitive   |
| :-------: | :---------: | :---------: |
| **True**  |     TP      | FN (Type 2) |
| **False** | FP (Type 1) |     TN      |

- Accuracy: $Acc=\frac{TP+TN}{TP+TN+FP+FN}$
- Precision (P): ratio of TRUE in test POSITIVE, $P=\frac{TP}{TP+FP}$
- Recall (R): ratio of test POSITIVE in actually TRUE, $R=\frac{TP}{TP+FN}$
- F1 Score: harmonic mean of P and R, $\frac{1}{F1}=\frac{1}{2}(\frac{1}{P}+\frac{1}{R}) \rightarrow F1=\frac{2PR}{P+R}$
- F Score: $\frac{1}{F}=\frac{\beta^2}{1+\beta^2}(\frac{1}{P}+\frac{1}{R})$

## 4 Convolutional Neural Networks

## 5 Natural Language Processing: Building sequence models

