# Triangle Minimum Path Sum

## Problem Statement
Given a triangular array of integers, return the minimum path sum from top to bottom. 

For each step, you may move to an adjacent number of the row below. More formally, if you are on index `i` on the current row, you may move to either index `i` or index `i + 1` on the next row.

## Approach
1. **Dynamic Programming (DP) Setup**:
   - We use a DP table `dp` of the same size as the input triangle to store the minimum path sum up to each cell.
   - Initialize the first row of `dp` with the value from the top of the triangle since the path starts here.
   - Precompute the leftmost column of the DP table since these cells can only be reached from the cell directly above them.

2. **DP Table Population**:
   - For each subsequent row, compute the minimum path sum to reach each cell by considering the minimum value from the two possible parent cells (top-left or top) in the previous row and adding the current cell's value.
   - Skip cells where both parent cells are unreachable (marked as `INT_MAX`).

3. **Result Extraction**:
   - The answer is the minimum value in the last row of the DP table, representing the smallest path sum from the top to the bottom of the triangle.

## Time Complexity (TC)
- **O(n²)**: We iterate through each cell of the triangular array exactly once to fill the DP table, where `n` is the number of rows.

## Space Complexity (SC)
- **O(n²)**: We use an additional `n x n` DP table to store intermediate results. This can be optimized to O(n) by using a 1D DP array or O(1) by modifying the input triangle in place, but the provided solution uses O(n²) space for clarity.

## Solution Code
```cpp
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int nr = triangle.size();
        vector<vector<int>> dp(nr, vector<int>(nr, INT_MAX));
        dp[0][0] = triangle[0][0];
        for (int i = 1; i < nr; i++) dp[i][0] = dp[i - 1][0] + triangle[i][0];

        for (int i = 1; i < nr; i++) {
            for (int j = 1; j < nr; j++) {
                if (dp[i - 1][j - 1] == INT_MAX && dp[i - 1][j] == INT_MAX) continue;
                dp[i][j] = min(dp[i - 1][j - 1], dp[i - 1][j]) + triangle[i][j];
            }
        }

        int ans = INT_MAX;
        for (int i = 0; i < nr; i++) ans = min(ans, dp[nr - 1][i]);
        return ans;
    }
};
```
