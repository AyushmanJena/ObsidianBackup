You should Know
### 1. Find GCD of two numbers using Recursion : 
- Using Euclid Division Method
```java
public static int gcd(int num1, int num2){
	if(num1 == 0)
		return num2;
	if(num2 == 0)
		return num1;
		
	if(num1 > num2)
		return gcd(num1%num2, num2);
	else
		return gcd(num1, num2%num1);
}
```

[50. Pow(x, n)](https://leetcode.com/problems/powx-n/)
```java
class Solution {
    public double myPow(double x, int n) {
        double ans = 1.0;
        long num = n;
        if (num < 0) {
            num = -1 * num;
        }

        while (num > 0) {
            if (num % 2 == 1) { // odd
                ans = ans * x;
                num = num - 1;
            } else { // even
                x = x * x;
                num = num / 2;
            }
        }
        if (n < 0) {
            ans = (double) (1.0) / (double) (ans);
        }
        return ans;
    }

}
```