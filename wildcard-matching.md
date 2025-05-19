# Wildcard Pattern Matching

## Problem Statement

Given an input string `s` and a pattern `p`, implement **wildcard pattern matching** with support for:

- `'?'` → Matches **any single character**.
- `'*'` → Matches **any sequence of characters** (including the empty sequence).

The matching should cover the **entire input string**.

---

## Approach

We use **Dynamic Programming** to solve this problem efficiently.

### DP Definition

Let `dp[i][j]` be `true` if the first `i` characters of `s` match the first `j` characters of `p`.

### Initialization

- `dp[0][0] = true` → Empty pattern matches empty string.
- `dp[0][j] = true` if all characters in `p[0..j-1]` are `'*'`.
- All other values default to `false`.

### Transition

- If `s[i - 1] == p[j - 1]` or `p[j - 1] == '?'`:
  - `dp[i][j] = dp[i - 1][j - 1]`
- If `p[j - 1] == '*'`:
  - `dp[i][j] = dp[i][j - 1]` (treat `*` as empty)
  - `dp[i][j] |= dp[i - 1][j]` (treat `*` as matching one character)
- Else:
  - `dp[i][j] = false`

---

## Time Complexity

- **O(n × m)** where `n = s.length()` and `m = p.length()`

## Space Complexity

- **O(n × m)** due to the DP table.

---

## Code

```cpp
class Solution {
public:
    bool helper(string& p, int index) {
        for (int i = 1; i <= index; i++) {
            if (p[i - 1] != '*') return false;
        }
        return true;
    }

    bool isMatch(string s, string p) {
        int ns = s.size(), np = p.size();
        vector<vector<bool>> dp(ns + 1, vector<bool>(np + 1, false));

        dp[0][0] = true;

        // Fill first row: pattern matches empty string
        for (int j = 1; j <= np; j++) {
            dp[0][j] = helper(p, j);
        }

        for (int i = 1; i <= ns; i++) {
            for (int j = 1; j <= np; j++) {
                if (p[j - 1] == s[i - 1] || p[j - 1] == '?') {
                    dp[i][j] = dp[i - 1][j - 1];
                }
                else if (p[j - 1] == '*') {
                    dp[i][j] = dp[i][j - 1] || dp[i - 1][j];
                }
                else {
                    dp[i][j] = false;
                }
            }
        }

        return dp[ns][np];
    }
};
