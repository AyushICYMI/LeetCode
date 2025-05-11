## Problem Statement

You are given an m x n matrix `grid` of positive integers. Your task is to determine if it is possible to make either one horizontal or one vertical cut on the grid such that:

- Each of the two resulting sections formed by the cut is non-empty.  
- The sum of the elements in both sections is equal.

Return `true` if such a partition exists; otherwise return `false`.

---

## Approach

### Calculate Cumulative Sums
- Compute running totals for rows (`rowWise`) and columns (`colWise`), where each entry represents the sum up to that row or column.

### Check for Valid Partition
- First, verify if the total sum (last element of the cumulative sum) is even. If it's odd, return `false`.
- Then, check if any earlier cumulative sum equals half of the total sum. This indicates a valid point to split the matrix.

### Final Check
- Return `true` if either the row-wise or column-wise cumulative sums satisfy the partition condition; otherwise, return `false`.

### Time Complexity (TC)
- Let **n** = number of rows, and **m** = number of columns.
- Calculating row-wise cumulative sums: **O(n × m)**
- Calculating column-wise cumulative sums: **O(n × m)**
- Scanning cumulative arrays for half sum: **O(n + m)**
- **Total Time Complexity:** `O(n × m)`

### Space Complexity (SC)
- Storing cumulative row sums: **O(n)**
- Storing cumulative column sums: **O(m)**
- **Total Space Complexity:** `O(n + m)`

---

## Code

```cpp
using ll = long long;
class Solution {
public:
    bool helper(vector<ll>& nums, int n) {
        if (nums[n - 1] & 1) return false;
        for (int i = 0; i < n - 1; i++) 
            if (nums[i] == nums[n - 1] / 2) 
                return true;
        return false;
    }

    bool canPartitionGrid(vector<vector<int>>& grid) {
        int nr = grid.size(), nc = grid[0].size();
        ll sum = 0;
        vector<ll> rowWise(nr);
        for (int i = 0; i < nr; i++) {
            for (int j = 0; j < nc; j++) 
                sum += grid[i][j];
            rowWise[i] = sum;
        }

        sum = 0;
        vector<ll> colWise(nc);
        for (int i = 0; i < nc; i++) {
            for (int j = 0; j < nr; j++) 
                sum += grid[j][i];
            colWise[i] = sum;
        }

        return helper(rowWise, nr) || helper(colWise, nc);
    }
};
