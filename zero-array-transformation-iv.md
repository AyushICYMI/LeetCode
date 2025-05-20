# Zero Array Transformation IV

## Problem Statement
Given an array `nums` and a list of queries `queries[i] = [l_i, r_i, val_i]`, determine the smallest `k` such that after applying the first `k` queries in order, all elements in `nums` become zero. If impossible, return -1.

Each query operation:
1. Selects indices in range `[l_i, r_i]`
2. Decrements each selected element by `val_i`

## Approach
### Key Insight
The solution involves:
1. Checking if each element can be reduced to zero using a subset of queries that cover it
2. Finding the earliest query sequence that collectively zeros all elements

### Solution Steps
1. **Check All Zero Case**: Immediately return 0 if already zero array
2. **Per-Element Analysis**:
   - For each `nums[i]`, use DP to find minimal queries that sum to `nums[i]` while covering index `i`
   - Track the maximum query index needed across all elements
3. **Validation**: Return -1 if any element can't be zeroed

## Complexity Analysis
| Complexity | Description |
|------------|-------------|
| **Time**   | O(n * q * t) | (n = nums length, q = queries count, t = max nums value) |
| **Space**  | O(q * t)     |

## Solution Code
```cpp
class Solution {
public:
    int helper(int target, int targetIndex, vector<vector<int>>& queries) {
        int n = queries.size();
        vector<vector<bool>> dp(n + 1, vector<bool>(target + 1, false));
        
        // Base case: zero sum is always possible
        for (int i = 0; i <= n; i++)
            dp[i][0] = true;
            
        // DP table construction
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= target; j++) {
                if (targetIndex >= queries[i-1][0] && 
                    targetIndex <= queries[i-1][1]) {
                    if (queries[i-1][2] <= j) {
                        dp[i][j] = dp[i-1][j-queries[i-1][2]] || dp[i-1][j];
                    } else {
                        dp[i][j] = dp[i-1][j];
                    }
                } else {
                    dp[i][j] = dp[i-1][j];
                }
            }
        }

        // Find minimal k if possible
        if (!dp[n][target]) return -1;
        
        for (int i = 1; i <= n; i++) {
            if (dp[i][target]) {
                return i;
            }
        }
        return -1;
    }

    int minZeroArray(vector<int>& nums, vector<vector<int>>& queries) {
        // Check if already zero array
        bool allZero = true;
        for (int num : nums) {
            if (num != 0) {
                allZero = false;
                break;
            }
        }
        if (allZero) return 0;

        vector<int> minQueries(nums.size(), 0);
        for (int i = 0; i < nums.size(); i++) {
            int res = helper(nums[i], i, queries);
            if (res == -1) return -1;
            minQueries[i] = res;
        }
        
        return *max_element(minQueries.begin(), minQueries.end());
    }
};
