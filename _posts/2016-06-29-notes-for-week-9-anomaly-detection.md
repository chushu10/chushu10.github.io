---
layout: post
title:  "Notes for Week 9 Anomaly Detection"
date:   2016-06-29
categories: machine-learning
---
这篇文章是我对[Coursera](coursera.org)上的课程[Machine Learning](https://www.coursera.org/learn/machine-learning/)
的总结，第九周前半部分有关[Anomaly Detection](https://en.wikipedia.org/wiki/Anomaly_detection)的内容。该课程由著名的机器学习专家[Andrew Ng](http://www.andrewng.org/)授课，是非常适合入门机器学习的课程。

# Notes for Week 9 Anomaly Detection

@(读书笔记)[Machine Learning]

## Anomaly Detection

- 主要应用于Unsupervised learning，但同时也用到了Supervised learning中的一些方法。
- 数据集通常是unlabelled。
- 常用的方法是密度分析（**Density estimation**）：
    - 使用数据集$ \{x^{(1)},x^{(2)},\dots.x^{(m)}\} $训练出一个model $ p(x) $
    - 当$ p(x_{test}) < \epsilon $时，则将$ x_{test} $标记为**anomaly**；$ p(x_{test}) \geq \epsilon $时，则将$ x_{test} $标记为**non-anomaly**
- 后文中使用到的方法就是密度分析（**Density estimation**）。

## Gaussian (Normal) Distribution

- $ x \backsim \mathcal{N}(\mu,\sigma^2) $表示$ x $服从参数为$ \mu,\sigma^2 $的高斯分布。
- $ \mu $表示均值（**mean**），$ \sigma^2 $表示方差（**variance**）。
- 高斯分布的方程：$ p(x;\mu,\sigma^2) = \frac{1}{\sqrt{2\pi}\sigma}\mathrm{exp}(-\frac{(x-\mu)^2}{2\sigma^2}) $
![gaussian-distribution-examples](https://raw.githubusercontent.com/Choosue/Choosue.github.io/master/img/week9/gaussian-distribution-examples.png)

### Parameter estimation

通常在给定数据集时，我们并不知道$ \{x^{(1)},x^{(2)},\dots.x^{(m)}\} \space x^{(i)} \in \mathbb{R} $服从怎样的高斯分布（即不知道参数$ \mu $和参数$ \sigma^2 $）。可以通过以下公式计算（根据极大似然定理**maximum likelihood**）：

$$ \mu = \frac{1}{m}\sum_{i=1}^mx^{(i)} $$

$$ \sigma^2 = \frac{1}{m}\sum_{i=1}^m(x^{(i)} - \mu)^2 $$

## Algorithm

假设我们有大小为$ m $的数据集$ \{x^{(1)},x^{(2)},\dots.x^{(m)}\} \space x^{(i)} \in \mathbb{R} $，总共$ n $个特征$ x_1,x_2,\dots,x_n $，model $ p(x) $的计算方法是：

$$ p(x) = p(x_1;\mu_1,\sigma_1^2)p(x_2;\mu_2,\sigma_2^2)p(x_3;\mu_3,\sigma_3^2)\dots p(x_n;\mu_n,\sigma_n^2) $$

$$ p(x) = \prod_{j=1}^np(x_j;\mu_j,\sigma_j^2) $$

总结一下Anomaly detection algorithm：
1. 选择可以区别出anomaly的特征$ x_j $
2. 计算参数$ \mu_1,\dots,\mu_n,\sigma_1^2,\dots,\sigma_n^2 $
    - $ \mu_j = \frac{1}{m}\sum_{i=1}^mx_j^{(i)} $
    - $ \sigma_j^2 = \frac{1}{m}\sum_{i=1}^m(x_j^{(i)} - \mu_j)^2 $
3. 对于新的测试数据$ x $，计算$ p(x) $：
    - $ p(x) = \prod_{j=1}^np(x_j;\mu_j,\sigma_j^2) = \prod_{j=1}^n\frac{1}{\sqrt{2\pi}\sigma_j}\mathrm{exp}(-\frac{(x_j-\mu_j)^2}{2\sigma_j^2})$
    - 如果$ p(x) < \epsilon $，则判定为anomaly

## Anomaly Detection vs. Supervised Learning

| Anomaly detection | Supervised Learning |
| :---------------- | :------------------ |
| Very small number of positive<br> examples ($ y=1 $). (0-20 is common).<br><br> Large number of negative ($ y=0 $)<br> examples.<br><br> Many defferent "types" of anomalies.<br> Hard for any algorithm to learn from<br> positive examples what the anomalies<br> look like;| Large number of both positive<br> and negative examples.<br><br><br><br><br> Enough positive examples for<br> algorithm to get a sense of what<br> positive examples are like, future<br> positive examples likely to be<br> similar to ones in training set. |
| **Fraud detection**<br><br> **Manufacturing (e.g. aircraft**<br> **engines)**<br><br> **Monitoring machines in a data**<br> **center** | **Email spam classification**<br><br> **Weather prediction**<br>**(sunny/rainy/etc).**<br><br> **Cancer classification** |

## Choosing Features

- 如果某个特征的数据集不呈现高斯分布，可以通过$ \log(x+c) $或者$ \sqrt{x} $等方法使其分布变为高斯分布。
- 如果正常样例（normal examples）和异常样例（anomaly examples）在$ p(x) $值上一样高，可以通过$ x_3 = \frac{x_1}{x_2} $的方法构造新的特征。

## Multivariate Gaussian Distribution

不需要分别计算$ p(x_1),p(x_2),\dots,\text{etc} $，而是一次性计算$ p(x) $，此时$ \mu \in \mathbb{R}^n,\Sigma \in \mathbb{R}^{n \times n} $：

$$ p(x;\mu,\Sigma) = \frac{1}{(2\pi)^{\frac{n}{2}}|\Sigma|^{\frac{1}{2}}}\mathrm{exp}(-\frac{1}{2}(x-\mu)^T\Sigma^{-1}(x-\mu)) $$

$$ \Sigma = \frac{1}{m}\sum_{i=1}^m(x^{(i)}-\mu)(x^{(i)}-\mu)^T $$

$ |\Sigma| $ = determinent of $ \Sigma $，在octave中可以通过 det(SIgma) 命令计算。

### Multivariate Gaussian (Normal) examples

同时改变$ \Sigma $对角线值，其余值保持为0，会让$ p(x) $变得锐利或者平坦：
![change sigma](https://raw.githubusercontent.com/Choosue/Choosue.github.io/master/img/week9/multivariate-gaussian-examples-1.png)

只改变$ \Sigma $对角线上的某一个值，其余值保持为0，会让$ p(x) $沿着某个特征的方向伸缩：
![change sigma](https://raw.githubusercontent.com/Choosue/Choosue.github.io/master/img/week9/multivariate-gaussian-examples-2.png)

增加或减小斜对角线，会使$ p(x) $朝着这两个方向伸缩：
![change sigma](https://raw.githubusercontent.com/Choosue/Choosue.github.io/master/img/week9/multivariate-gaussian-examples-3.png)

当斜对角线的值为负时，会使$ p(x) $会转变方向：
![change sigma](https://raw.githubusercontent.com/Choosue/Choosue.github.io/master/img/week9/multivariate-gaussian-examples-4.png)

改变$ \mu $值会让$ p(x) $的最大值点移动
![change mu](https://raw.githubusercontent.com/Choosue/Choosue.github.io/master/img/week9/multivariate-gaussian-examples-5.png)

## Original Model vs. Multivariate Gaussian

| Original Model | Multivariate Gaussian |
| :------------- | :-------------------- |
| $ p(x_1;\mu_1,\sigma_1^2)\times\dots\times p(x_n;\mu_n,\sigma_n^2) $<br><br><br> Manually create extra features to<br> capture anomalies where $x_1,x_2$<br> take unusual combinations of<br> values ($ x_3 = \frac{x_1}{x_2} $).<br><br> Computaionally cheaper<br> (alternatively, scales better to large<br> $n$).<br><br> OK even if $ m $ (training set size) is<br> small. | $ p(x;\mu,\Sigma) = \frac{1}{(2\pi)^{\frac{n}{2}}|\Sigma|^{\frac{1}{2}}}\mathrm{exp}(-\frac{1}{2}(x-\mu)^T\Sigma^{-1}(x-\mu)) $<br><br> Automatically captures<br> correlations between features.<br><br><br><br> Computationally more expensive<br> (hard to compute inverse when $ n $<br> is too large)<br><br> Must have $ m > n $ or else $ \Sigma $ is<br> non-invertible ($ m \geq 10n $) |