[338. Counting Bits](https://leetcode.com/problems/counting-bits/)
```java
class Solution {
    public int[] countBits(int n) {
        int[] ans = new int[n+1];
        int count = 0;
        for(int i = 0; i<=n; i++){
            int num = i;
            while(num != 0){
                count += num % 2;
                num = num >> 1;
            }
            ans[i] = count;
            count = 0;
        }
        return ans;
    }
}
```

[136. Single Number](https://leetcode.com/problems/single-number/)
using hashmap
```java
class Solution {
    public int singleNumber(int[] nums) {
        HashMap<Integer, Integer> map = new HashMap<>();
        for(int i = 0; i<nums.length; i++){
            map.putIfAbsent(nums[i], 0);
            map.put(nums[i], map.get(nums[i])+1);
        }

        for(int key : map.keySet()){
            if(map.get(key) == 1){
                return key;
            }
        }
        return 0;
    }
}
```
using bit manipulation (to use constant space)
```java
class Solution {
    public int singleNumber(int[] nums) {
        int value = 0;
        for(int num : nums){
            value = value ^ num; // 1 xor 1 = 0, so the remaining number which is not chancelled out has to be the answer
        }
        return value;
    }
}
```

[1318. Minimum Flips to Make a OR b Equal to c](https://leetcode.com/problems/minimum-flips-to-make-a-or-b-equal-to-c/)
```java
class Solution {
    public int minFlips(int a, int b, int c) {
        int ans = 0;
        while(a > 0 || b > 0 || c > 0){
            int bitA = a & 1;
            int bitB = b & 1;
            int bitC = c & 1;

            if(bitC == 0){ // change bitA and bitB if they are 1 to make both 0
                ans += (bitA + bitB);
            }
            else{ // if bitC is 1
                if(bitA == 0 && bitB == 0){
                    ans += 1; // change either one of them 
                }
            }

            a = a >> 1; // shift to the next bit for all
            b = b >> 1;
            c = c >> 1;
        }

        return ans;
    }
}
```