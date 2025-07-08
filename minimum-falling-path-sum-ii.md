# Minimum Falling Path Sum II

## 🧩 Problem Statement

Given an `n x n` integer matrix `grid`, return the **minimum sum of a falling path with non-zero shifts**.

A falling path with non-zero shifts is a choice of exactly one element from each row of `grid` such that no two elements chosen in adjacent rows are in the same column.

---

## 💡 Intuition

We need to select one element from each row while ensuring that no two selected elements from adjacent rows come from the same column.  
This suggests using **Dynamic Programming**, where we track the minimum path sum to each cell, considering only valid transitions from the previous row.

---

## 🛠️ Approach

1. Create a 2D DP table `dp` of size `n x n`.
2. Initialize the first row of `dp` with the first row of `grid`.
3. For each cell `(i, j)` from the second row onwards:
   - Look at all columns `k` in the previous row where `k != j`.
   - Find the minimum `dp[i-1][k]` and add `grid[i][j]`.
   - Store the result in `dp[i][j]`.
4. The final answer is the minimum value in the last row of `dp`.

---

## ⏱️ Time and Space Complexity

- **Time Complexity:** `O(n³)`  
  For each cell, we check all `n` other columns in the previous row.

- **Space Complexity:** `O(n²)`  
  Due to the 2D DP table.

---

## 💻 Code

```cpp
class Solution {
public:
    int minFallingPathSum(vector<vector<int>>& grid) {
        int n = grid.size();
        vector<vector<int>> dp(n, vector<int>(n, 0));
        
        // Initialize first row
        for (int j = 0; j < n; j++) 
            dp[0][j] = grid[0][j];
        
        // Fill DP table
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < n; j++) {
                int mini = INT_MAX;
                for (int k = 0; k < n; k++) {
                    if (k != j)
                        mini = min(mini, dp[i - 1][k]);
                }
                dp[i][j] = grid[i][j] + mini;
            }
        }
        
        // Get the minimum from the last row
        int ans = INT_MAX;
        for (int j = 0; j < n; j++) 
            ans = min(ans, dp[n - 1][j]);
        
        return ans;
    }
};
