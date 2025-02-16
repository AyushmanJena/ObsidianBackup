---
lastSync: Fri Oct 11 2024 18:56:12 GMT+0530 (India Standard Time)
---
[283. Move Zeroes](https://leetcode.com/problems/move-zeroes/)
```java
class Solution {
    public void moveZeroes(int[] nums) {
        int s = 0;
        for(int e = 0; e < nums.length; e++){
            if(nums[e] != 0){
                int temp = nums[s];
                nums[s] = nums[e];
                nums[e] = temp;
                s++;
            }
        }
    }
}
```

[392. Is Subsequence](https://leetcode.com/problems/is-subsequence/)
```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        int slow = 0;

        if(s.length() == 0){
            return true;
        }
        
        for(int fast = 0; fast < t.length(); fast++){
            if(slow >= s.length()){
                break;
            }
            if(s.charAt(slow) == t.charAt(fast)){
                slow++;
            }
        }
        if(slow == s.length()){
            return true;
        }
        return false;
    }
}
```

[11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)
```java
class Solution {
    public int maxArea(int[] height) {
        int left = 0;
        int right = height.length -1;
        int max = 0;

        while(left < right){
            int t = ((right -left)*Math.min(height[right], height[left]));
            if(t > max){
                max = t;
            }
            if(height[left] < height[right]){
                left++;
            }
            else{
                right--;
            }
        }
        return max;
    }
}
```

[1679. Max Number of K-Sum Pairs](https://leetcode.com/problems/max-number-of-k-sum-pairs/)
```java
class Solution {
    public int maxOperations(int[] nums, int k) {
        
        int count = 0;
        int left = 0;
        int right = nums.length -1;
        
        Arrays.sort(nums);
        
        while(left < right){
            if(nums[left] + nums[right] == k){
                left++;
                right--;
                count++;
            }
            else if(nums[left] + nums[right] > k){
                right--;
            }
            else{
                left++;
            }
        }

        return count;
    }
}
```
