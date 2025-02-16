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

