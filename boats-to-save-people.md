# Boats to Save People

## Problem Statement

You are given an integer array `people` where `people[i]` is the weight of the `i-th` person, and an integer `limit` representing the maximum weight a boat can carry.

Each boat can carry at most **two people** at the same time, but only if the **sum of their weights is less than or equal to** the `limit`.

You can use an **infinite number of boats**.

Return the **minimum number of boats** required to carry every person.

---

## Intuition

To minimize the number of boats, we want to pair the **lightest** person with the **heaviest** one whenever possible.

### Strategy:
1. Sort the `people` array.
2. Use a two-pointer approach:
   - One pointer `l` starts at the lightest person.
   - The other `r` starts at the heaviest person.
3. If the sum of both `people[l] + people[r] <= limit`, they can share a boat — move both pointers.
4. If they can't share, the heavier person `r` goes alone — only move the right pointer.
5. Count each boat used.

---

## Code

```cpp
class Solution {
public:
    int numRescueBoats(vector<int>& people, int limit) {
        sort(people.begin(), people.end());
        int l = 0, r = people.size() - 1, boats = 0;
        while (l <= r) {
            if (people[l] + people[r] <= limit) l++;
            r--;
            boats++;
        }
        return boats;
    }
};
```

---

## Time Complexity

- **O(N log N)**: for sorting the array `people`.
- **O(N)**: for the two-pointer traversal.

## Space Complexity

- **O(1)**: in-place sorting and constant space usage.
