# Hash Functions

A random function that maps a large universe to a small range.

The function should distribute the items "evenly".

> Low collision probability: for any $x≠y$, we want $Pr(h(x) = h(y))$ to be small.
> 
> Fast evaluation.
> 
> Small space.

## Universal hashing

Let $H$ be a family of functions mapping a universe $U$ to $\{0, ..., m-1\}$.

$H$ is universal if for any $x ≠ y$ in $U$ and $h$ chosen uniformly at random from $H$

$$
Pr(h(x) = h(y)) \leq \frac{1}{m}
$$

* Multiply-mod-prime: $h_{a,b}(x) = (a \cdot x + b) \bmod p ,\quad H = \left\{ h_{a,b} \,\middle|\, a \in \{1, \dots, p - 1\},\ b \in \{0, \dots, p - 1\} \right\}$
* Multiply-shift: $h_a(x) = (a \cdot x \bmod 2^k) \gg (k - \ell), \quad H = \left\{ h_a \,\middle|\, a \text{ is odd in } \{1, \dots, 2^k - 1\} \right\}$

## Chained Hashing (Zipper method, Open hashing)

Dictionary problem is to maintain a dynamic set of integers subject $S \subseteq U$ to support LOOKUP(x), INSERT(x) and DELETE(x).

<p align="center"><img src=".data/hashing_chained.png" alt="pic" width="40%" /></p>

> Choose universal hash function $h$ from $U$ to ${0, ..,m-1}$.
> 
> Initialize an array $A[0, ..., m-1]$.
> 
> $A[i]$ stores a linked list containing the keys in $S$ whose hash value is $i$.
>
> Space: $O(m + n) = O(n)$
> 
> Time: $O(1 + |A[h(x)]|)$
>

```cpp
constexpr int SIZE = 1000000;
constexpr int M = 999997;

struct HashTable {
  struct Node {
    int next, value, key;
  } data[SIZE];

  int head[M], size;

  int f(int key) { return (key % M + M) % M; }

  int get(int key) {
    for (int p = head[f(key)]; p; p = data[p].next)
      if (data[p].key == key) return data[p].value;
    return -1;
  }

  int modify(int key, int value) {
    for (int p = head[f(key)]; p; p = data[p].next)
      if (data[p].key == key) return data[p].value = value;
  }
};
```
### Open Addressing (Closed hashing)

The closed hashing method stores all records directly in the hash table, and if a conflict occurs, the search continues in some way. 

For example, the linear probing method: if a conflict occurs at $d$ , check $d + 1$ , $d + 2$ , etc. in turn. Also, there are Quadratic Probing and Double Hashing.

## Perfect Hashing

Static dictionary problem is to give a set $S \subseteq U = \{0,..,u-1\}$ of size $n$ for preprocessing support LOOKUP(x).

A perfect hash function for $S$ is a collision-free hash function on $S$.

### Collision-free but with too much space

<p align="center"><img src=".data/hashing_perfect_solution1.png" alt="pic" height="25%" /></p>

> Array $A$ of size $n^2$
> 
> Universal hash function mapping $U$ to $\{0, ..., n^2-1\}$. Choose randomly during preprocessing until collision-free on $S$. Store each $x \subseteq S$ at position $A[h(x)]$.
>
> Space: $O(n^2)$
>
> Time: $O(1)$

### Many Collision but linear Space

> E: $\frac{n}{2}$

### FKS-Scheme

<p align="center"><img src=".data/hashing_FKS.png" alt="pic" width="60%" /></p>

> At level 1 use solution with many collisions and linear space.
> Resolve each collisions at level 1 with collision-free solution at level 2.

## String Hashing

Karp-Rabin 