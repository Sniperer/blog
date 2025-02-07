---
layout: default
title: Linear Programming
parent: Math
nav_order: 1
has_children: False
---
# Terminology
| 中文        | 英文                    | 发音               |
|-----------|-----------------------|------------------|
| 垂直        | perpendicular         | ˌpərpənˈdikyələr |
| 减少        | subtract              | səbˈtrak(t)      |
| 坐标系       | coordinate system     |                  |
| 横轴        | horizontal axis       |                  |
| 纵轴        | vertical axis         | ˈaksəs           |
| 通过直觉或直接观察 | solving by inspection ||
|升|liter | |


# Linear Programming

## Introduction

### decision variable
变量，重音在前，/'vereabel/,very啊bo

### objective function
在优化任务中，我们想最大或最小化的那个函数

### constraints
所有的约束，由以decision variable的线性组合组成的不等式或等式决定。

### slack variable
有时，我们或许想将一个等式约束转换成一个不等式约束，使用slack variable松弛变量就是一个非常自然的事。例如，$a_1x_1 + a_2x_2 + ... + a_nx_n \leq b$就等价于$a_1x_1 + a_2x_2 + ... + a_nx_n + w = b, w > 0$。当然，我们也可以将等式约束转换成不等式约束，同时使用小于等于，和大于等于。

数学上，我们约定俗称的喜欢将问题写成$$\begin{align} maximize\ \ \ \ \ &c_1x_1 + c_2x_2 + ... + c_nx_n,\\ subject\ to \ \ \ \ \ &a_{11}x_1 + a_{12}x_2 + ... + a_{1n}x_n \leq b_1 \\ &a_{21}x_1 + a_{22}x_2 + ... + a_{2n}x_n \leq b_2 \\ &a_{m1}x_1 + a_{m2}x_2 + ... + a_{mn}x_n \leq b_m \\ &x_1, x_2, ..., x_n \geq 0 \end{align}$$

换句话说，我们更倾向于将所有约束写成小于等于的形式，并要求decision variable非负。我们管这种形式叫standard form。我们使用$m$表示约束的数量，使用$n$表示decision variable的数量。

一组对decision variable的赋值叫做solution，如果这组赋值，可以满足全部约束，我们叫feasible solution。如果又最大化了目标函数，那就是optimal solution。如果目标函数最大值可以取到无限，我们称其为unbounded。

## Simplex Method
