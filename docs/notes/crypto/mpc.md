---
layout: default
title: Multiparty Computation
parent: Cryptology
grand_parent: Notes
nav_order: 0
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

simulation argument：给定一个player的输入输出，存在PPT可以模拟出在协议中该player所见的全部信息，或者说模拟出相同的信息分布。

## More than addition

考虑一个巨tm有意思的例子，安全约会问题。

沸羊羊想和美羊羊约会，但沸羊羊不想尴尬。美羊羊不想和沸羊羊约会并不会令纯爱战士沸羊羊尴尬，真正尴尬的是美羊羊不想和沸羊羊约会同时又知道舔狗沸羊羊想和自己约会，此时沸羊羊会化身小丑。聪明的你能否帮助沸羊羊呢？

分析沸羊羊的需要，他想要计算$f = ab$，其中$a$是美羊羊是否愿意约会，$b$是沸羊羊的意愿。愿意为1，不愿意为0。那么对于沸羊羊来说，如果可以在不泄露$b$的情况下计算出$f$，他的目的便会达到。因为一旦美羊羊不愿意约会就意味着$f$恒为零，她无法以此推理出沸羊羊是舔狗还是海王。

我们可以使用Protocol Secure Multiplication完成安全的计算。但值得一提的是理论存在局限，美羊羊可能连零都懒得按。XD

必须要注意的是，一旦沸羊羊的输入是1，他必然可以根据结果和他的输入推理出美羊羊的输入，这也是为什么上述证明make sense的原因。MPC并不是要求什么信息都学不到，只是要求学到的信息恰好和学到$f$同样多。这非常重要。

## variation

一个自然的问题是，如果player并不遵守规则，我们能否发现或限制。一种违反规则是，某一个player就不想计算出结果，他一直提供错误的input。这种问题是没有办法从协议上避免的，但我们可以用博弈论的效用设计，此处超纲。一个例子是，美羊羊知道了沸羊羊使用安全约会技术，所以美羊羊一直点同意但一直不赴约，只为一探沸羊羊内心。另一种，也是我们主要探讨的是，player不按照协议的指令做事，例如在addition中，他向$P_2, P_3$传递不同的$x_1$。但这种恶意行为可以被解决，我们让$P_2, P_3$交换$P_1$就限制了这种行为。这也是为什么我们在addition阶段提供两个share而不是一个。同样的，在loaclly compute $s_2$之后，$P_1$同样可以恶意的提供一个错误的值，但由于$P_3$也同样计算了$s_2$，这种恶意行为也会被避免。

## More than 3 players

本阶段的讨论依然假设所有player会遵循protocol的指令行事，哪怕是恶意player。我们称这种安全为semi-honest或者passive security。确切的是，对于Corrut players $t < n/2$，注意是严格小于。

### secret sharing for more players

如上所述，在三方计算中，我们设计了一个比较trivial的方法实现secret sharing，对于多方来说，思路来自algebra的简单事实，要确定一个t次多项式，我们需要t+1个在该多项式上的点。那么，给定一个秘密s，和player人数t，不妨让$s = h(0), s_i = h(i)$，那么将秘密$s$以这种形式分发出去，players可以由lagrange interpolation恢复出$s$。*但我们必须要证明插值不会泄露信息。*

#### proof

首先要明确要证明的论断是，如果uniformly random的选取t次多项式，那么对于至多t个人的恶意行为是安全的。不安全是指，$C$是作弊用户集合，这些用户获得了$(h(i))_{i \in C}$，这些用户会猜到$s$分布的一些信息。

从数学角度讲，如果在一个空间上的一个均匀分布能一一映射到另一个等大的空间上，那么在新空间上的分布也是均匀的。以此，不难得出证明成立。多项式各项系数$(a_j)_{j\leq t}$在给定$s$和$C$的情况下，一一映射到$(h(i))_{i \in C}$。多项式系数均匀选择，则每个player接受到的sharing也同样是均匀选择的，不包含$s$的任何信息。QED

### arithmetic circuits

三种gate：add, multipy, multipy-by-constant

### CEPS

主体上是组合上述门电路，特别的是，multipy会使polynomial由t次变为2t次，多次的multipy会使polynomial超出n-1次，从而拉格朗日插值失效，因此我们需要在完成乘法之后，此时各方share了新的秘密，在不泄露秘密的情况下，重建一个新的polynomial。

假定我们想要计算$[a;f_a]_t * [b;f_b]_t = [ab;f_af_b]_{2t}$。此时每个玩家已经被分配了$[a;f_a]_t, [b;f_b]_t$，每个player计算出$a_{i}b_{i}$，我们令其为$h(i)$，但我们不能直接用$(i,h(i))$插值出$h$，因为 $h$ degree是$2t$，我们固然可以计算出全部$n$个点$((i,h(i)))_{i<=n}$(方法就是$a_j = \sum_{i \in C} \delta_i(j)h(i)$，同理计算出b，得到$h(j)$)，但degree的倍增，使得这种策略难以为继。注意到，$h(0)=\sum_{i=1}^n \delta_i(0)h(i)$，由于h本身就是$2t$的，这里必须要用到n个点，但我们只需要计算出来$h(0)$，并不需要秘密继续以2t多项式的形式共享。

因此，我们让每个player i 分发一个新的多项式$[h(i);f_{h(i)}]_t$。定义$(r_i)_{i \leq n} = (\delta_i(0))_{i \leq n}$。我们有$\sum_{i=1}^n r_i[h(i);f_{h(i)}] = \sum_{i=1}^n [r_ih(i);r_if_{h(i)}]=[ab;\sum_{i=1}^nr_if_{f(i)}]_t$。如此，在计算出乘法的同时，使得share的多项式继续保持t degree。

#### proof

1. correctness: 不难看出
2. privacy: 亦不难证明。Intuitively，我们将所有操作分成两种类型，第一类是input sharing和multiplication gate，其实也就是不使用output y，我们就能模拟的操作，第二类是output reconstruction，就是使用output y，我们可以推理出在该操作中，corrupt players所见的全部信息。

不过，只讲直觉，实在有违理论的优雅美感。

#### formalizing proof

分两步证明，首先把上述两类操作定义清楚。

1. 定义了一个函数$\text{Strip(view}_j\text{)}$。作用就是，剔除掉诚实用户的sharing，恶意用户输出的distribution.证明的方法就是，给定两个输入向量，其中只用corrupted players的输入相同，如此证明$\{\text{Strip(view}$ $_{j}^{0}\text{))}\}_{P_j \in C}$ $=\{\text{Strip(view}$ $_{j}^{1}\text{))}\}_{P_j \in C}$。逐gate证明，核心就是对于一个t次的多项式，至少需要t+1个点可以插值出原多项式，同时只给定t个点，无法或者任何有关原多项式截距的信息，即uniform distribution。也用到了数学归纳法。

2. 再定义函数$\text{Dress}_{\textbf{y}_C}$，输入一个被剥夺的Strip(View)，和协议输出，可以补全view。因为多了一个点$(0,y_j)$，所以可以正确补全。

然后，定义simulator S:
1. The input to $S$ is $\{x_j, y_j\}_{j \in C}$
2. It defines a global input vector $x^{0} = (x_1^{0}, ..., x_n^{0})$, where $x_j^0 = x_j$ if $P_j \in C$ and $x_j^0 = 0$ otherwise.
3. It runs CEPS on $x^0$
4. Outputs $\text{Dress}_{\textbf{y}_C}\text{(\{Strip(view}_j^0\text{)\}}_{P_j \in C}$ 

$\lvert C\rvert = t$很好证明，$\lvert C\rvert < t$需要特殊注意。*如果有空，可以证明一下*。

## optimal bound of corruption

如果我们想计算所有函数，$t < n/2$是最佳的bound了，因为在这种情况下，我们证明了可以计算算术电路，一旦超过这个bound，哪怕是简单的AND电路都不可安全计算。

证明亦没有难度。



