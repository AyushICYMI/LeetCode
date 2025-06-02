# Candy

## Problem Statement

There are `n` children standing in a line. Each child is assigned a rating value given in the integer array `ratings`.

You are to distribute candies to these children according to the following rules:

1. Each child must receive at least **one candy**.
2. Children with a **higher rating** must receive **more candies** than their immediate neighbors.

Return the **minimum number of candies** you must give to fulfill these requirements.

---

## Intuition

To satisfy both constraints efficiently:
- First, **left to right** pass: Give one more candy than the left neighbor if the current child has a higher rating.
- Then, **right to left** pass: Give one more candy than the right neighbor if the current child has a higher rating, and if they don't already have enough candies.

This greedy two-pass solution ensures both left and right neighbor conditions are satisfied.

---

## Code

```cpp
class Solution {
public:
    int candy(vector<int>& ratings) {
        int n = ratings.size();
        vector<int> res(n, 1);

        for (int i = 1; i < n; i++)
            if (ratings[i] > ratings[i - 1])
                res[i] = res[i - 1] + 1;

        for (int i = n - 2; i >= 0; i--)
            if (ratings[i] > ratings[i + 1] && res[i] <= res[i + 1])
                res[i] = res[i + 1] + 1;

        int sum = 0;
        for (int i : res) sum += i;
        return sum;
    }
};
```

---

## Time Complexity

- **O(n)** — two linear passes through the array.

## Space Complexity

- **O(n)** — for storing the candies in a separate vector.
