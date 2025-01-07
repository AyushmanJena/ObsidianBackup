---
lastSync: Mon Oct 21 2024 21:13:35 GMT+0530 (India Standard Time)
---

[104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
```java
class Solution {
    public int maxDepth(TreeNode root) {
        return maxDepth(root, 0);
    }

    public int maxDepth(TreeNode node, int height){
        if(node == null){
            return height;
        }

        int maxLeft = maxDepth(node.left, height +1);
        int maxRight = maxDepth(node.right, height +1);

        return Math.max(maxLeft, maxRight);
    }
}
```

[872. Leaf-Similar Trees](https://leetcode.com/problems/leaf-similar-trees/)
```java
class Solution {
    public boolean leafSimilar(TreeNode root1, TreeNode root2) {
        ArrayList<Integer> list1 = leafSimilar(root1, new ArrayList<Integer>());
        ArrayList<Integer> list2 = leafSimilar(root2, new ArrayList<Integer>());

        return list1.equals(list2);
    }

    public ArrayList<Integer> leafSimilar(TreeNode node, ArrayList<Integer> list){

        if(node == null){
            return list;
        }

        if(node.left == null && node.right == null){
            list.add(node.val);
            return list;
        }

        leafSimilar(node.left, list);
        leafSimilar(node.right, list);
        return list;
    }
}
```

[1448. Count Good Nodes in Binary Tree](https://leetcode.com/problems/count-good-nodes-in-binary-tree/)
```java
class Solution {
    public int goodNodes(TreeNode root) {
        return goodNodes(root, root.val);
    }

    public int goodNodes(TreeNode node, int max){
        if(node == null){
            return 0;
        }
        int count = 0;
        if(node.val >= max){
            count = 1;
            max = node.val;
        }

        count = count + goodNodes(node.left, max);
        count = count + goodNodes(node.right, max);

        return count;
    }
}
```

[437. Path Sum III](https://leetcode.com/problems/path-sum-iii/) 
#todo 
```java
class Solution {
    public int pathSum(TreeNode root, int targetSum) {
        if (root == null) {
            return 0;
        }

        return countPaths(root, targetSum) + pathSum(root.left, targetSum) + pathSum(root.right, targetSum);
    }
    private int countPaths(TreeNode node, long targetSum) {
        if (node == null) {
            return 0;
        }

        int count = 0;
        if (node.val == targetSum) {
            count++;
        }
        count += countPaths(node.left, targetSum - node.val);
        count += countPaths(node.right, targetSum - node.val);

        return count;
    }
}
```

[1372. Longest ZigZag Path in a Binary Tree](https://leetcode.com/problems/longest-zigzag-path-in-a-binary-tree/)
```java
class Solution {
    public int longestZigZag(TreeNode root) {
        

        int leftCount = 0;
        int rightCount = 0;
        if(root.left != null){
            leftCount = longest(root.left, 1, 1);
        }
        if(root.right != null){
            rightCount = longest(root.right, 0, 1);
        }

        return Math.max(leftCount, rightCount);
    }

    public int longest(TreeNode node, int swap, int count){ // swap : 0 -> go left, 1 -> go right
        if(node == null){
            return count-1;
        }
        if(swap == 0){
            return Math.max(longest(node.left, 1, count+1), longest(node.right, 0, 1));
        }
        else{
            return Math.max(longest(node.left, 1, 1), longest(node.right, 0, count+1));
        }

    }
}
```

[236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)
```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode node, TreeNode p, TreeNode q) {
        if(node == null){
            return null;
        }
        TreeNode potentialAns = null;
        if(node == p || node == q){
            potentialAns = node;
        }
        
        TreeNode leftCall = lowestCommonAncestor(node.left, p, q);
        TreeNode rightCall = lowestCommonAncestor(node.right, p, q);

        if(leftCall != null && rightCall != null){
            return node;
        }
    
        if(leftCall != null && potentialAns == null){
            return leftCall;
        }
        if(rightCall != null && potentialAns == null){
            return rightCall;
        }
        return potentialAns;
    }
}
```