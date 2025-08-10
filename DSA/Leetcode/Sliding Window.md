 [1695. Maximum Erasure Value](https://leetcode.com/problems/maximum-erasure-value/)
Sliding Window and Hashmap implementation
```java
class Solution {
    public int maximumUniqueSubarray(int[] nums) {
        int left = 0;
        int right = 1;
        int sum = nums[0];
        int maxSum = nums[0];
        HashMap<Integer, Integer> map = new HashMap<>();
        map.put(nums[0], 0);
        while(right < nums.length){
            if(map.containsKey(nums[right])){
                int temp = map.get(nums[right]) + 1;
                // remove clutter numbers from hashmap
                while(left < nums.length && left != temp){
                    sum -= nums[left];
                    map.remove(nums[left]);
                    left++;
                }

                map.put(nums[right], right);
                sum += nums[right];
            }
            else{
                map.put(nums[right], right);
                sum += nums[right];
            }

            if(sum > maxSum){
                maxSum = sum;
            }

            right++;
        }
        return maxSum;
    }
}
```

