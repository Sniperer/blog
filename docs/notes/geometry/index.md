## convex hull

It cannot be better than $O(nlogn)$ because it should be as hard as sorting. We want to care more about output-sensitive which means less output is better performance is. Thus, we have $O(nh)$ gift wrapping algorithm and $O(nlogh)$ Chan's algorithm and marriage-before-conquest.

### Marriage-before-Conquest

#### Prove it's O(nlogh)

Obviously, each bridge is an edge in convex hull. Consider the recursion tree. Each layer would add $2^i$ bridges. Thus there are at most $log h$ layers in recrusion tree. Consider the wrost case where no points were deleted in computation until last layer. Thus there are n points in each layer. Because we use linear time algorithm each layer. We can conclude it's $O(nlogh)$

#### Prove $O(\sum_{i = 1}^{h - 1} n_i log(\frac{n}{n_i}))$

Consider the recursion tree. In any node of $i$-th layer, the size of searching space is $O(n/(2^i))$. It means for any $n_i$, it can survive at most $log(n/n_i)$ layers. Because in $log(n/n_i)$-th layer, the size of seearching space is $2n_i$. The median must be in a $n_i$-size part.

Then we can consider the contribution of each $n_i$. It would be $n_1 O(n/n_1) +... + n_i O(n/n_i) + ... + O(n/n_{h-1}) = O(\sum_{i = 1}^{h - 1} n_i log(\frac{n}{n_i}))$.

### Separator

First, prove there always exist some separator. When $n = 3$, there is no possible separator because each edge is boundary of polygon. For $n > 3$ there is always some triangulation of simple polygon, just choose any possible triangulation. Moreover, there are $n - 2$ triangles. Every line in triangles which lies inside polygon is a possible separator.

When $n = 4$, every possible separator is with balance at least 1/3. Next, we would show the case $n > 4$.

We claim there must be some triangles in any triangulation which have at least two lines lie inside polygon. Otherwise, $n - 2$ triangles would have $2(n-2)$ different edges which are boundary of $n$-edge polygon. However, when $n \leq 4$ it's impossible to hold $2(n-2) \leq n$.

Pick any triangle which has exactly three lines lie inside polygon. Then it would divide the whole polygon into three parts. Without loss of generality, we denote the size of vertices in three parts as $n_1, n_2, n_3$. If none of these three lines are with balance at least $\frac{1}{3}$, it implies $\frac{n_1}{n} < 1/3,\frac{n_2}{n} < 1/3, \frac{n_3}{n} < 1/3$. Then $n_1 + n_2 + n_3 < n = (n_1 + n_2 + n_3 -3)$. It's impossible. Thus, there must be one of three edges are separator with balance at least 1/3.

Then, we just choose any triangle which has two lines lie inside polygon and one boundary of polygon. It would divide the whole polygon into three parts. Without loss of generality, we denote the size of vertices in two parts as $n_1, n_2$. If none of two lines inside polygon are with balance at lest $\frac{1}{3}$, it implies $\frac{n_1}{n} < 1/3, \frac{n_2}{n} < 1/3$. Then $n_1 + n_2 < \frac{2}{3} (n_1 + n_2 - 1)$. It's still impossible.

Now, we can conclude that we can always choose one type of above triangles and then some line of them are separator with balance at least 1/3.
