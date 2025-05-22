# Longest Palindromic Substring

## Problem

Given a string `s`, return the longest palindromic substring in `s`.

---

## Approach

This approach uses the **Longest Common Substring (LCS)** idea by comparing the string with its reverse. However, we need to ensure that the matched substring corresponds to a **palindromic** substring in the original string.

---

## Key Idea

- A palindrome reads the same forward and backward.
- If we reverse the string and find a **common substring** between the original and the reversed string, that substring **might** be a palindrome.
- But not all matching substrings between `s` and `reverse(s)` are palindromes. So we **verify** if the position of the substring matches the expected mirrored index.

---

## Intuition

Let’s denote:
- `s` as the original string
- `rev_s` as the reversed string

We construct a 2D `dp` table where:
- `dp[i][j]` stores the length of the longest common substring ending at `s[i - 1]` and `rev_s[j - 1]`.

However, to ensure this common substring is actually a **palindromic substring in the original string**, we check the **index alignment**:

> For a substring of length `len` ending at index `i-1` in `s` and at index `j-1` in `rev_s`, the substring is palindromic **only if**
>  
> `start_in_s == (n - (start_in_rev + len))`

This condition ensures the substring in `s` and its match in `rev_s` are exact reverse positions.

---

## Time and Space Complexity

- **Time:** O(n²)
- **Space:** O(n²)

---

## Code

```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        string rev_s = s;
        reverse(rev_s.begin(), rev_s.end());
        int n = s.size();
        vector<vector<int>> dp(n + 1, vector<int>(n + 1, 0));
        int max_len = 0;
        int end_idx = -1;

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                if (s[i - 1] == rev_s[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;

                    int len = dp[i][j];
                    int start_in_s = i - len;
                    int start_in_rev = j - len;

                    if (start_in_s == (n - (start_in_rev + len))) {
                        if (len > max_len) {
                            max_len = len;
                            end_idx = i - 1;
                        }
                    }
                } else {
                    dp[i][j] = 0;
                }
            }
        }

        if (max_len == 0) return "";
        return s.substr(end_idx - max_len + 1, max_len);
    }
};
