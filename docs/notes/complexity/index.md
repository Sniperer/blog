---
layout: default
title: Computational Complexity
parent: Notes
nav_order: 1
has_children: True
---
* TOC
{:toc}

# Preliminary

## Notation

<table>
  <tr>
    <th>Motation</th>
    <th>Name</th>
    <th>Formal Definition</th>
  </tr>
  <tr>
    <td>$f(n) = o(g(n))$</td>
    <td>Little O</td>
    <td><span style="color: red">$\forall k > 0 \exists n_0 \forall n > n_0.f(n) < kg(n)$</span></td>
  </tr>
  <tr>
    <td>$f(n) = O(g(n))$</td>
    <td>Big O</td>
    <td>$\exists k >0 \exists n_0\forall n>n_0.f(n) \leq kg(n)$</td>
  </tr>
  <tr>
    <td>$f(n) = \Theta(g(n))$</td>
    <td>Big Theta</td>
    <td>$\exists k_1 >0 \exists k_2 > 0 \exists n_0 \forall n > n_0.k_1g(n) \leq f(n) \leq k_2g(n)$</td>
  </tr>
    <tr>
    <td>$f(n) \backsim g(n)$</td>
    <td>On the order of</td>
    <td>$\forall \epsilon > 0 \exists n_0 \forall n > n_0.\lvert \frac{f(n)}{g(n)} - 1 \rvert < \epsilon $</td>
  </tr>
    <tr>
    <td>$f(n) = \Omega(g(n))$</td>
    <td>Big Omega</td>
    <td>$ \exists k > 0 \exists n_0 \forall n > n_0. f(n) \geq kg(n)$</td>
  </tr>
    <tr>
    <td>$f(n) = \omega(g(n))$</td>
    <td>Small Omega</td>
    <td><span style="color: red">$\forall k > 0 \exists n_0 \forall n>n_0.f(n) > kg(n)$</span></td>
  </tr>
</table>

## Probabilistic Method

# Turing machine
## euqailty between various Turing machine
All of these are sequential storage Turing machine.
### one tape Turing machine

### two tapes Turing machine

### K tapes Turing machine

### oblivious Turing machine
这是一个重要的模型，是证明复杂度下界的重要手段，也揭示了circuit和TM之间的关系。

理解上的难度主要在于oblivious，我认为翻译成不经意为佳，一个算法是oblivious的也就是说当找到解后并不停止，而是继续遍历其他的情况。以查找为例，典型的顺序查找可以实现为oblivious的，但二分查找就很难实现，因为每次查询范围由查询结果确定，找到结果后算法如何继续呢？

[Pippenger and Fischer, “Relations Among Complexity Measures.”](/docs/notes/complexity/Relations%20Among%20Complexity%20Measures.html)


上面引用的论文，主要证明了两个结论：(在one-dimension条件下)

1. If M is a transducer with one-dimensional tapes, there is an oblivious machin M' with two one-dimensional tapes that simulates M on-line in time O(nlogn).
2. If M is a transducer with one-dimensional tapes, there is a combinational logic network imiplementing n steps by M with cost O(nlogn) and depth O(n).

据此，我们可以引申为：***Any T-time Turing Machine can be simulated by an O(TlogT)-size circuit***

# reduction
## LOGSPACE REDUCTION
Informally, there is a $O(\log \lvert x \rvert)$ space machine that given input (x, i) outputs f(x) i-th bit provided $i \leq \lvert f(x) \rvert$. We say that f is implicitly logspace computable.

# circuit
## CNF and DNF
CNF ***$\bigwedge_{i}(\bigvee_{j} x)$*** is short for Conjunctive Normal Form, and DNF ***$\bigvee_{i}(\bigwedge_{j} x)$*** is short for Disjunctive Normal From.

### SAT

Let SAT be the language of all satisfiable CNF formulae and 3SAT be the language of all satisfiable 3CNF formulae. Then, SAT and 3SAT are both NP-complete. (Cook-Levin Theorem)

### universality of boolean operation
> For every boolean function f : $ \\{ 0,1 \\}^{l} \rightarrow \\{ 0,1 \\}$, there is an l-variable CNF formular $\phi$ of size $l2^l$ such that $\phi(u) = f(u)$ for every $u \in \\{ 0,1\\}^{l}$.

It's easy to show because for every possible input, we can construct a boolean circuit to identify it. For example, if the input is $\langle 1,1,0,1 \rangle$, we can use $\bar x_1 \vee \bar x_2 \vee x_3 \vee \bar x_4$. And then we conjunct all identify circuit of inputs that have output 1. The size is $O(l2^l)$.

## circuit complexity

### P/poly
P/poly = $\cup_{c} \text{SIZE}(n^c)$

## uniform computation vs. nonuniform computation

Turing machine is a canonical nonuniform computation because it can compute on inputs with any length, but circuit cannot do that. 我们为什么需要这个东西呢，因为我们可以用硬编码将一些本身undecidable的问题，转换成线性复杂度的circuit。例如我们要判定一个长度为n的input是否是unary language，也就是是否属于是一进制串的子集的某个语言。我们可以将这个语言转换到长度小于等于n的，每个一进制串是否在该语言中。这个子判定电路与输入无关也就是常数的，所以整体就是线性的。这就是在耍赖了，也因此我们需要给电路一些限制，使其更具有现实意义。

### LOGSPACE-UNIFORM CIRCUIT FAMILIES

# Interactive proofs

verifier验证证明，prover提供证明。值得一提的是，这一章很多定理由Shafi Goldwasser提出并证明，也因此两获哥德尔奖，12年获图灵奖，杰出的女性理论计算机学家。

## Deterministic Proof System
$$
(completeness) x \in L \Rightarrow \exists P: \{ 0, 1\}^* \rightarrow \{ 0, 1\}* out_V\langle V, P\rangle(x) = 1
$$
$$
(Soundness) x \notin L \Rightarrow \forall P: \{ 0, 1\}^* \rightarrow \{ 0, 1\}* out_V\langle V, P\rangle(x) = 0
$$
$out_V\langle V, P\rangle(x)$ represents $V(x, a_1,..., a_k)$值得注意的是，soundness中out是1还是0没有区别。***dIP*** contains all languages with a k(n)-round deterministic interactive proof systems with k(n) polynomial in n. ***dIP = NP*** <span style="color:red">证明比较trivial，后补</span>。

## IP

$IP = \cup_{c\geq 1}IP[n^c]$

我们假设验证者是probabilistic的，事实上可以证明prover是否是probabilistic对IP无影响。***IP $ \subseteq $ PSPACE***

> Arora and Barak, Computational Complexity.

There are some insights about IP in P8.4(150).

### GNI and GI
Graph Isomorphic is in NP, but we don't konw if it's a P or NP-complete. It's still an open question. The complement of GI, GNI is short for Graph Not Isomorphic.

***GNI属于IP***，非常巧妙的构造。甚至一轮交互就可以证明。我们均匀随机的从两个图选其一，然后随机的生成一个这个图的节点排列，让prover确定这个排列是由哪个图产生的。如果这两个图不isomoriphic，prover产生的结果一定是对的；如果这两个图同构，上帝来了也得抛硬币，因此对的概率小于1/2。

自然而然的我们想到，prover猜不出来的原因是verifier掷硬币的结果不可见，那如果可见呢，是否会弱化IP的计算能力？答案是会弱化，但弱化很小，具体的可以证明[$IP[k] \subseteq AM[K + 2]$](./Private%20coins%20versus%20public%20coins%20in%20interactive%20proof%20systems.html)

***GNI属于AM[k]***

首先我们先定义identity permutation是指形如[1,2,...,n]这种的排列，也就是说$\pi(G) = G$，non-identity permutation是形不如上述的排列。现在有暴论如下，如果一个n个顶点的图没有n!个同构图，则存在non-identity permutation使得$\pi(G) = G$。我们考虑将edge固定为数组里两个元素的链接，则该数组上的全排列都满足同构条件，因此至多n!，但由于可能存在上述non-identity permutation，导致存在一些排列$\pi, \pi^2, \pi^3,..., \pi^k$，使得与原图相同，故我们定义$(H, \pi)$，其中$\pi(H) = H$，如此满足当$G_1 \equiv G_2$，恰有n!上述数对，当$G_1 \not\equiv G_2$，恰有2(n!)个。Then, it can be done by set lower bound protocol.

### Set Lower Bound Protocol

Arora and Barak, Computational Complexity.书里证明有一点疏漏，并且有的比较关键。首先是对pairwise independent hash function的定义就有问题，
> DEFITION(Pairwise Independent hash functions): Let $H_{n,k}$ be a collection of functions from $\\{ 0,1\\}^{n}$ to $\\{ 0,1\\}^{k}$. We say that $H_{n,k}$ is pairwise independent if for every $x, x' \in \\{0, 1\\}^{n}$ with $x \neq x'$ and for every $y, y'\in \\{ 0,1\\}^{k}, \Pr_{h \in_R H_{n,k}}[h(x) = y \wedge h(x') = y'] = 2^{-2k}$.

书里写的是$ 2^{-2n} $，这是没有道理的，因为用这个等式进行双重边缘概率求和，不难发现和不为1，因此我认为这里应该是k，并且这样一来independent也更顺理成章。例子中affine function的证明没有问题。

对set lower bound protocol的证明在使用inclusion-exclusion principle时，乘了个系数1/2，这并不是任何放缩，仅仅是为了便于计数的trick，n个事件两两相交共有n(n-1)/2种，作者把这个1/2从求和空间中写到了系数里，十分迷惑。不难归纳，余项大于0，因此放缩掉了余项。

最重要的问题是，按照作者的逻辑，证明的假设需要满足$ \lvert S\rvert \leq \frac{2^k}{2} $，但我们仅仅在定义里假设$\vert S \rvert \geq K$，其中$2^{k-2} \leq K \leq 2^{k-1} $。实际上，这并不是证明的不完全，如果$\lvert S\rvert > \frac{2^k}{2}$，我们可以将K再扩大二倍，总能找到这样的K和k。问题的关键是，***接收集至少是拒绝集的二倍***。

### IP = SPACE

***Arithmetization***