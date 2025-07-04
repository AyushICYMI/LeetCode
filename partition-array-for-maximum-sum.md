# Partition Array for Maximum Sum

## Problem Statement
Given an integer array `arr`, partition the array into (contiguous) subarrays of length at most `k`. After partitioning, each subarray's values are changed to become the maximum value of that subarray. Return the largest possible sum of the array after partitioning.

**Note**: Test cases are generated so that the answer fits in a 32-bit integer.

## Approach
We solve this problem using dynamic programming (DP) with memoization. The idea is to break the problem into smaller subproblems and store intermediate results to avoid redundant computations.

### Dynamic Programming Approach
1. **Recursive DP with Memoization**:
   - Define a helper function `f` that computes the maximum sum for the subarray starting at a given index
   - **Base Case**: If the index reaches the end of the array, return 0
   - **Memoization**: Use a DP array to store results of subproblems
   - **Partitioning Logic**:
     - For each possible partition starting at current index (up to `k` elements):
       - Calculate subarray length and maximum value
       - Compute current sum contribution
       - Add recursive result for remaining array
     - Track maximum sum from all possible partitions
   - **Result Storage**: Store computed maximum sum in DP array

## Solution Code
```cpp
class Solution {
public:
    int f(vector<int>& arr, int k, int index, vector<int>& dp) {
        int n = arr.size();
        if (index == n) return 0;
        if (dp[index] != -1) return dp[index];
        
        int len = 0;
        int maxi = 0;
        int ans = 0;
        
        for (int j = index; j < min(index + k, n); j++) {
            len++;
            maxi = max(maxi, arr[j]);
            int sum = len * maxi + f(arr, k, j + 1, dp);
            ans = max(ans, sum);
        }
        return dp[index] = ans;
    }

    int maxSumAfterPartitioning(vector<int>& arr, int k) {
        int n = arr.size();
        vector<int> dp(n, -1);
        return f(arr, k, 0, dp);
    }
};
