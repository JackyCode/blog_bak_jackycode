---
layout: post
title: "R语言绘图1：绘图基础"
date: 2014-05-24 14:48:02 +0800
comments: true
categories: DataVisualization
---

![article 36](/images/article/article36.jpg)
<!-- more -->

图片为本文中的某一张~~

*“文章原创，转载请注明出处”*

***

以数字和文字展示统计分析的结果，很多时候并不直观，也不能吸引人！而且，从数字和文字中寻找规律、异常等等的东西也并不容易。但是，对于图形来说，这些就容易解决得多了。

图形的作用不言而喻，就不想多说了。R语言具有强大的绘图功能，这篇就从最基础的绘图工具讲起，谈谈如何在R中绘制各种各样的图形。

#### 一、点
***

散点图，无论是在处理数据之前，还是在处理数据的过程中，它都显得非常的重要。在R语言中，绘制散点图非常简单，这就需要使用到函数**`plot()`**。

构造以下的数据以作演示使用：


```r
set.seed(1234)
x <- sample(1:100, 80, replace = FALSE)
y <- 2 * x + 20 + rnorm(80, 0, 10)
```


###### 1. 首先绘制一个最简单的散点图：


```r
plot(x = x, y = y)  ## 或者使用: plot(x, y)
```

![plot of chunk 2](/images/a36/2.png)


其中，`x = x`左侧的`x`表示横坐标，右侧的是数据名，`y`的相同。当然，左侧的`x`和`y`都是可以省略的。也可以使用`plot(<formula>)`这样的形式去绘制散点图：


```r
plot(y ~ x)
```

![plot of chunk 3](/images/a36/3.png)


##### 2. 添加标题和标签


```r
plot(x, y, xlab = "name of x", ylab = "name of y", main = "Scatter Plot")
```

![plot of chunk 4](/images/a36/4.png)


##### 3. 设置坐标界限


```r
## 可先用以下函数查看x和y的取值范围 range(x) range(y)
plot(x, y, xlab = "name of x", ylab = "name of y", main = "Scatter Plot", xlim = c(1,
    100), ylim = c(0, 250))
```

![plot of chunk 5](/images/a36/5.png)


##### 4. 更改字符

默认情形下，绘图字符为空心点，可以使用`pch`选项参数进行更改：


```r
plot(x, y, xlab = "name of x", ylab = "name of y", main = "Scatter Plot", xlim = c(1,
    100), ylim = c(0, 250), pch = 19)
```

![plot of chunk 6](/images/a36/6.png)


`pch`的取值与其对应的图形字符，也可以使用`plot`绘制：


```r
a <- rep(1:5, each = 5)
b <- rep(1:5, 5)
plot(a, b, pch = 1:25)
text(a, b, 1:25, pos = 2)
```

![plot of chunk 7](/images/a36/7.png)


##### 5. 更改颜色

默认情况下，R绘制的图像是黑白的。但其实，R中有若干和颜色相关的参数：


```r
plot(x, y, main = "Plot", sub = "Scatter Plot", col = "red", col.axis = "green",
    col.lab = "blue", col.main = "#999000", col.sub = "#000999", fg = "gray",
    bg = "white")
```

![plot of chunk 8](/images/a36/8.png)


其中：

参数    |   作用
:------:|:----------:
col     |   绘图字符的颜色
col.axis|   坐标轴文字颜色
col.lab |   坐标轴标签颜色
col.main|   标题颜色
col.sub |   副标题颜色
fg      |   前景色
bg      |   背景色

##### 6. 更改尺寸

与颜色类似，存在若干参数可以用来设置图形中元素的尺寸，而且与上表中设置颜色的参数相对应，只需将`col`更换成`cex`即可：


```r
plot(x, y, main = "Plot", sub = "Scatter Plot", cex = 0.5, cex.axis = 1, cex.lab = 0.8,
    cex.main = 2, cex.sub = 1.5)
```

![plot of chunk 9](/images/a36/9.png)


#### 二、线

有时候，我们不仅需要散点图，更需要折线图，比如在时间序列当中：


```r
t <- 1:50
set.seed(1234)
v <- rnorm(50, 0, 10)
plot(t, v, type = "l")
```

![plot of chunk 10](/images/a36/10.png)


##### 1. 更改线条类型


R中提供了很多类型的线条，可以通过`lty`选项来设定，如下：


```r
a <- c()
plot(a, xlim = c(0, 10), ylim = c(1, 6), xlab = "", ylab = "lty")
for (i in 1:6) {
    abline(h = i, lty = i)
}
```

![plot of chunk 11](/images/a36/11.png)


##### 2. 更改颜色

与前面更改点的颜色方法相同！

##### 3. 线条宽度


```r
plot(t, v, type = "l", lwd = 2)
```

![plot of chunk 12](/images/a36/12.png)



##### 4. 点与线

有时候，我们还需要将点突出出来，此时需要利用`type`选项：


```r
plot(t, v, type = "b")
```

![plot of chunk 13](/images/a36/13.png)


`type`选项可以指定`plot()`函数绘制的类型，可以使用`help(plot)`查看。

#### 三、散点图与平滑线

在做线性回归时，我们常常会在散点图中添加一条拟合直线以查看效果：


```r
model <- lm(y ~ x)
plot(x, y)
abline(model, col = "blue")
```

![plot of chunk 14](/images/a36/14.png)


有时候，我们需要在散点图上画一条拟合的平滑线，一般使用loess：


```r
plot(x, y)
model_loess <- loess(y ~ x)
fit <- fitted(model_loess)
ord <- order(x)
lines(x[ord], fit[ord], lwd = 2, lty = 2, col = "blue")
```

![plot of chunk 15](/images/a36/15.png)


当然，在R语言中要实现上面的图形有很多简单地方法，这里介绍两个：

##### 1. 使用`car`包中的`scatterplot()`函数


```r
library(car)
scatterplot(y ~ x, spread = FALSE, lty = 1, lwd = 2, smoother.args = list(lty = 2,
    lwd = 2), smoother = loessLine)
```

![plot of chunk 16](/images/a36/16.png)


其中，直线为线性拟合，虚线为loess曲线拟合，边界为箱线图！

在函数中`smoother.args`选项指定曲线拟合的参数；`smoother`选项指定是否使用曲线拟合，以及用什么方法进行曲线拟合。当然，如果不需要曲线平滑拟合，那么可以将`smoother = FALSE`。

##### 2. 使用`ggplot2`包

`ggplot2`是学习R语言制图的一座大山，它提供了一个基于全面而连贯的语法的绘图系统。功能非常强大，而且弥补了R中自带图形绘制的很多缺点。唯一的问题，学习`ggplot2`的成本比较大，需要理解很多概念，也需要学习新的语法。

**如果你是刚刚入门R语言，对于R语言基础的绘图工具还不是很熟的话，建议先跳过这一段，不要去弄懂其中的意思，试试效果就可以了！等对R语言基础绘图有了一定的了解之后，再来学习ggplot2，个人觉得会比较好一些！**

***当然，如果你跟我一样喜欢折腾，不怕纠结，我不介意你一块学！不过，你遇到什么样的问题，那我就不保证了，反正我当时好长一段时间是蛮惨的！***

使用这个包，实现的方法一般有两种，就是使用`qplot()`函数和`ggplot()`函数：


```r
qplot(x, y, geom = c("point", "smooth"), method = "lm", se = FALSE) + geom_smooth(method = "loess",
    color = "red", se = FALSE, lty = 2)
```

![plot of chunk 17](/images/a36/17.png)


或者：


```r
dataSet <- data.frame(x = x, y = y)
ggplot(aes(x, y), data = dataSet) + geom_point() + geom_smooth(method = "lm",
    color = "green", lty = 1, se = FALSE) + geom_smooth(method = "loess", color = "red",
    lty = 2, se = FALSE)
```

![plot of chunk 18](/images/a36/18.png)
