# Reducing Dishes

## Problem Statement

A chef has `n` dishes, each with a satisfaction level represented by the array `satisfaction[]`.

Each dish takes exactly 1 unit of time to cook.

The **like-time coefficient** of a dish is defined as:

`time[i] * satisfaction[i]`

Where `time[i]` is the time taken to cook the current dish (including time spent on previous dishes).

The chef can:

- Cook the dishes in any order.
- Discard any number of dishes (including all or none).

Return the **maximum total like-time coefficient** the chef can obtain.

---

## Intuition

To maximize the total like-time coefficient:

- Dishes with negative satisfaction values may reduce the total and can be skipped.
- However, keeping some of them may help increase the total coefficient due to their effect on later dishes with higher satisfaction.
- Therefore, we try all possible suffixes of the sorted array (i.e., subsets of the most satisfying dishes) and compute the total like-time coefficient for each.

---

## Approach

1. Sort the `satisfaction` array in ascending order.
2. Try all possible starting indices from `0` to `n - 1`:
   - For each index `i`, compute the total like-time coefficient using dishes from index `i` to the end.
   - Multiply each satisfaction value by the time it would be cooked (starting from 1).
3. Keep track of and return the maximum value obtained.

---

## Time and Space Complexity

- **Time Complexity:** `O(n²)`  
  Outer loop for all possible starting points.  
  Inner loop to compute the coefficient for each suffix.

- **Space Complexity:** `O(1)`  
  Only scalar variables are used for tracking results.

---

## Code

```cpp
class Solution {
public:
    int maxSatisfaction(vector<int>& satisfaction) {
        sort(satisfaction.begin(), satisfaction.end());
        int n = satisfaction.size();
        int maxi = 0;

        for (int i = 0; i < n; i++) {
            int currTime = 1;
            int currResult = 0;

            for (int j = i; j < n; j++) {
                currResult += satisfaction[j] * currTime;
                currTime++;
            }

            maxi = max(maxi, currResult);
        }

        return maxi;
    }
};
