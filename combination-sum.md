# Combination Sum

## Problem Statement
Given an array of distinct integers `candidates` and a target integer `target`, return a list of all unique combinations of `candidates` where the chosen numbers sum to `target`. The same number may be chosen from `candidates` an unlimited number of times. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

**Constraints:**
- All elements of `candidates` are distinct.
- The number of unique combinations that sum up to `target` is less than 150.

## Approach
The solution uses a **backtracking (DFS)** approach to explore all possible combinations:
1. **Sort** the candidates to process them in order and optimize pruning.
2. Use a recursive DFS function that:
   - **Skips** the current candidate (move to next index without taking it).
   - **Takes** the current candidate (stay on same index and reduce remaining target).
3. **Base cases**:
   - If remaining target becomes 0, save the current combination.
   - If remaining target is negative or we've processed all candidates, backtrack.
4. **Prune** unnecessary branches where remaining target is less than the current candidate (since candidates are sorted).

## Complexity
- **Time Complexity (TC):** O(2^target) in the worst case, where each recursive call branches into two paths (take/skip). However, pruning reduces this significantly in practice.
- **Space Complexity (SC):** O(target) for the recursion stack depth (in the worst case where we use the smallest candidate repeatedly).

## Solution Code
```cpp
class Solution {
public:
    void dfs(int index, int remaining, vector<int>& candidates,
             vector<vector<int>>& combinations, vector<int>& currentCombo) {
        if (remaining == 0) {
            combinations.push_back(currentCombo);
            return;
        }

        if (index >= candidates.size() || remaining < candidates[index]) return;
        dfs(index + 1, remaining, candidates, combinations, currentCombo);

        currentCombo.push_back(candidates[index]);
        dfs(index, remaining - candidates[index], candidates, combinations, currentCombo);
        currentCombo.pop_back();
    }
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        vector<vector<int>> combinations;
        vector<int> currentCombo;
        dfs(0, target, candidates, combinations, currentCombo);
        return combinations;
    }
};
