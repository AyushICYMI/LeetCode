# Snakes and Ladders - Minimum Dice Throws

## Problem Statement

Given an `n x n` board filled in Boustrophedon style (alternating left-to-right, right-to-left), you start at square 1 and aim to reach square `n^2`.  
Each move is equivalent to a dice roll (1 to 6). If a cell has a snake or ladder (`board[r][c] != -1`), you must follow it.  
Find the **minimum number of moves** to reach square `n^2`. Return `-1` if it's impossible.

---

## Intuition

We treat the board as a 1D array for easier traversal. BFS (Breadth-First Search) is the most suitable approach for finding the shortest path when each move (dice roll) has equal weight.  
The idea is to simulate all dice rolls from each position and jump to the appropriate square, respecting ladders and snakes. A `visited` array prevents revisiting positions.

Steps:
- Reverse every alternate row of the board to convert it to a standard linear order.
- Flatten the 2D board to a 1D array for easier access.
- Use BFS to explore each possible move up to 6 steps ahead.
- If a cell has a ladder or snake (value not `-1`), jump to the indicated destination.
- Stop when you reach the final square.

---

## Code

```cpp
class Solution {
public:
    int snakesAndLadders(vector<vector<int>>& board) {
        int n = board.size();
        int destination = n * n;

        // Reverse every alternate row for Boustrophedon order
        for (int i = n - 1; i >= 0; i -= 2)
            reverse(board[i].begin(), board[i].end());

        // Flatten the board to a 1D array
        vector<int> nums;
        nums.emplace_back(0); // 0th index is dummy
        for (int i = n - 1; i >= 0; i--)
            for (int j = n - 1; j >= 0; j--)
                nums.emplace_back(board[i][j]);

        vector<bool> vis(n * n + 1, false);
        vis[1] = true;

        queue<pair<int, int>> q;
        q.push({1, 0}); // position, move count

        while (!q.empty()) {
            auto [pos, moves] = q.front();
            q.pop();

            if (pos == destination)
                return moves;

            for (int i = 1; i <= 6 && pos + i <= destination; ++i) {
                int nextPos = pos + i;
                if (nums[nextPos] != -1)
                    nextPos = nums[nextPos];

                if (!vis[nextPos]) {
                    vis[nextPos] = true;
                    q.push({nextPos, moves + 1});
                }
            }
        }

        return -1;
    }
};
```

---

## Time Complexity

- **O(n²)** — Each of the `n²` cells is visited at most once.

## Space Complexity

- **O(n²)** — For the `visited` array and the BFS queue.

