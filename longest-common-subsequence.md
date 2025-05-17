# Longest Common Subsequence

## Problem Statement

Given two strings `text1` and `text2`, return the length of their longest common subsequence.

A **subsequence** of a string is a new string generated from the original string by deleting some characters (can be none) without changing the relative order of the remaining characters.

For example, `"ace"` is a subsequence of `"abcde"`.

A **common subsequence** of two strings is a subsequence that appears in both strings.

If there is no common subsequence, return `0`.

---

## Approach

We solve this problem using **dynamic programming** by building a table that stores the results of subproblems:

1. Create a 2D table `dp` where `dp[i][j]` represents the length of the longest common subsequence between the first `i` characters of `text1` and the first `j` characters of `text2`.
2. Initialize the table with zeros. The first row and column are zeros because the LCS with an empty string is 0.
3. Iterate through each character of both strings:
   - If the characters match (`text1[i-1] == text2[j-1]`), it means we can add 1 to the length of the LCS found for the previous substrings (`dp[i-1][j-1]`), so set `dp[i][j] = dp[i-1][j-1] + 1`.
   - If the characters don’t match, choose the maximum LCS length by either skipping the current character from `text1` or `text2`, so `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`.
4. The value at `dp[n1][n2]` (bottom-right cell) gives the length of the longest common subsequence for the two strings.

---

## Time Complexity

- The algorithm iterates through all pairs of characters from the two strings.
- **Time Complexity:** O(n1 * n2), where `n1` and `n2` are the lengths of `text1` and `text2`.

---

## Space Complexity

- We use a 2D table of size `(n1 + 1) x (n2 + 1)`.
- **Space Complexity:** O(n1 * n2).

---

## Code

```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int n1 = text1.size();
        int n2 = text2.size();

        vector<vector<int>> dp(n1 + 1, vector<int>(n2 + 1, 0));

        for (int i = 1; i <= n1; i++) {
            for (int j = 1; j <= n2; j++) {
                if (text1[i - 1] == text2[j - 1]) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } else {
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }

        return dp[n1][n2];
    }
};
