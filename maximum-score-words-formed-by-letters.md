# Maximum Score Words Formed by Letters

## Problem Statement

You are given:

- A list of `words`, where each word is a string.
- A list of `letters`, where each element is a single lowercase letter (letters can repeat).
- An array `score` of length 26, where `score[i]` is the score of the letter `'a' + i`.

You may use each letter in `letters` **at most once** to form a valid set of words. A word can only be used once, and **only if all of its characters can be found in the available letters**.

Return the **maximum total score** of any subset of `words` that can be formed under these conditions.

---

## Intuition

- We need to evaluate all **subsets** of the given words list and determine if each subset is valid (i.e., can be formed using the available letters).
- For every valid subset, we compute the total score based on the characters in the words and update the maximum score found.
- Since the number of subsets of a list of size `n` is `2^n`, this is a brute-force solution optimized with pruning (skip invalid subsets early).

---

## Approach

1. **Generate all subsets** of the `words` list using backtracking.
2. **For each subset**:
   - Count how many times each letter is used.
   - Check if the subset is valid by comparing the required letters with those available in `letters`.
   - If valid, compute the total score using the `score` array and update the result.
3. Return the **maximum score** found among all valid subsets.

---

## Time and Space Complexity

- **Time Complexity:** `O(2^n * k)`  
  Where `n` is the number of words, and `k` is the average length of a word. We check all subsets and validate/count letters for each.

- **Space Complexity:** `O(2^n * n)`  
  For storing all subsets (can be optimized to constant space with inline recursion and backtracking).

---

## Code

```cpp
class Solution {
public:
    void generateSubsets(vector<string>& words, vector<vector<string>>& subsets,
                         vector<string>& current, int index) {
        if (index == words.size()) {
            subsets.push_back(current);
            return;
        }

        // Exclude current word
        generateSubsets(words, subsets, current, index + 1);

        // Include current word
        current.push_back(words[index]);
        generateSubsets(words, subsets, current, index + 1);
        current.pop_back();
    }
    
    vector<vector<string>> getAllSubsets(vector<string>& words) {
        vector<vector<string>> subsets;
        vector<string> current;
        generateSubsets(words, subsets, current, 0);
        return subsets;
    }

    int maxScoreWords(vector<string>& words, vector<char>& letters,
                      vector<int>& score) {
        vector<vector<string>> subsets = getAllSubsets(words);
        vector<int> letterCount(26, 0);
        for (auto ch : letters) {
            letterCount[ch - 'a']++;
        }
        
        int maxScore = 0;
        
        for (auto& subset : subsets) {
            vector<int> tempCount(26, 0);
            bool valid = true;
            int currentScore = 0;
            
            // Count letters in current subset
            for (auto& word : subset) {
                for (char c : word) {
                    tempCount[c - 'a']++;
                }
            }
            
            // Validate subset
            for (int i = 0; i < 26; i++) {
                if (tempCount[i] > letterCount[i]) {
                    valid = false;
                    break;
                }
            }
            
            // Calculate score if valid
            if (valid) {
                for (int i = 0; i < 26; i++) {
                    currentScore += tempCount[i] * score[i];
                }
                maxScore = max(maxScore, currentScore);
            }
        }
        
        return maxScore;
    }
};
