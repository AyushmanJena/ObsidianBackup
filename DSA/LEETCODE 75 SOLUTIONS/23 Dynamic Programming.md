[1137. N-th Tribonacci Number](https://leetcode.com/problems/n-th-tribonacci-number/)
```java
class Solution {
    public int tribonacci(int n) {
        int[] dp = new int[n+1];
        if(n < 2) return n;

        dp[0] = 0;
        dp[1] = 1;
        dp[2] = 1;

        for(int i = 3; i <= n; i++){
            dp[i] = dp[i-1] + dp[i-2] + dp[i-3];
        }
        return dp[n];
    }
}
```

[746. Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)
```java
class Solution {
    public int minCostClimbingStairs(int[] cost) {
        int[] dp = new int[cost.length];

        for(int i = cost.length-1; i>=0; i--){
            if(i+1 >= cost.length-1){
                dp[i] = cost[i];
            }
            else{
                dp[i] = cost[i] + Math.min(dp[i+1], dp[i+2]);
            }
        }
        return Math.min(dp[0], dp[1]);
    }
}
```

[198. House Robber](https://leetcode.com/problems/house-robber/)
```java
class Solution {
    public int rob(int[] nums) {
        if(nums.length == 1){
            return nums[0];
        }
        int[] dp= new int[nums.length];
        for(int i = nums.length-1; i >= 0; i--){
            if(i >= nums.length - 2){
                dp[i] = nums[i];
            }
            else if(i == nums.length - 3){
                dp[i] = nums[i] + nums[nums.length-1];
            }
            else{
                dp[i] = nums[i] + Math.max(dp[i+2], dp[i+3]);
            }
        }
        return Math.max(dp[0], dp[1]);
    }
}
```

[790. Domino and Tromino Tiling](https://leetcode.com/problems/domino-and-tromino-tiling/)
guide : https://www.youtube.com/watch?v=o75alNvYqHU
```java
class Solution {
    public int numTilings(int n) {
        if(n <= 2){
            return n;
        }

        long[] dp = new long[n+1];
        dp[0] = 1;
        dp[1] = 1;
        dp[2] = 2;

        for(int i = 3; i <= n; i++){
            dp[i] = (2 * dp[i-1] + dp[i-3]) % 1000000007;
        }

        return (int)dp[n];
    }
}
```