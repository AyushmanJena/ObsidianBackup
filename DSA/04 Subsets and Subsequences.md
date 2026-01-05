### SubSets using Iterations 
Find all the subsets of a given array (does not contain duplicates) :
```java
static List<List<Integer>> subset(int[] arr) {
    List<List<Integer>> outer = new ArrayList<>();
    outer.add(new ArrayList<>());
    for (int num : arr) {
        int n = outer.size(); // find the size of the old list
        for (int i = 0; i < n; i++) {
            // create a copy of the old list
            //  new list with the element at ith index in the old list
            List<Integer> internal = new ArrayList<>(outer.get(i));
            internal.add(num); // adds the current number to the new list created
            outer.add(internal); // adds the new list to the outer list
        }
    }
    return outer;
}
```

Find all the subsets of a given array containing duplicates : 
```java
static List<List<Integer>> subsetDuplicate(int[] arr) {
    Arrays.sort(arr);
    List<List<Integer>> outer = new ArrayList<>();
    outer.add(new ArrayList<>());
    int start = 0;
    int end = 0;
    for (int i = 0; i < arr.length; i++) {
        start = 0;
        // if current and previous element is same, s = e + 1
        if (i > 0 && arr[i] == arr[i-1]) {
            start = end + 1;
        }
        end = outer.size() - 1;
        int n = outer.size();
        for (int j = start; j < n; j++) {
            List<Integer> internal = new ArrayList<>(outer.get(j));
            internal.add(arr[i]);
            outer.add(internal);
        }
    }
    return outer;
}
```

# String Permutation
input : "ABC"
output : {ABC, BAC, BCA, ACB, CAB, CBA} 
```java
// permutationsList(processed, unprocessed) // PermutationRecursion
static ArrayList<String> permutationsList(String p, String up) {
    if (up.isEmpty()) {
        ArrayList<String> list = new ArrayList<>();
        list.add(p);
        return list;
    }
    char ch = up.charAt(0);

    // local to this call
    ArrayList<String> ans = new ArrayList<>();

    for (int i = 0; i <= p.length(); i++) {
        String f = p.substring(0, i);
        String s = p.substring(i, p.length());
        ans.addAll(permutationsList(f + ch + s, up.substring(1)));
    }
    return ans;
}
```

# String Subsequence 
input : "ABC"
output : ABC, AB, AC, A, BC, B, C
```java
static ArrayList<String> subseqRet(String p, String up) {
    if (up.isEmpty()) {
        ArrayList<String> list = new ArrayList<>();
        list.add(p);
        return list;
    }
    char ch = up.charAt(0);
    ArrayList<String> left = subseqRet(p + ch, up.substring(1));
    ArrayList<String> right = subseqRet(p, up.substring(1));

    left.addAll(right);
    return left;
}
```

Dice sum
Produce all possible combinations to get the target as sum 
```java
static ArrayList<String> diceRet(String p, int target) {
    if (target == 0) {
        ArrayList<String> list = new ArrayList<>();
        list.add(p);
        return list;
    }
    ArrayList<String> list = new ArrayList<>();
    for (int i = 1; i <= 6 && i <= target; i++) {
        list.addAll(diceRet(p + i, target - i));
    }
    return list;
}
```

Dial Pad combinations (modified for uniformity)
```java
static ArrayList<String> padRet(String p, String up) {
    if (up.isEmpty()) {
        ArrayList<String> list = new ArrayList<>();
        list.add(p);
        return list;
    }
    int digit = up.charAt(0) - '0'; // this will convert '2' into 2

    ArrayList<String> list = new ArrayList<>();

    for (int i = (digit - 1) * 3; i < digit * 3; i++) {
        char ch = (char) ('a' + i);
        list.addAll(padRet(p + ch, up.substring(1)));
    }
    return list;
}
```

Original Dialpad question : 
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

# LEETCODE QUESTIONS

[46. Permutations](https://leetcode.com/problems/permutations/)
Permutations with Numbers
```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        helper(nums, 0, new ArrayList<>(), result);
        return result;
    }

    public void helper(int[] arr, int i, ArrayList<Integer> processed, List<List<Integer>> result){
        if(i == arr.length){
            result.add(new ArrayList<>(processed));
            return;
        }

        int num = arr[i];

        for(int j = 0; j<= processed.size(); j++){
            processed.add(j, num);
            helper(arr, i+1, processed, result);
            processed.remove(j);
        }
    }
}
```