---
layout: post
title:  "Notes for Week 2"
date:   2016-05-10
categories: machine-learning
mathjax: true
---
这篇文章是我对[Coursera](coursera.org)上的课程[Machine Learning](https://www.coursera.org/learn/machine-learning/)
的总结，第二周的内容。该课程由著名的机器学习专家[Andrew Ng](http://www.andrewng.org/)授课，是非常适合入门机器学习的课程。

# Notes for Week 2

@(读书笔记)[Machine Learning]

## Linear Regression with Multiple Features

- **Hypothesis:**

$$ h_{\theta}(x) = \theta_0 + \theta_1x_1 + \theta_2x_2 + ... + \theta_nx_n $$

为了方便处理，我们令$ x_0 = 1 $，于是可以将所有的变量$ x_i $存储在一个$ n+1 \times 1 $的矩阵中，或者说一个$ n + 1 $维的向量$ \vec x $里：

$$ \vec x = \begin{bmatrix}
                   x_0 \\
                   x_1 \\
                   x_2 \\
                   ...    \\
                   x_n \\
                   \end{bmatrix} \in \mathbb{R}^{n + 1} $$
                   
同理，我们也可以将所有的参数$ \theta_i $存储在一个$ n+1 \times 1 $的矩阵中，或者说一个$ n + 1 $维的向量$ \vec \theta $里：

$$ \vec \theta = \begin{bmatrix}
                   \theta_0 \\
                   \theta_1 \\
                   \theta_2 \\
                   ...            \\
                   \theta_n \\
                   \end{bmatrix} \in \mathbb{R}^{n + 1} $$
                   
最后，我们便可以将$ h_{\theta}(x) $简化成：

$$ h_{\theta}(x) = {\vec \theta}^{\mathrm{T}}\vec x $$

- **Gradient descent**
重复以下过程直至收敛

$$ \theta_j := \theta_j - \alpha\frac{\partial}{\partial\theta_j}J(\theta_0, \theta_1, ..., \theta_n) $$

同样的，$ J(\theta_0, \theta_1, ..., \theta_n) $可以简化成$ J(\vec \theta) $。
通过求出$ J(\vec \theta) $相对于每个$ \theta_j $的偏导数，我们可以将上述公式转换为：

$$ \theta_j := \theta_j - \alpha\frac{1}{m}\sum_{i = 1}^m(h_{\theta}(x^{(i)}) - y^{(i)})x_j^{(i)} $$

例如，对于$ \theta_0, \theta_1, \theta_2 $：

$$ \theta_0 := \theta_0 - \alpha\frac{1}{m}\sum_{i = 1}^m(h_{\theta}(x^{(i)}) - y^{(i)})x_0^{(i)} \space (x_0^{(i)} = 1)$$

$$ \theta_1 := \theta_1 - \alpha\frac{1}{m}\sum_{i = 1}^m(h_{\theta}(x^{(i)}) - y^{(i)})x_1^{(i)} $$

$$ \theta_2 := \theta_2 - \alpha\frac{1}{m}\sum_{i = 1}^m(h_{\theta}(x^{(i)}) - y^{(i)})x_2^{(i)} $$

- **Feature scaling**
特征的单位处于相似的数量级，可以让*Gradient descent*算法的收敛速度加快。差不多接近$ -1 \leqslant x_i \leqslant 1 $就行了。
通常，如果$ x_i $的平均数是$ \mu_i $，我们可以将该特征平均化：
$$ x_i := \frac{x_i - \mu_i}{s_i} $$
其中，$ s_i $是特征$ x_i $的取值范围（最大值减最小值）。
- **Learning rate**
$ \alpha $应该让$ J(\vec \theta) $在Gradient descent的每一次迭代中都可以变小（使用$ J(\vec \theta) $为纵轴，迭代次数为横轴的坐标轴，画出$ J(\vec \theta) $随着迭代次数变化的曲线，如果$ J(\vec \theta) $增大，就使用小一点的$ \alpha $）。
- **Design of features**
不同的特征对应不同的预测函数，例如下面三类预测函数：

$$ h_\theta(x) = \theta_0 + \theta_1(size) + \theta_2(size)^2 $$

$$ h_\theta(x) = \theta_0 + \theta_1(size) + \theta_2\sqrt{size} $$

$$ h_\theta(x) = \theta_0 + \theta_1(size) + \theta_2(size)^2 + \theta_3(size)^3$$

## Normal Equation

简而言之，就是直接找到使$ J(\vec\theta) $最小的$ \vec\theta $值。

例如，有以下$ m = 4 $的数据集：

| Size (feet<sup>2</sup>)<br/><br/>$ x_1 $ | Number of<br/>bedrooms<br/>$ x_2 $ | Number of<br/>floors<br/>$ x_3 $ | Age of home<br/>(years)<br/>$ x_4 $ | Price(\$1000)<br/><br/>$ y $ |
| :----: | :-: | :-: | :--: | :---: |
|  2104  |  5  |  1  |  45  |  460  |
|  1416  |  3  |  2  |  40  |  232  |
|  1534  |  3  |  2  |  30  |  315  |
|  852   |  2  |  1  |  36  |  178  |

我们首先构造$ x_0 = 1 $的列（与文章[开头](#linear-regression-with-multiple-features)所说的**方便处理**的道理一样），加入这一新列后如下：

| <br/><br/>$ x_0 $ | Size (feet<sup>2</sup>)<br/><br/>$ x_1 $ | Number of<br/>bedrooms<br/>$ x_2 $ | Number of<br/>floors<br/>$ x_3 $ | Age of home<br/>(years)<br/>$ x_4 $ | Price(\$1000)<br/><br/>$ y $ |
| :-: | :----: | :-: | :-: | :--: | :---: |
|  1  |  2104  |  5  |  1  |  45  |  460  |
|  1  |  1416  |  3  |  2  |  40  |  232  |
|  1  |  1534  |  3  |  2  |  30  |  315  |
|  1  |  852   |  2  |  1  |  36  |  178  |

我们将$ x_0 ... x_4 $对应的四条数据放进一个矩阵（这个矩阵也叫*Design matrix*）中：

$$ X = \begin{bmatrix}
                   1 & 2104 & 5 & 1 & 45 \\
                   1 & 1416 & 3 & 2 & 40 \\
                   1 & 1534 & 3 & 2 & 30 \\
                   1 & 852   & 2 & 1 & 36 \\
                   \end{bmatrix} \in \mathcal{M}_{m \times (n + 1)}(\mathbb{R})$$

将结果$ y $表示成一个向量：

$$ \vec y = \begin{bmatrix}
                   460 \\
                   232 \\
                   315 \\
                   178 \\
                   \end{bmatrix} \in \mathbb{R}^{m} $$

可以看到，前面所有预处理都与gradient descent如出一辙，只不过我们会通过最后计算：

$$ \vec\theta = (X^TX)^{-1}X^T\vec y $$

即可算出使$ J(\vec\theta) $最小的$ \vec\theta $值，使用Octave计算的命令是：

```
pinv(X'*X)*X'*y
```

## Gradient Descent vs Normal Equation

| Gradient Descent | Normal Equation |
| :--------------- | :-------------- |
| 1.Need to choose $ \alpha $.<br/>2.Needs many iterations.<br/>3.Works well even when $ n $ is large. | 1.No Need to choose $ \alpha $.<br/>2.Don't need to iterate.<br/>3.Need to compute $ (X^TX)^{-1} $<br/>4.Slow if $ n $ is very large.($ n \geqslant 10^4 $) |

