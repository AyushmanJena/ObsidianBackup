[3417. Zigzag Grid Traversal With Skip](https://leetcode.com/problems/zigzag-grid-traversal-with-skip/)
```java
class Solution {
    public List<Integer> zigzagTraversal(int[][] grid) {
        ArrayList<Integer> list = new ArrayList<>();
        for(int i = 0 ;i<grid.length ;i++){
            if(i%2 == 0){
                helper(grid,i, 0, grid[0].length , true, list);
            }
            else{
                helper(grid,i, grid[0].length -1, -1, false, list);
            }
        }
        return list;
    }

    public void helper(int[][] grid,int i, int start, int end, boolean left, ArrayList<Integer> list){ // left -> left to right
        for(int j = start;; ){
            if((i+j)%2 == 0){
                list.add(grid[i][j]);
            }
            if(left){
                j++;
                if(j >= end){
                    return;
                }
            }else{
                j--;
                if(j <= end){
                    return;
                }
            }
        }
    }
}
```