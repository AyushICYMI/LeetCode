# Largest Divisible Subset

## Problem Statement

You are given an array `nums` of **distinct positive integers**. Your task is to return the **largest subset** such that for every pair `(a, b)` in the subset:

- Either `a % b == 0`, or  
- `b % a == 0`

Return any one such subset with the maximum number of elements.

---

## Intuition

This problem is a variation of the **Longest Increasing Subsequence**, but instead of increasing order, we want each number to be divisible by the previous one.

The main idea is:
- Sort the array to ensure we only look forward.
- For each number, find the largest divisible subset it can extend from previously considered numbers.

---

## Approach

1. **Sort the array** in ascending order to ensure if `a % b == 0`, then `a >= b`.
2. Initialize a dynamic programming structure `dp`:
   - `dp[i]` holds the largest divisible subset ending at `nums[i]`.
3. For each number `nums[i]`, iterate over all previous `nums[j]`:
   - If `nums[i] % nums[j] == 0`, consider extending `dp[j]`.
   - Keep track of the subset with the maximum size that `nums[i]` can extend.
4. After processing all elements, return the longest subset found in `dp`.

---

## Time Complexity

- **O(n²)** — For each number, you may compare with all previous numbers.
- Sorting takes **O(n log n)**, but is dominated by the DP loop.

## Space Complexity

- **O(n²)** — In the worst case, each number could maintain a separate subset of size up to `n`.

---

## Code

```cpp
class Solution {
public:
    vector<int> largestDivisibleSubset(vector<int>& nums) {
        int n = nums.size();
        if (n == 0) return {};

        sort(nums.begin(), nums.end());

        // 'dp' will store the largest divisible subset ending at a given number.
        vector<vector<int>> dp;

        for (int num : nums) {
            // Find the longest subset in 'dp' that we can extend with 'num'.
            vector<int> max_subset;
            for (const auto& subset : dp) {
                if (num % subset.back() == 0) {
                    if (subset.size() > max_subset.size()) {
                        max_subset = subset;
                    }
                }
            }
            // Extend the best subset found with the current number.
            max_subset.push_back(num);
            // Add this newly formed subset to our list of candidates.
            dp.push_back(max_subset);
        }

        // Find the largest subset among all the candidates we've generated.
        vector<int> res;
        for (const auto& subset : dp) {
            if (subset.size() > res.size()) {
                res = subset;
            }
        }
        return res;
    }
};
