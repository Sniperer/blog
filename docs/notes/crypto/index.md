---
layout: default
title: Cryptology
parent: Notes
nav_order: 3
has_children: True
---
## Generate random prime numbers
先提纲挈领的想，我们的目的是生成一个k-bits的素数，也就是说这个素数应该是$2^{k-1} \leq x < 2^{k}$。一个简单的想法就是，我们随机生成这个区间内的数，然后测试生成的数是不是素数。自然的，我们首先要解决的问题是在这样一个给定区间里，是否存在素数。然后就是如何进行素性测试。
### The Prime Number Theorem
$$\lim_{N\to\infty}(\frac{\pi_N}{N/\ln(N)}) = 1 , \pi_N \text{ means the number of primes less than N}$$
$$\pi_{2^k}-\pi_{2^{k-1}} = \Omega(2^{k-1}/k)$$
Then we choose a random k-bits number which could be a prime with the probability of $\Omega(1/k)$. Thus we can choose k candidates expectedly.
### Miller-Rabin Primlity Test
1. 输入x，则将x-1改写成$2^ab$
2. 随机选择一个非零$\omega \in Z_x$, 计算$gcd(\omega,x)$。如果gcd不为1，则不是素数。
3. 当gcd = 1，则可知$\omega \in Z_x^*$，我们构造如下序列$\omega^b \mod x, \omega^{2b} \mod x, \omega^{2^2b} \mod x, ..., \omega^{2^{a-1}b} \mod x$，如果这个序列中有任何一个元素是-1或者$\omega^b \mod x = 1$, 则接受为素数，否则，拒绝。

目前为止，我见到的和computational complexity最相关的一个结论或者应用，这里用复杂度的方式重写一下completeness和soundness。

$$ x \in Primes \Rightarrow \Pr_{w \in Z_x}(MR(x,w)=1) = 1$$

$$ x \notin Primes \Rightarrow \Pr_{w \in Z_x}(MR(x, w)=1) < \frac{1}{4}$$

同样的success amplification也同样可以应用在这里。soundness不作证明，因为这毕竟不是在研究复杂度。只证明completeness。

如果x是素数，则$Z_x^*$的阶为x-1，那我们考虑最后一项的平方，

$(\omega^{2^{a-1}b})^2 \mod x = \omega^{2^{a}b} \mod x = \omega^{x-1} \mod x = \omega^{x-1 \mod x-1} \mod x = 1 \mod x$

我们又知道如果x是素数，那么

$\forall n \in Z_x^*. n^2 \mod x = 1 \mod x \rightarrow n^2 - 1 \mod x = 0 \rightarrow (n-1)(n+1) \mod x = 0$

由于x本身除了1没有任何因子，则有$x\vert n-1$ or $x\vert n+1$. Thus, we have $n \mod x = 1 \mod x$ or $n \mod x = -1$. 因此我们能得到倒数第二项必模x余1或-1. 如果是-1，我们接受x, 否则以此递推。所以如果有任何数是-1，就能接受；否则推到第一项也能接受。

我们需要对soundness有个正确的理解，如果一个数不是素数，他被接受的概率小于s。这并不等价于MR测试出错的概率，后者是指一个通过测试的数，他不是素数。很显然出错的概率和素数的分布有关。除了可以使用success amplification外，soundness本身也随素数位数收敛到0，Ivan给出了这个更强的结论。考虑到Ivan数学系的背景，对于一般的计算机学生，这个结论并不一般。
> Let p_{k,t} be the probability that the prime generation algorithm outputs a composite on input k and when using $MR^t$ as primality test. Then for $k \geq 2$ we have $p_{k,1} \leq k^24^{2-\sqrt k}. Furthermore, for 3 \leq t \leq k/9$ and $k \geq 88$, we have $p^{k,t} \leq k^{3/2}t^{-1/2}4^{2+t/2-\sqrt{tk}}$.

至此，MR primality test足够正确，我们考虑是否足够高效.<span style="color:red"> O(k^3) ？？？ </span>

# Symmetric Authentication Systems

1. Adversary can only change the message
2. Adversary can change both message and authenticator
    1. Sender and receiver share a secret key
    2. Sender and receiver share no secret keys in advance

## Hash Functions

基本知识不用多介绍，密码学的角度理解将任意长比特串映射到固定长，是为了这个计算出来的新比特串，能够被合理的保存，如果计算出来很长一大串，别人也没法处理，应该和tcs的角度殊途同归。剩下的就是一些名词：

**generator**散列函数簇，family of hash function, or whatever，接受一个参数k，产生一个具体的hash function。我不太清楚这个概念是tcs先有还是密码学先有，但确实解释了tcs没解释过的。为什么我们需要一个family而不是一个特定的简单的函数。显然任意单一函数都不是collision intractable，所以我们就找一大堆函数，从里面挑一个，adversary没办法在PPT时间内针对每一个具体的函数。

**message digest or message fingerprint**就是经过散列函数计算出来的新串

**second preimage attack**二次原象攻击，知道了m，找一个m'，使得$h(m) = h(m')$

**collision attack**碰撞攻击，不同于second preimage attack，如果一个散列函数存在碰撞，我们可以引导最忠实的用户，传输产生碰撞的信息。很明显，能抵抗collision attack的散列函数强于second preimage attack
