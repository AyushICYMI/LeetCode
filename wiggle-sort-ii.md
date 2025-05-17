# Wiggle Sort II

## Problem Statement
Given an integer array `nums`, reorder it such that `nums[0] < nums[1] > nums[2] < nums[3]...`. 

You may assume the input array always has a valid answer.

## Approach
1. **Sort and Interleave**:
   - First, create a sorted copy of the input array.
   - Use two pointers to interleave the smaller and larger halves of the sorted array:
     - One pointer (`midx`) starts from the middle of the array (for even indices)
     - Another pointer (`lidx`) starts from the end of the array (for odd indices)
   - Place elements from these pointers alternately to achieve the wiggle pattern.

2. **Key Insight**:
   - By taking elements from the middle and end of the sorted array alternately, we ensure that:
     - Even-indexed elements are from the smaller half
     - Odd-indexed elements are from the larger half
   - This guarantees the wiggle condition `nums[0] < nums[1] > nums[2] < nums[3]...`

## Time Complexity (TC)
- **O(n log n)**: Dominated by the sorting step. The subsequent interleaving is O(n).

## Space Complexity (SC)
- **O(n)**: We use an additional array of size n to store the sorted copy.

## Solution Code
```cpp
class Solution {
public:
    void wiggleSort(vector<int>& nums) {
        int n = nums.size();
        vector<int> numsCopy = nums;
        sort(numsCopy.begin(), numsCopy.end());
        int midx = (n - 1) >> 1;
        int lidx = n - 1;
        for (int i = 0; i < n; i++) {
            if (i & 1) nums[i] = numsCopy[lidx--];
            else nums[i] = numsCopy[midx--];
        }
    }
};
```

## Explanation
1. **Sorting**: We first sort the array to easily access the smaller and larger halves.
2. **Pointer Placement**:
   - `midx` starts at the middle of the array (for even indices)
   - `lidx` starts at the end of the array (for odd indices)
3. **Interleaving**:
   - For even indices (`i & 1 == 0`), take elements from `midx` (smaller half)
   - For odd indices (`i & 1 == 1`), take elements from `lidx` (larger half)
4. **Result**: This creates the desired wiggle pattern where each even-indexed element is smaller than its adjacent odd-indexed elements.

## Notes
- The solution assumes the input always has a valid answer.
- The wiggle condition is strictly maintained (`<` and `>` with no equals).
- For arrays with duplicates, this approach ensures proper separation of values to maintain the wiggle property.
