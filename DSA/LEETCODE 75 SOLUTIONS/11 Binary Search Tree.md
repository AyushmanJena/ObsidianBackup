
[700. Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/)
```java
class Solution {
    public TreeNode searchBST(TreeNode root, int val) {
        if(root == null || root.val == val){
            return root;
        }
        else if(root.val > val){
            return searchBST(root.left, val);
        }
        else{
            return searchBST(root.right, val);
        }
    }
}
```

[450. Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/)
```java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        if(root == null){
            return null;
        }
        if(root.val == key){
            if(root.left == null){
                return root.right;
            }
            joinAtEnd(root.right, root.left);
            return root.left;
        }
        root.left = deleteNode(root.left, key);
        root.right = deleteNode(root.right, key);

        return root;
    }

    public TreeNode joinAtEnd(TreeNode toJoin, TreeNode onNode){
        if(onNode == null){
            return null;
        }
        if(onNode.right == null){
            onNode.right = toJoin;
            return onNode;
        }
        onNode.right = joinAtEnd(toJoin, onNode.right);
        return onNode;
    }
}
```