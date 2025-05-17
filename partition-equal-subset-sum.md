# Partition Equal Subset Sum

## Problem Statement

Given an integer array `nums`, determine if it is possible to partition the array into **two subsets** such that the sum of elements in both subsets is **equal**.

Return `true` if such a partition exists, otherwise return `false`.

---

## Approach

This problem reduces to a classic **subset sum problem**:

- Calculate the **total sum** of all elements in `nums`.
- If the sum is **odd**, it's impossible to split into two equal subsets → return `false`.
- Otherwise, try to find a subset with sum equal to `total_sum / 2`.

We use **dynamic programming** to solve this:

- Define a 2D boolean DP table `dp[n+1][sum/2+1]` where:
  - `dp[i][j]` indicates whether it's possible to form a subset of the first `i` elements that sums to `j`.
- Initialization:
  - `dp[i][0] = true` for all `i` (subset sum of 0 is always possible — empty subset).
- Transition:
  - For each element `nums[i-1]`, if it is less than or equal to the current sum `j`, then:
    - `dp[i][j] = dp[i-1][j - nums[i-1]] || dp[i-1][j]`
  - Else:
    - `dp[i][j] = dp[i-1][j]`

---

## Time Complexity

- **O(n * sum)** where `n` is the number of elements and `sum` is half of the total sum.

## Space Complexity

- **O(n * sum)** for the DP table.

> Note: Space can be optimized to **O(sum)** using a 1D DP array.

---

## C++ Code

```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int n = nums.size(), sum = 0;
        for (int num : nums) sum += num;

        if (sum & 1) return false; // If sum is odd, partition not possible

        sum /= 2;
        vector<vector<bool>> dp(n + 1, vector<bool>(sum + 1, false));

        for (int i = 0; i <= n; i++) dp[i][0] = true;

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= sum; j++) {
                if (nums[i - 1] <= j) {
                    dp[i][j] = dp[i - 1][j - nums[i - 1]] || dp[i - 1][j];
                } else {
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }

        return dp[n][sum];
    }
};
