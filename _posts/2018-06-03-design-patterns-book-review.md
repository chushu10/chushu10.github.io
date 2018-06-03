---
layout: post
title: "《设计模式》读书笔记之一"
date: 2018-06-03
categories: Booklist
---

# 读设计模式之一

最近在读[Design Patterns](https://book.douban.com/subject/1436745/)，有一些关键点需要多多理解，细细琢磨。

## 什么是设计模式（Design pattern)

第一章定义了设计模式的关键四要素：

> In general, a pattern has four essential elements:
> 1. The **pattern name** is a handle we can use to describe a design problem, its solutions, and consequences in a word or two. Naming a pattern immediately increases our design vocabulary and allows us to design at a higher level of abstraction. Having a vocabulary for patterns lets us talk about them with our colleagues, in our documentation, and even to ourselves. It makes it easier to think about designs and to communicate them and their trade-offs to others. Finding good names has been one of the hardest parts of developing our catalog.
> 2. The **problem** describes when to apply the pattern and explains the problem and its context. It might describe specific design problems such as how to represent algorithms as objects. In addition, it might describe class or object structures that are symptomatic of an inflexible design. Sometimes the problem will include a list of conditions that must be met before it makes sense to apply the pattern.
> 3. The **solution** describes the elements that make up the design, their relationships, responsibilities, and collaborations. The solution does not describe a particular concrete design or implementation, because a pattern is like a template that can be applied in many different situations. Instead, the pattern provides an abstract description of a design problem and how a general arrangement of elements (classes and objects in our case) solves it.
> 4. The **consequences** are the results and trade-offs of applying the pattern. Though consequences are often unvoiced when we describe design decisions, they are critical for evaluating design alternatives and for understanding the costs and benefits of applying the pattern. The consequences for software often concern space and time trade-offs and may address language and implementation issues as well. Since reuse is often a factor in object-oriented design, the consequences of a pattern include its impact on a system's flexibility, extensibility, or portability. Listing these consequences explicitly helps you understand and evaluate them.

在我看来，就是what, when, how, result, 理解了设计模式的这四个基础要素，才能明白作者后面要说什么。