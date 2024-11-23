---
layout: default
title: Game Theory
parent: Notes
nav_order: 1
has_children: True
---
# Games

## Noncooperative and Cooperative Games

## Strategic Games and Extensive Games

In stategic games, each player choose action once and simultaneously. By contrast, each player can consider his plan of action not only at the beginning of the game but also whenever he has to make a decision in extensive games.

## Games with perfect and Imperfect Information

# Two-person Zero-sum Games

两方零和博弈。定义无需赘述，一个有趣的例子是奇偶游戏。

## Odd or Even

两个玩家一和玩家二，每个玩家可以选择出1或者2。当玩家选择的总和是奇数则玩家一赢，否则玩家二赢。每轮赢家会获得这个总和。显然这个游戏是零和的，不存在合作的可能。以玩家一的收益定义，payoff function如下：

$$
\begin{bmatrix}
-2&+3 \\
+3&-4 \\
\end{bmatrix}
$$

### Is it fair?

并不公平。

直觉上看，玩家一可以控制出2的几率让-4以极少概率出现的同时保证自己的正收益。但玩家二控制不了，因为玩家二最大的正收益发生在两人都出2的时候，因此玩家一在某种程度上可以掌控游戏。

正式的分析如下：

定义玩家一出1的概率为p。首先考虑，玩家一期望收益至少是多少。我们要明确，玩家二可以任选策略，因此能存在最值，则必然是无论玩家二出1或2，玩家一会获得相同的收益。也就是说，如果我们声称”最“，只有当我们不假设玩家二的选择时，才有意义。一般称为，equalizing strategy。由于，该游戏是零和博弈，玩家一至少的期望收益也就是玩家二至多的期望损失。因此，我们有：
$$-2p+3(1-p)=3p-4(1-p)$$
$$p=\frac{7}{12}$$

对于这样的策略，我们称之为minimax strategy.

上述的策略是一种mixed strategy，也就是说，我们的出值并不是行动集合中单纯的一种，而是以某种概率混合在了一起。pure strategy可以看作一种特殊的mixed strategy。

### Is our model reasonable?

我们现在想象一个非常极端的场景，一个资深的赌徒，面对两个选择：一个是直接拿走500万，另一个是有二分之一的几率拿走1000万。对于一个身无分文的人来说，他会有更大几率直接拿走500万。对于一个世界首富来说，他可能会选择第二个选择。这也就是说1000w的效用不等同于500w的两倍，随着人身价的增长，这个效用趋近于两倍。毕竟对于我们大家来说，二分之一的几率拿走两块钱，和直接拿走一块钱，似乎是一个公平的游戏。

也因此，在我们的模型中，我们同样认为，只需要考虑期望收益。这是一个相当强的假设，但在理论上非常合理。

# The Minimax Theorem

如果两人行动集是有限的，我们称这是一个finite game。

对于任意双人零和博弈，
1. There is a number V, called the value of the game.
2. 存在一个混合策略，使得无论玩家二如何选择玩家一的期望收益至少是V.
3. 存在一个混合策略，使得无论玩家一如何选择玩家二的期望损失至多是V.

根据V的值，我们称这个博弈是公平的或者有偏好的。

# Matrix Game

# Two-person Gerneral-Sum

我们还是以矩阵的形式表示payoff function，但不同于zero-sum，我们称这种新的形式为bimatrix。我们也可以用两个矩阵，分别表示两个玩家的payoff。当且仅当，这两个矩阵对应元素之和为0，我们称之为zero-sum。

