# Coin Change II 

## Problem Statement
You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money. Return the number of combinations that make up that amount. If that amount cannot be made up by any combination of the coins, return 0.

You may assume:
- You have an infinite number of each kind of coin
- The answer is guaranteed to fit into a signed 32-bit integer

## Approach
This is a classic dynamic programming problem for counting combinations. The approach uses a 2D DP table where:
- `dp[i][j]` represents the number of ways to make amount `j` using the first `i` coins
- Initialize `dp[0][0] = 1` (1 way to make amount 0 with 0 coins)
- For each coin, we either:
  - Don't use it: `dp[i-1][j]` (inherit from previous coins)
  - Use it: `dp[i][j - coins[i-1]]` (add ways to make the reduced amount)
- Sum these two possibilities when the coin can be used

## Complexity
- **Time Complexity**: O(n * amount) where n is the number of coins
- **Space Complexity**: O(n * amount) for the DP table (can be optimized to O(amount))

## Solution Code
```cpp
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        int n = coins.size();
        vector<vector<unsigned long long>> dp(n + 1, vector<unsigned long long>(amount + 1, 0));
        dp[0][0] = 1;
        for (int i = 1; i <= n; i++) {
            for (int j = 0; j <= amount; j++) {
                if (coins[i - 1] <= j) {
                    dp[i][j] = dp[i][j - coins[i - 1]] + dp[i - 1][j];
                }
                else dp[i][j] = dp[i - 1][j];
            }
        }
        return dp[n][amount];
    }
};
