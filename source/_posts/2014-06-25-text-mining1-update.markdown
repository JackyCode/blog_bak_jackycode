---
layout: post
title: "数据科学19：文本挖掘1-更新"
date: 2014-06-25 19:21:26 +0800
comments: true
categories: DataScience TextMining
---

![article 41](/images/article/article41.jpg)
<!-- more -->

图片由本文中数据生产~

*“文章原创，转载请注明出处”*

***

前几天，R中的’tm’包从0.5-10更新到了0.6版本。其中更新了不少的东西，对于上一篇中的代码，已经是不能够正确运行了。所以这里需要先更新一下上一篇中的一些代码，正好可以回顾一些之前的流程。

``` r
## Load Packages needed
require(tm)
require(SnowballC)
require(ggplot2)

## Loading Corpus 这里直接读成文本格式
xml.path <- system.file("texts", "crude", package = "tm")
docs <- Corpus(DirSource(xml.path), readerControl = list(reader = readReut21578XMLasPlain))

## 查看
docs[[1]]
```

```
## <<PlainTextDocument (metadata: 15)>>
## Diamond Shamrock Corp said that
## effective today it had cut its contract prices for crude oil by
## 1.50 dlrs a barrel.
##     The reduction brings its posted price for West Texas
## Intermediate to 16.00 dlrs a barrel, the copany said.
##     "The price reduction today was made in the light of falling
## oil product prices and a weak crude oil market," a company
## spokeswoman said.
##     Diamond is the latest in a line of U.S. oil companies that
## have cut its contract, or posted, prices over the last two days
## citing weak oil markets.
##  Reuter
```

``` r
## 去除特殊字符 上一篇中的代码已不能胜任
## 因为上一篇中的代码会修改Corpus中每个文档的class
## 所以这里需要使用一个新的函数'content_transformer'来保证class还是TextDoument
removeSpecialCharacter <- function(x, pattern) {
    gsub(pattern, " ", x)
}
docs <- tm_map(docs, content_transformer(removeSpecialCharacter), "[@/-]")
docs[[1]]
```

```
## <<PlainTextDocument (metadata: 15)>>
## Diamond Shamrock Corp said that
## effective today it had cut its contract prices for crude oil by
## 1.50 dlrs a barrel.
##     The reduction brings its posted price for West Texas
## Intermediate to 16.00 dlrs a barrel, the copany said.
##     "The price reduction today was made in the light of falling
## oil product prices and a weak crude oil market," a company
## spokeswoman said.
##     Diamond is the latest in a line of U.S. oil companies that
## have cut its contract, or posted, prices over the last two days
## citing weak oil markets.
##  Reuter
```

``` r
## 转换成小写字母 与上面问题类似，需要添加'content_transformer'函数
docs <- tm_map(docs, content_transformer(tolower))
docs[[1]]
```

```
## <<PlainTextDocument (metadata: 15)>>
## diamond shamrock corp said that
## effective today it had cut its contract prices for crude oil by
## 1.50 dlrs a barrel.
##     the reduction brings its posted price for west texas
## intermediate to 16.00 dlrs a barrel, the copany said.
##     "the price reduction today was made in the light of falling
## oil product prices and a weak crude oil market," a company
## spokeswoman said.
##     diamond is the latest in a line of u.s. oil companies that
## have cut its contract, or posted, prices over the last two days
## citing weak oil markets.
##  reuter
```

``` r
## 去除数字
docs <- tm_map(docs, removeNumbers)
docs[[1]]
```

```
## <<PlainTextDocument (metadata: 15)>>
## diamond shamrock corp said that
## effective today it had cut its contract prices for crude oil by
## . dlrs a barrel.
##     the reduction brings its posted price for west texas
## intermediate to . dlrs a barrel, the copany said.
##     "the price reduction today was made in the light of falling
## oil product prices and a weak crude oil market," a company
## spokeswoman said.
##     diamond is the latest in a line of u.s. oil companies that
## have cut its contract, or posted, prices over the last two days
## citing weak oil markets.
##  reuter
```

``` r
## 去除停止词
docs <- tm_map(docs, removeWords, stopwords("english"))
docs[[1]]
```

```
## <<PlainTextDocument (metadata: 15)>>
## diamond shamrock corp said
## effective today   cut  contract prices  crude oil
## . dlrs  barrel.
##      reduction brings  posted price  west texas
## intermediate  . dlrs  barrel,  copany said.
##     " price reduction today  made   light  falling
## oil product prices   weak crude oil market,"  company
## spokeswoman said.
##     diamond   latest   line  u.s. oil companies
##  cut  contract,  posted, prices   last two days
## citing weak oil markets.
##  reuter
```

``` r
## 去除标点符号
docs <- tm_map(docs, removePunctuation)
docs[[1]]
```

```
## <<PlainTextDocument (metadata: 15)>>
## diamond shamrock corp said
## effective today   cut  contract prices  crude oil
##  dlrs  barrel
##      reduction brings  posted price  west texas
## intermediate   dlrs  barrel  copany said
##      price reduction today  made   light  falling
## oil product prices   weak crude oil market  company
## spokeswoman said
##     diamond   latest   line  us oil companies
##  cut  contract  posted prices   last two days
## citing weak oil markets
##  reuter
```

``` r
## 去除多余的空格
docs <- tm_map(docs, stripWhitespace)
docs[[1]]
```

```
## <<PlainTextDocument (metadata: 15)>>
## diamond shamrock corp said effective today cut contract prices crude oil dlrs barrel reduction brings posted price west texas intermediate dlrs barrel copany said price reduction today made light falling oil product prices weak crude oil market company spokeswoman said diamond latest line us oil companies cut contract posted prices last two days citing weak oil markets reuter
```

``` r
## 词干化
docs <- tm_map(docs, stemDocument)
docs[[1]]
```

```
## <<PlainTextDocument (metadata: 15)>>
## diamond shamrock corp said effect today cut contract price crude oil dlrs barrel reduct bring post price west texa intermedi dlrs barrel copani said price reduct today made light fall oil product price weak crude oil market compani spokeswoman said diamond latest line us oil compani cut contract post price last two day cite weak oil market reuter
```

``` r
## 创建Document Term Matrix
dtm <- DocumentTermMatrix(docs)
dtm
```

```
## <<DocumentTermMatrix (documents: 20, terms: 780)>>
## Non-/sparse entries: 1570/14030
## Sparsity           : 90%
## Maximal term length: 13
## Weighting          : term frequency (tf)
```

``` r
inspect(dtm[1:5, 1:10])
```

```
## <<DocumentTermMatrix (documents: 5, terms: 10)>>
## Non-/sparse entries: 2/48
## Sparsity           : 96%
## Maximal term length: 7
## Weighting          : term frequency (tf)
##
##      Terms
## Docs  abdul abil abl abroad accept accord across activ add address
##   127     0    0   0      0      0      0      0     0   0       0
##   144     0    2   0      0      0      0      0     0   0       4
##   191     0    0   0      0      0      0      0     0   0       0
##   194     0    0   0      0      0      0      0     0   0       0
##   211     0    0   0      0      0      0      0     0   0       0
```

到这里，就已经完成了上一篇所做的事情了。留意一下，也可以发现，'tm'包更新过之后，处理的结果与之前有一些不同，可以自己动手试试看，看看哪里不一样？