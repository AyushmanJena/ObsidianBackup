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