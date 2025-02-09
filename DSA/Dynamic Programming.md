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

