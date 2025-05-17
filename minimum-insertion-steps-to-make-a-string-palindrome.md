# Minimum Insertions to Make a String Palindrome

## Problem Statement

Given a string `s`, return the **minimum number of insertions** required to make `s` a palindrome.

You can insert any character at any position in the string.

A **palindrome** is a string that reads the same backward as forward.

---

## Approach

To solve this problem efficiently, we can think in terms of the **Longest Palindromic Subsequence (LPS)**:

- The **minimum number of insertions** needed to make a string a palindrome is equal to the **difference between the string’s length and the length of its LPS**.
- Why? Because the characters that are already part of a palindromic subsequence do not need to be changed. The rest must be "balanced" by insertions.

### Steps:

1. Reverse the string `s` to get `rev_s`.
2. Use **dynamic programming** to compute the **Longest Common Subsequence (LCS)** between `s` and `rev_s`. This gives us the length of the **Longest Palindromic Subsequence**.
3. The minimum number of insertions = `n - LPS`, where `n` is the length of the string.

---

## Time Complexity

- The algorithm uses two nested loops over the length of the string `n`.
- **Time Complexity:** O(n²)

---

## Space Complexity

- A 2D DP table of size `(n+1) x (n+1)` is used.
- **Space Complexity:** O(n²)

---

## Code

```cpp
class Solution {
public:
    int minInsertions(string s) {
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

        return n - dp[n][n];
    }
};
