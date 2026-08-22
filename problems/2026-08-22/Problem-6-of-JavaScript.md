# The Ancestral Resource Balancer

## Description

You are managing a distributed system where processes form a **perfect binary tree** hierarchy. Each node in the tree represents a process with a specific `resource_load`. 

A system is considered "Balanced" if, for every node in the tree, the sum of resource loads in its **left subtree** is exactly equal to the sum of resource loads in its **right subtree**.

Given the root of a binary tree, your task is to determine the **minimum total resource increment** required to make the entire tree balanced. You can only increase the `resource_load` of any node; you cannot decrease it.

*(Note: Since it's a perfect binary tree, all nodes have either 0 or 2 children.)*

## Examples

**Example 1:**
Input: `root = [1, 2, 2]`
- Node 1 (Root) has left child 2 and right child 2.
- The subtrees are already balanced.
Output: `0`

**Example 2:**
Input: `root = [10, 5, 2]`
- Left subtree sum is 5, right subtree sum is 2.
- To balance the root, we must increase the right child (2) to 5.
Output: `3`

**Example 3:**
Input: `root = [1, 10, 2, 4, 1, 1, 1]`
- The bottom level is balanced. The middle nodes (10, 2) need to be equalized to the max of their subtrees.
Output: `8`

## Solution (JavaScript)

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */

/**
 * @param {TreeNode} root
 * @return {number}
 */
var minResourceIncrement = function(root) {
    let totalIncrements = 0;

    function traverse(node) {
        if (!node) return 0;
        if (!node.left && !node.right) return node.val;

        let leftSum = traverse(node.left);
        let rightSum = traverse(node.right);

        // Find the difference and add to totalIncrements
        const diff = Math.abs(leftSum - rightSum);
        totalIncrements += diff;

        // Return the sum of this subtree including the increments 
        // to propagate the balanced values up the tree
        return node.val + Math.max(leftSum, rightSum) * 2;
    }

    traverse(root);
    return totalIncrements;
};
```

## Complexity

**Time: O(N)**
We visit every node in the binary tree exactly once during the post-order traversal to calculate the subtree sums.

**Space: O(H)**
Where H is the height of the tree. In the worst case (a skewed tree, though here it is a perfect binary tree, so O(log N)), this is the space required by the recursion stack.