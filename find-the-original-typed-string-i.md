# Find the Original Typed String I

## Problem Statement

Alice is typing a string on her keyboard, but she might have pressed a key too long **at most once**, causing a character to repeat one or more extra times.

Given the final string `s` that appeared on the screen, determine how many possible original strings Alice might have *intended* to type. 

The only allowed mistake is a **single extra repetition** for a character that may occur once per string. The goal is to count the total number of distinct original strings that could have produced the given string `s` with **at most one long press**.

---

## Intuition

To find the number of possible original strings:
- Start with the string as-is being the original (always 1 valid way).
- For each group of consecutive same characters (like `"aaa"`), Alice may have pressed the key longer than intended, so:
  - If the group has length `k > 1`, then **(k - 1)** additional ways exist by removing one character at each position (e.g., `aa` from `aaa`, or `a` from `aa`).
- Since the long press could have happened **only once**, we add `(k - 1)` for each group, but only once per group.

---

## Code

```cpp
class Solution {
public:
    int possibleStringCount(string s) {
        int n = s.size();
        int cnt = 1;  // start with 1 (original string itself)
        int currentCharFreq = 1;

        for (int i = 1; i < n; i++) {
            if (s[i] == s[i - 1]) {
                currentCharFreq += 1;
            } else {
                if (currentCharFreq > 1)
                    cnt += currentCharFreq - 1;
                currentCharFreq = 1;
            }
        }

        // Handle the last group
        if (currentCharFreq > 1)
            cnt += currentCharFreq - 1;

        return cnt;
    }
};
```

---

## Example

Suppose `s = "aabbc"`:

- `"aabbc"` (original string)
- `"abbc"` (remove one `'a'`)
- `"aabc"` (remove one `'b'`)
- `"abc"` (remove one `'a'` and one `'b'` — but not allowed since only one long press)

Only 3 possible strings: `"aabbc"`, `"abbc"`, `"aabc"`

Return: **3**

---

## Time Complexity

- **O(n)** — Single pass through the string.

## Space Complexity

- **O(1)** — Constant space usage.
