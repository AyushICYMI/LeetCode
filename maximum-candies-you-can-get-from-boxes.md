# Maximum Candies You Can Collect From Boxes

## Problem Statement

You have `n` boxes labeled from `0` to `n - 1`. You are given four arrays:

- `status[i]`: is `1` if the `i-th` box is open and `0` if the box is closed.
- `candies[i]`: is the number of candies in the `i-th` box.
- `keys[i]`: is a list of box labels whose keys are found in the `i-th` box.
- `containedBoxes[i]`: is a list of box labels found inside the `i-th` box.

You are also given a list `initialBoxes` that contains the labels of boxes you initially have.

You can:
- Take all the candies from any open box.
- Use the keys in a box to open other boxes.
- Use the boxes found inside a box as new boxes you possess.

Return the **maximum number of candies** you can collect following the above rules.

---

## Intuition

We simulate the process using a BFS approach:

- Maintain three arrays to track:
  - Which boxes you have (`hasBox`)
  - Which boxes you have keys for (`hasKey`)
  - Which boxes are already opened (`opened`)
- Start by marking your initial boxes. If a box is already open, process it immediately.
- Use a queue to process each accessible box. When you open a box:
  - Add its candies to the total.
  - For each key inside, mark the corresponding box as now openable.
  - For each box inside, mark that you now possess it.
- If a newly possessed box is openable (open or has a key), enqueue it for future processing.

---

## Code

```cpp
class Solution {
public:
    int maxCandies(vector<int>& status, vector<int>& candies, vector<vector<int>>& keys, vector<vector<int>>& containedBoxes, vector<int>& initialBoxes) {
        int n = status.size();
        vector<bool> hasKey(n, false);
        vector<bool> hasBox(n, false);
        vector<bool> opened(n, false);

        queue<int> q;
        for (int i = 0; i < initialBoxes.size(); i++) {
            hasBox[initialBoxes[i]] = true;
            if (status[initialBoxes[i]]) {
                q.push(initialBoxes[i]);
                opened[initialBoxes[i]] = true;
            }
        }

        int ans = 0;
        while(!q.empty()) {
            int box = q.front();
            q.pop();
            ans += candies[box];

            for (int i : keys[box]) {
                hasKey[i] = true;
                if (hasBox[i] && !opened[i] && (status[i] == 1 || hasKey[i])) {
                    opened[i] = true;
                    q.push(i);
                }
            }

            for (int i : containedBoxes[box]) {
                hasBox[i] = true;
                if (!opened[i] && (status[i] == 1 || hasKey[i])) {
                    opened[i] = true;
                    q.push(i);
                }
            }
        }
        return ans;
    }
};
```

---

## Time Complexity

- **O(N + M)** where:
  - `N` is the number of boxes.
  - `M` is the total number of keys and contained boxes across all boxes.
- Each box is processed at most once.

## Space Complexity

- **O(N)** for tracking:
  - Visited boxes (`opened`)
  - Keys possessed (`hasKey`)
  - Boxes in possession (`hasBox`)
  - BFS queue.
