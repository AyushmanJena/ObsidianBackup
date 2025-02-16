[1732. Find the Highest Altitude](https://leetcode.com/problems/find-the-highest-altitude/)
```java
class Solution {
    public int largestAltitude(int[] gain) {
        int sum = 0;
        int max = sum;
        for(int i = 0; i<gain.length; i++){
            sum = sum+gain[i];
            if(sum > max){
                max = sum;
            }
        }
        return max;
    }
}
```

[724. Find Pivot Index](https://leetcode.com/problems/find-pivot-index/)
```java
class Solution {
    public int pivotIndex(int[] nums) {
        int leftSum = 0;
        int rightSum = 0;

        for(int i = 1; i<nums.length; i++){ // start 1 not 0
            rightSum = rightSum + nums[i];
        }

        for(int i = 0; i<nums.length; i++){
            if(leftSum == rightSum){
                return i;
            }
            leftSum = leftSum + nums[i];
            if(i+1 != nums.length){
                rightSum = rightSum - nums[i+1];
            }
        }

        return -1;
    }
}
```