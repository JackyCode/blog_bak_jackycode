---
layout: post
title: "R语言绘图4：R中的绘图系统"
date: 2014-06-08 15:48:27 +0800
comments: true
categories: DataVisualization
---

![article 39](/images/article/article39.jpg)
<!-- more -->

图片为本文中的某一张~~

*“文章原创，转载请注明出处”*

***


前面三篇大致介绍了绘图的基础以及一些基础图形的绘制，其中重点使用R自带的基础绘图系统实现，部分也使用了`ggplot2`做了简单地演示。其实，在R中存在四套绘图系统，分别是：**基础绘图系统**(The Base Plotting System)、**grid图形系统**、**Lattice绘图系统**(The Lattice System)以及**ggplot2绘图系统**(The ggplot2 System)。

grid图形系统提供了一种比标准图形系统更低水平的方法，使用者可以在图形设备上随意创建矩形区域，然后再该区域定义坐标系统，其后使用绘图的基础单元来控制图形！由此可见，grid图形非常的灵活，但是grid包没有提供生成统计图形，甚至没有提供完整绘图的函数，所以，一般来说，我们不会直接使用grid。

Lattice绘图系统是基于grid图形系统的，我们在使用Lattice的时候，其实就会使用grid，只是不会直接去调用而已。这一篇，就简单看看这三个绘图系统：基础绘图系统、Lattice绘图系统以及ggplot2绘图系统。

### 1. 基础绘图系统
***

基础绘图系统，即是R语言自带的绘图系统。R中核心的绘图系统主要基于以下两个包：

1. **graphics**包：此包包含了基础绘图系统的绘图函数，比如：`plot()`，`hist()`等等；
2. **grDevices**包：此包包含了各类型的图形设备(graphics devices)，包括：*X11*, *PDF*, *PNG*等等。

基础绘图系统总是从一幅空画布开始绘制图形，**首先使用`plot()`这样的函数在空画布上画出图形，然后使用注释函数对图形进行添加或者修改**，一般来说就是添加或者修改显示的文本、辅助线、辅助点以及标题等等。对于这样的一个流程，很显然，它可以很方便我们去使用，因为这样的流程可以完全将我们的思路反应出来。**它就是通过一系列的R语言命令，一步一步地按照心中预想的绘图方式将其绘制出来**。

这样的绘图方式在给予我们方便的同时，也带来了一些弊端。首先，我们必须事先安排好图形应该如何绘制，因为一旦我们开始绘制就不能后退了；此外，当一副图形绘制完成，我们就不能再对其进行修改了，因为基础绘图系统没有绘图语言(graphical "language")（这个可以跟ggplot2进行一个比对，就能明白其中的意思了！）。

基础绘图一般来说：**就是使用一系列单独的函数调用，将图形的各个零散的部分组合起来**。这种方式比较简单，也比较切合自己的思路。正因为如此，我个人比较喜欢在分析问题的最开始，使用它进行一些简单地图形绘制，将其绘制到屏幕上，让我能够直观先去看看数据的基本状况。

利用`mtcar`数据集，做个示例：


```r
## 取出感兴趣的数据
dataSet <- mtcars[c("mpg", "disp", "carb")]

## 转换数据的格式
dataSet <- transform(dataSet, carb = factor(carb))

## 绘图查看数据 散点图
plot(dataSet$mpg, dataSet$disp, xlab = "mpg", ylab = "disp")
title("The Scatter Plot")
model <- lm(disp ~ mpg, data = dataSet)
abline(model, col = "red")
```

![plot of chunk 4base](/images/a39/4base1.png)

```r

## 加上条件
with(dataSet, plot(mpg, disp, type = "n"))
for (i in unique(dataSet$carb)) {
    with(subset(dataSet, carb == i), points(mpg, disp, col = i))
}
## 对于carb == 4的数据，添加拟合直线
model1 <- lm(disp ~ mpg, data = subset(dataSet, carb == 4))
abline(model1, lty = 2, lwd = 2)
```

![plot of chunk 4base](/images/a39/4base2.png)


### 2. Lattice
***

Lattice一般会使用以下两个包：

1. **lattice**包：包含与基础绘图独立的，能够生成Trellis graphics的代码，包括`xyplot`,`bwplot`等等；
2. **grid**包：lattice包建立在此包之上，在使用中很少直接调用这个包。

Lattice与基础绘图系统的一个非常明显的区别在于：**所有的绘图以及注释都在一次函数调用中完成**。

Lattice很擅长绘制需要分类的图形，比如说，想要根据不同的level，绘制y与x的散点图。这个在基础绘制系统中还是蛮麻烦的，但是在Lattice中就比较容易。也就是说，**lattice通过一维、二维或三维的条件绘图(即是Trellis)，对多元变量关系进行直观展示**。

由于其所有的绘图与注释都需要在一次函数调用中完成，所以在使用之前就需要做好准备，因为一旦图形绘制好，就什么也不能添加了。

接着上面的数据集做示例：


```r
xyplot(disp ~ mpg, data = dataSet)
```

![plot of chunk 4lattice](/images/a39/4lattice1.png)

```r
xyplot(disp ~ mpg | carb, data = dataSet, layout = c(6, 1))
```

![plot of chunk 4lattice](/images/a39/4lattice2.png)

```r
## 使用panel function
xyplot(disp ~ mpg | carb, data = dataSet, panel = function(x, y, ...) {
    panel.xyplot(x, y, ...)
    panel.abline(h = median(y), lty = 2, col = "gray")
    panel.lmline(x, y, col = "red")
})
```

![plot of chunk 4lattice](/images/a39/4lattice3.png)


**更多有关Lattice的内容可以看Springer出的这本书：'Lattice: Multivariate Data Visualization with R'。**

### 3. ggplot2
***

以前就说过，ggplot2是基于一种全面的图形语法，提供了一种全新的图形创建方法。能够自动处理位置、文本等等注释，也能够按照需求自定义设置。默认情况下有很多选项以供选择，在不设置时会直接使用默认值。

在ggplot2中，最简单最基础的绘图函数就是`qplot()`，我们可以来试试这个函数：


```r
## 查看qplot函数
str(qplot)
```

```
## function (x, y = NULL, ..., data, facets = NULL, margins = FALSE,
##     geom = "auto", stat = list(NULL), position = list(NULL), xlim = c(NA,
##         NA), ylim = c(NA, NA), log = "", main = NULL, xlab = deparse(substitute(x)),
##     ylab = deparse(substitute(y)), asp = NA)
```

```r
## 散点图
qplot(mpg, disp, data = dataSet)
```

![plot of chunk 4ggplot2](/images/a39/4ggplot21.png)

```r
## 按carb区分 按颜色
qplot(mpg, disp, data = dataSet, color = carb)
```

![plot of chunk 4ggplot2](/images/a39/4ggplot22.png)

```r
### 按形状
qplot(mpg, disp, data = dataSet, shape = carb)
```

![plot of chunk 4ggplot2](/images/a39/4ggplot23.png)

```r
## 添加拟合直线
qplot(mpg, disp, data = dataSet, geom = c("point", "smooth"), method = "lm")
```

![plot of chunk 4ggplot2](/images/a39/4ggplot24.png)

```r
## 使用facets
qplot(mpg, disp, data = dataSet, facets = . ~ carb)
```

![plot of chunk 4ggplot2](/images/a39/4ggplot25.png)

```r
### 添加颜色和形状
qplot(mpg, disp, data = dataSet, facets = . ~ carb, color = carb, shape = carb)
```

![plot of chunk 4ggplot2](/images/a39/4ggplot26.png)


当然，ggpplot2中不止这一点东西。**更多有关ggplot2的内容可以参看Spinger出的这本书：ggplot2:Elegant Graphics for Data Analysis'。**

### 总结
***

一般来说，使用R绘制图形就是使用以上的三种绘图系统。基础的绘图系统跟人的思路相仿，你可以边思考边画图，边做边画，比较方便。Lattice擅长绘制Trellis图形，并且其所有的绘图注释都只能在一个函数中实现。ggplot2则是集中了前两者的元素，更加富有创造性，当然学习成本就稍微高一些。










