---
layout: post
title:  "Notes for Week 1"
date:   2016-05-05 
categories: machine-learning
---
这篇文章是我对[Coursera](coursera.org)上的课程[Machine Learning](https://www.coursera.org/learn/machine-learning/)
的总结，第一周的内容。该课程由著名的机器学习专家[Andrew Ng](http://www.andrewng.org/)授课，是非常适合入门机器学习的课程。

# Notes for Week 1

@(读书笔记)[Machine Learning]

## Supervised Learning

- **"right answers" given.** 测试集中，每一个自变量都对应了其结果，例如在房价预测（Housing price prediction）中，自变量房屋面积，和结果价格都已经给出了。
- **Regression: Predict continuous valued output(price).** 如果预测的结果是一个实数，或者连续的数据，这种有监督的学习被称为**回归分析**。
- **Classification: Predict discrete valued output(0 or 1).** 如果预测的结果是离散的数字，这种有监督的学习被称为**分类**。
- **Support Vector Machine.** 让机器学习可以处理无穷多特征的算法。

## Unsupervised Learning

- **"right answers" not given.** 测试集中的数据都是没有被打上标签的。
- **Clustering.** 无监督学习通常都是聚类分析。

## Linear Regression with One Variable

- **Hypothesis.** 在单变量的linear regression中，我们用直线代表hypothesis：

$$ h_{\theta}(x) = \theta_0 + \theta_1x$$

- **Cost function:**

$$ J(\theta_0, \theta_1) = \frac{1}{2m}\sum_{i=1}^m(h_{\theta}(x^{(i)}) - y^{(i)})^2 $$

- 找出使$ J(\theta_0, \theta_1) $最小化的$ \theta_0, \theta_1 $就表示找到了最合适的Hypothesis，同时意味着建立了最佳model。为了找出使$ J(\theta_0, \theta_1) $最小化的$ \theta_0, \theta_1 $，我们将使用一种叫做**Gradient descent**的算法，如下。
- **Gradient descent:**
	- 已知误差函数 $ J(\theta_0, \theta_1) $
	- 求 $ \min\limits_{\theta_0, \theta_1}J(\theta_0, \theta_1) $
	- **Outline:**
		- 随机初始化一对 $ \theta_0, \theta_1 $
		- 不断地改变 $ \theta_0, \theta_1 $ 以使得 $ J(\theta_0, \theta_1) $ 的值不断减小，直到找到其最小值
- **Gradient descent algorithm:**
重复以下过程直至收敛

$$ \theta_j := \theta_j - \alpha\frac{\partial}{\partial\theta_j}J(\theta_0, \theta_1) \space\space\space\space\space (\mathrm{for} \space j=0 \space \mathrm{and} \space j=1)$$

- **注意:**
	- 上述过程的正确打开方式是：（又叫做“同步更新”）

	$$ \mathrm{temp0} := \theta_0 - \alpha\frac{\partial}{\partial\theta_0}J(\theta_0, \theta_1) $$

	$$ \mathrm{temp1} := \theta_1 - \alpha\frac{\partial}{\partial\theta_1}J(\theta_0, \theta_1) $$

	$$ \theta_0 :=  \mathrm{temp0} $$

	$$ \theta_1 :=  \mathrm{temp1} $$

	- 错误打开方式是：（$ \theta_1 $更新时$ \theta_0 $已经先于它更新了，故不正确）

	$$ \mathrm{temp0} := \theta_0 - \alpha\frac{\partial}{\partial\theta_0}J(\theta_0, \theta_1) $$

	$$ \theta_0 :=  \mathrm{temp0} $$

	$$ \mathrm{temp1} := \theta_1 - \alpha\frac{\partial}{\partial\theta_1}J(\theta_0, \theta_1) $$

	$$ \theta_1 :=  \mathrm{temp1} $$

## Linear Algebra

将矩阵运算巧妙地运用在了model比较上，这一章复习了线性代数的基本内容，比较简单。