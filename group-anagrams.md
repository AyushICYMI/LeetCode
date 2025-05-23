# Group Anagrams

## Problem Statement

Given an array of strings `strs`, group the anagrams together. You can return the answer in any order.

---

## Intuition

Anagrams are words that have the same characters but in different order. If we sort each word alphabetically, all anagrams will map to the same sorted string. For example:

- `"eat"`, `"tea"`, and `"ate"` → sorted becomes `"aet"`

This gives us a unique way to group them.

---

## Approach

1. Iterate through the list of strings.
2. For each string, sort it and use the sorted string as a key in a map.
3. Store the original string in the map under the sorted key.
4. After processing all strings, collect all the grouped anagram lists from the map.

---

## Code

```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        int n = strs.size();
        map<string, vector<int>> m;
        for (int i = 0; i < n; i++) {
            string s = strs[i];
            sort(s.begin(), s.end());
            m[s].push_back(i);
        }

        vector<vector<string>> ans;
        for (auto i : m) {
            vector<string> v;
            for (auto j : i.second) {
                v.push_back(strs[j]);
            }
            ans.push_back(v);
        }
        return ans;
    }
};
```

---

## Time Complexity

- Sorting each string takes **O(K log K)** where **K** is the max string length.
- For **N** strings, total time is:  
  **O(N · K log K)**

## Space Complexity

- Map stores each group: **O(N · K)** in total, where **K** is the max length of any string.

---
