Maze Questions

Return the count of all the possible paths to reach the end
- given that you can only travel right or down 
- and there are no obstacles 
- starting from 3, 3  and ends at 1, 1
```java
static int count(int r, int c){
	if(r == 1 || c == 1){
		return 1;
	}
	int left = count(r, c-1);
	int right = count(r-1, c);
	return left + right;
}
```

Printing all possible paths 
- with the constraints same as above
```java
public static void path(String s, int r, int c){
    if(r==1 && c== 1){
        System.out.println(s);
        return;
    }

    if(r > 1){
        path(s + 'D', r-1, c);
    }
    if(c > 1){
        path(s + 'R', r, c-1);
    }
}
```

Returning arraylist of strings 
- with the constraints same as above
```java
static ArrayList<String> pathRet(String p, int r, int c) {
    if (r == 1 && c == 1) {
        ArrayList<String> list = new ArrayList<>();
        list.add(p);
        return list;
    }

    ArrayList<String> list = new ArrayList<>();
    if (r > 1) {
        list.addAll(pathRet(p + 'D', r-1, c));
    }
    if (c > 1) {
        list.addAll(pathRet(p + 'R', r, c-1));
    }
    return list;
}
```

Including diagonals as a possibility
```java
public static void pathWithDiagonal(String s, int r, int c){
    if(c == 1 && r== 1){
        System.out.println(s);
        return;
    }

    if(r > 1 && c >1){
        pathWithDiagonal(s + 'D', r-1, c-1); //diagonally
    }
    if(r>1){
        pathWithDiagonal(s+'V', r-1, c); // down
    }
    if(c>1){
        pathWithDiagonal(s+'H', r, c-1);//right
    }
}
```

Mazes with closed paths 
- only down and right paths allowed 
- starts from 0, 0 ends at last cell
```java
//    boolean[][] maze = {
//            {true, true, true},
//            {true, false, true},
//            {true, true, true}
//    };
//    path("", maze, 0, 0);


public static void path(String s, boolean[][] maze, int r, int c){
    if(r == maze.length-1 && c == maze[0].length -1){
        System.out.println(s);
        return;
    }

    if(maze[r][c] == false){
        return;
    }

    if(r < maze.length-1){
        path(s + 'D',maze, r+1, c ); // down
    }
    if(c < maze[0].length -1){
        path(s+'R', maze, r, c+1);  // right
    }
}
```


NQueens Problem 
In a board of size n x n place n queens such that, none of the queens eliminate each other
Return how many ways we can do it 
```java
static int queens(boolean[][] board, int row) { // board size n x n
    if (row == board.length) {
        display(board);
        System.out.println();
        return 1;
    }

    int count = 0;

    // placing the queen and checking for every row and col
    for (int col = 0; col < board.length; col++) {
        // place the queen if it is safe
        if(isSafe(board, row, col)) {
            board[row][col] = true;
            count += queens(board, row + 1);
            board[row][col] = false;
        }
    }

    return count;
}

private static boolean isSafe(boolean[][] board, int row, int col) {
    // check vertical row
    for (int i = 0; i < row; i++) {
        if (board[i][col]) {
            return false;
        }
    }

    // diagonal left
    int maxLeft = Math.min(row, col);
    for (int i = 1; i <= maxLeft; i++) {
        if(board[row-i][col-i]) {
            return false;
        }
    }

    // diagonal right
    int maxRight = Math.min(row, board.length - col - 1);
    for (int i = 1; i <= maxRight; i++) {
        if(board[row-i][col+i]) {
            return false;
        }
    }

    return true;
}

private static void display(boolean[][] board) {
    for(boolean[] row : board) {
        for(boolean element : row) {
            if (element) {
                System.out.print("Q ");
            } else {
                System.out.print("X ");
            }
        }
        System.out.println();
    }
}
```


N Knights Problem
In a board of size n x n place n Knights such that, none of the Knights eliminate each other
Return how many ways we can do it 

```java
static void knight(boolean[][] board, int row, int col, int knights) {
    if (knights == 0) {
        display(board);
        System.out.println();
        return;
    }

    if (row == board.length - 1 && col == board.length) {
        return;
    }

    if (col == board.length) {
        knight(board, row + 1, 0, knights);
        return;
    }

    if (isSafe(board, row, col)) {
        board[row][col] = true;
        knight(board, row, col + 1, knights - 1);
        board[row][col] = false;
    }

    knight(board, row, col + 1, knights);
}

private static boolean isSafe(boolean[][] board, int row, int col) {
    if (isValid(board, row - 2, col - 1)) {
        if (board[row - 2][col - 1]) {
            return false;
        }
    }

    if (isValid(board, row - 1, col - 2)) {
        if (board[row - 1][col - 2]) {
            return false;
        }
    }

    if (isValid(board, row - 2, col + 1)) {
        if (board[row - 2][col + 1]) {
            return false;
        }
    }

    if (isValid(board, row - 1, col + 2)) {
        if (board[row - 1][col + 2]) {
            return false;
        }
    }

    return true;
}

// do not repeat yourself, hence created this function
static boolean isValid(boolean[][] board, int row, int col) {
    if (row >= 0 && row < board.length && col >= 0 && col < board.length) {
        return true;
    }
    return false;
}

private static void display(boolean[][] board) {
    for(boolean[] row : board) {
        for(boolean element : row) {
            if (element) {
                System.out.print("K ");
            } else {
                System.out.print("X ");
            }
        }
        System.out.println();
    }
}
```

Sudoku Solver
```java
public static void main(String[] args) {
    int[][] board = new int[][]{
            {3, 0, 6, 5, 0, 8, 4, 0, 0},
            {5, 2, 0, 0, 0, 0, 0, 0, 0},
            {0, 8, 7, 0, 0, 0, 0, 3, 1},
            {0, 0, 3, 0, 1, 0, 0, 8, 0},
            {9, 0, 0, 8, 6, 3, 0, 0, 5},
            {0, 5, 0, 0, 9, 0, 6, 0, 0},
            {1, 3, 0, 0, 0, 0, 2, 5, 0},
            {0, 0, 0, 0, 0, 0, 0, 7, 4},
            {0, 0, 5, 2, 0, 6, 3, 0, 0}
    };

    if (solve(board)) {
        display(board);
    } else {
        System.out.println("Cannot solve");
    }

}

static boolean solve(int[][] board) {
    int n = board.length;
    int row = -1;
    int col = -1;

    boolean emptyLeft = true;

    // this is how we are replacing the r,c from arguments
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (board[i][j] == 0) {
                row = i;
                col = j;
                emptyLeft = false;
                break;
            }
        }
        // if you found some empty element in row, then break
        if (emptyLeft == false) {
            break;
        }
    }

    if (emptyLeft == true) {
        return true;
        // soduko is solved
    }

    // backtrack
    for (int number = 1; number <= 9; number++) {
        if (isSafe(board, row, col, number)) {
            board[row][col] = number;
            if (solve(board)) {
                // found the answer
                return true;
            } else {
                // backtrack
                board[row][col] = 0;
            }
        }
    }
    return false;
}

private static void display(int[][] board) {
    for(int[] row : board) {
        for(int num : row) {
            System.out.print(num + " ");
        }
        System.out.println();
    }
}


static boolean isSafe(int[][] board, int row, int col, int num) {
    // check the row
    for (int i = 0; i < board.length; i++) {
        // check if the number is in the row
        if (board[row][i] == num) {
            return false;
        }
    }

    // check the col
    for (int[] nums : board) {
        // check if the number is in the col
        if (nums[col] == num) {
            return false;
        }
    }

    int sqrt = (int)(Math.sqrt(board.length));
    int rowStart = row - row % sqrt;
    int colStart = col - col % sqrt;

    for (int r = rowStart; r < rowStart + sqrt; r++) {
        for (int c = colStart; c < colStart + sqrt; c++) {
            if (board[r][c] == num) {
                return false;
            }
        }
    }
    return true;
}
```

# LeetCode Questions

[22. Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)
```java
class Solution {
    public List<String> generateParenthesis(int n) {
        return helper(1, 0, new StringBuilder("("), n);
    }

    public List<String> helper(int open, int close, StringBuilder s, int n){
        if(open == n && close == n){
            ArrayList<String> ans = new ArrayList<>();
            ans.add(s.toString());
            return ans;
        }

        ArrayList<String> list = new ArrayList<>();

        if(open < n){
            s.append('(');
            list.addAll(helper(open+1, close, s, n));
            s.deleteCharAt(s.length() - 1); // backtrack
        }
        if(close <n && close < open){
            s.append(')');
            list.addAll(helper(open, close+1, s, n));
             s.deleteCharAt(s.length() - 1); // backtrack
        }

        return list;
    }
}
```

[39. Combination Sum](https://leetcode.com/problems/combination-sum/)
```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<>();
        helper(0, candidates, target, new ArrayList<>(), result);
        return result;
    }

    public void helper(int i, int[] arr, int target, List<Integer> current, List<List<Integer>> result){
         if(target == 0){
            result.add(new ArrayList<>(current));
            return;
         }

         if(i >= arr.length || target < 0){
            return;
         }

        // include current element
         current.add(arr[i]);
         helper(i, arr, target - arr[i], current, result);
         current.remove(current.size() - 1); // backtrack

        // exclude current element
        helper(i+1, arr, target, current, result);
    }
}
```

[131. Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)
```java
class Solution {
    public List<List<String>> partition(String s) {
        List<List<String>> result = new ArrayList<>();
        helper(s, 0, new ArrayList<>(), result);
        return result;
    }

    public void helper(String s, int start, List<String> path, List<List<String>> result){
        if(start == s.length()){
            result.add(new ArrayList<>(path));
            return;
        }

        for(int end = start; end < s.length(); end++){
            if(isPalindrome(s, start, end)){
                path.add(s.substring(start, end+ 1));
                helper(s, end+1, path, result);
                path.remove(path.size() -1);
            }
        }
    }

    public boolean isPalindrome(String s, int left, int right){
        while (left < right) {
            if (s.charAt(left) != s.charAt(right)) return false;
            left++;
            right--;
        }
        return true;
    }
}
```

[40. Combination Sum II](https://leetcode.com/problems/combination-sum-ii/)
```java
class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        Arrays.sort(candidates);
        List<List<Integer>> result = new ArrayList<>();
        helper(candidates,0, target, 0, new ArrayList<>(), result);
        return result;
    }

    public void helper(int[] arr, int index, int target, int sum, List<Integer> list, List<List<Integer>> result){
        
        if(sum == target){
            result.add(new ArrayList<>(list));
            return;
        }
        
        if(sum > target || index >= arr.length){
            return;
        }
        

        // including current element
        list.add(arr[index]);
        helper(arr,index+1, target, sum + arr[index], list, result);
        list.remove(list.size() -1);


        // if the current element is not being included remove all duplicates of it as well
        int temp = index+1;
        while(temp < arr.length && arr[temp] == arr[index]){
            temp++;
        }

        // excluding current element
        helper(arr, temp, target, sum, list, result);

    }
}
```

[1219. Path with Maximum Gold](https://leetcode.com/problems/path-with-maximum-gold/)
```java
class Solution {
    public int getMaximumGold(int[][] grid) {
        int n = grid.length;
        int m = grid[0].length;
        int max = 0;
        for(int i = 0; i < n; i++){
            for(int j = 0; j < m; j++){
                max = Math.max(max, path(0, grid, i, j));
            }
        }
        return max;
    }

    public int path(int sum, int[][] maze, int r, int c){
        if(r < 0 || r >= maze.length || c < 0 || c >= maze[0].length || maze[r][c] == 0){
            return sum;
        }
        
        int temp = maze[r][c];
        maze[r][c] = 0;
        int max = 0;
        
        max = Math.max(max, path(sum + temp, maze, r+1, c));
        max = Math.max(max, path(sum + temp, maze, r-1, c));
        max = Math.max(max, path(sum + temp, maze, r, c+1));
        max = Math.max(max, path(sum + temp, maze, r, c-1));
        
        maze[r][c] = temp;
        return max;
    }
}
```