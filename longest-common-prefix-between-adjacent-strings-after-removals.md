# Longest Common Prefix Between Adjacent Strings After Removals

## Problem Statement

You're given an array of strings `words`.

For each index `i` in `words`, do the following:
1. Remove the word at index `i`.
2. Compute the **longest common prefix (LCP)** length among **all adjacent pairs** in the new array.
3. Store the **maximum LCP** found after this removal in `answer[i]`.

Return the array `answer`, where `answer[i]` corresponds to the above result after removing `words[i]`.

---

## Intuition

- To avoid re-computing the LCP from scratch `n` times (once per removal), precompute:
  - `adjLCPs[i]` = LCP length between `words[i]` and `words[i+1]`.
  - `prefMax[i]` = max LCP from the prefix subarray of `adjLCPs` up to index `i`.
  - `suffMax[i]` = max LCP from the suffix subarray starting at index `i`.

When a word at index `i` is removed:
- If `i > 0` and `i < n - 1`, consider the LCP between `words[i-1]` and `words[i+1]`.
- Additionally, the remaining LCPs are unaffected outside of that local change, so we use prefix/suffix max arrays to find the max among the rest.

---

## Code

```cpp
class Solution {
public:
    int helper(string& a, string& b) {
        int l = 0;
        while (l < a.size() && l < b.size() && a[l] == b[l])
            l++;
        return l;
    }

    vector<int> longestCommonPrefix(vector<string>& words) {
        int n = words.size();
        vector<int> ans(n, 0);
        if (n <= 1)
            return ans;

        // Precompute adjacent LCPs
        vector<int> adjLCPs(n - 1);
        for (int i = 0; i < n - 1; i++) {
            adjLCPs[i] = helper(words[i], words[i + 1]);
        }

        // Prefix maximums of adjacent LCPs
        vector<int> prefMax(n - 1);
        prefMax[0] = adjLCPs[0];
        for (int i = 1; i < n - 1; i++) {
            prefMax[i] = max(prefMax[i - 1], adjLCPs[i]);
        }

        // Suffix maximums of adjacent LCPs
        vector<int> suffMax(n - 1);
        suffMax[n - 2] = adjLCPs[n - 2];
        for (int i = n - 3; i >= 0; i--) {
            suffMax[i] = max(suffMax[i + 1], adjLCPs[i]);
        }

        for (int i = 0; i < n; i++) {
            int currMax = 0;

            // Middle element removed
            if (i > 0 && i < n - 1)
                currMax = helper(words[i - 1], words[i + 1]);

            // Left part (before i-1)
            if (i - 2 >= 0)
                currMax = max(currMax, prefMax[i - 2]);

            // Right part (after i)
            if (i + 1 < n - 1)
                currMax = max(currMax, suffMax[i + 1]);

            ans[i] = currMax;
        }

        return ans;
    }
};
```

---

## Time and Space Complexity

- **Time:** `O(n * k)` where `n` is the number of words and `k` is the average word length (for LCP computation).
- **Space:** `O(n)` for storing LCPs and prefix/suffix arrays.

---

## Example

```txt
Input: words = ["flower", "flow", "flight"]
Output: [2, 1, 2]

Explanation:
- Remove "flower" ➝ ["flow", "flight"] ➝ LCP = 2 ("fl")
- Remove "flow"   ➝ ["flower", "flight"] ➝ LCP = 1 ("f")
- Remove "flight" ➝ ["flower", "flow"]   ➝ LCP = 2 ("flo")
```
