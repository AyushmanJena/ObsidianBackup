
# LeetCode

[2022. Convert 1D Array Into 2D Array](https://leetcode.com/problems/convert-1d-array-into-2d-array/)
```java
class Solution {
    public int[][] construct2DArray(int[] original, int m, int n) {
        int[][] ans = new int[m][n];
        if(m*n != original.length){
            return new int[][] {}; // return empty array if they size if different
        }
        int row;
        int col;
        for(int i = 0; i<original.length; i++){
            row = i/n;    				// row of 2d array element = original array index / number of columns
            col = i %n;					// column of 2d array element = original array index % number of columns 
            ans[row][col] = original[i];
        }
        return ans;
    }
}
```


[1894. Find the Student that Will Replace the Chalk](https://leetcode.com/problems/find-the-student-that-will-replace-the-chalk/)
```java
class Solution {
    public int chalkReplacer(int[] chalk, int k) {
        long sum = 0;
        for(int i = 0; i<chalk.length; i++){
            sum += chalk[i];
        }

        while(sum < k){
            k -= sum;
        }

        for(int i = 0; i<chalk.length; i++){
            if(k < chalk[i]){
                return i;
            }

            k -= chalk[i];
            System.out.println("k = "+k + " i = "+i);

        }
        return 0;
    }
}
```


[2441. Largest Positive Integer That Exists With Its Negative](https://leetcode.com/problems/largest-positive-integer-that-exists-with-its-negative/)
```java
class Solution {
    public int findMaxK(int[] nums) {
        int largest;
        int notAns = 1001;
        for(int i = 0; i<nums.length; i++){
            largest = 0;
            for(int j = 0; j < nums.length; j++){
                if(nums[j] > 0 && nums[j] > largest && nums[j] < notAns){
                    largest = nums[j];
                }
            }
            for(int j = 0; j<nums.length; j++){
                if(nums[j] *-1 == largest){
                    return largest;
                }
            }
            notAns = largest;
        }
        return -1;
    }
}
```


Move Zeros to End
[283. Move Zeroes](https://leetcode.com/problems/move-zeroes/)
```java
class Solution {
    public void moveZeroes(int[] nums) {
       int n = nums.length;
        int ans = 0;
        
        for (int num : nums) {
            if (num != 0) {
                nums[ans++] = num;
            }
        }
        while (ans < n) {
            nums[ans++] = 0;
        }
    }
}
```


[349. Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/)
```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Arrays.sort(nums1);
        Arrays.sort(nums2);
        int i = 0;
        int j = 0;
        ArrayList<Integer> ans = new ArrayList<>();
        while (i < nums1.length && j < nums2.length) {
            if (nums1[i] == nums2[j]) {
                if (ans.isEmpty() || nums1[i] != ans.get(ans.size() - 1)) {
                    ans.add(nums1[i]);
                }
                i++;
                j++;
            } else if (nums1[i] > nums2[j]) {
                j++;
            } else {
                i++;
            }
        }
        int[] result = new int[ans.size()];
        for (int k = 0; k < ans.size(); k++) {
            result[k] = ans.get(k);
        }
        return result;
    }
}
```
Efficient Solution:
```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        int[] a = new int[1001];
        for(int i : nums1){
            a[i]++;
        }
        int[] ans = new int[nums1.length];
        int k = 0;
        for(int j : nums2){
            if(a[j] != 0){
                a[j] = 0;
                ans[k++] = j;
            }
        }

        return Arrays.copyOfRange(ans,0,k);
    }
}
```


[108. Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)
```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return sortedArrayToBST(nums, 0, nums.length-1);
    }
    public TreeNode sortedArrayToBST(int[] nums, int left, int right){
        if(left > right){
            return null;
        }
        int mid = left + (right - left)/2;
        TreeNode node = new TreeNode(nums[mid]);
        node.left = sortedArrayToBST(nums, left, mid-1);
        node.right = sortedArrayToBST(nums, mid+1, right);
        return node;
    }
}
```


[238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)
```java
public static int[] productExceptSelf(int[] nums) {
    int n = nums.length;
    int[] result = new int[n];
    for (int i = 0; i < n; i++) {
        result[i] = 1;
    }
    int product = 1;
    for (int i = 0; i < n; i++) {
        result[i] = product;
        product *= nums[i];
    }
    product = 1;
    for (int i = n - 1; i >= 0; i--) {
        result[i] *= product;
        product *= nums[i];
    }

    return result;
}
```


[334. Increasing Triplet Subsequence](https://leetcode.com/problems/increasing-triplet-subsequence/)
```java
// My Not so optimised solution 
class Solution {
    public boolean increasingTriplet(int[] nums){
		boolean[] prefix = new boolean[nums.length];
		boolean[] postfix = new boolean[nums.length];
		int min = nums[0];

		for(int i = 1; i<nums.length; i++){
			
			if(min <  nums[i]){
				prefix[i] = true;
			}
			else{
				min = nums[i];
				prefix[i] = false;
			}
		}
		int max = nums[nums.length -1];
		for(int i = nums.length -1; i >= 0; i--){
			if(max > nums[i]){
				postfix[i] = true;
			}
			else{
				max = nums[i];
				postfix[i] = false;
			}
		}
		for(int i = 0; i < nums.length; i++){
			if(prefix[i] && postfix[i]){
				return true;
			}
		}
		return false;
	}
}
```
Optimised Solution
```java
class Solution {
    public boolean increasingTriplet(int[] nums) {
        if(nums == null || nums.length < 3){
            return false;
        }

        int first = Integer.MAX_VALUE;
        int second = Integer. MAX_VALUE;

        for(int num : nums){
            if(num <= first){
                first = num;
            }else if(num <= second){
                second = num;
            }else{
                return true;
            }
        }
        return false;
    }
}
```


[74. Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)
```java
public class LeetCode {
	public static void main(String[] args){
		int[][] arr = {
			{1}
		};
		System.out.println(searchMatrix(arr,  1    ));
	}
	public static boolean searchMatrix(int[][] matrix, int target){
		int row = 0;
		int col = matrix[0].length -1;
		while(row < matrix.length && col >= 0 ){
			if(matrix[row][col] == target){
				return true;
			}
			else if(matrix[row][col] > target){
				col--;
			}
			else{
				row++;
			}
		}
		return false;
	}
}
```

Max Array in Array
```java
public class MaxArrayInArray {
    public static void main(String[] args){
        int[][] arr = {
                {9,9,8,1},
                {5,6,2,6},
                {8,2,6,4},
                {6,2,2,2}};
        int[][] ans = largestLocal(arr);
        for(int i = 0; i<ans.length; i++){
            for(int j = 0; j < ans.length; j++){
                System.out.print(ans[i][j]);
            }
            System.out.println();
        }
    }
    public static int[][] largestLocal(int[][] grid) {
        int m = grid.length;
        int x = 0;
        int y = 0;
        int[][] arr = new int[m-2][m-2];
        for(int i = 0; i <= m-3; i++){ // row till-1 for index -2 for size n-2
            y = 0;
            for(int j = 0;j <= m-3; j++){ // col
                arr[x][y] = findMax(grid, i,i+2, j, j+2);
                y++;
            }
            x++;
        }
        return arr;
    }
    public static int findMax(int[][] grid, int srow,int erow, int scol, int ecol ){
        int max = 0;
        for(int i = srow; i<= erow; i++){
            for(int j = scol; j <= ecol; j++){
                if(max < grid[i][j]){
                    max = grid[i][j];
                }
            }
        }
        return max;
    }
}

```


[2326. Spiral Matrix IV](https://leetcode.com/problems/spiral-matrix-iv/)
```java

/*
Input: m = 3, n = 5, head = [3,0,2,6,8,1,7,9,4,2,5,5,0]
Output: [[3,0,2,6,8],[5,0,-1,-1,1],[5,2,4,9,7]]
*/

class Solution {
    public int[][] spiralMatrix(int m, int n, ListNode head) {
        int[][] ans = new int[m][n];

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                ans[i][j] = -1;
            }
        }

        int topRow = 0;
        int leftCol = 0;
        int rightCol = n - 1;
        int bottomRow = m - 1;

        int row = 0;
        int col = 0;

        int rowSum = 0;
        int colSum = 1;

        while (head != null) {
            if (colSum == 1) {
                while (head != null && col != rightCol) {
                    ans[row][col] = head.val;
                    head = head.next;

                    col++;
                }

                topRow++;
                rowSum = 1;
                colSum = 0;

            }
            if (rowSum == 1) {
                while (head != null &&row != bottomRow) {
                    ans[row][col] = head.val;
                    head = head.next;

                    row++;
                }

                rightCol--;
                rowSum = 0;
                colSum = -1;

            }
            if (colSum == -1) {
                while (head != null &&col != leftCol) {
                    ans[row][col] = head.val;
                    head = head.next;

                    col--;
                }
                bottomRow--;
                rowSum = -1;
                colSum = 0;

            }
            if (rowSum == -1) {
                while (head != null &&row != topRow) {
                    ans[row][col] = head.val;
                    head = head.next;
                    row--;
                }
                leftCol++;
                colSum = 1;
                rowSum = 0;

            }
        }
        return ans;
    }
}
```


[2373. Largest Local Values in a Matrix](https://leetcode.com/problems/largest-local-values-in-a-matrix/)
```java
class Solution {
    public int[][] largestLocal(int[][] grid) {
        int m = grid.length;
        int x = 0;
        int y = 0;
        int[][] arr = new int[m-2][m-2];
        for(int i = 0; i <= m-3; i++){ // row till-1 for index -2 for size n-2
            y = 0;
            for(int j = 0;j <= m-3; j++){ // col
                arr[x][y] = findMax(grid, i,i+2, j, j+2);
                y++;
            }
            x++;
        }
        return arr;
    }
    public int findMax(int[][] grid, int srow,int erow, int scol, int ecol ){
        int max = 0;
        for(int i = srow; i<= erow; i++){
            for(int j = scol; j <= ecol; j++){
                if(max < grid[i][j]){
                    max = grid[i][j];
                }
            }
        }
        return max;
    }
}
```

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


[2154. Keep Multiplying Found Values by Two](https://leetcode.com/problems/keep-multiplying-found-values-by-two/)
```java
class Solution {
    public int findFinalValue(int[] nums, int original) {
        Set<Integer> numSet = new HashSet<>();
        for (int num : nums) {
            numSet.add(num);
        }
        
        while (numSet.contains(original)) {
            original *= 2;
        }
        
        return original;
    }
}
```


[36. Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)
```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        int i =0;
        int j = 0;

        HashSet<Character> set = new HashSet<>();

        // check rows
        for(i = 0; i< 9; i++){
            for(j = 0; j < 9; j++){
                if(board[i][j] != '.'){
                    if(set.contains(board[i][j])){
                        return false;
                    }
                    set.add(board[i][j]);
                }
            }
            set.clear();
        }


        // check columns
        for(i = 0; i<9; i++){
            for(j = 0; j< 9; j++){
                if(board[j][i] != '.'){
                    if(set.contains(board[j][i])){
                        return false;
                    }
                    set.add(board[j][i]);
                }
            }
            set.clear();
        }

        i = 0;
        j = 0;

        


        // check grids
        for(i =0; i<9; i+=3){
            for(j = 0; j <9;j+=3){
                boolean check = checkGrid(board, i, j, set);
                if(check == false){
                    return false;
                }
                set.clear();
            }
        }

        return true;
    }

    public boolean checkGrid(char[][] board, int r, int c, HashSet<Character> set){
        for(int i = r; i<r+3; i++){
            for(int j = c; j< c+3; j++){
                if(board[i][j] != '.'){
                    if(set.contains(board[i][j])){
                        return false;
                    }
                    set.add(board[i][j]);
                }
            }
        }
        return true;
    }
}
```


[48. Rotate Image](https://leetcode.com/problems/rotate-image/)
```java
class Solution {
    public void rotate(int[][] matrix) {
        // transpose the matrix
        for(int i =0; i <matrix.length; i++){
            for(int j =0; j< matrix[0].length; j++){
                if(j > i){
                    int temp = matrix[i][j];
                    matrix[i][j] = matrix[j][i];
                    matrix[j][i] = temp;
                }
            }
        }

        // reverse rows 
        for(int i =0 ; i < matrix.length; i++){
            for(int j = 0; j < matrix.length / 2; j++){
                int temp = matrix[i][j];
                matrix[i][j] = matrix[i][matrix[i].length - 1 - j];
                matrix[i][matrix[i].length- 1 - j] = temp;
            }
        }
    }
}
```

[1200. Minimum Absolute Difference](https://leetcode.com/problems/minimum-absolute-difference/)
```java
class Solution {
    public List<List<Integer>> minimumAbsDifference(int[] arr) {
        Arrays.sort(arr);
        int min = Integer.MAX_VALUE;
        for(int i = 1 ; i < arr.length;i++){
            if(arr[i] - arr[i-1] < min){
                min = arr[i] - arr[i-1];
            }
        }

        List<List<Integer>> ans = new ArrayList<>();

        for(int i = 1; i< arr.length ;i++){
            if(arr[i] - arr[i-1] == min){
                ArrayList<Integer> temp = new ArrayList<>();
                temp.add(arr[i-1]);
                temp.add(arr[i]);
                ans.add(temp);
            }
        }
        return ans;
    }
}
```