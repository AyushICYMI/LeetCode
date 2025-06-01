# Check Equal Product Partitions

## Problem Statement

You are given an array `nums` consisting of distinct positive integers and an integer `target`.

Determine whether you can partition `nums` into two **non-empty**, **disjoint** subsets such that the **product** of elements in **both** subsets is equal to `target`.

Each element must belong to exactly one subset.

---

## Intuition

We are checking if the array can be split into two subsets such that both subsets have a product of exactly `target`.  

Important insights:
- The total product of all elements must be equal to `target * target`.
- If it's not, we can return `false` immediately.
- Since all numbers are distinct and positive, we can use **backtracking** to check if there exists a subset with product equal to `target`.
- The other subset (complement) will automatically also have product `target` (because the total product is `target²`).

The `helper` function recursively decides for each number:
- Either include it in the subset we're building for the first target.
- Or skip it.

We also track how many elements are used to ensure the subset is non-empty and not equal to the full array.

---

## Code

```cpp
class Solution {
public:
    bool helper(vector<int>& nums, long long target, int index, int count) {
        if (target == 1) 
            return count > 0 && count < nums.size();

        if (index < 0) 
            return false;

        if (target % nums[index] == 0)
            return helper(nums, target / nums[index], index - 1, count + 1) ||
                   helper(nums, target, index - 1, count);
        else
            return helper(nums, target, index - 1, count);
    }

    bool checkEqualPartitions(vector<int>& nums, long long target) {
        int n = nums.size();
        int max_elem = *max_element(nums.begin(), nums.end());

        if (max_elem > target)
            return false;

        unsigned long long product = 1;
        for (int i = 0; i < n; i++) 
            product *= nums[i];

        if (product != (unsigned long long)target * target) 
            return false;

        return helper(nums, target, n - 1, 0);
    }
};
```

---

## Time Complexity

- **O(2ⁿ)** in the worst case — due to exploring all subsets in the recursive `helper` function.

## Space Complexity

- **O(n)** — recursion stack depth.

