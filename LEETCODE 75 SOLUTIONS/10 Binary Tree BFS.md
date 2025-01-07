
[199. Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)
```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        if (root == null){
            return new LinkedList<>();
        }
        List<Integer> res = new LinkedList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);

        // perform BFS level by level
        while(!queue.isEmpty()){
            int size = queue.size();
            for(int i = 0; i<size; i++){
                TreeNode top = queue.remove();
                if(i == size-1){
                    res.add(top.val);
                }
                if(top.left != null){
                    queue.add(top.left);
                }
                if(top.right != null){
                    queue.add(top.right);
                }
            }
        }
        return res;
    }
}
```

[1161. Maximum Level Sum of a Binary Tree](https://leetcode.com/problems/maximum-level-sum-of-a-binary-tree/)
```java
class Solution {
    public int maxLevelSum(TreeNode root) {
        int max = Integer.MIN_VALUE;
        int maxLevel = 1;
        int currentLevel = 1;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);

        while(!queue.isEmpty()){
            int size = queue.size();
            int sum = 0;
            for(int i = 0; i<size; i++){
                TreeNode curr = queue.poll();
                if(curr.left != null){
                    queue.add(curr.left);
                }
                if(curr.right != null){
                    queue.add(curr.right);
                }
                sum = sum + curr.val;
            }
            if(sum > max){
                max = sum;
                maxLevel = currentLevel;
            }
            currentLevel++;
        }
        return maxLevel;
    }
}
```
