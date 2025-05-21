# Longest Univalue Path in a Binary Tree

## Problem Statement

Given the root of a binary tree, return the length of the longest path where each node in the path has the same value.

- The path may or may not pass through the root.
- The length of the path is represented by the number of edges between nodes.

## Approach

This problem is solved using **Depth-First Search (DFS)** in a **post-order traversal** manner.

### Key Idea

- At each node, we calculate the length of the longest path in its left and right subtree **with the same value as the current node**.
- We maintain a global `ans` variable to track the maximum length found.
- For each node:
  - Recursively calculate `leftPath` and `rightPath` from children.
  - If the left child exists and has the same value, increase the left path.
  - If the right child exists and has the same value, increase the right path.
  - Update `ans` with `left + right` (as the path can go through the node).
  - Return `max(left, right)` to propagate the valid path up.

## Time and Space Complexity

- **Time Complexity**: O(n), where n is the number of nodes in the tree.
- **Space Complexity**: O(h), where h is the height of the tree (due to recursion stack).

## Code (C++)

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int ans = 0;

    int helper(TreeNode* root) {
        if (root == nullptr) return 0;

        int leftPath = helper(root->left);
        int rightPath = helper(root->right);

        int left = 0, right = 0;
        if (root->left && root->left->val == root->val)
            left = leftPath + 1;
        if (root->right && root->right->val == root->val)
            right = rightPath + 1;

        ans = max(ans, left + right);
        return max(left, right);
    }

    int longestUnivaluePath(TreeNode* root) {
        helper(root);
        return ans;
    }
};
