## Problem Statement

You are given a string `s` and an integer `t`, representing the number of transformations to perform.

In one transformation, every character in `s` is replaced according to the following rules:

- If the character is `'z'`, replace it with the string `"ab"`.
- Otherwise, replace it with the next character in the alphabet. For example:
  - `'a'` becomes `'b'`
  - `'b'` becomes `'c'`
  - ...
  - `'y'` becomes `'z'`

You need to return the **length** of the resulting string after exactly `t` transformations.

Since the result can be large, return it modulo **10⁹ + 7**.

---

## Approach

Instead of simulating the transformation on the actual string (which would be very inefficient and cause memory issues), we use a **frequency-based approach**.

### Steps:

1. **Frequency Initialization**:  
   Create a frequency array `freq` of size 26 to count the number of occurrences of each character in the input string `s`.

2. **Apply Transformations**:  
   For `t` steps:
   - Create a new frequency array `new_freq` of size 26 initialized to zero.
   - For each character from `'a'` to `'z'`:
     - If it's `'z'`, update `new_freq['a']` and `new_freq['b']` with the frequency of `'z'`.
     - Otherwise, shift the frequency of character `c` to `c+1`.

3. **Count Final Length**:  
   Sum all values in the `freq` array after `t` transformations to get the total length.

This approach works efficiently even for large `t`, since we never actually build the string — just track the character counts.

---

## Time and Space Complexity

### Time Complexity:
- **O(t × 26)** → For each transformation step, we iterate through all 26 characters.

### Space Complexity:
- **O(26)** → We use two frequency arrays of size 26.

Both are effectively **O(1)** since 26 is constant, but linearly dependent on `t`.

---

## C++ Code

```cpp
class Solution {
public:
    int lengthAfterTransformations(string s, int t) {
        const int MOD = 1e9 + 7;
        vector<long long> freq(26, 0);
        
        for (char c : s) {
            freq[c - 'a']++;
        }

        for (int i = 0; i < t; ++i) {
            vector<long long> new_freq(26, 0);
            for (int j = 0; j < 26; ++j) {
                if (j == 25) {
                    new_freq[0] = (new_freq[0] + freq[j]) % MOD;
                    new_freq[1] = (new_freq[1] + freq[j]) % MOD;
                } else {
                    new_freq[j + 1] = (new_freq[j + 1] + freq[j]) % MOD;
                }
            }
            freq = new_freq;
        }

        long long total = 0;
        for (long long count : freq) {
            total = (total + count) % MOD;
        }

        return total;
    }
};
