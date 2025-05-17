# House Robber II (Circular Street)

## Problem Statement

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses are arranged **in a circle**, which means the **first house is the neighbor of the last**.

Adjacent houses are connected with a security system, and robbing two **adjacent** houses will trigger an alarm.

Given an integer array `nums` representing the amount of money at each house, return the **maximum amount of money** you can rob tonight **without alerting the police**.

### Constraints

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 1000`

---

## Approach

This is a variation of the classic **House Robber (Linear)** problem with an added twist: the houses form a **circle**. This means the first and last houses are also adjacent.

To solve this:

1. **Observation**:
   - You cannot rob both the **first** and the **last** house in the same night due to the circular layout.
   
2. **Divide into two cases**:
   - Case 1: Rob houses from index `0` to `n-2` (exclude the last house).
   - Case 2: Rob houses from index `1` to `n-1` (exclude the first house).

3. **Use standard dynamic programming (DP)** to solve both cases and return the **maximum** of the two results.

4. The helper function uses bottom-up DP:
   - `dp[i] = max(dp[i-1], dp[i-2] + nums[i])`

---

## Time Complexity

- **O(n)**: Each DP run processes at most `n` elements twice (once for each case).

## Space Complexity

- **O(n)**: For the DP array used in the helper function.

> Note: Space can be optimized to **O(1)** using two variables instead of a DP array.

---

## C++ Code

```cpp
class Solution {
public:
    int helper(vector<int>& nums, int s, int e) {
        int n = e - s + 1;
        if (n == 1) return nums[s];
        vector<int> dp(n, 0);
        dp[0] = nums[s];
        dp[1] = max(nums[s], nums[s + 1]);
        for (int i = s + 2; i <= e; i++) {
            int idx = i - s;
            dp[idx] = max(dp[idx - 1], dp[idx - 2] + nums[i]);
        }
        return dp[n - 1];
    }
    
    int rob(vector<int>& nums) {
        int n = nums.size();
        if (n == 1) return nums[0];
        if (n == 2) return max(nums[0], nums[1]);
        int res1 = helper(nums, 0, n - 2);
        int res2 = helper(nums, 1, n - 1);
        return max(res1, res2);
    }
};
