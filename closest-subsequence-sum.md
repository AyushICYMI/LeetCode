# Closest Subsequence Sum

## Problem Statement

You are given an integer array `nums` and an integer `goal`.

You want to choose a **subsequence** of `nums` such that the **sum** of its elements is the **closest possible** to `goal`. That is, if the sum of the subsequence's elements is `sum`, then you want to minimize the absolute difference `abs(sum - goal)`.

Return the **minimum possible value** of `abs(sum - goal)`.

> **Note:** A subsequence of an array is an array formed by removing some elements (possibly all or none) of the original array **without changing the order** of the remaining elements.

## Constraints

- `1 <= nums.length <= 40`
- `-10^7 <= nums[i] <= 10^7`
- `-10^9 <= goal <= 10^9`

---

## Approach

This problem is solved using a **Meet-in-the-middle** strategy:

1. **Split the array into two halves.**
2. Generate **all possible subset sums** from each half using recursion.
3. Sort one half (say `v2`) to allow efficient binary search.
4. For each subset sum `s1` from the first half `v1`, compute `target = goal - s1` and use binary search to find the closest value `s2` in `v2` such that `s1 + s2` is close to `goal`.
5. Track the minimum absolute difference during these comparisons.

This approach is efficient because each half can generate at most `2^20` subset sums, which is manageable.

---

## Time Complexity

- Generating subset sums: `O(2^(n/2))` for each half
- Sorting one half: `O(2^(n/2) * log(2^(n/2)))`
- For each element in the first half, binary search in the second half: `O(2^(n/2) * log(2^(n/2)))`

**Total:** `O(2^(n/2) * log(2^(n/2)))`  
Which is acceptable since `n <= 40`.

## Space Complexity

- Storage for subset sums: `O(2^(n/2))` for each half.

---

## C++ Code

```cpp
class Solution {
public:
    void helper(int s, int e, vector<int>& nums, vector<int>& v, int sum) {
        if (s > e) {
            v.push_back(sum);
            return;
        }
        helper(s + 1, e, nums, v, sum);
        helper(s + 1, e, nums, v, sum + nums[s]);
    }

    int bs(vector<int>& v, int target) {
        int s = 0, e = v.size() - 1;
        int closest = v[0];
        while (s <= e) {
            int m = s + (e - s) / 2;
            if (v[m] == target) {
                return v[m];
            } else if (v[m] < target) {
                closest = v[m];
                s = m + 1;
            } else {
                e = m - 1;
            }
        }
        if (s < v.size() && abs(v[s] - target) < abs(closest - target)) {
            closest = v[s];
        }
        return closest;
    }

    int minAbsDifference(vector<int>& nums, int goal) {
        int n = nums.size();
        if (n == 0) return abs(goal);
        if (n == 1) return min(abs(nums[0] - goal), abs(goal)); 

        int mid = n / 2;
        vector<int> v1, v2;
        helper(0, mid - 1, nums, v1, 0); 
        helper(mid, n - 1, nums, v2, 0);

        sort(v2.begin(), v2.end());

        int ans = INT_MAX;
        for (int s1 : v1) {
            int target = goal - s1;
            int s2 = bs(v2, target);
            ans = min(ans, abs(s1 + s2 - goal));
            if (ans == 0) return 0;
        }
        return ans;
    }
};
