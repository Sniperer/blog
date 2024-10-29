# PPAD

PPAD is the class of all problems in TFNP that are polynomial-time reducible to END-OF-LINE.

## END-OF-LINE

Given two boolean circuits P and S with n input bits and n output bits such that $P(0) = 0 \neq S(0)$, find x such that P(S(x)) != x or $S(P(x)) \neq x$ or $S(P(x)) \neq x \neq 0$.

In other words, given a source 0, find another notrivial source or sink $x \neq 0$ using P and S, where trivial point is isolated point. It's a total search problems.

## PPAD-complete

A problem is complete for PPAD if it belongs to PPAD and if END-OF_LINE reduces in polynomial time to that problem.

# PLS

PLS is the class of all problems in TFNP that are polynomial-time reducible to LOCALOPT.

## LOCALOPT

Given two boolearn circuits V and S, where $V: [2^n] \rightarrow \mathbb{R}$ and  $S: [2^n] \rightarrow [2^n]$. Find an $v \in [2^n]$ such that $V(S(v)) \geq V(v)$.

PLS embodiees general local search methods where one attempts to optimize some objective function by considering local improving moves.

## PLS-complete

The definition is similar to PPAD-complete.

There is a problem which is known to be PLS-complete.

### ITER

Given a boolean circuit $C: [2^n] \rightarrow [2^n]$ such that $C(v) \leq v$ for all $v \in [2^n]$ and $C(1) > 1$. Find v such that $C(v) > v$ and $C(C(v)) = C(v)$.

It means that for any node that is not a fixed point, but is mapped to a fixed point. It cannot be held that for all $v \in [2^n]$ such that $C(v) > v$. There must be some fix points and, if $C(1) > 1$, there must be some points that map to fix points.

## Arithmetic Circuit

An arithmetic circuit is a circuit using gates in $\{+, -, \times,\div, max, min\}$ as well as rational constants.

There are some questions in this definition, which is mentioned in linked paper. If we don't limit the number of multiplication gate, the size could be $2^{size(f), size(x)}$. But I don't totally understand.

### PL Arithmetic Circuit

A PL arithmetic circuit is a circuit using gates in $\{+, -, max, min, \times \zeta \}$ as well as rational constants, where $\times \zeta$ denotes multiplication by a rational constant.
