---
layout: post
title: "数据科学20：文本挖掘2"
date: 2014-06-26 15:02:09 +0800
comments: true
categories: DataScience TextMining
---

![article 42](/images/article/article42.jpg)
<!-- more -->

图片由本文中数据生产~

*“文章原创，转载请注明出处”*

***

一、对词条-文档矩阵的操作
-------------------------

* * * * *

在'tm'包中，提供了一些常用的函数，可以对得到的Document Term
Matrix进行一些操作。当然，我们也可以使用自己的方式，对该矩阵进行一些探索，比如，我们先来看看词条的频数：

### 1.1 词条频数

* * * * *

``` r
freq <- colSums(as.matrix(dtm))
length(freq)

## [1] 780
```

如果我们想看看，哪些频数是大于等于30的，我们可以：

``` r
names(freq[freq >= 30])

## [1] "market" "mln"    "oil"    "opec"   "price"  "said"
```

但其实，在'tm'包中有自带的函数可以解决：

``` r
findFreqTerms(dtm, lowfreq = 30)

## [1] "market" "mln"    "oil"    "opec"   "price"  "said"
```

如果得到了每个词条的频数，我们可以列一个表查看一些其它的东西：

``` r
ord <- order(freq, decreasing = TRUE)
## 查看频数排在前十的词条及其频数
head(freq[ord], 10)

##    oil   said  price   opec    mln market barrel   last    bpd   dlrs
##     86     73     63     47     31     30     26     24     23     23

## 查看频数的频数
tail(table(freq), 10)

## freq
## 21 23 24 26 30 31 47 63 73 86
##  2  2  1  1  1  1  1  1  1  1
```

### 1.2 相关性

* * * * *

除了词条的频数，我们大都还会对词条之间的相关性感兴趣。如何计算，其实很简单：

``` r
corr <- cor(as.matrix(dtm))
corr[1:5, 1:5]

##           abdul     abil      abl   abroad   accept
## abdul   1.00000 -0.08812 -0.05263 -0.05263 -0.05263
## abil   -0.08812  1.00000 -0.08812  0.79309 -0.08812
## abl    -0.05263 -0.08812  1.00000 -0.05263 -0.05263
## abroad -0.05263  0.79309 -0.05263  1.00000 -0.05263
## accept -0.05263 -0.08812 -0.05263 -0.05263  1.00000

dim(corr)

## [1] 780 780
```

比如我们想知道，与词条'oil'相关性超过0.8的词条有哪些：

``` r
op <- options()
options(digits = 2)
name <- setdiff(names(corr[, "oil"][corr[, "oil"] > 0.8]), "oil")
value <- corr[name, "oil"]
matrix <- matrix(value[order(value, decreasing = TRUE)])
rownames(matrix) = names(value)
colnames(matrix) = "oil"
matrix

##       oil
## name 0.87
## opec 0.81
## tri  0.81
## want 0.81

options(op)
```

但其实，'tm'包中提供了现成函数：

``` r
findAssocs(dtm, "oil", 0.8)

##       oil
## opec 0.87
## name 0.81
## tri  0.81
## want 0.81
```

可以看到，这一个函数实现了上面我们那一长串的功能。**但是，我觉得，提供的现成函数有好处，但还是需要自己想想如何实现这个现成的函数。这不仅对概念理解有帮助，也对程序编写以及了解别人写的函数有更加深刻的理解。**

### 1.3 删减稀疏条目

* * * * *

大部分时候，Document Term
Matrix是一个很大的矩阵，而且是行数远小于列数。也就是说，这个矩阵实际上是很稀疏的，不妨来看看我们这个只有20个文档的矩阵，稀疏程度如何：

``` r
Sparse <- length(which(matrix(dtm) == 0))
Sparse

## [1] 14030

Non <- length(which(matrix(dtm) != 0))
Non

## [1] 1570

sprintf("%d%%", round(Sparse/(Non + Sparse), 2) * 100)

## [1] "90%"
```

可以看到，矩阵中等于零的数目是14030，不为零的为1570，稀疏程度达到了90%，显然是非常稀疏的。当然，其实这个可以不用自己算的：

``` r
dtm

## <<DocumentTermMatrix (documents: 20, terms: 780)>>
## Non-/sparse entries: 1570/14030
## Sparsity           : 90%
## Maximal term length: 13
## Weighting          : term frequency (tf)
```

上面的输出，前两个就知道它的意思了。那么另外两个呢？“Maximal term
length”，顾名思义，就是最长的那个词条的长度。不妨来看一下：

``` r
name <- colnames(as.matrix(dtm))
name.length <- sapply(name, nchar)
max.name <- name[name.length == max(name.length)]
max.value <- name.length[max.name]
max.value

## responsibilit
##            13
```

可以看到是吻合的。至于最后一个“Weighting”，是指Document Term
Matrix中的每一条目的值是如何计算的。这里用的是“term
frequency”，就是词条频率，简写为“tf”。除了这个，还有一些其它计算方法，比如：tf-idf，即term
frequency-inverse document frequency。这个以后再说。

* * * * *

**回到主题：**计算出了稀疏条目的比例，证实了矩阵的确是很稀疏的，那么下面就是要去删除一些条目。有些条目在很少的文档中出现，甚至只在一篇文档中出现了。一般来说，删除这样的条目不会对矩阵的信息继承带来显著的影响。

``` r
remove.sparseterms <- function(dtm, per) {
    dtm.Matrix <- as.matrix(dtm)
    term.per <- apply(dtm.Matrix, 2, function(item) length(which(item == 0))/length(item))
    name <- names(term.per[which(term.per < per)])
    return(dtm.Matrix[, name])
}
remove.sparseterms(dtm, 0.4)

##      Terms
## Docs  barrel oil one price reuter said
##   127      2   5   0     5      1    3
##   144      0  12   1     6      2   11
##   191      1   2   0     2      1    1
##   194      1   1   1     2      1    1
##   211      0   1   0     0      1    3
##   236      4   7   1     8      1   10
##   237      0   4   1     1      1    1
##   242      0   3   2     2      1    3
##   246      1   5   1     2      1    5
##   248      3   9   1    10      1    7
##   273      3   5   1     5      1    8
##   349      0   4   0     1      1    1
##   352      1   5   0     5      1    2
##   353      1   4   1     2      1    1
##   368      0   3   0     0      1    3
##   489      3   4   2     3      1    2
##   502      3   5   2     3      1    2
##   543      1   3   1     3      1    4
##   704      0   3   4     3      1    4
##   708      2   1   0     0      1    1
```

这边的计算就是：检查词条的稀疏程度是否低于给定的系数`per`，如果是，就保留该词条；如果不是，则舍弃。

当然，在'tm'包中也有自带的函数可以解决：

``` r
rsterms <- removeSparseTerms(dtm, 0.4)
inspect(rsterms)

## <<DocumentTermMatrix (documents: 20, terms: 6)>>
## Non-/sparse entries: 103/17
## Sparsity           : 14%
## Maximal term length: 6
## Weighting          : term frequency (tf)
##
##      Terms
## Docs  barrel oil one price reuter said
##   127      2   5   0     5      1    3
##   144      0  12   1     6      2   11
##   191      1   2   0     2      1    1
##   194      1   1   1     2      1    1
##   211      0   1   0     0      1    3
##   236      4   7   1     8      1   10
##   237      0   4   1     1      1    1
##   242      0   3   2     2      1    3
##   246      1   5   1     2      1    5
##   248      3   9   1    10      1    7
##   273      3   5   1     5      1    8
##   349      0   4   0     1      1    1
##   352      1   5   0     5      1    2
##   353      1   4   1     2      1    1
##   368      0   3   0     0      1    3
##   489      3   4   2     3      1    2
##   502      3   5   2     3      1    2
##   543      1   3   1     3      1    4
##   704      0   3   4     3      1    4
##   708      2   1   0     0      1    1
```

与之前的结果相同。

二、相关的绘图
--------------

* * * * *

介绍完Document Term
Matrix之后，其实已经可以开始使用模型去处理了。之前介绍过聚类、分类啊等等的内容，有了这个矩阵之后，其实都可以去做了。不过，在建立模型之前，我想先说说有关绘图的问题。

我们前面算了很多的量，比如频数啊、相关系数啊等等。但其实，我们大多时候需要用图形将它们展示出来。在R中，我们可以使用`Rgraphviz`包来绘制词条之间的关系图：

``` r
require(Rgraphviz)
plot(dtm, terms = findFreqTerms(dtm, lowfreq = 20))
```

![plot of chunk
TM2.2.1](/images/a42/TM2.2.11.png)

``` r
## 指定相关性必须在0.5之上才可连线
plot(dtm, terms = findFreqTerms(dtm, lowfreq = 20), corThreshold = 0.5)
```

![plot of chunk
TM2.2.1](/images/a42/TM2.2.12.png)

在默认不指定`terms`和`corThreshold`参数的情况下，`plot`函数会绘制随机20个词条，相关性至少0.7的图形。

***

当然，我们还可以绘制熟悉的频数直方图：

``` r
freq <- sort(colSums(as.matrix(dtm)), decreasing = TRUE)
tf <- data.frame(term = names(freq), freq = freq)

g <- ggplot(subset(tf, freq > 20), aes(term, freq))
g <- g + geom_bar(stat = "identity", aes(fill = freq))
print(g)
```

![plot of chunk TM2.2.2](/images/a42/TM2.2.2.png)

***

另外一个比较流行好看的图，应该就是文本云(wordcloud)了，可以使用`wordcloud`包实现：

``` r
require(wordcloud)
wordcloud(names(freq), freq, min.freq = 5, max.words = 80)
```

![plot of chunk
TM2.2.3](/images/a42/TM2.2.31.png)

``` r
## 添加颜色
require(RColorBrewer)
wordcloud(names(freq), freq, min.freq = 5, max.words = 80, colors = brewer.pal(8,
    "Paired"))
```

![plot of chunk
TM2.2.3](/images/a42/TM2.2.32.png)

当然，`wordcloud()`函数提供了很多选项，可以自行查看，我就不多讲了。
