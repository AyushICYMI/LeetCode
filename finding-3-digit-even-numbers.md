## Problem Statement

You are given an integer array `digits`, where each element is a digit. The array may contain duplicates.

You need to find all the unique integers that follow the given requirements:

- The integer consists of the concatenation of three elements from `digits` in any arbitrary order.
- The integer does not have leading zeros.
- The integer is even.

For example, if the given digits were `[1, 2, 3]`, integers `132` and `312` follow the requirements.

Return a sorted array of the unique integers.

---

## Approach

The problem requires finding all even three-digit numbers that can be formed using the digits from a given list, where each digit can be used only as many times as it appears in the list. The solution involves the following steps:

### 1. Frequency Count
Count the frequency of each digit in the input list using a hash map (or dictionary). This helps in quickly checking how many times each digit is available for use.

### 2. Iterate Through Valid Numbers
Iterate through all even three-digit numbers (from 100 to 998, incrementing by 2 each time). For each number:
- Break it down into its individual digits.
- Count the frequency of each digit in the number.

### 3. Check Feasibility
Check if the digits of the number can be formed using the digits from the input list:
- Ensure each digit in the number exists in the input list's frequency map.
- Verify that the count of each digit in the number does not exceed its count in the input list.

### 4. Collect Valid Numbers
If a number passes the feasibility check, add it to the result list.

---

## Time and Space Complexity

### Time Complexity:
- Creating the frequency map of the input digits: **O(N)**, where N is the size of the input array.
- Iterating through 450 even numbers and checking feasibility: **450 × O(1)** = **O(1)** overall.
- Hence, total time complexity is **O(N)**.

### Space Complexity:
- Frequency maps store at most 10 digit counts → **O(1)**.
- The result list can have at most 450 elements → **O(1)**.
- So, the total space complexity is **O(1)**.

**Overall:**
- **Time:** O(N)  
- **Space:** O(1)

---

## C++ Code

```cpp
class Solution {
public:
    vector<int> findEvenNumbers(vector<int>& digits) {
        unordered_map<int, int> digitCount;
        for (int digit : digits) {
            digitCount[digit]++;
        }

        vector<int> result;
        for (int num = 100; num < 1000; num += 2) {
            int temp = num;
            int d1 = temp % 10;
            temp /= 10;
            int d2 = temp % 10;
            temp /= 10;
            int d3 = temp;

            unordered_map<int, int> numDigitCount;
            numDigitCount[d1]++;
            numDigitCount[d2]++;
            numDigitCount[d3]++;

            bool isValid = true;
            for (const auto& entry : numDigitCount) {
                if (digitCount.find(entry.first) == digitCount.end() || 
                    digitCount.at(entry.first) < entry.second) {
                    isValid = false;
                    break;
                }
            }

            if (isValid) {
                result.push_back(num);
            }
        }

        return result;
    }
};
