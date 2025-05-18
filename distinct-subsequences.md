# Distinct Subsequences

## Problem Statement

Given two strings `s` and `t`, return the **number of distinct subsequences** of `s` which equals `t`.

A subsequence of a string is a new string generated from the original string by deleting some (possibly none) characters **without changing the relative order** of the remaining characters.

The test cases are guaranteed to fit within a **32-bit signed integer**.

---

## Approach

We use **dynamic programming** to count the number of distinct ways to form the string `t` from `s` by deleting some characters.

### Intuition

Let `dp[i][j]` represent the number of ways to form the first `j` characters of `t` from the first `i` characters of `s`.

- **Base case**:  
  Any prefix of `s` can form the empty string `t` (when `j == 0`) in exactly one way → `dp[i][0] = 1`.

- **Transition**:  
  For `s[i-1]` and `t[j-1]`:
  - If they match:  
    `dp[i][j] = dp[i-1][j-1] + dp[i-1][j]`
    - `dp[i-1][j-1]`: using `s[i-1]` to match `t[j-1]`
    - `dp[i-1][j]`: skipping `s[i-1]`
  - If they don't match:  
    `dp[i][j] = dp[i-1][j]` (we skip `s[i-1]`)

---

## Time Complexity

- **Time**: `O(n * m)` where `n = s.length()` and `m = t.length()`

---

## Space Complexity

- **Space**: `O(n * m)` due to the 2D DP table

---

## Code

```cpp
class Solution {
public:
    int numDistinct(string s, string t) {
        int ns = s.size(), nt = t.size();
        vector<vector<unsigned long long>> dp(ns + 1, vector<unsigned long long>(nt + 1, 0));

        // Base case: empty t can always be matched by any prefix of s
        for (int i = 0; i <= ns; i++) dp[i][0] = 1;

        for (int i = 1; i <= ns; i++) {
            for (int j = 1; j <= nt; j++) {
                if (s[i - 1] == t[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
                } else {
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }

        return dp[ns][nt];
    }
};
