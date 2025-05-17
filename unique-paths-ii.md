# Unique Paths II

## Problem Statement

You are given an `m x n` integer array `obstacleGrid`. A robot is initially located at the **top-left corner** (`grid[0][0]`).

The robot tries to move to the **bottom-right corner** (`grid[m - 1][n - 1]`). The robot can only move either **down** or **right** at any point in time.

Cells with a `1` represent **obstacles**, and cells with a `0` represent **free space**. The robot **cannot** travel through obstacles.

Return the **number of unique paths** from start to finish that avoid obstacles.

---

## Approach

This is a dynamic programming problem that modifies the classic **Unique Paths** algorithm to account for obstacles.

- Let `dp[i][j]` represent the number of unique paths to cell `(i, j)`.
- If a cell is an obstacle (`obstacleGrid[i][j] == 1`), then `dp[i][j] = 0`.
- Otherwise, the number of ways to reach `(i, j)` is the sum of:
  - ways to reach from **above**: `dp[i-1][j]`
  - ways to reach from **left**: `dp[i][j-1]`

### Initialization

- The first row and first column must be initialized carefully:
  - If an obstacle is encountered, all following cells in that row or column will also be unreachable (remain 0).

---

## Time Complexity

- **O(m * n)** — Each cell is processed once.

## Space Complexity

- **O(m * n)** — A 2D DP table is used to store path counts.

> Note: This can be optimized to **O(n)** with a single-row DP array.

---

## C++ Code

```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int nr = obstacleGrid.size(), nc = obstacleGrid[0].size();
        vector<vector<int>> dp(nr, vector<int>(nc, 0));

        for (int i = 0; i < nr; i++) {
            if (obstacleGrid[i][0] == 1) break;
            dp[i][0] = 1;
        }

        for (int i = 0; i < nc; i++) {
            if (obstacleGrid[0][i] == 1) break;
            dp[0][i] = 1;
        }

        for (int i = 1; i < nr; i++) {
            for (int j = 1; j < nc; j++) {
                if (obstacleGrid[i][j] == 1) continue;
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }

        return dp[nr - 1][nc - 1];
    }
};
