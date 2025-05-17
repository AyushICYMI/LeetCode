# House Robber (Linear)

## Problem Statement

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. 

The only constraint stopping you from robbing each of them is that **adjacent houses** have security systems connected and it will automatically contact the police if **two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return the **maximum amount** of money you can rob tonight **without alerting the police**.

### Constraints

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 400`

---

## Approach

This problem is a classic **dynamic programming** scenario. You cannot rob two adjacent houses, so at each house you decide:

- **Rob it** → add the amount in the current house to the **max loot two houses ago** (`dp[i-2] + nums[i]`)
- **Skip it** → take the **max loot up to the previous house** (`dp[i-1]`)

We build a `dp` array where:

- `dp[i]` represents the **maximum amount robbed from house 0 to house i**.
- Transition:  
  `dp[i] = max(dp[i-1], dp[i-2] + nums[i])`

### Base Cases

- `dp[0] = nums[0]`
- `dp[1] = max(nums[0], nums[1])`

---

## Time Complexity

- **O(n)**: We iterate through the array once.

## Space Complexity

- **O(n)**: We use a `dp` array of size `n`.

> Note: Space can be optimized to **O(1)** by storing only the last two computed values.

---

## C++ Code

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        if (n == 0) return 0;
        if (n == 1) return nums[0];

        vector<int> dp(n, 0);
        dp[0] = nums[0];
        dp[1] = max(nums[0], nums[1]);
        for (int i = 2; i < n; i++) {
            dp[i] = max(dp[i - 1], dp[i - 2] + nums[i]);
        }
        return dp[n - 1];
    }
};
