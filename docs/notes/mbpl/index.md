---
layout: default
title: Mathematical Basis of Programming Logic
parent: Notes
has_children: true
nav_order: 2
---
# Preliminary

## Call-by-value versus. Call-by-name
[call-by-value & call-by-name](https://www.cs.princeton.edu/courses/archive/fall16/cos326/lec/17-cbn-cbv.pdf)


Thus, in an application first the term in the function position is evaluated to a value and only then the argument is evaluated.

## monoid
A set S equipped with a binary operation S × S → S, which we will denote •, is a monoid if it satisfies the following two axioms:

***Associativity***

For all a, b and c in S, the equation (a • b) • c = a • (b • c) holds.

***Identity element***

There exists an element e in S such that for every element a in S, the equalities e • a = a and a • e = a hold.

### communative monoid
Any commutative monoid is endowed with its algebraic preordering ≤, defined by x ≤ y if there exists z such that x + z = y.

# Montone RA


