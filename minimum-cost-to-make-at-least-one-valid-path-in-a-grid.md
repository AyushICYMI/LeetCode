# Minimum Cost to Make at Least One Valid Path in a Grid

## Problem Statement

You are given an `m x n` grid of integers `grid` where each cell has a direction:

- `1`: right
- `2`: left
- `3`: down
- `4`: up

You can move to an adjacent cell in the direction indicated by the current cell at no cost.  
If you move in a different direction, you must **pay a cost of 1** to change the direction.

Return the **minimum cost** to make at least one valid path from the **top-left** cell `(0,0)` to the **bottom-right** cell `(m-1,n-1)`.

---

## Intuition

This is a shortest path problem where edge weights are either `0` (if you move in the cell's suggested direction) or `1` (if you change direction).  
This fits well with **Dijkstra's algorithm**, where we prioritize visiting cheaper paths first.

### Key Ideas:
- Use a **priority queue** (min-heap) to always expand the lowest cost cell.
- If we follow the direction in the grid, we move with **0 cost**.
- If we take a different direction, we pay a **cost of 1**.
- Track the minimum cost to reach each cell and only proceed if we find a cheaper path.

---

## Code

```cpp
class Solution {
public:
    int minCost(vector<vector<int>>& grid) {
        int nr = grid.size(), nc = grid[0].size();
        vector<vector<int>> cost(nr, vector<int>(nc, INT_MAX));
        priority_queue<tuple<int, int, int>, vector<tuple<int, int, int>>, greater<>> pq;
        pq.push({0, 0, 0});
        cost[0][0] = 0;
        
        vector<int> drow = {0, 0, 1, -1};
        vector<int> dcol = {1, -1, 0, 0};
        
        while (!pq.empty()) {
            auto [currentCost, row, col] = pq.top();
            pq.pop();
            
            if (row == nr - 1 && col == nc - 1) {
                return currentCost;
            }
            
            for (int i = 0; i < 4; i++) {
                int nrow = row + drow[i];
                int ncol = col + dcol[i];
                if (nrow >= 0 && nrow < nr && ncol >= 0 && ncol < nc) {
                    int newCost = currentCost + (i + 1 == grid[row][col] ? 0 : 1);
                    if (newCost < cost[nrow][ncol]) {
                        cost[nrow][ncol] = newCost;
                        pq.push({newCost, nrow, ncol});
                    }
                }
            }
        }
        return cost[nr - 1][nc - 1];
    }
};
```

---

## Time Complexity

- **O(m × n × 4 × log(m × n))**:
  - Each cell can be visited multiple times (but typically only once with Dijkstra).
  - Priority queue has at most `m * n` entries.

## Space Complexity

- **O(m × n)** for the `cost` matrix and the priority queue.
