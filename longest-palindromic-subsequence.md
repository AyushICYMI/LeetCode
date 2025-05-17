# Longest Palindromic Subsequence

## Problem Statement

Given a string `s`, return *the length of the longest palindromic subsequence* in `s`.

A **subsequence** is a sequence that can be derived from another sequence by deleting some or no characters without changing the order of the remaining characters.

A **palindrome** is a sequence that reads the same backward as forward.

---

## Approach

To find the **longest palindromic subsequence**, observe the following:

- A palindromic subsequence in `s` is also a common subsequence between `s` and its reverse.
- So, we reverse the string `s` to get `rev_s`, and then compute the **Longest Common Subsequence (LCS)** between `s` and `rev_s`.
- The LCS will be the longest sequence that appears in both `s` and `rev_s` and maintains the order — which is exactly what we want for a palindromic subsequence.

### Steps:
1. Reverse the original string `s` to get `rev_s`.
2. Create a 2D DP table `dp` of size `(n+1) x (n+1)`, where `n` is the length of the string.
3. Use dynamic programming to fill the table:
   - If characters match (`s[i-1] == rev_s[j-1]`), then `dp[i][j] = 1 + dp[i-1][j-1]`.
   - If they don’t match, take the max from the previous row or column: `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`.
4. The bottom-right cell `dp[n][n]` gives the length of the longest palindromic subsequence.

---

## Time Complexity

- We use two nested loops of size `n`, where `n` is the length of the input string.
- **Time Complexity:** O(n²)

---

## Space Complexity

- We use a 2D DP table of size `(n+1) x (n+1)`.
- **Space Complexity:** O(n²)

---

## Code

```cpp
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        int n = s.size();
        string rev_s = s;
        reverse(rev_s.begin(), rev_s.end());

        vector<vector<int>> dp(n + 1, vector<int>(n + 1, 0));

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                if (s[i - 1] == rev_s[j - 1]) {
                    dp[i][j] = 1 + dp[i - 1][j - 1];
                } else {
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }

        return dp[n][n];
    }
};
