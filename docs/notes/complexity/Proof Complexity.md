---
layout: default
title: Proof Complexity
parent: Computational Complexity
grand_parent: Notes
nav_order: 0
---

# Proof Complexity 

## Propositional Logic

假设一个可以用公式$\alpha(p_1, p_2, ..., p_n)$计算的真值表，我们定义$tt_{\alpha}: \bar{b} = (b_1, ..., b_n) \in \{0,1\}^n \rightarrow \alpha(b_1, ..., b_n) \in \{0,1\}$。我们取该表中，所有值为1的格子，假设此时对于公式$\alpha$的输入$(p_1,...,p_n)$，不难发现$p_1^{b_1} \wedge p_2^{b_2} \wedge ... \wedge p_n^{b_n} = 1$当且仅当$b_i = p_i$。因此$\bigvee{(p_1^{b_1}\wedge ... \wedge p_n^{b_n})}$恰好可以表示这个真值表。我们定义这种表示形式为DNF。

### Boolean functions' Monotone
我们定义，如果两个赋值向量$\bar{a}\leq \bar{b}$，其中$\leq$意味所有赋值都小于等于，我们有$f(\bar{a}) \leq f(\bar{b})$。我们称这种性质为monotone。

注意到，对于monotone的boolean function，我们实际上可以在DNF中不使用negation。如果在上述真值表中，存在一个$tt(0,p_2,...,p_n) = 1$，由monotone可知，$tt(1,p_2,...,p_n) = 1$。也因此，我们可以以上述方法构造一个DNF，对于$(0,p_2,...,p_n)$以及$(1,p_2,...,p_n)$我们可以将这两项融合，这导致原本对于$p_1$的negation被消融掉。

### Interpolant Theorem
Let $\bar{p}, \bar{q}, \bar{r}$ be disjoint (possibly empty) tuples of atoms. Assume that $\alpha(\bar{p}, \bar{q}) \models \beta(\bar{p}, \bar{r})$. Then there exists a formula $\gamma(\bar{p})$ such that $\alpha \models \gamma$ and $\gamma \models \beta$.

Proof：这个证明惊人的简单，考虑两个集合$U = \{\bar{a} | \exists {b},tt_{\alpha}(\bar{a}, \bar{b}) = 1\}$和$V = \{\bar{a} | \exists \bar{c}, tt_{\beta}(\bar{a}, \bar{c}) = 0\}$。我们断言这两个集合不交。由假设$\alpha \models \beta$可得。由此U和V将$\{0,1\}^*$划分成了两类，存在一个$\gamma$，使得在U上的所有值输出为1，在V上的所有值输出为0。如果有一个为真的$\alpha$，自然$\bar{a}$在U中，因此$\gamma$为真。如果有一个为真的$\gamma$，自然$\bar{a}$不应在V中，也就是说对于任意$\bar{c}$，$tt_{\beta}(\bar{a}, \bar{c})$都为真。因此，蕴含$\beta$。

### size of a formula
在学习的过程中应该加强思考啊。如果我们想衡量一个boolean formula的大小，一个自然的方式可以是在公式中一共出现了多少个literal，但是，我们在自动机或者图灵机这种一般的计算模型下，假设字符集有限。这也就是说在formula中可能出现n个literal，但有限字符集并不能表示出所有$x_1, x_2,..., x_n$。因此至少需要额外$\log{n}$大小，来表示所有的literal。

并且，如果我们只从size的角度看，使用$\oplus$和只使用$\wedge,\vee,\neg$的formula，不能简单的具有相同的计算能力。$\alpha \oplus \beta = (\alpha \wedge \neg \beta) \vee (\neg \alpha \vee \beta)$，那么n次嵌套的XOR会导致指数级大小的或与非。因此，我们引入了logical depth的概念。

### depth of a formula
顾名思义。但我们有如下结论，构建了以size无法衡量的计算能力。

Let $\alpha$ be any formula in the DeMorgan language augmented by the binary connective $\oplus$ and assume that the size of $\alpha$ is $s$. Then there is a DeMorgan formula logically equivalent to $\alpha$ and of size at most $2^{O(\log{s})} \le s^{O(1)}$.

想要证明上述结论，我们首先需要引入Spira's Theorem。

Let T be a finite k-ary tree and $|T| > 1$. Then there is a node $a \in T$ such that $(1/(k+1))|T| \le |T_a|, |T^a| \le (k/(k+1))|T|$.

思路如下：从根向叶子节点遍历，叶子节点的数量会从$|T|$减小到0。每层我们都选取拥有最大叶子数的子节点。直到减少到$(k/(k+1))|T|$。不难验证，此时符合所有要求。

Let $\alpha$ be a formula in a language consisting of at most k-ary connectives and assume its size is $s$. Then there is a logically equivalent DeMorgan formula $\beta$ of logical depth $ldp(\beta) \le O(\log_{(k+1)/k}{s}) = O(\log{s})$.

这个证明书上写的前半段很对，后半段我认为有点问题。前半段的思路是，我们知道一个formula是由几个命题决定的，那么我们抠掉一个clause或者任一的一个部分，此时剩下的部分可能依旧是由原来的全部命题决定，或者是其中的一部分；此外，我们同样可以使用原来的命题的一部分，或者全部，来计算我们扣掉的这一部分。这样就构成了一个substitution，我们可以刻画一个等价命题。具体来说，假设在$\alpha$中的命题为$\bar{p}$，我们定义一个新的formula $\gamma(\bar{p}, q)$,其中$q$是新引入的命题，我们使用$\delta(\bar{p})$替换命题q。

于是$\alpha$可以被表示为$\alpha = \gamma(q/\delta)$。因此等价于$(\gamma(\bar{p},1)\wedge\delta)\vee(\gamma(\bar{p},0)\wedge\neg\delta)$。

那么，新的formula的depth就更新为了$2 + \max{(ldp(\gamma), ldp(\delta))}$。

上面使用$\gamma, \beta$替换原formula的过程，也就是寻找一个$\alpha$的子树。因此我们可以使用Spira定理。使得划分出的两个部分，每个的size小于$\alpha$的$k/(k+1)$倍。递归地使用上述方法，我们可以将$\gamma', \delta'$的电路深度降到0通过$O(\log_{(k+1)/k}s)$，那么新的电路深度就会达到$2O(\log_{(k+1)/k}s) = O(\log{s})$。

但是，Spira定理中的$|T|$按照书上的说法是叶子节点数，并不能等同于一个非满树的所有节点数。但我们似乎可以直接把原定理改成size数，<font color="red">证明亦相同（？</font>。

由于对数深度，那么我们即使是计算添加了XOR门的formula也可以通过多项式大小的size。


## first-order logic vs second-order logic

这东西太TM抽象了。

我们以Peano公理为例，在一阶逻辑的Peano公理中，我们可以有$\forall x(x + 0 = x), \forall x\forall y(x + S(y) = S(x+y))$，这些公理只能对自然数进行量化，不能对所有关系进行量化。但在二阶Peano公理中，我们可以有$\forall P ((P(0) \wedge \forall x(P(x) \rightarrow P(S(x)))) \rightarrow \forall x P(x))$，这就是数学归纳法的完整表达。<font color="red"> 另一个例子是集合论上的，ZFC公理，留个坑。</font>

还要再解释一下L-structure，是一个赋予特定语言L解释的数学结构，它由一个非空的基础集合universe和一组符合L语言的符号的解释组成。我们假设L包含，一个二元关系$<$，一个常数符号0，一个一元函数符号s。（表示后继）。那么一个可能的L-structure M可以是，基础集合为自然数集，对于其他常数，函数和关系我们都有对应的解释。这构成了标准的自然数结构$(\mathbb{N}, <, 0, s)$。

我们已经知道在谓词逻辑中，任何命题，可以表示为对数深度的电路，换句话讲，我们可以用对数深度的电路判断一个谓词逻辑中的命题。那么，对于一阶逻辑呢。问题棘手的地方在于，一阶逻辑中允许使用量词。仅以电路来说，我们fix一个assignment，自然可以使用和谓词逻辑相同的方法判断L-structure是否包蕴含一个仅有连接词的命题，可是怎么判断是否蕴含呢。但是，一旦允许量词，这对电路的计算能力是一个挑战，如果我们有n个绑定变量，那么我认为，我们需要线性深度的电路。（？因为我们至少要覆盖$2^n$个取值，再使用对数深度构造逻辑电路，就是线性的。

如果使用TM，我们有定理如下：
Let L be finite and let $B(x_1, ..., x_n)$ be a first-order L-formula. Then there exists a deterministic log-space machine that upon receiving a finite L-structure A and an n-tuple $\bar{a}$ of its element decides whether the satisfaction relation $A \models B(a_1, ..., a_n)$ holds.

我们定义语言strict $\sum_1^1$-formulas, 写作$s\sum_1^1$，由所有形如$\exists X_1\exists X_2 ... \exists X_k B(X_1,...,X_k)$组成。

我们有Fagin's theorem: Let L be a finite language. A class of finite L-structure is s$\sum_1^1$-definable if and only if it's in the computational class NP.

这个证明书上没写，说是和SAT是NP-complete相似。可以参考wiki上对此的证明。也就是[Cook-Levin theorem](https://en.wikipedia.org/wiki/Cook%E2%80%93Levin_theorem#cite_note-6)。

