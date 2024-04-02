---
layout: default
title: Multiparty Computation
parent: Notes
nav_order: 3
has_children: True
---
# background
The fundamental question: Can we compute a result that depends on private data from all involved parties without relying on a trusted party?

Thus, the model is: the parties $P_1, ..., P_n$. Each party holds a secret input $x_i$, and they agree on some function $f$ that takes $n$ inputs. Their goal is to compute $y = f(x_1,...,x_n)$ while making sure that the following two conditions are satisfied:
1. Corretness: the correct value of $y$ is computed
2. Privacy: $y$ is the only new information that is released.

## Secret Sharing
首先考虑针对三方的secret sharing, $P_1, P_2, P_3$。我们的目的是，除了拥有秘密的$P_1$之外，三方共同拥有s,但任一方都不单独拥有s。 

### One possible solution
考虑一个素数$p$，秘密s的拥有方$P_1$，uniformly随机选取$r_1,r_2$和$r_3 = s - r_1 - r_2 \mod p$. $P_1$将$r_1, r_3$交给$P_2$, 将$r_1, r_2$交给$P_3$. 不难看出，这满足了我们的要求，且仅当两方共享后，可以计算出s。

## Special case for voting
考虑计算函数$f = \sum_1^n x_i$，这是一个可以直接应用在投票系统上的函数，$x_i$代表每个party对一项表决赞同与否，在获取投票结果的同时，不泄露每个特定的参与者的数据。

一种可能的实现方式，是将三个$x_i$以secret sharing的方式分发出去。

| Party | privacy | sharing secret |
| ---- | ---- | ---- |
| $P_1$ | $x_1$ | $x_{12},x_{13},x_{22},x_{23},x_{32},x_{33} \rightarrow s_2,s_3$ |
| $P_2$ | $x_2$ | $x_{11},x_{13},x_{21},x_{23}, x_{31}, x_{33} \rightarrow s_1,s_3$ |
| $P_3$ | $x_3$ | $x_{11},x_{12},x_{21},x_{22},x_{31},x_{32} \rightarrow s_1, s_2$|

我们可以计算$f = s_1 + s_2 + s_3$。correctness显然，需要说明的是为什么没有泄露关于$x_i$，不妨以$P_1$的视角考虑：由于$s_2,s_3$都是用secret sharing方式分发的，因此没有泄露$x_2,x_3$，交换$s$之后，由于$P_1$已知$s_2,s_3$，因此我们只需要说明$P_1$得知$s_1$仅仅意味着$P_1$得知$f$。

#### 一个不太正式的证明

由$f = s_1 + s_2 + s_3 \mod p$，且已知$s_2, s_3$，那么知道$f$自然等价于知道$s_1$。这是一个脑筋急转弯，如果得知$s_1$意味着player可以知道更多，那么palyer可以由$f$计算出$s_1$，因此得到同样多信息。

#### 正式的证明

simulation argument

## More than addition

考虑一个巨tm有意思的例子，安全约会问题。

沸羊羊想和美羊羊约会，但沸羊羊不想尴尬。美羊羊不想和沸羊羊约会并不会令纯爱战士沸羊羊尴尬，真正尴尬的是美羊羊不想和沸羊羊约会同时又知道舔狗沸羊羊想和自己约会，此时沸羊羊会化身小丑。聪明的你能否帮助沸羊羊呢？

分析沸羊羊的需要，他想要计算$f = ab$，其中$a$是美羊羊是否愿意约会，$b$是沸羊羊的意愿。愿意为1，不愿意为0。那么对于沸羊羊来说，如果可以在不泄露$b$的情况下计算出$f$，他的目的便会达到。因为一旦美羊羊不愿意约会就意味着$f$恒为零，她无法以此推理出沸羊羊是舔狗还是海王。

我们可以使用Protocol Secure Multiplication完成安全的计算。但值得一提的是理论存在局限，美羊羊不会接受沸羊羊的邀约。XD