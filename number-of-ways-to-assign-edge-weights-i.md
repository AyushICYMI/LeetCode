# Assign Edge Weights for Odd Cost Path

## Problem Statement

You are given a tree with `n` nodes, rooted at node `1`. Each edge can be assigned a weight of **1** or **2**. You are to:

- Pick a node `x` at **maximum depth**.
- Consider only the path from the root (node `1`) to node `x`.
- Count the number of ways to assign weights **only along this path** such that the total cost (sum of edge weights) is **odd**.

Return the answer modulo `10^9 + 7`.

---

## Intuition

To find valid assignments of weights resulting in **odd total cost**, observe:

- For a path of length `L`, there are `2^L` total ways to assign weights (each edge gets either `1` or `2`).
- We want only those assignments where the **sum of weights is odd**.

Key observation:
- Half of all possible weight combinations of `L` numbers from `{1, 2}` yield an **odd sum**.
  - This is because:
    - If you list all 2^L combinations of `{1,2}`, half will have an odd sum, half even.

So:
- Compute `L = maxDepth - 1` (path from root to deepest node has `L` edges).
- Answer = `2^(L-1)` mod `10^9 + 7` (number of ways with odd sum).

---

## Approach

1. Build the adjacency list.
2. Use DFS to find `maxDepth`.
3. Compute `2^(maxDepth - 1)` modulo `10^9 + 7`.

---

## Code

```cpp
using namespace std;

class Solution {
public:
    int getMaxDepth(vector<vector<int>>& adj, int node, int depth) {
        int max_depth = depth;
        for (int child : adj[node]) {
            max_depth = max(max_depth, getMaxDepth(adj, child, depth + 1));
        }
        return max_depth;
    }

    int assignEdgeWeights(vector<vector<int>>& edges) {
        int n = edges.size() + 1;
        vector<vector<int>> adj(n + 1);

        for (auto& e : edges) {
            int u = e[0];
            int v = e[1];
            adj[u].push_back(v);
        }

        int max_depth = getMaxDepth(adj, 1, 0);

        if (max_depth == 0) return 0;

        const int MOD = 1e9 + 7;
        long long ans = 1;
        for (int i = 1; i <= max_depth - 1; ++i) {
            ans = (ans * 2) % MOD;
        }

        return ans;
    }
};
```

---

## Time Complexity

- **O(n)** — DFS to find maximum depth traverses each node once.
- **O(D)** — for computing `2^(D-1)` where `D` is the depth.

Overall: **O(n)**

## Space Complexity

- **O(n)** — for adjacency list and recursion stack.

---
