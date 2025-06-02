# Text Justification

## Problem Statement

Given an array of strings `words` and a width `maxWidth`, format the text such that each line has exactly `maxWidth` characters and is fully (left and right) justified.

You should pack your words using a greedy approach, fitting as many words as possible in each line. Pad extra spaces `' '` when necessary so that each line has exactly `maxWidth` characters.

- Extra spaces should be distributed as evenly as possible between words.
- If the number of spaces doesn't divide evenly, the slots on the **left** get more spaces than the ones on the right.
- The **last line** should be left-justified, and **no extra space** is inserted between words.

**Note:**
- A word is defined as a sequence of non-space characters.
- Each word's length is guaranteed to be greater than 0 and not exceed `maxWidth`.
- The input array `words` contains at least one word.

---

## Intuition

We use a greedy approach to pack words into lines. For each line:
- Accumulate words until the total length plus necessary spaces exceeds `maxWidth`.
- Justify the line:
  - If only one word, left-justify and fill with spaces.
  - Otherwise, distribute spaces evenly between words and handle the remainder by adding extra spaces to the leftmost gaps.
- Handle the last line separately by left-justifying and padding the end with spaces.

---

## Code

```cpp
class Solution {
public:
    vector<string> fullJustify(vector<string>& words, int maxW) {
        int n = words.size();
        vector<int> v(n);
        for (int i = 0; i < n; i++) v[i] = words[i].size();
        int left = 0, right = 0, sum = 0;
        vector<string> res;
        while (right < n) {
            sum += v[right];
            if (sum + (right - left) > maxW) {
                sum -= v[right];
                int numWords = right - left;
                int totalSpaces = maxW - sum;

                if (numWords == 1) {
                    string s = words[left];
                    s.append(maxW - s.size(), ' ');
                    res.push_back(s);
                } else {
                    int numberOfSpacesAfterEveryWord = totalSpaces / (numWords - 1);
                    int remainingSpaces = totalSpaces % (numWords - 1);
                    string s = words[left];
                    for (int i = left + 1; i < right; i++) {
                        s.append(numberOfSpacesAfterEveryWord, ' ');
                        if (remainingSpaces > 0) {
                            s += " ";
                            remainingSpaces--;
                        }
                        s += words[i];
                    }
                    res.push_back(s);
                }
                left = right;
                sum = v[right];
            }
            right++;
        }

        string lastLine = words[left];
        for (int i = left + 1; i < n; i++) {
            lastLine += " " + words[i];
        }
        lastLine.append(maxW - lastLine.size(), ' ');
        res.push_back(lastLine);

        return res;
    }
};
```

---

## Time Complexity

- **O(n * k)** where `n` is the number of words and `k` is the average word length — due to multiple string operations.

## Space Complexity

- **O(n)** — for storing line results and word lengths.
