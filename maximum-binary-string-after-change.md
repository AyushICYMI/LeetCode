# Maximum Binary String After Change

## Problem Statement

You are given a binary string `binary` consisting only of `'0'`s and `'1'`s. You can apply the following operations any number of times:

- **Operation 1**: If the string contains `"00"`, you can replace it with `"10"`.
  - Example: `"00010"` → `"10010"`
- **Operation 2**: If the string contains `"10"`, you can replace it with `"01"`.
  - Example: `"00010"` → `"00001"`

Return the **maximum binary string** you can obtain after applying the operations any number of times.

A binary string `x` is greater than binary string `y` if the decimal representation of `x` is greater than `y`.

---

## C++ Solution

```cpp
class Solution {
public:
    string maximumBinaryString(string binary) {
        int n = binary.size();
        int firstZero = -1;
        int zeroCount = 0;

        for (int i = 0; i < n; ++i) {
            if (binary[i] == '0') {
                if (firstZero == -1) firstZero = i;
                zeroCount++;
            }
        }

        if (zeroCount <= 1) return binary;

        string result(n, '1');
        result[firstZero + zeroCount - 1] = '0';
        return result;
    }
};
