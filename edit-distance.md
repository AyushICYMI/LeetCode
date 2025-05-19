# Edit Distance

## Problem Statement

Given two strings `word1` and `word2`, return the **minimum number of operations** required to convert `word1` into `word2`.

You are allowed to perform the following three operations:

1. **Insert** a character
2. **Delete** a character
3. **Replace** a character

---

## Approach

This is a classic **Dynamic Programming** problem known as **Edit Distance** or **Levenshtein Distance**.

We define a 2D DP table where:

- `dp[i][j]` represents the minimum number of operations required to convert the **first `i` characters** of `word1` into the **first `j` characters** of `word2`.

### Base Cases

- If `word1` is empty (`i = 0`), we need `j` insertions → `dp[0][j] = j`.
- If `word2` is empty (`j = 0`), we need `i` deletions → `dp[i][0] = i`.

### Transition

- If `word1[i - 1] == word2[j - 1]`, no operation is needed:  
  `dp[i][j] = dp[i - 1][j - 1]`
  
- Else, choose the **minimum** of:
  - Insert: `dp[i][j - 1] + 1`
  - Delete: `dp[i - 1][j] + 1`
  - Replace: `dp[i - 1][j - 1] + 1`

---

## Time Complexity

- **O(n1 × n2)** where `n1` and `n2` are the lengths of `word1` and `word2`.

## Space Complexity

- **O(n1 × n2)** due to the 2D DP table.

---

## Code

```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        int n1 = word1.size(), n2 = word2.size();
        vector<vector<int>> dp(n1 + 1, vector<int>(n2 + 1, 0));

        // Initialize base cases
        for (int i = 0; i <= n1; i++) dp[i][0] = i;
        for (int i = 0; i <= n2; i++) dp[0][i] = i;

        // DP computation
        for (int i = 1; i <= n1; i++) {
            for (int j = 1; j <= n2; j++) {
                if (word1[i - 1] == word2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1];  // Characters match
                } else {
                    dp[i][j] = 1 + min({
                        dp[i][j - 1],    // Insert
                        dp[i - 1][j],    // Delete
                        dp[i - 1][j - 1] // Replace
                    });
                }
            }
        }

        return dp[n1][n2];
    }
};
