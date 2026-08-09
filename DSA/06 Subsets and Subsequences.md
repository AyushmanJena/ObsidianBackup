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

[401. Binary Watch](https://leetcode.com/problems/binary-watch/)
medium and involved bit manipulation
```java
class Solution {
    public List<String> readBinaryWatch(int turnedOn) {
        List<String> result = new ArrayList<>();
        helper(turnedOn, 0, 0, 0, result);
        return result;
    }

    public void helper(int turnedOn, int hours, int minutes, int index, List<String> result){
        if(hours > 11 || minutes > 59){
            return;
        }
        if(turnedOn == 0){
            result.add(hours + ":" + ((minutes < 10)? "0"+minutes : minutes));
            return;
        }

        if(index == 10)return;

        // without including current index
        helper(turnedOn, hours, minutes, index+1, result);

        // including current index
        if(index < 4){
            helper(turnedOn -1, hours + (1 << index), minutes, index + 1, result);
        }else{
            helper(turnedOn -1, hours, minutes + (1 << (index - 4)), index+ 1, result);
        }
    }
}
```


[3699. Number of ZigZag Arrays I](https://leetcode.com/problems/number-of-zigzag-arrays-i/)
#hard #dp #array #greedy 
https://www.youtube.com/watch?v=rlTXRJdMAso
```java
class Solution {
    public int zigZagArrays(int n, int l, int r) {
        final int MOD = 1_000_000_007;
        int m = r - l + 1;

        long[] up   = new long[m];
        long[] down = new long[m];
        Arrays.fill(up, 1);
        Arrays.fill(down, 1);

        for (int i = 2; i <= n; i++) {
            long[] preDown = new long[m + 1];
            for (int j = 0; j < m; j++)
                preDown[j + 1] = (preDown[j] + down[j]) % MOD;

            long[] sufUp = new long[m + 1];
            for (int j = m - 1; j >= 0; j--)
                sufUp[j] = (sufUp[j + 1] + up[j]) % MOD;

            long[] newUp   = new long[m];
            long[] newDown = new long[m];
            for (int j = 0; j < m; j++) {
                newUp[j]   = preDown[j];
                newDown[j] = sufUp[j + 1];
            }

            up = newUp;
            down = newDown;
        }

        long ans = 0;
        for (int j = 0; j < m; j++){
            ans = (ans + up[j] + down[j]) % MOD;
        }
        return (int) ans;
    }
}
```


[3700. Number of ZigZag Arrays II](https://leetcode.com/problems/number-of-zigzag-arrays-ii/)
soln : https://www.youtube.com/watch?v=ayEV16AVCq4
#hard #dp #array #greedy 
```java
class Solution {
    int mod = 1_000_000_007;

    public int zigZagArrays(int n, int l, int r) {
        int m = r - l + 1;

        long[][] T = new long[m][m];
        for (int j = 0; j < m; j++) {
            for (int i = m - j; i < m; i++) {
                T[i][j] = 1; 
            }
        }

        long[][] powT = matPow(T, n - 1, m);

        long total = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < m; j++) {
                total = (total + powT[i][j]) % mod;
            }
        }

        return (int) (total * 2 % mod);
    }

    long[][] matPow(long[][] base, long e, int m) {
        long[][] res = new long[m][m];
        for (int i = 0; i < m; i++) res[i][i] = 1;

        while (e > 0) {
            if (e % 2 == 1) res = mul(res, base);
            base = mul(base, base);
            e >>= 1;
        }
        return res;
    }

    long[][] mul(long[][] A, long[][] B) {
        int m = A.length;
        long[][] C = new long[m][m];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < m; j++) {
                long sum = 0;
                for (int k = 0; k < m; k++) {
                    sum = (sum + A[i][k] * B[k][j]) % mod;
                }
                C[i][j] = sum;
            }
        }
        return C;
    }
}
```