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

### Possible Steps


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