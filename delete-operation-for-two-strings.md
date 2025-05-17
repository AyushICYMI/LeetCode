# Delete Operation for Two Strings

## Problem Statement

Given two strings `word1` and `word2`, return the minimum number of steps required to make them equal.

In one step, you can delete exactly one character in either string.

---

## Approach

To solve this problem, we use the concept of the Longest Common Subsequence (LCS).

### Key Insight

The minimum number of deletions required to make the strings equal is:

min_deletions = (length of word1 - LCS) + (length of word2 - LCS)  
               = (n1 + n2 - 2 × LCS)

### Why?

- The characters in the LCS are already shared between both strings.
- So, we only need to delete the characters that are not part of the LCS from both strings.

### Steps

1. Compute the LCS between `word1` and `word2` using dynamic programming.
2. Return `n1 + n2 - 2 * LCS` as the result.

---

## Time Complexity

Time: O(n1 × n2), where `n1` and `n2` are the lengths of the input strings.

---

## Space Complexity

Space: O(n1 × n2) due to the 2D DP table.

---

## Code

```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        int n1 = word1.size();
        int n2 = word2.size();
        
        vector<vector<int>> dp(n1 + 1, vector<int>(n2 + 1, 0));
        
        for (int i = 1; i <= n1; i++) {
            for (int j = 1; j <= n2; j++) {
                if (word1[i - 1] == word2[j - 1]) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } else {
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
        
        return n1 + n2 - 2 * dp[n1][n2];
    }
};
