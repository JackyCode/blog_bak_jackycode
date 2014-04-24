---
layout: post
title: "数据科学之机器学习2：线性回归1"
date: 2014-03-30 19:02:30 +0800
comments: true
categories: DataScience MachineLearning Regression
---

![artical 16](/images/artical/artical16.jpg)
<!-- more -->

*“文章原创，转载请注明出处”*

***

#### 一、回归分析

***

在统计分析中，最大的两支应该算是相关分析和回归分析。而回归分析应该是统计学的核心。回归分析，就是研究因变量$y$与自变量$x$之间的关系，存在条件数学期望：$f(x)=E(y\|x)$。此时有：$y=f(x)+\varepsilon$，一般假设$\varepsilon \sim N(0,\sigma^2)$。

回归分析有很多变种：简单线性回归；多项式回归；Logistic回归；非参数回归；非线性回归等等。本篇就介绍最简单的线性回归，首先来看看一元线性回归。

***

#### 二、一元线性回归

***

对于一元线性回归来说，$f(x)$就是线性的，则有：$f(x)=E(y\|x)=\beta\_0 + \beta\_1 x$。通过已知的数据，可以估计出$\beta\_0,\beta\_1$的估计值：$\hat{\beta}\_0,\hat{\beta}\_1$。那么就有$y$的预测值：$\hat{y} = \hat{\beta}\_0 + \hat{\beta}\_1 x$。

***

##### 1. 如何计算$\beta\_0,\beta\_1$的估计值$\hat{\beta}\_0,\hat{\beta}\_1$呢？

***

定义离差平方和：

$$Q(\beta_0,\beta_1) = \sum_{i=1}^{n}(y_i-f(x_i))^2$$

显然，我们希望$f(x\_i)$的值与真实值$y\_i$越接近越好。那么就是需要离差平方和越小越好。则得到目标：

$$ \min_{\beta_0,\beta_1}{\sum_{i=1}^{n}(y_i-f(x_i))^2} $$

如何寻找$\hat{\beta}\_0,\hat{\beta}\_1$使得上面方程达到最小呢？这个就需要对其对$\hat{\beta}\_0,\hat{\beta}\_1$求偏导，得到：

$$\frac{\partial Q}{\partial \beta_0}=-2\sum_{i=1}^{n}{(y_i-\beta_0-\beta_1x_i)}$$

$$\frac{\partial Q}{\partial \beta_1}=-2\sum_{i=1}^{n}{(y_i-\beta_0-\beta_1x_i)x_i}$$

令上述两式都等于0，计算得到：

$$\hat{\beta}_0=\overline{y}-\hat{\beta}_1\overline{x}$$

$$\hat{\beta}_1=\frac{\sum_{i=1}^{n}{(x_i-\overline{x})(y_i-\overline{x})}}{\sum_{i=1}^{n}{(x-\overline{x})^2}}$$

这样就得到$\beta\_0,\beta\_1$的估计值$\hat{\beta}\_0,\hat{\beta}\_1$。这个方法就叫做OLS，即普通最小二乘(ordinary least squares)。

***

##### 2. R语言实现

***

在R语言中有自带的函数可以处理线性回归，那就是`lm`函数。这里使用自带的数据`cars`做演示：

``` r
> attach(cars) # 使用数据集cars，与with函数类似
> lingre <- lm(dist ~ speed)
> summary(lingre)

Call:
lm(formula = dist ~ speed)

Residuals:
    Min      1Q  Median      3Q     Max 
-29.069  -9.525  -2.272   9.215  43.201 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept) -17.5791     6.7584  -2.601   0.0123 *  
speed         3.9324     0.4155   9.464 1.49e-12 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 15.38 on 48 degrees of freedom
Multiple R-squared:  0.6511,	Adjusted R-squared:  0.6438 
F-statistic: 89.57 on 1 and 48 DF,  p-value: 1.49e-12

> plot(dist ~ speed, pch=4) # 画出散点图
> abline(lingre, col='red') # 添加拟合直线
> detach(cars) # 使用完记得释放
```

从这里可以得到回归方程：$\hat{dist} = -17.5791 + 3.9324 \times speed$。（对于其它的结果是什么意思，可以去查看线性回归的相关书籍）

另外，得到拟合直线的图像：

![lingre_one](\images\a16\lingre_one.jpg)

***

#### 三、多元线性回归

***

对于多元线性回归来说，其计算方式与一元线性回归类似，区别在于，多元的时候需要利用矩阵来处理。首先看一下回归模型：

$$ y = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \dots + \beta_p x_p + \varepsilon $$

其中$p$代表自变量的个数。

若取$x^T\_0=[1, 1, \dots, 1]\_{1 \times n}$，则可将上述模型改写成：$$y=X\beta+\varepsilon$$。其中：

$$y^T=[y_1,y_2,\dots,y_n], X=[x_0,x_1,\dots,x_p], \beta^T=[\beta_0,\beta_1,\dots,\beta_p], \varepsilon^T=[\varepsilon_1,\varepsilon_2,\dots,\varepsilon_n]$$

其中$$x^T_i=[x_{1i},x_{2i},\dots,x_{ni}]$$。

这样我们就可以将离差平方和$$\sum_{i=1}^{n}{(y_i-\beta_0 + \beta_1 x_1 + \beta_2 x_2 + \dots + \beta_p x_p)^2}$$写成矩阵形式：

$$(y-X\beta)^T(y-X\beta)$$

求导可得：$$-2X^T(y-X\beta)$$(这里用到矩阵求导的知识，一般介绍**线性模型**的书籍中会讲到；当然也可以直接对上面不是矩阵形式的离差平和求导)。令其等于0，可得：

$$\hat{\beta} = (X^TX)^{-1}X^Ty$$

***

##### R语言实现

***

对于R语言的实现，依旧使用`lm`函数：

``` r
lingre_mul <- lm(y ~ x1 + x2, data=datasets)
summary(lingre_mul)
```

这里就不再用实际数据去演示了。

***

#### 四、最后

***

至此，就把线性回归的基础内容介绍完了。但其实线性回归还存在很多其它的问题。比如说回归诊断（就是检查回归的效果），变量选择等等等等。感兴趣的话，可以找本讲线性回归的书看看，有很多！