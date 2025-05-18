# Shortest Common Supersequence (SCS)

## Problem Statement

Given two strings `str1` and `str2`, return the **shortest string** that has both `str1` and `str2` as **subsequences**. If there are multiple valid strings, return any of them.

A string `s` is a subsequence of string `t` if deleting some number of characters from `t` (possibly 0) results in the string `s`.

---

## Approach

We use **Dynamic Programming** to find the **Longest Common Subsequence (LCS)** of `str1` and `str2`.  
Once we know the LCS, we can construct the **Shortest Common Supersequence (SCS)** by combining both strings while reusing the LCS characters only once.

### Why LCS helps?

- The LCS represents the common parts of both strings that **don't need to be repeated**.
- To construct the SCS, we:
  - Merge characters from both strings.
  - Insert the LCS characters **only once**.
  - Insert the **non-matching characters** from both strings in their respective order.

---

### Steps

1. **Compute LCS** using dynamic programming.
2. **Backtrack through the DP table** to build the SCS:
   - If characters match → take one and move diagonally.
   - If they don’t match → take the character from the direction with the larger LCS value.
3. **Append remaining characters** if one string is exhausted.
4. Reverse the result since it’s built from the end to the beginning.

---

## Time Complexity

- **Time**: `O(n × m)`  
  (for filling the DP table and constructing the result)

## Space Complexity

- **Space**: `O(n × m)`  
  (due to the 2D DP table)

---

## Code

```cpp
class Solution {
public:
    string shortestCommonSupersequence(string str1, string str2) {
        int n = str1.size();
        int m = str2.size();
        vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));
        
        // Step 1: Compute LCS
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if (str1[i - 1] == str2[j - 1]) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } else {
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        
        // Step 2: Reconstruct the SCS from the DP table
        int i = n, j = m;
        string scs;
        while (i > 0 && j > 0) {
            if (str1[i - 1] == str2[j - 1]) {
                scs += str1[i - 1];
                i--;
                j--;
            } else if (dp[i - 1][j] > dp[i][j - 1]) {
                scs += str1[i - 1];
                i--;
            } else {
                scs += str2[j - 1];
                j--;
            }
        }

        // Add remaining characters from both strings
        while (i > 0) {
            scs += str1[i - 1];
            i--;
        }
        while (j > 0) {
            scs += str2[j - 1];
            j--;
        }

        // Reverse the constructed result
        reverse(scs.begin(), scs.end());
        return scs;
    }
};
