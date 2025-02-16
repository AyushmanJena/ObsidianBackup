[17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)
```java
class Solution {
    public List<String> letterCombinations(String digits) {
        
        String[] arr = {"","abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};

        if(digits.length() == 0){
            return new ArrayList<String>();
        }

        return solution("", digits, arr);
    }

    public List<String> solution(String p, String up, String[] arr){
        if(up.isEmpty()){
            ArrayList<String> list = new ArrayList<>();
            list.add(p);
            return list;
        }

        int digit = up.charAt(0) - '0';

        ArrayList<String> list = new ArrayList<>();

        for(int i = 0; i < arr[digit-1].length(); i++){
            char ch = arr[digit-1].charAt(i);
            list.addAll(solution(p + ch, up.substring(1), arr));
        }
        return list;
    }
}
```

[216. Combination Sum III](https://leetcode.com/problems/combination-sum-iii/)
```java
class Solution {
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<List<Integer>> result = new ArrayList<>();
        findCombinations(1, new ArrayList<>(), k, 0, n, result);
        return result;
    }

    private void findCombinations(int curr, List<Integer> list, int k, int count, int n, List<List<Integer>> result) {
        // Base case: if the sum is reached and the count is correct
        if (n == 0 && count == k) {
            result.add(new ArrayList<>(list)); // Add a copy of the current list
            return;
        }

        // If the current number exceeds n or count exceeds k, stop recursion
        if (n < 0 || count > k || curr > 9) {
            return;
        }

        // Exclude the current number and move to the next
        findCombinations(curr + 1, list, k, count, n, result);

        // Include the current number
        list.add(curr);
        findCombinations(curr + 1, list, k, count + 1, n - curr, result);
        list.remove(list.size() - 1); // Backtrack
    }
}
```

Without taking the answer in arguments (but slower)
```java
class Solution {
    public List<List<Integer>> combinationSum3(int k, int n) {
        List<Integer> list = new ArrayList<>();
        return solution(1, list ,  k, n);
    }

    public List<List<Integer>> solution(int curr,List<Integer> list, int k, int n){
        List<List<Integer>> result= new ArrayList<>();
        
        if(n== 0 && list.size() == k){
            result.add(new ArrayList<>(list));
            return result;
        }

        if(curr > 9 || n < 0 || list.size() > k){
            return result;
        }

        // excluding the current number
        result.addAll(solution(curr+1, list, k, n));

        // including the current number
        list.add(curr);
        result.addAll(solution(curr+1, list, k, n-curr));
        list.remove(list.size()-1); // backtrack

        return result;
    }
}
```