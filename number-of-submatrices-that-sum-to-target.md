# Number of Submatrices That Sum to Target

## Problem Statement

You are given a matrix `matrix` and an integer `target`.

Return the number of **non-empty submatrices** that sum to `target`.

A submatrix `(x1, y1, x2, y2)` is defined as all cells `matrix[x][y]` with `x1 ≤ x ≤ x2` and `y1 ≤ y ≤ y2`.

Two submatrices are different if they differ in at least one coordinate.

---

## Intuition

The problem is similar to the **1D subarray sum equals k**, but extended to **2D**.

The key is to fix two columns and collapse the 2D matrix into a 1D array representing the sum between those two columns for each row. Then, apply the prefix sum + hashmap technique to count the number of subarrays that sum to the target.

### Key Steps:

1. Compute **prefix sums** row-wise to quickly get sum of any submatrix between two columns.
2. For each pair of columns `c1` and `c2`, create a running row-sum array (collapsed into 1D).
3. Use a hashmap to track prefix sums and count how many times `current_sum - target` has occurred.

---

## Code

```cpp
class Solution {
public:
    int numSubmatrixSumTarget(vector<vector<int>>& m, int target) {
        int nr = m.size();
        int nc = m[0].size();

        // Compute row-wise prefix sum
        for (int i = 0; i < nr; i++) {
            for (int j = 1; j < nc; j++) {
                m[i][j] += m[i][j - 1];
            }
        }

        int ans = 0;
        // Fix two columns and reduce to 1D problem
        for (int c1 = 0; c1 < nc; c1++) {
            for (int c2 = c1; c2 < nc; c2++) {
                unordered_map<int, int> map;
                map[0] = 1;
                int sum = 0;
                for (int r = 0; r < nr; r++) {
                    // Get the row sum between c1 and c2
                    int rowsum = m[r][c2] - (c1 > 0 ? m[r][c1 - 1] : 0);
                    sum += rowsum;
                    ans += map[sum - target];
                    map[sum]++;
                }
            }
        }
        return ans;
    }
};
```

---

## Time Complexity

- **O(n² × m)** where `n = number of columns`, `m = number of rows`
  - There are `O(n²)` column pairs.
  - For each, we scan all `m` rows with prefix sum + hashmap logic.

## Space Complexity

- **O(m)** for the hashmap used in each row iteration.

---
