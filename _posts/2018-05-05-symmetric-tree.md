---
layout: post
title: "Symmetric Tree"
date: 2018-05-05
categories: algorithm
img: 
mathjax: true
---

## How to summarize this question in one sentence？

Judge whether a tree is symmetric or not.

A symmetric tree is like: when you fold the tree aligned at center, you got perfect match.

![symmetric tree1]({{site.baseurl}}/assets/img/algorithm/symmetric_tree1.png)

## What algorithm(s) and data structure(s) are involved?

Recursion, bottom-up

## Thought(s) and Solution(s)?

First, let me introduce to you two principles quoted from [leetcode](leetcode.com):

> When you meet a tree problem, ask yourself two questions: can you determine some parameters to help the node know the answer of itself? Can you use these parameters and the value of the node itself to determine what should be the parameters parsing to its children? If the answers are both yes, try to solve this problem using a "top-down" recursion solution.

> Or you can think the problem in this way: for a node in a tree, if you know the answer of its children, can you calculate the answer of the node? If the answer is yes, solving the problem recursively from bottom up might be a good way.

In this symmetric tree problem, we use the "bottom-up" rule, because we know that if:

1. If the left child tree and the right child tree of root are mirrors, then root is a symmetric tree
2. Two trees are mirrors when:
  2.1 The values of the root of the trees(say `r1` and `r2`) are the same
  2.2 The left child tree of `r1` is mirror to the right child tree of `r2`; the right child tree of `r1` is mirror to the left child tree of `r2`

## What have you learned from this question?

## The BUG(s) found?
