# Algorithms on Multi-Objective Assortment Optimization (MAO) Problem
Implementation of the algorithm proposed in the paper [Multi-Objective Assortment Optimization: Profit, Risk, Customer Utility, and Beyond](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4425135), and related numerical experiments.
All algorithms in this project are implemented in Python.  
Particularly, the SPBFS algorithm relies on the `multiprocessing` library, since Windows and Linux handle multiprocessing differently, the code encounters errors when executed on Windows.


# A Brief Description of Algorithms
## Candidate Generation (CG)
Corollary 1 in our manuscript establishes that the optimal solution to the MAO problem is composed of products ordered by their “pseudo-revenue.”  
Specifically, there exists a threshold $\tau>0$ such that every product whose pseudo-revenue is at least $\tau$ is included in the optimal assortment.  

When the number of objectives is two, each pseudo-revenue is a linear function of a common parameter. Hence, by arranging these $n$ linear functions in descending order of their values and examining all resulting permutations, we obtain a set of candidate assortments that is guaranteed to contain the optimal solution.

The CG algorithm is developed to determine the permutations of linear functions in two-dimensional space.  
It partitions the $x$-axis into a set of intervals, within each of which the relative ranking of all linear functions is fixed.  
These orderings directly yield the corresponding candidate assortments.

## Improved Candidate Generation (ICG)
The Improved Candidate Generation (ICG) algorithm is an improved algorithm that likewise applies to the two-objective setting.  
Although less intuitive than CG, it also builds on geometric insights in two-dimensional space.  
Compared with CG, ICG shows a faster theoretical running time ($O(n^2)$ and $O(n^2\log n)$) and produces fewer candidate assortments ($O(n)$ and $O(n^2)$ ).  

The geometric intuition behind ICG can be summarized as follows:  
The objective function is the piecewise maximum of finitely many linear functions. Each segment of this piecewise-linear function corresponds to a candidate assortment including products that corresponds to the linear functions lying above the segment on that interval.  Starting at the parameter value $\gamma = 0$, we can compute the first candidate assortment and its associated linear expression.  We then repeatedly execute the following steps until the piecewise-linear function meets the $x$-axis: identify the first line that intersects the current segment and the product it represents; if the product is absent from the current assortment, add it—otherwise, remove it; finally, update the linear expression based on the new assortment.

## Space Partition Breadth First Search (SPBFS)
The SPBFS algorithm extends the problem addressed by the CG and ICG algorithms to higher dimensions.  Consider three-dimensional space as an example.  
Each hyperplane represents the collection of points where the pseudo-revenues of two products are equal.  The two open half-spaces defined by that hyperplane impose opposite, but fixed, pseudo-revenue orderings for this pair of product.  
Therefore, every distinct pair of products defines one hyperplane, and an instance with $n$ products induces $O(n^{2})$ hyperplanes that partition the space into polyhedra.  Inside each polyhedron, the pseudo-revenues of all products have a unique total order.  Traversing all polyhedra therefore enumerates every ordering and, by extension, every candidate assortment.  To cope with the enormous number of polyhedra, our SPBFS implementation exploits Python’s `multiprocessing` package, enabling parallel processing of multiple polyhedra and thereby markedly shortening the computation time.

## FW algorithm
The FW algorithm in our paper is simply an application of the Frank–Wolfe algorithm in the assortment optimization setting. 
Given any current assortment $\tilde{S}$, one can use Theorem 1 in our paper to linearize the objective function at the current solution by solving
```math
\max_{S\in\mathcal{F}}\,\sum_{i\in S}\mu_i(\tilde{S})q_i(S),
```
where $\mu_i(\cdot)$ is the "pseudo revenue" of $\tilde{S}$. Iterating in this fashion, the algorithm may potentially lead to a good assortment. 
But we run this algorithm on synthetic instances in the Cost-AO problem to demonstrate that classic greedy algorithms fail to deliver reliable accuracy or solution quality.
