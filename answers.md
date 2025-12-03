# CMPS 2200 Assignment 4
## Answers

**Name:** Tri Huynh






- **1a.**

Let $n$ be the number of elements in the heap.

A $d$-ary heap is a complete $d$-ary tree, so the maximum depth is about how many times you can divide $n$ by $d$ before you get to 1.
So the maximum depth is $O(log_d n)$.

- **1b.**

For `delete-min`, we remove the root, move the last leaf to the root, and then, at each level, we look at up to $d$ children to find the smallest, which is $O(d)$ work per level.
Since the heap has $O(log_dn)$ levels, the total work is $O(dlog_dn)$.

- **1c.** 

With a d-ary heap, Dijkstra does at most $|V|$ delete-min operations and $|E|$ insert operations into the heap. With the costs from (b), the heap work is $O(|V|dlog_d|E|)+O(|E|log_d|E|). So the overall work is $O((|E|+d|V|)log_d|E|+|E|)$, simplified to $O((|E|+d|V|)log_d|E|)$

- **1d.**

Assume $|E| = |V|^{1+\epsilon}$ with $0 < \epsilon < 1$, and choose $d = \frac{|E|}{|V|} = |V|^\epsilon$.
Then we would have $d|V| = |E|$, and $log_d|E| = \frac{log|E|}{logd}$ = $\frac{(1+\epsilon)log|V|}{\epsilon log|V|}$ = $O(1)$

Plugging into the bound from (c) gives total heap work $O((|E|+|E|) * constant) = O(|E|)$, so with this choice of $d$, Dijkstra runs in $O(|E|)$ time.

- **2a.**

k = -1

| i \ j | 0   | 1    | 2   |
|-------|-----|------|-----|
| **0** | 0   | -2   | 2   |
| **1** | inf | 0    | 1   |
| **2** | inf | inf  | 0   |

k = 0

| i \ j | 0   | 1    | 2   |
|-------|-----|------|-----|
| **0** | 0   | -2   | 2   |
| **1** | inf | 0    | 1   |
| **2** | inf | inf  | 0   |

k = 1

| i \ j | 0   | 1    | 2   |
|-------|-----|------|-----|
| **0** | 0   | -2   | -1  |
| **1** | inf | 0    | 1   |
| **2** | inf | inf  | 0   |

k = 2

| i \ j | 0   | 1    | 2   |
|-------|-----|------|-----|
| **0** | 0   | -2   | -1  |
| **1** | inf | 0    | 1   |
| **2** | inf | inf  | 0   |


- **2b.**

Yes. When we go from $k = 1$ to $k = 2$, the only new thing we allow is using vertex 2 as an intermediate, so $\min\big(APSP(i,j,1),\ APSP(i,2,1) + APSP(2,j,1)\big).$


- **2c.**

The shortest path cost $APSP(i,j,k)$ is either the old best path that does not use vertex $k$, which is $APSP(i,j,k-1)$, or a path that goes from $i$ to $k$ and then from $k$ to $j$, both using only intermediates in $\{0,\dots,k-1\}$.  
So the optimal substructure property is $\min\big(APSP(i,j,k-1),\ APSP(i,k,k-1) + APSP(k,j,k-1)\big).$

- **2d.**

Each subproblem can be identified by a triple $(i,j,k)$, and each of $i$, $j$, and $k$ can take a total of $n$ different values, so there are $\Theta(n^3)$ distinct subproblems. With memoization, each is computed once with $O(1)$ work, so the total work of the dynamic programming algorithm is $\Theta(n^3)$.

- **2e.**

Johnson’s algorithm runs in $O(|V||E|\log |V|)$ time, while this dynamic programming algorithm runs in $O(|V|^3)$ time.  
For sparse graphs where $|E| \approx |V|$, Johnson is about $O(|V|^2 \log |V|)$ and is faster; for dense graphs where $|E| = \Theta(|V|^2)$, Johnson becomes $O(|V|^3 \log |V|)$ and the $O(|V|^3)$ DP algorithm is preferable.


- **3a.**

Yes because a MST is also a minimum–maximum-edge tree. Aalgorithms like Kruskal’s add edges in nondecreasing weight and never replace a lighter edge with a heavier one while keeping the graph connected. Therefore the heaviest edge in the MST is as small as possible among all spanning trees.


- **3b.**

First compute an MST T of the graph. For each edge e = (u, v) that is not in T, look at the unique path between u and v in T; adding e creates a cycle with that path. Let $f$ be the heaviest edge on that path, form the candidate tree T' = T − ${f}$ $\cup$ ${e}$, compute its total weight, and keep the lightest candidate whose weight is strictly larger than the MST.

- **3c.**

Building the MST takes $O(E log V)$ time. Preprocessing the tree for max-edge-on-path (using an LCA-style structure) takes $O(V log V)$, and processing all non-tree edges with one max-edge query each costs $O(E log V)$. Thus the overall work of this algorithm is $O(E log V)$.