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

[3487. Maximum Unique Subarray Sum After Deletion](https://leetcode.com/problems/maximum-unique-subarray-sum-after-deletion/)
```java
class Solution {
    public int maxSum(int[] nums) {
        int maxElement = Integer.MIN_VALUE;
        int positiveSum = 0;
        Set<Integer> seen = new HashSet<>();

        for (int num : nums) {
            if (!seen.contains(num)) {
                seen.add(num);
                if (num > 0) {
                    positiveSum += num;
                }
                maxElement = Math.max(maxElement, num);
            }
        }

        return positiveSum > 0 ? positiveSum : maxElement;
    }
}

```

[118. Pascal's Triangle](https://leetcode.com/problems/pascals-triangle/)
```java
class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> ans = new ArrayList<>();

        for(int i = 0; i<numRows; i++){
            ans.add(new ArrayList<Integer>());
            for(int j = 0; j<i+1; j++){
                ans.get(i).add(1);
            }
        }

        for(int i = 2; i<numRows; i++){
            for(int j = 1; j < i; j++){
                int top = ans.get(i-1).get(j-1);
                int left = ans.get(i-1).get(j);
                ans.get(i).set(j, top+left);
            }
        }

        return ans;
    }
}
```

[3477. Fruits Into Baskets II](https://leetcode.com/problems/fruits-into-baskets-ii/)
```java
class Solution {
    public int numOfUnplacedFruits(int[] fruits, int[] baskets) {
        int unplaced = 0;
        boolean unp = true;
        for(int i = 0; i<fruits.length; i++){
            for(int j = 0; j< baskets.length; j++){
                if(baskets[j] >= fruits[i]){
                    baskets[j] = 0;
                    unp = false;
                    break;
                }
            }
            if(unp){
                unplaced++;
            }
            unp = true;
        }
        return unplaced;
    }
}
```

[2348. Number of Zero-Filled Subarrays](https://leetcode.com/problems/number-of-zero-filled-subarrays/)
```java
class Solution {
    public long zeroFilledSubarray(int[] nums) {
        long total = 0;
        long zeroCount = 0;
        for(int i = 0; i<nums.length+1; i++){
            if(i < nums.length && nums[i] == 0){
                zeroCount++;
            }
            else{
                if(zeroCount > 0){
                    total += calculateCombinations(zeroCount);
                    zeroCount = 0;
                }
            }
        }
        return total;
    }

    public long calculateCombinations(long count){
        return count * (count + 1)/2;
    }
}
```