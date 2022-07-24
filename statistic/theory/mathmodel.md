---
title: 数模记录
categories: [Statistic,Theory]
tags:
  - Statistic
  - python
  - Theory
  - Matlab
toc: true
date: 2022-07-22 17:30:20
update: 2022-07-28 9:30:20
latex: true
---
<!--more-->
一点数模记录，数据处理、模型理论&代码
### 数据部分
数据类型：csv、txt、xlxs
Matlab直接导入（excel预览数据、简单处理）
Python代码导入、处理、

#### 处理
降维（特征提取）、指标转化（定性、定量数据）
#### PCA

#### 因子分析

### 模型部分
预测、分类
#### 预测(回归)
多元线性


##### [时间序列（ARMA）](https://zh.wikipedia.org/wiki/ARMA%E6%A8%A1%E5%9E%8B)
ARMA模型（Autoregressive moving average model，自回归滑动平均模型）。研究时间序列的重要方法，由自回归模型（简称AR模型）与移动平均模型（简称MA模型）为基础“混合”构成。
- 在市场研究中常用于长期追踪资料的研究，如：Panel研究中，用于消费行为模式变迁研究；
- 在零售研究中，用于具有季节变动特征的销售量、市场规模的预测等。

__自回归AR模型__
- $ X_t=c+\sum_{i=1}^p \varphi_i X_{t-1}+\varepsilon_t $
- 自回归模型描述当前值与历史值的关系，该模型认为通过时间序列过去时点的线性组合加上白噪声即可预测当前时点，是随机游走的一个简单扩展
- c为常数项，$\epsilon_t$ 被假设为平均数等于0（白噪音），标准差等于$\sigma$的随机误差值；$\epsilon_t$被假设为对于任何的t都不变。

__移动平均MA（q）模型__
- $X_t = \mu + \varepsilon_t +\sum_{i=1}^q\vartheta_i\varepsilon_{t-i}$ 
- 移动平均模型描述的是自回归部分的误差累计，是历史白噪声的线性组合
- 其中 μ 是序列的均值，$\vartheta_1,\dots,\vartheta_q$ 是参数，$\varepsilon_t.,\varepsilon_{t-1},\dots,\varepsilon_{t-q}$ 都是白噪声

__ARMA(p,q)模型__
- ARMA(p,q)模型中包含了p个自回归项和q个移动平均项
- $X_{t}=c+\varepsilon _{t}+\sum _{i=1}^{p}\varphi _{i}X_{t-i}+\sum _{j=1}^{q}\theta _{j}\varepsilon _{t-j}$
- 最佳参数p和q的选择：采用循环在p和q各自0到5（或者更大）的范围内搜索最小AIC或BIC的(p,q)组合

__ARMA滞后算子表示法__
有时ARMA模型可以用滞后算子（Lag operator）$L $来表示，$L^{i}X_{t}=X_{t-i}L^{i}X_{t}=X_{t-i}$
这样AR(p)模型可以写成为：
- $ \varepsilon _{t}=(1-\sum _{i=1}^{p}\varphi _{i}L^{i})X_{t}=\varphi (L)X_{t}$
- 其中$\varphi$表示多项式

$\varphi (L)=1-\sum _{i=1}^{p}\varphi _{i}L^{i}$
MA(q)模型可以写成为：
$X_{t}=\left(1+\sum _{i=1}^{q}\theta _{i}L^{i}\right)\varepsilon _{t}=\theta (L)\varepsilon _{t}$
- 其中θ 表示多项式$ \theta(L)=1+\sum_{i=1}^{q}  \theta_{i}L^{i}$
- ARMA(p,q)模型可以表示为：$ (1-\sum_{i=1}^{p}\varphi _{i}L^{i})X_{t}=(1+\sum _{i=1}^{q}\theta _{i}L^{i}) \varepsilon_{t} $
- 或者 $\varphi (L)X_{t}=\theta (L)\varepsilon _{t}$

> 若$\varphi (L)=1$,则ARMA过程退化为MA(q)过程 若$\theta (L)=1$,则ARMA过程退化为AR(p)过程。

##### ARIMA
ARIMA模型是在ARMA模型的基础上解决非平稳序列的模型，因此在模型中会对原序列进行差分。这里是一个简单模拟：
- `x <- arima.sim(list(order = c(1,1,1), ar = 0.6, ma=-0.5), n = 1000`
- `arima(x, order=c(1, 1, 1))`
  
{% note info %}
对序列差分之后用arima预测得到的结果，需要逐项加回来，才能得到正确的预测结果
{% endnote %}
__建模框架__
R && python
``` R
# 对时间序列进行平稳性检验。
library('TSA')
adf.test(series)

# 若无法通过检验则需要对原序列进行差分，直到差分后的结果能够通过平稳性检验来确定d
df1 = diff(series)
adf.test(df1)

# 查看ACF和PACF图。
acf(df1)
pacf(df1)

# 模式选择：通过EACF、AIC等技术，确定p、q的取值。
# 训练模型，得到参数值。
res = arima(series, order = c(p,d,q))
# 检验残差是否是白噪声序列。
Box.test(res$residuals,lag=20, type="Ljung-Box")

# 对时间序列未来的值进行预测。
play_forecast <- predict(res, n.ahead = 21)
plot(res, n.ahead = 21, type = 'o')
```

``` pytohn

```
#### 分类（）

#####  logistic












