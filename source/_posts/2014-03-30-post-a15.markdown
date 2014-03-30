---
layout: post
title: "数据科学之机器学习1：简介"
date: 2014-03-30 16:39:09 +0800
comments: true
categories: DataScience MachineLearning
---

![artical 15](/images/artical/artical15.jpg)
<!-- more -->

*“文章原创，转载请注明出处”*

***

上一篇“[数据科学简介](http://jackycode.github.io/blog/2014/03/27/an-introduction-to-data-science/)”简单地说了一下数据科学是什么，以及它包罗的东西。这一篇打算简单介绍一下**机器学习**，并理一理机器学习中涉及的内容。

***

#### 机器学习的定义

***

一般来说，教科书介绍一样东西，首先会给它下一个确切的定义。不过，对于机器学习的定义，我还真不知道该怎么去下。有太多的版本，太多的述说方式，不知道用哪个好。这里就列举一些我觉得有代表性的，讲的容易懂的那些定义。对于机器学习是什么，看看这些定义，应该就能够有个大致的了解了。

首先，在“[Machine Learning: the art and science of algorithms that make sense of data](http://www.amazon.cn/Machine-Learning-The-Art-and-Science-of-Algorithms-That-Make-Sense-of-Data-Flach-Peter/dp/1107422221/ref=tmm_pap_title_0)”一书中，有这样一个定义：

> Machine Learning is the systematic study of algorithms and systems that improve their knowledge or performance with experience.

这个定义说的比较简单直白，就是说，机器学习就是研究如何通过经验(其实就是数据)去改进性能的算法(这个翻译学得不好，见谅！)。我们可以简单这样理解，机器学习就是研究这么一类算法，通过这类算法呢，系统可以从数据中获取信息来提升自己的性能。最简单的说法就是将数据转换成有用的信息。

在“[Machine Learning: the art and science of algorithms that make sense of data](http://www.amazon.cn/Machine-Learning-The-Art-and-Science-of-Algorithms-That-Make-Sense-of-Data-Flach-Peter/dp/1107422221/ref=tmm_pap_title_0)”书中，还有这么一句话，将机器学习的流程说的很清楚：

> Machine learning is concerned with using the right features to build the right models that achieve the right task.

这句话就是说，机器学习关心的就是：如何通过对特征(数据)建立模型去完成一些任务。在Andrew Ng的机器学习视频中，还列出了Tom Mitchell给出的定义，与上面的说法有些相近：

> A computer program is said to learn from experience E with respect to some task T and some performance measure P, if its performance on T, as measured by P, improves with experience E.

***上一篇博文里曾说，数据科学是统计、计算机科学以及专业知识的结合。而其实，机器学习就是统计与计算机科学的结合。正因如此，机器学习是数据科学的核心。***简单来说，机器学习就是：研究让机器通过学习过去的经验，达到提高处理相同任务能力的算法。

***

#### 机器学习的类别

****

机器学习大致可以分为两类：监督学习(supervised learning)和无监督学习(unsupervised learning)。

如何区分呢？**区别就在于，测试数据有没有给出确切的类别或者结果。若给出了就是监督学习，若没有则是无监督学习**。

比如说回归：其给定的测试样本中，一组自变量的值肯定对应一个确定的因变量的值；再说分类：对于给出的测试样本中的每一个数据，其都会有自己所属的类别。所以回归与分类就属于监督学习的范畴，也就是可以通过既定的数据去做预测。

而无监督学习不同，其给出的数据不存在任何其它信息，比方说聚类，给你一批数据，没有任何信息，让你把数据分成几个类别。一般来说，这时候就需要计算数据之间的相似度(距离)，然后把距离近的归位一类。

*** 

#### 最后

***

到这里，基本上来说，机器学习是干什么的应该有了一些了解了。至于机器学习的分类，监督学习和无监督学习还不明白也不要紧。等弄明白了回归，分类以及聚类等等的是干什么的，自然就明白这两者之间的区别了。
