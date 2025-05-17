# Target Sum

## Problem Statement
You are given an integer array `nums` and an integer `target`. You need to build an expression out of `nums` by adding either a '+' or '-' before each integer in `nums` and then concatenating all the integers. 

For example, if `nums = [2, 1]`, you can add a '+' before 2 and a '-' before 1 to form the expression "+2-1" which evaluates to 1. The task is to return the number of different expressions that can be built which evaluate to the given `target`.

## Approach
The problem can be transformed into a subset sum problem with dynamic programming. Here's the intuition:

1. Let the sum of elements with '+' be S1 and with '-' be S2.
2. We have: S1 - S2 = target
3. Also, S1 + S2 = total sum of the array (let's call it `sum`)
4. Adding these two equations: 2*S1 = target + sum => S1 = (target + sum)/2
5. Now the problem reduces to finding the number of subsets with sum equal to (target + sum)/2

### Steps:
1. Calculate the total sum of the array.
2. Check for edge cases:
   - If the absolute value of target is greater than sum, return 0 (not possible)
   - If (target + sum) is odd, return 0 (since we can't have a fractional sum)
3. The new target becomes (target + sum)/2
4. Use dynamic programming to count subsets that sum to this new target:
   - `dp[i][j]` represents the number of ways to get sum `j` using the first `i` elements
   - Initialize dp[0][0] = 1 (one way to get sum 0 with 0 elements)
   - For each element, either include it (if it doesn't exceed current sum) or exclude it

## Complexity
- **Time Complexity**: O(n * newTarget), where n is the number of elements and newTarget is (target + sum)/2. We fill a DP table of size n * newTarget.
- **Space Complexity**: O(n * newTarget) for the DP table. This can be optimized to O(newTarget) using a 1D array.

## Solution Code
```cpp
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int n = nums.size(), sum = 0;
        for (int i = 0; i < n; i++) sum += nums[i];
        int newTarget = target + sum;
        if (abs(target) > sum || newTarget & 1) return 0;
        else newTarget /= 2;
        vector<vector<int>> dp(n + 1, vector<int>(newTarget + 1, 0));
        dp[0][0] = 1;
        for (int i = 1; i <= n; i++) {
            for (int j = 0; j <= newTarget; j++) {
                if (nums[i - 1] <= j) {
                    dp[i][j] = dp[i - 1][j - nums[i - 1]] + dp[i - 1][j];
                }
                else dp[i][j] = dp[i - 1][j];
            }
        }
        return dp[n][newTarget];
    }
};
