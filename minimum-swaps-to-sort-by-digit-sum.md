# Minimum Swaps to Sort by Digit Sum

## Problem Statement

You are given an array `nums` of distinct positive integers. You need to sort the array in increasing order based on the **sum of the digits** of each number. 

If two numbers have the same digit sum, the smaller number should appear first in the sorted order.

Return the **minimum number of swaps** required to rearrange `nums` into this sorted order.

A swap is defined as exchanging the values at two distinct positions in the array.

---

## Approach

This is a variation of the classic minimum swaps to sort problem. The only difference is the custom sorting rule, which is based on digit sum and value.

### Steps

1. Define a `digitSum` function to calculate the sum of digits of a number.
2. Pair each number with its digit sum and original index: `(digitSum, number, original index)`.
3. Sort this list based on:
   - Digit sum in ascending order.
   - If digit sums are equal, use the number's value.
4. Extract the original indices from the sorted list to determine where each element in the original array should go.
5. Count the number of swaps required to bring the array to this new order using cycle detection.

---

## Cycle Detection Explanation

To count the minimum number of swaps needed to sort the array, we detect **cycles** in the mapping of current indices to their target positions in the sorted array. Each cycle of `k` elements requires `k - 1` swaps.

### Example

Original array:  
`nums = [31, 42, 13, 24]`

Digit sums:  
- 31 → 4  
- 42 → 6  
- 13 → 4  
- 24 → 6

Sorted by digit sum and then value:  
`[13, 31, 24, 42]`  
With original indices:  
- (4, 13, 2)  
- (4, 31, 0)  
- (6, 24, 3)  
- (6, 42, 1)

Mapping original index to sorted position:
- 31 (index 0) → position 1
- 42 (index 1) → position 3
- 13 (index 2) → position 0
- 24 (index 3) → position 2

So the position mapping is:  
`P = [1, 3, 0, 2]`

Now we trace cycles:
- Start at 0 → 1 → 3 → 2 → 0  
  (Cycle of size 4 → requires 3 swaps)

Total swaps needed = 3

---

## Time Complexity

Time: O(n log n)  
- Sorting the array dominates the time complexity.

---

## Space Complexity

Space: O(n)  
- Additional vectors are used for tracking sorting info and visited indices.

---

## Code

```cpp
class Solution {
public:
    int digitSum(int num) {
        int sum = 0;
        while (num > 0) {
            sum += num % 10;
            num /= 10;
        }
        return sum;
    }

    int minSwaps(vector<int>& nums) {
        int n = nums.size();
        vector<tuple<int, int, int>> numInfo;

        // Step 1: Create (digitSum, value, originalIndex) tuples
        for (int i = 0; i < n; ++i) {
            numInfo.emplace_back(digitSum(nums[i]), nums[i], i);
        }

        // Step 2: Sort based on digit sum, then value
        sort(numInfo.begin(), numInfo.end());

        // Step 3: Build index mapping of sorted positions
        vector<int> originalIndicesInSortedOrder(n);
        for (int i = 0; i < n; ++i) {
            originalIndicesInSortedOrder[i] = get<2>(numInfo[i]);
        }

        // Step 4: Count cycles to compute minimum swaps
        vector<int> visited(n, false);
        int swaps = 0;
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                int cycleSize = 0;
                int j = i;
                while (!visited[j]) {
                    visited[j] = true;
                    j = originalIndicesInSortedOrder[j];
                    cycleSize++;
                }
                if (cycleSize > 1) {
                    swaps += (cycleSize - 1);
                }
            }
        }
        return swaps;
    }
};
