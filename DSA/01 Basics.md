You should Know

[693. Binary Number with Alternating Bits](https://leetcode.com/problems/binary-number-with-alternating-bits/)
```java
class Solution {
    public boolean hasAlternatingBits(int n) {
        StringBuilder str = new StringBuilder();
        while(n != 1){
            int rem = n % 2;
            n = n /2;
            str.append(rem);
        }
        str.append(1);

        str.reverse();
        System.out.println(str);

        char f = str.charAt(0);

        for(int i = 0; i<str.length(); i++){
            if(i % 2 == 0){
                if(str.charAt(i) != f){
                    return false;
                }
            }else{
                if(str.charAt(i) == f){
                    return false;
                }
            }
        }
        return true;
    }
}
```


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

Find Two smallest Numbers in an array
```java
public void twoSmallest(int[] nums) {
        int first = Integer.MAX_VALUE;
        int second = Integer.MAX_VALUE;

        for (int i = 0; i < nums.length; i++) {
            int x = nums[i];
            if (x < first) {
                second = first;
                first = x;
            } else if (x < second) {
                second = x;
            }
        }
        
        System.out.println(first);
        System.out.println(second);
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

Happy Number Check
At any point sum of square equals to 1
```java
public static void main(String[] args) {
    int n = 19;
    System.out.println(isHappy(12));
}

public static boolean isHappy(int n){
    int slow = n;
    int fast = n;
    do{
        slow = squareSum(slow);
        fast = squareSum(squareSum(fast));
    }while(slow != fast);

    if(slow == 1){
        return true;
    }

    return false;
}

public static int squareSum(int num){
    int ans = 0;
    while(num != 0){
        int rem = num%10;
        ans += rem*rem;
        num /= 10;
    }
    return ans;
}

```

Sieve of Eratosthenes 
Approach to find all the prime Numbers in a range
```java
static void sieveOfEratosthenes(int n){
    boolean prime[] = new boolean[n+1];
    for(int i = 0; i<=n ; i++){
        prime[i] = true;
    }

    for(int p = 2; p*p <= n; p++){
        if(prime[p] == true){
            for(int i = p*p; i<= n; i += p){
                prime[i] = false;
            }
        }
    }

    for(int i = 2; i<= n; i++){
        if(prime[i] == true){
            System.out.print(i + " ");
        }
    }
}
```

