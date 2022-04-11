# 评价指标


根据模型预测值和真实值的区别来评价模型。

<!--more-->

## 1.回归

### 1.1 MAE

**平均绝对误差，MAE，Mean Absolute Error**

$$
MAE(y,\hat{y})=\frac{1}{n}\sum_{i=1}^n|y_i-\hat{y}_i|
$$

### 1.2 MAPE

**平均绝对百分比误差，MAPE，Mean Absolute Percentage Error**

$$
MAPE(y,\hat{y})=\frac{1}{n}\sum_{i=1}^n\frac{|y_i-\hat{y}_i|}{|y_i|}
$$

### 1.3 MSE

**均方误差，MSE，Mean Squared Error**

$$
MSE(y,\hat{y})=\frac{1}{n}\sum_{i=1}^n|y_i-\hat{y}_i|_2^2
$$

### 1.4 RMSE

**均方根误差，RMSE，Root Mean Squared Error**

$$
RMSE(y,\hat{y})=\sqrt{\frac{1}{n}\sum_{i=1}^n|y_i-\hat{y}_i|_2^2}
$$

### 1.5 MSLE

**均方误差对数，MSLE，Mean Squared Log Error**

$$
MSLE(y,\hat{y})=\frac{1}{n}\sum_{i=1}^n\big(\log{(1+y_i)}-\log{(1+\hat{y}_i)}\big)^2
$$

### 1.6 MedAE

**中位绝对误差，MedAE，Median Absolute Error**

$$
MedAE(y,\hat{y})=median(|y_1-\hat{y}_1|,\cdots,|y_n-\hat{y}_n|)
$$

### 1.7 $R^2$

**拟合优度/可决系数，$R^2$，R Squared**

$$
R^2(y,\hat{y})=1-\frac{ \sum_{i=1}^n (y_i-\hat{y_i})^2 }{ \sum_{i=1}^n (y_i-\bar{y})^2 }
$$

## 参考

1. [回归模型的评价指标比较 - 知乎](https://zhuanlan.zhihu.com/p/143169742)

