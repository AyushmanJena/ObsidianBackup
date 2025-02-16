[643. Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)
```java
class Solution {
    public double findMaxAverage(int[] nums, int k) {
        int l = 0;
        int r;
        double sum = 0;
        for(int i = 0; i<k; i++){
            sum = sum + nums[i];
        }
        double maxSum = sum;
        for (r = k; r < nums.length; r++) {
            sum = sum - nums[l] + nums[r];
            maxSum = Math.max(maxSum, sum);
            l++;
        }
        return maxSum/k;
    }
}
```

[1456. Maximum Number of Vowels in a Substring of Given Length](https://leetcode.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length/)
```java
class Solution {
    public int maxVowels(String s, int k) {
        int r = k;
        int l = 0;
        int vowelCount = 0;

        for(int i = 0; i<k; i++){
            if(isVowel(s.charAt(i))){
                vowelCount++;
            }
        }
        int maxLen = vowelCount;

        while(r < s.length()){
            if(r < s.length() && isVowel(s.charAt(r))){
                vowelCount++;
            }
            if(l < s.length() && isVowel(s.charAt(l))){
                vowelCount--;
            }
            if(maxLen < vowelCount){
                maxLen = vowelCount;
            }      
            l++;
            r++;
        }
        return maxLen;
    }
    public boolean isVowel(char c){
        if(c == 'a'|| c == 'e'|| c == 'i'|| c == 'o'|| c == 'u'){
            return true;
        }
        return false;
    }
}
```

[1004. Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/)
```java
class Solution {
    public int longestOnes(int[] nums, int k) {
        int zeroCount = 0;
        int l = 0;
        int r = 0;
        int len = 0;
        int maxLen = 0;

        while(r < nums.length){
            if(nums[r] == 0){
                zeroCount++;
            }

            if(zeroCount > k){
                if(nums[l] == 0){
                    zeroCount--;
                }
                l++;
            }

            if(zeroCount <= k){
                len = r-l + 1;
                if(len > maxLen){
                    maxLen = len;
                }
            }
            r++;
        }
        return maxLen;
    }
}
```

[1493. Longest Subarray of 1's After Deleting One Element](https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element/)
```java
class Solution {
    public int longestSubarray(int[] nums) {
        int l = 0, r = 0;
        int maxLen = 0;
        int zeroCount = 0;

        while(r < nums.length){
            if(nums[r] == 0){
                zeroCount++;
            }
            if(zeroCount > 1){
                if(nums[l] == 0){
                    zeroCount--;
                }
                l++;
            }
            if(r-l+1 > maxLen){
                maxLen = r-l;
            }
            r++;
        }

        return maxLen;
    }
}
```