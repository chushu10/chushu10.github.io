---
layout: post
title:  "Implement Queue by Two Stacks"
date:   2016-04-15 11:46:30 +0800
categories: algorithm
---
# [Implement Queue by Two Stacks](http://www.lintcode.com/en/problem/implement-queue-by-two-stacks/)

@(算法)[算法, Stack, Queue]

### 这一题怎样用一句话描述？

只用两个栈（FILO）实现一个队列（FIFO）

### 用到什么算法？什么数据结构？

栈（Stack）

### 通过这题学到了什么？

这一题非常有意思，用两个栈实现一个队列。想象一下一个栈装满了元素，而另一个栈为空，如下图：

![Two stacks](https://raw.githubusercontent.com/Choosue/Choosue.github.io/master/img/implement-queue-by-two-stacks-01.png)

通过图可以清楚的知道，左边栈的入栈序列是1->2->3，如果顺序出栈，出栈的序列就是3->2->1；如果把左边的栈元素依次出栈同时依次入右边的栈，那么右边的栈就会像这样被堆满：

![Two stacks](https://raw.githubusercontent.com/Choosue/Choosue.github.io/master/img/implement-queue-by-two-stacks-02.png)

想象这两个栈是两个量筒，1、2、3分别是三个做了记号的乒乓球，那么就不难理解了，把左边量筒里的乒乓球往右边量筒里面倒，它们的顺序就正好颠倒过来。此时右边栈的出栈序列是1->2->3，与左边栈的入栈序列一致（1->2->3），这样我们就实现了队列。

你也许会想，万一1出队后，我又往“队列”里入元素了怎么办？

没关系，我们可以把元素先往左边的栈push，如下图：

![Two stacks](https://raw.githubusercontent.com/Choosue/Choosue.github.io/master/img/implement-queue-by-two-stacks-03.png)

等右边的栈取空了之后，再把左边积压已久的元素一股脑地“倒”进右边的栈，如下图：

![Two stacks](https://raw.githubusercontent.com/Choosue/Choosue.github.io/master/img/implement-queue-by-two-stacks-04.png)

### 可能(已经)遇到的BUG有？

无

