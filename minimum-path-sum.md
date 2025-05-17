# Minimum Path Sum

## Problem Statement

You are given an `m x n` grid filled with **non-negative numbers**.

A robot is initially located at the **top-left corner** (`grid[0][0]`) and needs to reach the **bottom-right corner** (`grid[m - 1][n - 1]`).

The robot can **only move either down or right** at any point in time.

Find the **minimum sum** of all numbers along a path from the top-left to the bottom-right.

---

## Approach

This is a classic **dynamic programming** problem.

- Let `dp[i][j]` represent the **minimum path sum** to reach cell `(i, j)` from the start.
- To reach `(i, j)`, the robot can come either:
  - From **above**: `dp[i-1][j]`
  - From **left**: `dp[i][j-1]`
- The minimum sum to reach `(i, j)` is the **minimum of those two options**, plus the cost at `grid[i][j]`.

### Recurrence Relation

`dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j]`

### Initialization

- `dp[0][0] = grid[0][0]`
- Fill the first row and first column by accumulating the path sums along the border.

---

## Time Complexity

- **O(m * n)** — Each cell is computed once.

## Space Complexity

- **O(m * n)** — A 2D DP table is used.

> Note: This can be optimized to **O(n)** using a single-row DP array.

---

## C++ Code

```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int nr = grid.size(), nc = grid[0].size();
        vector<vector<int>> dp(nr, vector<int>(nc, 0));
        dp[0][0] = grid[0][0];

        for (int i = 1; i < nr; i++) dp[i][0] = grid[i][0] + dp[i - 1][0];
        for (int i = 1; i < nc; i++) dp[0][i] = grid[0][i] + dp[0][i - 1];

        for (int i = 1; i < nr; i++) {
            for (int j = 1; j < nc; j++) {
                dp[i][j] = min(dp[i - 1][j], dp[i][j - 1]) + grid[i][j];
            }
        }

        return dp[nr - 1][nc - 1];
    }
};
