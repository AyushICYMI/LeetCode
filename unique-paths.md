# Unique Paths

## Problem Statement

There is a robot on an `m x n` grid. The robot is initially located at the **top-left corner** of the grid (i.e., `grid[0][0]`).

The robot can **only move either down or right** at any point in time. The goal is to reach the **bottom-right corner** of the grid (i.e., `grid[m - 1][n - 1]`).

Given the two integers `m` and `n`, return the **number of possible unique paths** that the robot can take to reach the destination.

---

## Approach

This is a classic **dynamic programming** problem.

- Let `dp[i][j]` represent the number of unique paths to reach cell `(i, j)` from the start.
- The robot can only come from the **top** `(i-1, j)` or from the **left** `(i, j-1)`.

### Recurrence Relation

`dp[i][j] = dp[i-1][j] + dp[i][j-1]`

### Base Case

- The first row and the first column can only be reached **in one way**, so:
  - `dp[i][0] = 1` for all `i`
  - `dp[0][j] = 1` for all `j`

The result is stored at `dp[m-1][n-1]`.

---

## Time Complexity

- **O(m * n)** — Each cell of the grid is computed once.

## Space Complexity

- **O(m * n)** — A 2D DP table is used to store intermediate results.

> Note: This can be optimized to **O(n)** using a 1D array since only the current and previous rows are needed.

---

## C++ Code

```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m, vector<int>(n, 0));
        for (int i = 0; i < m; i++) dp[i][0] = 1;
        for (int i = 0; i < n; i++) dp[0][i] = 1;
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m - 1][n - 1];
    }
};
