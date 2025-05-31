# Minimum Operations to Make Array Elements Equal to Query Value

## Problem Statement

You are given a `nums` array consisting of positive integers and an array `queries`. For each `queries[i]`, you want to transform every element in `nums` to be equal to `queries[i]`, where a single operation is defined as either incrementing or decrementing a number by 1.

Return an array `answer` where `answer[i]` is the **minimum number of operations** needed for each query.

---

## Intuition

To make every element of `nums` equal to some value `q`, the number of operations needed is the sum of the absolute differences:  
`|nums[0] - q| + |nums[1] - q| + ... + |nums[n-1] - q|`.

Instead of computing this from scratch for each query (which is inefficient), we can:
- Sort `nums` once.
- Precompute prefix sums.
- For each query `q`, binary search to find how many elements are ≤ `q` and ≥ `q`.
- Use prefix sums to calculate the total cost efficiently.

---

## Code

```cpp
using ll = long long;

class Solution {
public:
    int bs1(vector<int>& nums, int target) {
        int s = 0, e = nums.size() - 1, ans = -1;
        while (s <= e) {
            int m = s + (e - s) / 2;
            if (nums[m] <= target) {
                ans = m;
                s = m + 1;
            } else {
                e = m - 1;
            }
        }
        return ans;
    }

    int bs2(vector<int>& nums, int target) {
        int s = 0, e = nums.size() - 1, ans = -1;
        while (s <= e) {
            int m = s + (e - s) / 2;
            if (nums[m] >= target) {
                ans = m;
                e = m - 1;
            } else {
                s = m + 1;
            }
        }
        return ans;
    }

    vector<long long> minOperations(vector<int>& nums, vector<int>& queries) {
        int n = nums.size();
        sort(nums.begin(), nums.end());

        vector<ll> pref(n + 1, 0);
        for (int i = 1; i <= n; ++i) {
            pref[i] = pref[i - 1] + nums[i - 1];
        }

        vector<ll> res;
        for (int q : queries) {
            ll total = 0;
            int le = bs1(nums, q); // last index where nums[i] <= q
            int ge = bs2(nums, q); // first index where nums[i] >= q

            if (le != -1) {
                total += (ll)(le + 1) * q - pref[le + 1];
            }
            if (ge != -1) {
                total += (pref[n] - pref[ge]) - (ll)(n - ge) * q;
            }

            res.push_back(total);
        }

        return res;
    }
};
```

---

## Time Complexity

- **O(n log n)** – for sorting and binary searches
- **O(m log n)** – for `m` queries using binary search
- **O(n)** – for prefix sum computation

**Total:** `O((n + m) log n)`

---

## Space Complexity

- **O(n)** – for prefix sum storage
- **O(m)** – for result array

**Total:** `O(n + m)`

---

## Example

```
nums = [1, 3, 5]
queries = [2, 4]

Result: [4, 4]

Explanation:
- To make all elements 2: |1-2| + |3-2| + |5-2| = 1 + 1 + 3 = 5
- To make all elements 4: |1-4| + |3-4| + |5-4| = 3 + 1 + 1 = 5
```

