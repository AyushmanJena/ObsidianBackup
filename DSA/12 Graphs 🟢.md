

## LeetCode Questions

[543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)
```java
class Solution {
    int maxPath = 0;
    public int diameterOfBinaryTree(TreeNode root) {
        traverse(root);
        return maxPath;
    }

    public int traverse(TreeNode node){
        if(node == null){ 
            return 0;
        }
 
        int left = traverse(node.left);
        int right = traverse(node.right);

        maxPath = Math.max(maxPath, left+right);

        return 1 + Math.max(left, right);
    }


}
```