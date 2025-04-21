# Algorithms on Multi-Objective Assortment Optimization (MAO) Problem
Implementation of the algorithm proposed in the paper [Multi-Objective Assortment Optimization: Profit, Risk, Customer Utility, and Beyond](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4425135), and related numerical experiments


# Algorithm Descriptions and Applications
## Candidate Generation (CG)

## Improved Candidate Generation (ICG)

## Space Partition Breadth First Search (SPBFS)

## FW algorithm
The FW algorithm in our paper is simply an application of the Frank–Wolfe algorithm in the assortment optimization setting. 
Given any current assortment $\tilde{S}$, one can use Theorem 1 in our paper to linearize the objective function at the current solution by solving
```math
\max_{S\in\mathcal{F}}\,\sum_{i\in S}\mu_i(\tilde{S})q_i(S).
```
Iterating in this fashion, the algorithm may potentially lead to a good assortment. 
But we run this algorithm on synthetic instances in the Cost-AO problem to demonstrate that classic greedy algorithms fail to deliver reliable accuracy or solution quality.
