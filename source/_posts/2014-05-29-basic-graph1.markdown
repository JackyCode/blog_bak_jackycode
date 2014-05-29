---
layout: post
title: "R语言绘图2：基础图形1"
date: 2014-05-29 14:50:05 +0800
comments: true
categories: DataVisualization
---

![artical 37](/images/artical/artical37.jpg)
<!-- more -->

图片为本文中的某一张~~

*“文章原创，转载请注明出处”*

***

### 一、 饼图
***

饼图，简单来说，就是将一个圆（或者圆饼）按分类变量分成几块，每一块所占的面积比例就是相对应的变量在总体中所占的比例。

饼图在统计上，其实并不被看好。一般都会推荐使用条形图去代替，因为相比较面积，长度更能让人产生直观的判断。


```r
## 构造一批数据
year <- 2001:2010
set.seed(1234)
counts <- sample(100:500, 10)

## 绘图 在同一张画布上绘制4幅图，对于`par`的使用，可以`?par`
par(mfrow = c(2, 2))

### 1
pie(counts, labels = year)

### 2
pie(counts, labels = year, col = gray(seq(0.1, 1, length = 10)), clockwise = TRUE)

### 3
lb <- paste(year, counts, sep = ":")
pie(counts, labels = lb, col = rainbow(10))

### 4
library(plotrix)
pie3D(counts, labels = year, explode = 0.1)
```

![plot of chunk 1](/images/a37/1.png)


### 二、 条形图
***

刚才我们就提到，很多时候，我们会使用条形图替代饼图。实际上，**条形图就是通过垂直或者水平的条形去展示分类变量的频数**。

对于上面的数据，可以绘制：


```r
par(mfrow = c(2, 2))
barplot(counts, names.arg = year)
barplot(counts, names.arg = year, horiz = TRUE)
barplot(counts, names.arg = year, col = rainbow(10))
barplot(counts, names.arg = year, col = grey(seq(0.1, 1, length = 10)), horiz = TRUE)
```

![plot of chunk 2](/images/a37/2.png)


当每一年有两个变量指标时，可以绘制堆砌条形图和分组条形图：


```r
## 构造新的数据
set.seed(4321)
counts2 <- sample(100:500, 10)
count <- cbind(c1 = counts, c2 = counts2)
rownames(count) <- year

##
par(mfrow = c(1, 1))
barplot(count, col = rainbow(10), legend = rownames(count))
```

![plot of chunk 3](/images/a37/31.png)

```r
barplot(count, col = rainbow(10), besid = TRUE, legend = rownames(count))
```

![plot of chunk 3](/images/a37/32.png)
