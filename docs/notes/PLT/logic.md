---
layout: default
title: Logic Basis
parent: Programming Language and Logic
grand_parent: Notes
nav_order: 0
---
# head 1
## Problem 1
First, we have $BPP \subseteq BPP^{BPP}$. Then we want to prove $BPP^{BPP} \subseteq BPP$.

By definition, for any langage L in $BPP^{BPP}$, there is an oracle language A which can be decided by a polynomial time PTM $M_0$ and a polynomially time-bounded probabilistic oracle Turing machine $M^A$, such that the probability of error $ P \leq \frac{1}{4} $.

We assume there is a PTM $M'$ on which we can simulate $M^A$. When $M^A$ query to orcale, we should simulate $M_0$ on $M'$. Suppose that there are totally $T(n)$ querys, which n is the length of input and $T(n) = 2O(n^c)$ which means $e^{T(n)} >= 4$ and at most has polynomial querys. If $ x \in L $, 
$P[M'(x) = 1] = P[M^A(x) = 1]\prod_{1}^{T(n)}P[Q_i]= P[M^A(x) = 1]P[Q]^{T(n)}$
$$\begin{array}
1 2 3
	\end{array}$$
where $Q_i$ means for query $q_i$, $M'$ have a correct assert.

We can use success amplification when we simulate $M_0$ on $M'$. The conclusion of success amplification is that we can achieve error probability exp(-q(n)) by running $k = \frac{p(n)^2q(n)}{2}$ if single error probability is less than $\frac{1}{2} - \frac{1}{p(n)}$. In this case, we let $q(n) = T(n), p(n) = 4$. Then $k = 8T(n), P[Q] = 1 - \frac{1}{e^{T(n)}}$.

We use success amplification when we simulate $M^A$ on $M'$ similarly. Assume that $q'(n) = 2$ which can lead $exp(-q'(n))$ $< \frac{1}{4}$. We can show that $$1 - P[M'(x) = 1] = 1 - \frac{3}{4}(1 - \frac{1}{e^{T(n)}})^{T(n)} = \frac{1}{2} - \frac{1}{p'(n)}$$ $$p'(n) = \frac{4e^{T(n)^2}}{3(e^{T(n)} - 1)^{T(n)} - 2e^{T(n)^2}} < \frac{4e^{T(n)^2}}{3(e^{T(n)} - \frac{1}{4}e^{T(n)})^{T(n)} - 2e^{T(n)^2}} = 16$$ $$k < 256$$.

Let's analysis time complexity. We at most repeat 256 times simulation of $M^A$. In every simulation there are 8T(n) times simulation of $M_0$. All of these are polynomials. So totally $M' \in BPP$. Qed.

## Problem 2
### question 1
For proving coRP $\subseteq $ MA, we use the alternative characterization that is $L' \in $P, L $\in$ coRP, $$x \in L \Rightarrow \Pr_{y\in\{0,1\}^{p(x)}}(\langle x,y \rangle \in L')=1$$
With this proposition, we can easily derive that
$$\exists z=\{0\}^{p(x)}.\Pr_{y\in\{0,1\}^{p(x)}}(\langle x,y\oplus z \rangle \in L')=1$$
The other side is,$$x \notin L \Rightarrow \Pr_{y\in\{0,1\}^{p(x)}}(\langle x,y \rangle \in L')< \frac{1}{4}$$
Because we choose y uniformly and XOR operation wouldn't change the distribution, we can imply that
$$\forall z.\Pr_{y\in\{0,1\}^{p(x)}}(\langle x,y\oplus z \rangle \in L')<\frac{1}{4}$$
Thus, we have coRP $\subseteq $ MA. MA is a special case of MA$_2$, so MA $\subseteq$ MA $_2$ is obvious. \newline \newline
For proving NP $\subseteq$ MA, we use the defination of nondeterministic Turing machine that is if a polynomial time bounded NTM accepts x then there exist a path of polynomial length which leads from start state to accept state. If NTM rejects x then all paths cannot lead to accept state. We use NTM with only two choices of transition function for encoding path as $\{0,1\}^{p(x)}$ which is exactly the string z.
\newline\newline
The proof of BPP $\subseteq$ MA$_2$ is same as that of coRP $\subseteq $ MA.
### question 2
I think it's obvious that we can repeat enough times to get enough probability.
### question 3
Assume M is a BPP algorithm deciding L and has probability of error at most $\frac{1}{4m}$ with success amplification, where $m = q(\lvert x\rvert)$. We define $S_x = \{y\in \{0, 1\}^m | \langle x,y\rangle \in L \}$.
$$x \in L \Rightarrow \lvert S_x\rvert \geq (1 - \frac{1}{4m})2^m$$ and $$x \notin L \Rightarrow \lvert S_x\rvert \leq \frac{1}{4m}2^m$$
If $x \in L$, we need to prove
$$\forall y\exists z.y\oplus z \in S_x$$
$$\forall y\exists z.y \in S_x\oplus z$$
That means with given $\{ z_1, z_2, ... , z_m\}$, $\bigcup_{i=1}^mz_i \oplus S_x$ can represent any string y of $\{0, 1\}^m$. We define an event $Q = \{\forall y.y \notin \bigcup_{i=1}^mz_i \oplus S_x\}$, then $$\Pr(Q, y)  = \Pr(y = y', y \notin \bigcup_{i=1}^mz_i \oplus S_x)=\Pr(\forall i.z_i \notin y' \oplus S_x) \leq (\frac{1}{4m})^m < \frac{1}{2^m}$$
$$\Pr(\bar Q, y) = \Pr(y) - \Pr(Q, y) > 0$$
$\forall y.y \in \bigcup_{i=1}^mz_i \oplus S_x $ holds.Then this side is proved.\newline\newline
If $x \notin L$, we need to prove
$$\Pr(\bigcup_{i=1}^m y \oplus z_i \in S_x) = \Pr(y \in \bigcup_{i=1}^m z_i \oplus S_x) < \frac{1}{4}$$
We have
$$\lvert \bigcup_{i=1}^m z_i \oplus S_x \rvert \leq m\lvert S_x \rvert = \frac{1}{4} 2^m = \frac{1}{4} \lvert \{0,1\}^m\rvert$$
Qed.
### question 4
We directly prove MA = MA$_2$. By definition, if x $\in$ MA$_2$ and a language L' $\in$ P $$\exists z = z_0.\Pr_{y\in\{0,1\}^{p(x)}}(\langle x,y,z_0 \rangle \in L') \geq \frac{3}{4}$$ Similarlay, we define $$S_{x,z_0} = \{y|\langle x, y, z_0 \rangle \in L'\}$$ $$\lvert S_{x, z_0}\rvert \geq (1-\frac{1}{4m})^{2^m}$$
We define a new L'' with input $\langle x, y, z\rangle$, L'' would simulate L' with input $\langle x, y\oplus z, z_0 \rangle$. Because $z_0$ is polynomial length, L'' $\in$ P obviously. Then we should prove,
$$x \in MA_2 \Rightarrow \exists z.\Pr_{y\in\{0,1\}^{p(x)}}(\langle x,y,z \rangle \in L'') = 1 $$
$$x \notin MA_2 \Rightarrow \forall z.\Pr_{y\in\{0,1\}^{p(x)}}(\langle x,y \rangle \in L')< \frac{1}{4}$$
The proof is same as that of question 3.
## Problem 4
First of all, we notice that LOG$^1$BCOUNT can be computed by constant depth circuits of size $n^{O(1)}$ of unbounded fanin AND and OR gates because the function size is $O(\log_2^n)$. \newline\newline
We consider the input as k-dimension matrix.
In the first time we sum bits in one direction, there are at most $\log_2^n$ non-zero elements totally. We can use LOG$^1$BCOUNT circuit with constant depth. So these elements in this dimension are replaced by a binary number with $\log_2^{\log_2^n}$ length. \newline\newline
We notice there is the same trick as what we use in note. Define $l(n) = \log_2^n$. So we can reduce this sub-problem to LOG$^1$BCOUNT with constant depth circuit. \newline\newline
Iterativelly, we can prove for any k, LOG$^k$BCOUNT can be computed by constant depth circuits of size $n^{O(1)}$ of unbounded fanin AND and OR gates.

