# Coin Change 

## Problem Statement
You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money. Return the fewest number of coins needed to make up that amount. If that amount cannot be made up by any combination of the coins, return -1. 

You may assume you have an infinite number of each kind of coin.

## Approach
This is a classic dynamic programming problem where we want to find the minimum number of coins to make up a given amount. The approach uses a 2D DP table where:
- `dp[i][j]` represents the minimum number of coins needed to make amount `j` using the first `i` coins
- We initialize the DP table with `INT_MAX - 1` (to avoid overflow) and set `dp[i][0] = 0` since 0 coins are needed to make amount 0
- For each coin, we either:
  - Don't use it: `dp[i-1][j]`
  - Use it: `1 + dp[i][j - coins[i-1]]` (since we can reuse coins)
- Take the minimum of these two choices

## Complexity
- **Time Complexity**: O(n * amount) where n is the number of coins
- **Space Complexity**: O(n * amount) for the DP table (can be optimized to O(amount))

## Solution Code
```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        int n = coins.size();
        vector<vector<int>> dp(n + 1, vector<int>(amount + 1, INT_MAX - 1));
        for (int i = 0; i <= n; i++) dp[i][0] = 0;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= amount; j++) {
                if (coins[i - 1] <= j) {
                    dp[i][j] = min(dp[i - 1][j], 1 + dp[i][j - coins[i - 1]]);
                }
                else dp[i][j] = dp[i - 1][j];
            }
        }
        return dp[n][amount] == INT_MAX - 1 ? -1 : dp[n][amount];
    }
};
