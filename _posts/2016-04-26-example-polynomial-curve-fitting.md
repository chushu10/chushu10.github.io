---
layout: post
title: "Example: Polynomial Curve Fitting"
date: 2016-04-26
categories: machine-learning
---
这篇文章是我对[*Pattern Recognition and Machine Learning*](http://research.microsoft.com/en-us/um/people/cmbishop/prml/)这本书的读书总结，第一章第一节的内容。这本教材是一本机器学习的入门教材，其经典程度不亚于[*Computer Networks*](https://www.pearsonhighered.com/program/Tanenbaum-Computer-Networks-5th-Edition/PGM270019.html)之于计算机网络。由于以后自己非常想要从事数据挖掘方面的工作，所以从现在开始就要阅读经典，总结经典。

# 1.1 Example: Polynomial Curve Fitting

@(读书笔记)[Machine Learning]

## 先来一堆概念热热身

- 有监督学习（*supervised learning*）

1. 分类（*classification*）：即给定输入向量，又给出了目标向量，并且目标向量是离散的。例如手写数字的识别，输入向量是从手写数字图片中提取的特征，目标向量是0到9的数字。
2. 回归分析（*regression*）：即给定输入向量，又给出了目标向量，但是目标输出是连续的。例如估计化学实验的结果，输入向量是化合物、温度、压力等数据，目标输出则不仅仅包含几个离散的值，而是一系列连续的数据。

- 无监督学习（*unsupervised learning*）：给定输入向量，但是没有给出目标向量。

1. 聚类（*clustering*）：找出数据中具有相似性的集合。
2. 密度估计（*density estimation*）：找出数据的分布特性。

## 简单介绍一下这一节面临的问题

给定一个自变量$ x $，预测该自变量对应的结果$ t $。我们的训练集是这样生成的：
使用一个正弦函数$ \sin(2\pi x) $，然后对其曲线上的每个点都加上一个随机的具有高斯分布（*Gaussian distribution*）的噪声（关于高斯分布，以后的笔记里会提到），再从定义域$ [0, 1] $中均匀地取出10个点加上噪声以后的点，就构成了我们的训练集点，如下图蓝点所示：
![训练集点](https://raw.githubusercontent.com/Choosue/Choosue.github.io/master/img/example-polynomial-curve-fitting-01.png)

也许你想说：晕，这里直接用$ \sin(2\pi x) $去预测不就行了吗？废话！要是知道数据是由$ \sin(2\pi x) $生成的还预测个蛋啊？这里的条件就是假设我们不知道数据是由$ \sin(2\pi x) $生成的啊！

## 向量

集合通常可以用一个向量表示，例如：

1. 我们可以将一个有$ N $个观察量的训练集描述成：

$$ \mathrm{x} \equiv (x_1, x_2, ..., x_N)^{\mathrm{T}} $$

2. 同样的，这$ N $个观测值对应的结果也可以描述成：

$$ \mathrm{t} \equiv (t_1, t_2, ..., t_N)^{\mathrm{T}} $$

这里的$ \mathrm{T} $代表将这里的行向量转置为一个列向量。

## 多项式预测方法

记住一个重要的多项式：

$$ y(x, \mathrm{w}) = w_0 + w_1x + w_2x^2 + ... + w_Mx^M = \sum_{j=0}^Mw_jx^j $$

这里的$ \mathrm{w} $表示向量：

$$ \mathrm{w} =  (w_0, w_1, ..., w_M)^{\mathrm{T}}$$

很容易想到，假设$ M = 10 $，那么我们只要把10个$ x $值代入到方程$ y(x, \mathrm{w}) $即可得到10个等式，进而解出$ w_0, w_1, ..., w_M $，但是问题关键是$ y(x, \mathrm{w}) $的值我们并不知道。

## 误差函数

所以，我们需要一个误差函数（*error function*）来求解$ w_0, w_1, ..., w_M $：

$$ E(\mathrm{w}) = \frac{1}{2}\sum_{n=1}^N\{y(x_n, \mathrm{w}) - t_n\}^2 $$

至于为什么用它，暂时先不深究，只要明白一点：$ E(\mathrm{w}) $是一个非负数，并且当且仅当函数$ y(x, \mathrm{w}) $穿过所有的训练集点，即$ y(x_n, \mathrm{w}) = t_n, n = 0, 1, 2, ..., N $时，$ E(\mathrm{w}) = 0 $。

好了，那么我们可以说：那干脆就取$ E(\mathrm{w}) = 0 $不就好了吗？没错，当$ E(\mathrm{w}) = 0 $时，函数$ y(x, \mathrm{w}) $完美的穿过了所有的训练集点。但是这种完美有点**太合适**了（*over-fitting*，过拟合）。要知道，我们的训练集是有**噪声**的，如果被这些噪声牵着鼻子走，那我们的估计就会不准确。

事实上，我们需要求出$ E(\mathrm{w}) $的一个最接近0的最小值，通过其方程，我们可以知道它是相对于系数向量$ \mathrm{w} $的一元二次方程，那么它的导数就是相对于向量$ \mathrm{w} $的一元一次方程，并且是线性的，一次方程我们肯定可以求解出它的最小值。假设这个最小值的解是向量$ \mathrm{w^\*} $，那么我们将$ \mathrm{w^\*} $代入到$ y(x, \mathrm{w}) $中就得到了我们的预测函数$ y(x, \mathrm{w^\*}) $。下次随便再来一个$ x_n $我们只要带入到这个方程，就能求出$ t_n $。

## 模型选取

经过上述过程以后，我们大致可以算出一个比较不错的匹配了（fitting）。也就是说，我们的函数能够与大多数的训练集点“擦肩而过”，同时也能够尽量准确的预测下一个点，但是这样就够了吗？并不是，我们只是解决了一个问题：也就是在给定了多项式的阶数$ M $时，求出了使得函数$ y(x, \mathrm{w}) $最能够准确预测下一个点的系数$ \mathrm{w} $，现在就要考虑这个多项式的阶$ M $怎么选取了。而这个阶的选取我们又称它为：

模型比较（*model comparison*）或者模型选取（*model selection*）

先说结论，$ M $不是越大越好。

这里我们给出$ M = 1, M = 2, M = 3, M = 9 $四种情况下，函数$ y(x, \mathrm{w}) $的图形，如下图，蓝点代表训练集点，绿色的线条代表$ \sin(2pi x) $函数的图形，红色的线条代表函数$ y(x, \mathrm{w}) $的图形：

![模型比较](https://raw.githubusercontent.com/Choosue/Choosue.github.io/master/img/example-polynomial-curve-fitting-02.png)

很容易看到，相对而言$ M = 3 $的结果是最好的，虽然$ M = 9 $完美无瑕地穿过了所有的训练集点，但是正如上面所说的，这是过拟合了（$ E(\mathrm{w}^*) = 0 $）。

这里提出了另一个误差函数：

$$ E_{RMS} = \sqrt{2E(\mathrm{w}^*) / N} $$

依然是不管三七二十一，记住先。聪明人一眼就能看出来，这其实就是把我们之前提出来的误差函数乘以2然后再除以$ N $最后再开平方得出的结果，也就是：

$$ E_{RMS} = \sqrt{\sum_{n=1}^N\{y(x_n, \mathrm{w}^*) - t_n\}^2/N} $$

这里除以$ N $使得我们可以比较不同数量的数据集的$ E_{RMS} $，而开根号则可以让$ E_{RMS} $与目标值$ t $处于相同的数量级，方便比较（总之数学家们说什么都是对的……）。

这里的误差函数也是越小越好，但是如果为0，却又不见得好（真矛盾……）。

接着，我们经过了一系列分析（一系列废话……）最后又得出：当$ M $不变时，增加数据集的数量，可以缓解过拟合的程度（外国人你不要这么跳跃好么……）。

又是一个结论，过拟合现象其实是极大似然（*maximum likelihood*）的某个特征；贝叶斯算法可以有效避免过拟合，并且不管模型的$ M $值超出数据点多少都没问题。不过一般情况下，在贝叶斯模型中，模型$ M $的大小会自动与数据集相适应。

但是现在我们先不用什么贝叶斯。

## 正规化

我们先使用正规化（*regularization*），又要开始背公式了，这里我们为误差函数$ E(\mathrm{w}) $引入一个惩罚量：

$$ \tilde{E}(\mathrm{w}) = \frac{1}{2}\sum_{n=1}^N\{y(x_n, \mathrm{w}) - t_n\}^2 + \frac{\lambda}{2}\lVert{\mathrm{w}\rVert}^2 $$

其中，

$$ \lVert{\mathrm{w}\rVert}^2 = \mathrm{w}^{\mathrm{T}}\mathrm{w} = w_0^2 + w_1^2 + ... + w_M^2 $$

其实也就是误差函数加上了一个惩罚量$ \frac{\lambda}{2}\lVert{\mathrm{w}\rVert}^2 $，这里的系数$ \lambda $就决定了正规化的程度有多深：$ \ln\lambda $越大，正规化后的系数向量$ \mathrm{w} $越小，正规化的作用越明显。注：$ \ln\lambda $的范围是$ (-\infty, 0] $，其中$ \ln\lambda = -\infty $表示误差没有经过正规化。

这种技术在统计学里被称为收缩（*shrinkage*）；二次方的正规化被称为脊状回归（*ridge regression*）；在神经网络中又被称为权值衰减（*weight decay*）。一下子涨了好多姿势。

## 总结

为了解决这一节所面临的问题，即*polynomial curve fitting*，我们可以从两个方面优化我们的预测方法：一个是选择合适的模型（即$ M $），并且进一步优化模型复杂度（即$ \lambda $）；另一个则是从误差函数入手（即系数向量$ \mathrm{w} $的求解）。

好了，本节结束，下一期是概率论（*probability theory*）。


