---
layout: post
title:  "Notes for Week 9 Recommender Systems"
date:   2016-06-30
categories: machine-learning
---
这篇文章是我对[Coursera](coursera.org)上的课程[Machine Learning](https://www.coursera.org/learn/machine-learning/)
的总结，第九周后半部分有关[Recommender Systems](https://en.wikipedia.org/wiki/Recommender_system)的内容。该课程由著名的机器学习专家[Andrew Ng](http://www.andrewng.org/)授课，是非常适合入门机器学习的课程。

# Notes for Week 9 Recommender Systems

@(读书笔记)[Machine Learning]

## Problem Formulation

有关电影评分的数据集：

|         Movie        | Alice (1) | Bob (2) | Carol (3) | Dave (4) |
| :------------------: | :-------: | :-----: | :-------: | :------: |
|     Love at last     |     5     |    5    |     0     |     0    |
|   Romance forever    |     5     |    ?    |     ?     |     0    |
| Cute puppies of love |     ?     |    4    |     0     |     ?    |
|  Nonstop car chases  |     0     |    0    |     5     |     4    |
|  Swords vs. karate   |     0     |    0    |     5     |     ?    |

- $$ n_u $$ = no. users
- $$ n_m $$ = no. movies
- $$ r(i,j) $$ = 1 if user $$ j $$ has rated movie $$ i $$
- $$ y^{(i,j)} $$ = rating given by user $$ j $$ to movie $$ i $$ (defined only if $$ r(i,j) $$ = 1)

## Content-based recommendations

加入特征$$ x_1 $$ (电影是romance类型的程度)和$$ x_2 $$ (电影是action类型的程度)以后

|         Movie        | Alice (1) | Bob (2) | Carol (3) | Dave (4) | $$ x_1$$<br>(romance) | $$ x_2 $$(action) |
| :------------------: | :-------: | :-----: | :-------: | :------: | :-: | :-: |
|     Love at last     |     5     |    5    |     0     |     0    | 0.9 |   0  |
|   Romance forever    |     5     |    ?    |     ?     |     0    | 1.0 | 0.01 |
| Cute puppies of love |     ?     |    4    |     0     |     ?    | 0.99 |   0  |
|  Nonstop car chases  |     0     |    0    |     5     |     4    | 0.1 |  1.0 |
|  Swords vs. karate   |     0     |    0    |     5     |     ?    |  0  |  0.9 |

基于上表，我们针对每一个用户$$  j $$，训练出一个参数$$ \theta^{(j)} \in \mathbb{R}^3 $$。最后，预测用户$$ j $$对电影$$ i $$的评分为$$ (\theta^{(j)})^Tx^{(i)} $$（对每一个用户，都训练一个**linear regression hypothesis**）

- $$ \theta^{(j)} $$ = parameter vector for user $$ j $$
- $$ x^{(i)} $$ = feature vector for movie $$ i $$
- $$ j:r(i,j) = 1 $$ = for all of the user $$ j $$ who has scored movie $$ i $$
- For user $$ j $$, movie $$ i $$, predict rating:  $$ (\theta^{(j)})^Tx^{(i)} $$

优化目标：
Given $$ x^{(i)} $$, to learn $$ \theta^{(j)} $$ (parameter for user $$ j $$):

$$$$ \min_{\theta^{(j)}}J(\theta^{(j)}) = \min_{\theta^{(j)}}\frac{1}{2}\sum_{i:r(i,j)=1}((\theta^{(j)})^Tx^{(i)} - y^{(i,j)})^2 + \frac{\lambda}{2}\sum_{k=1}^n(\theta_k^{(j)})^2 $$$$

Given $$ x^{(i)} $$, to learn $$ \theta^{(1)},\theta^{(2)},\dots,\theta^{(n_u)} $$:

$$$$ \min_{\theta^{(1)},\dots,\theta^{(n_u)}}J(\theta^{(1)},\dots,\theta^{(n_u)}) = \min_{\theta^{(1)},\dots,\theta^{(n_u)}}\frac{1}{2}\sum_{j=1}^{n_u}\sum_{i:r(i,j)=1}((\theta^{(j)})^Tx^{(i)} - y^{(i,j)})^2 + \frac{\lambda}{2}\sum_{j=1}^{n_u}\sum_{k=1}^n(\theta_k^{(j)})^2 $$$$

**思考**：gradient descent怎么写？

$$$$ \theta_k^{(j)} := \theta_k^{(j)} - \alpha(\sum_{i:r(i,j)=1}((\theta^{(j)})^Tx^{(i)} - y^{(i,j)})x_k^{(i)} + \lambda \theta_k^{(j)}) $$$$

## Collaborative Filtering (Not Content-based)

与content-based (given $$ x^{(i)} $$, to learn $$ \theta^{(1)},\theta^{(2)},\dots,\theta^{(n_u)} $$)正相反，collaborative filtering是given $$ \theta^{(1)},\theta^{(2)},\dots,\theta^{(n_u)} $$, to learn $$ x^{(i)} $$:

$$$$ \min_{x^{(i)}}J(x^{(i)}) = \min_{x^{(i)}}\frac{1}{2}\sum_{i:r(i,j)=1}((\theta^{(j)})^Tx^{(i)} - y^{(i,j)})^2 + \frac{\lambda}{2}\sum_{k=1}^n(x^{(i)}_k)^2 $$$$

given $$ \theta^{(1)},\theta^{(2)},\dots,\theta^{(n_u)} $$, to learn $$ x^{(1)},\dots,x^{(n_m)} $$:

$$$$ \min_{x^{(i)}}J(x^{(1)},\dots,x^{(n_m)}) = \min_{x^{(i)}}\frac{1}{2}\sum_{i=1}^{n_m}\sum_{i:r(i,j)=1}((\theta^{(j)})^Tx^{(i)} - y^{(i,j)})^2 + \frac{\lambda}{2}\sum_{i=1}^{n_m}\sum_{k=1}^n(x^{(i)}_k)^2 $$$$

**思考**：gradient descent怎么写？

$$$$ x_k^{(i)} := x_k^{(i)} - \alpha(\sum_{j:r(i,j)=1}((\theta^{(j)})^Tx^{(i)} - y^{(i,j)})\theta_k^{(j)} + \lambda x_k^{(i)}) $$$$

通常的做法是：
1. 猜测一组$$ \theta^{(1)},\theta^{(2)},\dots,\theta^{(n_u)} $$值
2. 使用猜测的$$ \theta^{(1)},\theta^{(2)},\dots,\theta^{(n_u)} $$值训练出特征$$ x^{(1)},\dots,x^{(n_m)} $$
3. 使用训练出的特征$$ x^{(1)},\dots,x^{(n_m)} $$优化$$ \theta^{(1)},\theta^{(2)},\dots,\theta^{(n_u)} $$值
4. 使用优化的$$ \theta^{(1)},\theta^{(2)},\dots,\theta^{(n_u)} $$训练特征$$ x^{(1)},\dots,x^{(n_m)} $$
5. 重复3、4的过程直到最终的hypothesis达到非常好的性能

```flow
st=>start: Start
e=>end
op1=>operation: Guess theta
op2=>operation: Use theta to learn feature x
op3=>operation: Use feature x to learn theta
cond=>condition: Is hypothesis optimized?

st->op1->op2->op3->cond
cond(yes)->e
cond(no)->op2
```

## Collaborative Filtering Algorithm

我们可以将上述流程图所示的学习过程合并成一个公式同时训练$$ \theta^{(1)},\theta^{(2)},\dots,\theta^{(n_u)} $$值和特征$$ x^{(1)},\dots,x^{(n_m)} $$：

$$$$ J(x^{(1)},\dots,x^{(n_m)},\theta^{(1)},\theta^{(2)},\dots,\theta^{(n_u)}) = $$$$

$$$$ \frac{1}{2}\sum_{(i,j):r(i,j)=1}((\theta^{(j)})^Tx^{(i)} - y^{(i,j)})^2 + \frac{\lambda}{2}\sum_{i=1}^{n_m}\sum_{k=1}^n(x^{(i)}_k)^2 + \frac{\lambda}{2}\sum_{j=1}^{n_u}\sum_{k=1}^n(\theta_k^{(j)})^2 $$$$

1. Initialize $$ x^{(1)},\dots,x^{(n_m)},\theta^{(1)},\theta^{(2)},\dots,\theta^{(n_u)} $$ to small random values.
2. Minimize $$ J(x^{(1)},\dots,x^{(n_m)},\theta^{(1)},\theta^{(2)},\dots,\theta^{(n_u)}) = $$ using gradient descent (or an advanced optimization algorithm). E.g. for every $$ j = 1,\dots,n_u,i=1,\dots,n_m $$:
    - $$ x_k^{(i)} := x_k^{(i)} - \alpha(\sum_{j:r(i,j)=1}((\theta^{(j)})^Tx^{(i)} - y^{(i,j)})\theta_k^{(j)} + \lambda x_k^{(i)}) $$
    - $$ \theta_k^{(j)} := \theta_k^{(j)} - \alpha(\sum_{i:r(i,j)=1}((\theta^{(j)})^Tx^{(i)} - y^{(i,j)})x_k^{(i)} + \lambda \theta_k^{(j)}) $$
3. For a user with parameters $$ \theta $$ and a movie with (learned) features $$ x $$, predict a star rating or $$ \theta^Tx $$.

## Collaborative Filtering Algorithm (Vectorized Implementation)

|         Movie        | Alice (1) | Bob (2) | Carol (3) | Dave (4) |
| :------------------: | :-------: | :-----: | :-------: | :------: |
|     Love at last     |     5     |    5    |     0     |     0    |
|   Romance forever    |     5     |    ?    |     ?     |     0    |
| Cute puppies of love |     ?     |    4    |     0     |     ?    |
|  Nonstop car chases  |     0     |    0    |     5     |     4    |
|  Swords vs. karate   |     0     |    0    |     5     |     ?    |

$$$$ Y = \begin{bmatrix}
                   5 & 5 & 0 & 0 \\
                   5 & ? & ? & 0 \\
                   ? & 4 & 0 & ? \\
                   0 & 0 & 5 & 4 \\
                   0 & 0 & 5 & 0 \\
                   \end{bmatrix} \in \mathbb{R}^{5\times 4} $$$$

### Low rank matrix factorization

$$$$ \begin{bmatrix}
                   (\theta^{(1)})^T(x^{(1)}) &  (\theta^{(2)})^T(x^{(1)}) & \dots &  (\theta^{(n_u)})^T(x^{(1)}) \\
                    (\theta^{(1)})^T(x^{(2)}) &  (\theta^{(2)})^T(x^{(2)}) & \dots &  (\theta^{(n_u)})^T(x^{(2)}) \\
                   \vdots & \vdots & \vdots & \vdots \\
                    (\theta^{(1)})^T(x^{(n_m)}) &  (\theta^{(1)})^T(x^{(n_m)}) & \dots &  (\theta^{(n_u)})^T(x^{(n_m)}) \\
                   \end{bmatrix} = X\times \Theta^T$$$$

$$$$ X = \begin{bmatrix}
                   -(x^{(1)})^T- \\
                   -(x^{(2)})^T- \\
                   \vdots \\
                   -(x^{(n_m)})^T- \\
                   \end{bmatrix} $$$$
                   
$$$$ \Theta = \begin{bmatrix}
                   -(\theta^{(1)})^T- \\
                   -(\theta^{(2)})^T- \\
                   \vdots \\
                   -(\theta^{(n_u)})^T- \\
                   \end{bmatrix} $$$$

### Finding related movies

只要找出特征最相似的就行：(例如电影$$ i $$与电影$$ j $$最相似的特征)

$$$$ \min |x^{(i)} - x^{(j)}| $$$$

## Mean Normalization

### Problem: users who have not rated any movies

|         Movie        | Alice (1) | Bob (2) | Carol (3) | Dave (4) | Eve (5) |
| :------------------: | :-------: | :-----: | :-------: | :------: | :--: |
|     Love at last     |     5     |    5    |     0     |     0    | ? |
|   Romance forever    |     5     |    ?    |     ?     |     0    | ? |
| Cute puppies of love |     ?     |    4    |     0     |     ?    | ? |
|  Nonstop car chases  |     0     |    0    |     5     |     4    | ? |
|  Swords vs. karate   |     0     |    0    |     5     |     ?    | ? |

如果用户Eve没有对任何电影进行评价，那么$$ \frac{1}{2}\sum_{(i,j):r(i,j)=1}((\theta^{(j)})^Tx^{(i)} - y^{(i,j)})^2 $$由于没有$$ r(i,j)=1 $$的组合$$ (i,j) $$，不会对$$ \theta $$值的确定起到任何作用。而$$ \frac{\lambda}{2}\sum_{j=1}^{n_u}\sum_{k=1}^n(\theta_k^{(j)})^2 $$倾向于将$$ \theta $$减到最小，所以对用户Eve的评分预测，我们只能得出

$$$$ \theta^{(5)} = \begin{bmatrix} 
                            0 \\
                            0 \end{bmatrix}$$$$

详细情况可以分析以下公式：

$$$$ \min_{x^{(1)},\dots,x^{(n_m)},\theta^{(1)},\theta^{(2)},\dots,\theta^{(n_u)}}J(x^{(1)},\dots,x^{(n_m)},\theta^{(1)},\theta^{(2)},\dots,\theta^{(n_u)}) = $$$$

$$$$ \min_{x^{(1)},\dots,x^{(n_m)},\theta^{(1)},\theta^{(2)},\dots,\theta^{(n_u)}}\frac{1}{2}\sum_{(i,j):r(i,j)=1}((\theta^{(j)})^Tx^{(i)} - y^{(i,j)})^2 + \frac{\lambda}{2}\sum_{i=1}^{n_m}\sum_{k=1}^n(x^{(i)}_k)^2 + \frac{\lambda}{2}\sum_{j=1}^{n_u}\sum_{k=1}^n(\theta_k^{(j)})^2 $$$$

### Mean normalization

$$$$ Y = \begin{bmatrix}
                   5 & 5 & 0 & 0 & ? \\
                   5 & ? & ? & 0 & ? \\
                   ? & 4 & 0 & ? & ? \\
                   0 & 0 & 5 & 4 & ? \\
                   0 & 0 & 5 & 0 & ? \\
                   \end{bmatrix} $$$$

对所有的电影评分做一个平均值，并得到以下评分向量$$ \mu $$：

$$$$ \mu = \begin{bmatrix}
                   2.5 \\
                   2.5 \\
                   2 \\
                   2.25 \\
                   1.25 \\
                   \end{bmatrix} $$$$

然后将原始数据集$$ Y $$与$$ \mu $$相减：

$$$$ Y_{new} := Y - \mu = \begin{bmatrix}
                   2.5 & 2.5 & -2.5 & -2.5 & ? \\
                   2.5 & ? & ? & -2.5 & ? \\
                   ? & 2 & -2 & ? & ? \\
                   -2.25 & -2.25 & 2.75 & 1.75 & ? \\
                   -1.25 & -1.25 & 3.75 & -1.25 & ? \\
                   \end{bmatrix} $$$$

使用新计算出来的数据集$$ Y_{new} $$训练新的参数$$ \theta_{new}^{(j)} $$和特征$$ x_{new}^{(i)} $$，预测用户$$ j $$对电影$$ i $$的预测变为：

$$$$ (\theta_{new}^{(j)})^T(x_{new}^{(i)}) + \mu^i $$$$