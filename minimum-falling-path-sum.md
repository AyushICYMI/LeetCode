# Minimum Falling Path Sum

## Problem Statement
Given an `n x n` array of integers `matrix`, return the minimum sum of any falling path through `matrix`.

A falling path starts at any element in the first row and chooses the element in the next row that is either directly below or diagonally left/right. Specifically, the next element from position `(row, col)` will be `(row + 1, col - 1)`, `(row + 1, col)`, or `(row + 1, col + 1)`.

## Approach
1. **Dynamic Programming (DP) Setup**: 
   - We use a DP table `dp` of the same size as the input matrix to store the minimum falling path sum up to each cell.
   - Initialize the first row of `dp` with the values from the first row of the matrix since the falling path starts here.

2. **DP Table Population**:
   - For each subsequent row, compute the minimum path sum to reach each cell by considering the minimum value from the three possible parent cells (top-left, top, top-right) in the previous row and adding the current cell's value.
   - Handle edge cases where cells are in the first or last column to avoid out-of-bounds access.

3. **Result Extraction**:
   - The answer is the minimum value in the last row of the DP table, representing the smallest falling path sum from the top to the bottom of the matrix.

## Time Complexity (TC)
- **O(n²)**: We iterate through each cell of the `n x n` matrix exactly once to fill the DP table.

## Space Complexity (SC)
- **O(n²)**: We use an additional `n x n` DP table to store intermediate results. This can be optimized to O(n) by using a rolling array or O(1) by modifying the input matrix in place, but the provided solution uses O(n²) space for clarity.

## Solution Code
```cpp
class Solution {
public:
    int minFallingPathSum(vector<vector<int>>& matrix) {
        int n = matrix.size();
        vector<vector<int>> dp(n, vector<int>(n, 0));
        for (int i = 0; i < n; i++) dp[0][i] = matrix[0][i];
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < n; j++) {
                int valToAdd = INT_MAX;
                if (j - 1 >= 0) valToAdd = min(valToAdd, dp[i - 1][j - 1]);
                valToAdd = min(valToAdd, dp[i - 1][j]);
                if (j + 1 < n) valToAdd = min(valToAdd, dp[i - 1][j + 1]);
                dp[i][j] = valToAdd + matrix[i][j];
            }
        }
        int ans = INT_MAX;
        for (int i = 0; i < n; i++) ans = min(ans, dp[n - 1][i]);
        return ans;
    }
};
```
