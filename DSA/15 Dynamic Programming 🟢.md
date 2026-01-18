# Notes
Approach : 
1. Recursion no dynamic programming
2. Memorization : Top Down Approach 
	- If the call has been made before use the previous result 
	- If not make the call and store the result
3. Tabulation : Bottom Up Approach
	- Keep storing the result in a dp
	- Use the previous result for future calls 
	- Removes the recursive stack
4. Further Space Optimization : 
	- Conditional
	- Do not store all the previous results 
	- Only store the last n results and keep updating the value of the n variables. Constant extra space.
### Fibonacci Series with Memorization
```java
public static int fibbo(int n){
	int[] dp = new int[n+1]; //store the value of repeating calls
	return fibbo(n, dp);
}

public static int fibbo(int n, int[] dp){
	if(n <= 1){
		return n;
	}
	if(dp[n] != 0){ // if the value exists use it 
		return dp[n];
	}
	dp[n] = fibbo(n-1, dp) + fibbo(n-2, dp);
	return dp[n];
}
```

### Frog Jump 1
Frog is at 1st step of n steps long staircase. Wants to reach nth stair. `Height[i]` is the height of (i+1)<sup>th</sup> stair. If frog jumps from i<sup>th</sup> to j<sup>th</sup> stair, the energy lost is (`height[i-1] - height[j-1]`).
The frog can either jump (i+1) or (i+2) steps. 
You have to find minimum total energy used by frog to reach 1st stair to Nth stair.

Using Recursive Memorization DP approach :  (top down)
```java
static int frogjump(int[] arr){
	int[] dp = new int[arr.length];
	Arrays.fill(dp, -1);
	return  helper(arr.length -1, arr, dp);
}
static int helper(int n, int[] arr, int[] dp){
	if(n == 0){
		return 0;
	}
	if(dp[n] != -1){
		return dp[n];
	}
	int left = helper(n -1, arr, dp) + Math.abs(arr[n] - arr[n - 1]);
	int right = Integer.MAX_VALUE; // for i = 1, when theres 0 but no -1
	if(n > 1){
		right = helper(n -2, arr, dp) + Math.abs(arr[n] - arr[n-2]);
	}
	dp[n] = Math.min(left, right);
	return dp[n];
}
```

Using Iterative Tabulation Approach : (bottom up)
```java
static int frogJump2(int[] arr){ // iterative tabulation approach
	int[] dp = new int[arr.length];
	Arrays.fill(dp, 0);
	for(int i = 1; i< arr.length;i++){
		int firstStep = dp[i - 1] + Math.abs(arr[i] - arr[i-1]);
		int secondStep = Integer.MAX_VALUE;
		if(i > 1){
			secondStep = dp[i-2] + Math.abs(arr[i] - arr[i-2]);
		}
		dp[i] = Math.min(firstStep, secondStep);
	}
	return dp[arr.length -1];
}
```

Space Optimized Solution :
- Since i only need (i-1) and (i-2) we can replace them with prev and prev2 and update their values with each iteration
```java
static int frogJump2(int[] arr){
	int prev = 0;
	int prev2 = 0;
	for(int i = 1; i< arr.length;i++){
		int firstStep = prev + Math.abs(arr[i] - arr[i-1]);
		int secondStep = Integer.MAX_VALUE;
		if(i > 1){
			secondStep = prev2 + Math.abs(arr[i] - arr[i-2]);
		}
		int currI = Math.min(firstStep, secondStep);
		prev2 = prev;
		prev = currI;
	}
	return prev;
}
```

### Frog Jump 2
- Similar to above question but the frog can jump till k distance 
- find minimum energy required.

Memorization
```java
public static int frogjump(int[] arr, int k){ 
	int[] dp = new int[arr.length];
	Arrays.fill(dp, -1);
	return  helper(arr.length -1, arr, dp, k);
}
public static int helper(int n, int[] arr, int[] dp, int k){
	if(n == 0){
		return 0;
	}
	if(dp[n] != -1){
		return dp[n];
	}
	int minSteps = Integer.MAX_VALUE;

	for(int j = 1; j <= k; j++){
		if(n - j >= 0){
			int jump = helper(n-j, arr, dp, k) + Math.abs(arr[n] - arr[n-j]);
			minSteps = Math.min(minSteps, jump);
		}
	}
	dp[n] = minSteps;
	return dp[n];
}
```
TC -> O(N x k)
SC -> O(N) + O(N) : dp array + recursion stack space

Iterative Tabulation
```java
public static int frogJump2(int[] arr, int k){
	int[] dp = new int[arr.length];
	Arrays.fill(dp, 0);
	for(int i = 1; i< arr.length;i++){
		int minSteps = Integer.MAX_VALUE;

		for(int j = 1; j <= k; j++){
			if((i - j) >= 0){
				int jump = dp[i-j] + Math.abs(arr[i] - arr[i-j]);
				minSteps = Math.min(minSteps, jump);
			}
		}
		dp[i] = minSteps;
	}
	return dp[arr.length -1];
}
```
TC -> O(N x k)
SC -> O(N) : dp array only

### Maximum Sum of Non-Adjacent Elements
Memorization
```java
public static int solution(int[] arr){
	int[] dp = new int[arr.length];
	return helper(arr.length-1, arr, dp);
}
public static int helper(int i, int[] arr, int[] dp){
	if(i == 0){
		return arr[i];
	}
	if(i < 0){
		return 0;
	}
	if(dp[i] != 0){
		return dp[i];
	}
	int pick = arr[i] + helper(i - 2, arr, dp); // include the number and not the previous
	int notPick = 0 + helper(i-1, arr, dp); // not include the number and call the prev
	dp[i] = Math.max(pick, notPick);
	return dp[i];
}
```

Tabulation
```java
public static int solution(int[] arr){
	int dp[] = new int[arr.length];
	dp[0] = arr[0];

	for(int i = 1; i< arr.length; i++){
		int take = arr[i];
		if(i > 1){
			take += dp[i-2];
		}

		int notTake = 0 + dp[i-1];

		dp[i] = Math.max(take, notTake);
	}

	return dp[dp.length-1];
} 
```

# 2D Dynamic Programming Questions

### Ninja Training 
- Given a 2d array of integers with each `arr[i]` of size 3
- given you have to pick a number from each array
- you cannot pick the same index for the next array
- maximize your output

Simple Recursive Solution 
```java
public static int solution(int[][] arr){
	return helper(arr, arr.length-1, 3);
}
public static int helper(int[][] arr, int day, int last){ // simple recursive solution
	if(day == 0){
		int maxi = 0;
		for(int i = 0; i<3; i++){
			if(i != last){
				maxi = Math.max(maxi, arr[0][i]);
			}
		}
		return maxi;
	}

	int maxi = 0;
	for(int i = 0; i<3; i++){
		if(i != last){
			int points = arr[day][i] + helper(arr, day-1, i);
			maxi = Math.max(maxi, points);
		}
	}
	return maxi;
}
```

Memorization 
```java
public static int solution(int[][] arr){
	int[][] dp = new int[arr.length][4];
	for(int[] day : dp){
		Arrays.fill(day, -1);
	}
	return helper2(arr, dp, arr.length-1, 3);
}

public static int helper2(int[][] arr, int[][] dp, int day, int last){ // memorization solution
	if(day == 0){
		int maxi = 0;
		for(int i = 0; i<3; i++){
			if(i != last){
				maxi = Math.max(maxi, arr[0][i]);
			}
		}
		return maxi;
	}
	if(dp[day][last] != -1){
		return dp[day][last];
	}
	int maxi = 0;
	for(int i = 0; i<3; i++){
		if(i != last){
			int points = arr[day][i] + helper(arr, day-1, i);
			maxi = Math.max(maxi, points);
		}
	}
	dp[day][last] = maxi;
	return maxi;
}
```

Tabulation and space optimized in notes.

### Total Unique Paths

##### a) Total Unique Paths
given m and n 
start from (0,0) and go till (m-1, n-1)
only moves allowed right and down, find total unique paths count possible

memorization solution
```java
class Main {
    public static void main(String[] args) {
        int m = 3;
        int n = 3;
        
        System.out.println(uniquePaths(m, n));
    }
    
    public static int uniquePaths(int m, int n){
        int[][] dp = new int[m][n];
        return helper(dp, m-1, n-1);
    }
    
    public static int helper(int[][] dp, int i, int j){
        if(i == 0 && j == 0){
            return 1;
        }
        if(i < 0 || j < 0){
            return 0;
        }
        if(dp[i][j] != 0){
            return dp[i][j];
        }
        int up = helper(dp, i-1, j);
        int left = helper(dp, i, j-1);
        dp[i][j] = up + left;
        return dp[i][j];
    }
}
```

tabulation solution
removing the recursion space complexity
bottom up approach
```java

class Main {
    public static void main(String[] args) {
        int m = 3;
        int n = 3;
        
        System.out.println(uniquePaths(m, n));
    }
    
    public static int uniquePaths(int m, int n){
        int[][] dp = new int[m][n];
        
        for(int i =0 ; i < m; i++){
            for(int j = 0; j< n; j++){
                if(i == 0 && j == 0){
                    dp[i][j] = 1;
                }else{
                    int up=0;
                    int left=0;
                    if(i>0)
                        up = dp[i-1][j];
                    if(j>0)
                        left = dp[i][j-1];
                    dp[i][j] = up + left;
                }
            }
        }
        return dp[m-1][n-1];
    }
}
```


##### b) Maze with obstacles
start 0,0
end m-1, n-1
0 -> visitable and -1 -> not visitable

Memorization solution
```java
public int mazeWithObstacles(int[][] arr){
	int[][] dp = new int[arr.length][arr[0].length];
	return helper(arr, arr.length - 1, arr[0].length -1, dp);
}
public int helper(int[][] arr, int i, int j, int[][] dp){
	if(i >= 0 && j >= 0 && arr[i][j] == -1){
		return 0;
	}
	if(i == 0  && j == 0){
		return 1;
	}
	if(i < 0 || j < 0){
		return 0;
	}
	if(dp[i][j] != -1){
		return dp[i][j];
	}
	int up = helper(arr, i-1, j, dp);
	int left = helper(arr, i, j-1, dp);
	dp[i][j] = up + left;
	return dp[i][j];
}
```

Tabulation Solution
```java
public int mazeWithObstacles(int[][] arr){
	for(int i =0; i< arr.length-1; i++){
		for(int j = 0; j<arr[0].length -1; j++){
			if(arr[i][j] == -1){
				dp[i][j] = 0;
			}
			else if(i== 0 && j == 0){
				dp[i][j] = 1;
			}
			else{
				int up = 0;
				int left = 0;
				if(i > 0){
					up = dp[i-1][j];
				}
				if( j> 0){
					left = dp[i][j-1];
				}
				dp[i][j] = up + left;
			}
		}
	}
	return dp[arr.length-1][arr[0].length-1];
}
```


##### c) Minimum Sum Path

Memorization
```java
public static int minimumSumPath(int[][] arr){
    int[][] dp = new int[arr.length][arr[0].length];
    
    for (int i = 0; i < dp.length; i++) {
        for (int j = 0; j < dp[0].length; j++) {
            dp[i][j] = -1;
        }
    }
    
    return helper(arr, arr.length-1, arr[0].length-1, dp);
}
public static int helper(int[][] arr, int i, int j, int[][] dp){
    if(i == 0 && j == 0){
        return arr[0][0];
    }
    if(dp[i][j] != -1){
        return dp[i][j];
    }
    
    int up = 0;
    int left = 0;
    
    if(i > 0){
        up = arr[i][j] + helper(arr, i-1, j, dp);
    }
    if(j > 0){
        left = arr[i][j] + helper(arr, i, j-1, dp);
    }
    
    
    if(up == 0){
        dp[i][j] = Math.min(Integer.MAX_VALUE, left);
        return dp[i][j];
    }
    if(left == 0){
        dp[i][j] =  Math.min(up, Integer.MAX_VALUE);
        return dp[i][j];
    }

    dp[i][j] = Math.min(up, left);
    return dp[i][j];
}
```

Tabulation 
```java
public static int minimumSumPath(int[][] arr){
    int[][] dp = new int[arr.length][arr[0].length];
    for(int i =0; i<arr.length; i++){
        for(int j = 0; j<arr[0].length; j++){
            if(i == 0 && j == 0){
                dp[i][j] = arr[0][0];
            }
            else{
                int up = 1000;
                int left = 1000; // assuming the largest possible value is 1000
                if(i > 0)
                    up = arr[i][j] + dp[i-1][j];
                if(j > 0)
                    left = arr[i][j] + dp[i][j-1];
                
                dp[i][j] = Math.min(up, left);
            }
        }
    }
    return dp[arr.length-1][arr[0].length -1 ];
}
```

##### d) Fixed start and Variable End on Triangular Grid
`[[1], [2, 3], [3, 6, 7], [8, 9, 6, 10]]`
Reach the bottom row with minimum cost starting from first row
```
1
2 3
3 6 7
8 9 6 10

you can either move down or diagonal right down only
```


Memorization Solution
```java
fun(i, j){
	if(i == n-1) return arr[i][j];
	
}
```





# LEETCODE QUESTIONS 

[518. Coin Change II](https://leetcode.com/problems/coin-change-ii/)
```java
class Solution {
    public int change(int amount, int[] coins) {
        Integer[][] dp = new Integer[coins.length][amount+1];
        return helper(amount, coins, 0, 0, dp);
    }

    public int helper(int amount, int[] coins, int i, int sum, Integer[][] dp){
            if(sum > amount || i >= coins.length){
                return 0;
            }
            if(sum == amount){
                return 1;
            }

            if(dp[i][sum] != null) return dp[i][sum];

            int ans = 0;

            ans += helper(amount, coins, i+1, sum, dp);
            ans += helper(amount, coins, i, sum+coins[i], dp); 

            dp[i][sum] = ans;
            return ans;   
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