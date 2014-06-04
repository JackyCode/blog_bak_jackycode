---
layout: post
title: "R语言绘图3：基础图形2"
date: 2014-06-04 16:53:08 +0800
comments: true
categories: DataVisualization
---

![artical 38](/images/artical/artical38.jpg)
<!-- more -->

图片为本文中的某一张~~

*“文章原创，转载请注明出处”*

***

### 三、直方图
***

前面介绍的两种图形，一般都是用来处理离散数据的。那么对于连续数据，常用的图形就有这里所说的直方图。直方图在横轴上将数据值域划分成若干个组别，然后在纵轴上显示其频数。通过这种方式，可以将连续的点离散化，从而来描述连续型变量的分布。

在R语言中，可以使用`hist()`函数来绘制直方图：


```r
## 构造数据
set.seed(1234)
x <- rnorm(100, 0, 1)
## 设置画布
par(mfrow = c(2, 2))
## 1
hist(x)
box()
## 2 修改颜色，组数
hist(x, breaks = 10, col = "gray")
box()
## 3 添加核密度曲线
hist(x, breaks = 10, freq = FALSE, col = "gray")
lines(density(x), col = "red", lwd = 2)
box()
## 4 添加正态密度曲线
h <- hist(x, breaks = 10, col = "gray")
xfit <- seq(min(x), max(x), length = 100)
yfit <- dnorm(xfit, mean = mean(x), sd = sd(x))
yfit <- yfit * diff(h$mids[1:2]) * length(x)
lines(xfit, yfit, col = "blue", lwd = 2)
box()
```

![plot of chunk hist](/images/a38/hist.png)


当然，可以使用`ggplot2`包中的函数绘制直方图：

先试试`qplot()`函数：


```r
qplot(x, geom = "histogram", binwidth = 0.5, fill = ..count..)
```

![plot of chunk hist_qplot](/images/a38/hist_qplot.png)


再试试`ggplot()`函数：


```r
dataSet <- data.frame(x)
h <- ggplot(data = dataSet, aes(x = x))
h + geom_histogram(aes(fill = ..count..), binwidth = 0.5)
```

![plot of chunk hist_ggplot](/images/a38/hist_ggplot1.png)

```r
## 添加核密度曲线
h + geom_histogram(aes(y = ..density.., fill = ..count..), binwidth = 0.5) +
    geom_density(color = "red")
```

![plot of chunk hist_ggplot](/images/a38/hist_ggplot2.png)

```r
## 添加正态密度曲线
dataSet2 <- data.frame(xfit, yfit)
h + geom_histogram(aes(fill = ..count..), binwidth = 0.5) + geom_line(data = dataSet2,
    aes(x = xfit, y = yfit), color = "red")
```

![plot of chunk hist_ggplot](/images/a38/hist_ggplot3.png)


直方图在统计中是非常重要的，我自己的感觉是：散点图用的最多最广泛，其次就是直方图！散点图之前已经说过，直方图的话，它可以直观地显示出样本数据大致的分布情况（经验分布函数），对选择什么样的分布、什么样的模型等等都有一定的作用。使用核函数以及正态密度曲线与之结合，能够更好地观察出样本数据的一些分布特征，所以，学会绘制直方图是十分重要的。

### 四、箱线图
***

还有一种常用的图形，就是**箱线图**，也称**盒须图**。它通过绘制连续型变量的五个分位数（最大最小值、25%和75%分位数以及中位数），描述变量的分布。


```r
## 构造数据
x1 <- x
x2 <- rnorm(100, 1, 2)
x3 <- rnorm(100, 2, 4)
all_x <- c(x1, x2, x3)
label <- rep(1:3, each = 100)
dataSet3 <- data.frame(x = all_x, label = label)
```


下面开始绘制箱线图：


```r
boxplot(dataSet3$x)
```

![plot of chunk boxplot](/images/a38/boxplot1.png)

```r
## 考虑类别
boxplot(x ~ label, data = dataSet3)
```

![plot of chunk boxplot](/images/a38/boxplot2.png)

