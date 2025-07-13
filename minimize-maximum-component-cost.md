# Minimize Maximum Component Cost

## Problem Statement

You're given an undirected, connected graph with `n` nodes (0-indexed), and a list of weighted edges. You may remove edges to split the graph into at most `k` connected components.

The **cost of a component** is the maximum weight of any edge within that component (or 0 if it has no edges). Your task is to **minimize the maximum cost** among all the components after splitting.

Return the minimum possible value of the maximum cost across all resulting components.

---

## Intuition

This is a variation of the **Minimum Spanning Tree (MST)** problem, but instead of constructing a single MST, we need to **form exactly `k` connected components**.

To achieve that, we can build a full MST (which connects all `n` nodes with `n-1` edges), and then **remove the `k-1` most expensive edges** to get `k` components. However, instead of removing edges after building the MST, we can stop merging once we have exactly `k` components. The last edge used before reaching `k` components determines the maximum cost.

---

## Approach

1. **Sort the edges** in ascending order of weights.
2. Use a **Union-Find (Disjoint Set Union - DSU)** structure to keep track of components.
3. Iterate through the sorted edges:
   - For each edge, if the two nodes belong to different components, merge them.
   - Track the weight of the last edge used.
   - Stop when we reach exactly `k` components.
4. Return the weight of the last added edge (i.e., the max edge weight among all remaining components).

---

## Time and Space Complexity

- **Time Complexity:** `O(E log E + α(N))`
  - Sorting the edges takes `O(E log E)`
  - Each union/find operation takes nearly constant time (`α(N)` is the inverse Ackermann function).
  
- **Space Complexity:** `O(N)`
  - For storing parent and rank in Union-Find.

---

## Code

```cpp
class UnionFind {
public:
    vector<int> parent;
    vector<int> rank;
    int count;

    UnionFind(int n) {
        parent.resize(n);
        rank.resize(n, 0);
        count = n;
        for (int i = 0; i < n; ++i) {
            parent[i] = i;
        }
    }

    int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }

    bool unionSet(int x, int y) {
        int xRoot = find(x);
        int yRoot = find(y);
        if (xRoot == yRoot) {
            return false;
        }
        if (rank[xRoot] < rank[yRoot]) {
            parent[xRoot] = yRoot;
        } else if (rank[xRoot] > rank[yRoot]) {
            parent[yRoot] = xRoot;
        } else {
            parent[yRoot] = xRoot;
            rank[xRoot]++;
        }
        count--;
        return true;
    }
};

class Solution {
public:
    int minCost(int n, vector<vector<int>>& edges, int k) {
        if (k >= n) {
            return 0;
        }

        sort(edges.begin(), edges.end(),
             [](const vector<int>& a, const vector<int>& b) {
                 return a[2] < b[2];
             });

        UnionFind uf(n);
        int result_cost = 0;

        for (auto& edge : edges) {
            if (uf.count == k) {
                break;
            }

            int u = edge[0];
            int v = edge[1];
            int w = edge[2];

            if (uf.find(u) == uf.find(v)) {
                continue;
            }

            uf.unionSet(u, v);
            result_cost = w;
        }

        return result_cost;
    }
};
