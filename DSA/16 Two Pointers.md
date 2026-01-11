# LeetCode Questions

Happy Number 
- Sum of square of digits at some point is 1
```java
public boolean isHappy(int n){
	int slow = n;
	int fast = n;

	do{
		slow = findSquareSum(slow);
		fast = findSquareSum(findSquareSum(fast));
	}while(slow != fast); // if we reach the same number again, its a loop and cannot become a happy number
	if(slow == 1) return true;
	else return false;
}
private int findSquareSum(int number){
	int ans = 0;
	while(number > 0){
		int rem = number % 10;
		ans += rem * rem;
		number /= 10;
	}
	return ans;
}
```


[167. Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int first = 0;
        int second = numbers.length-1;

        while(first<second){
            if(numbers[first] + numbers[second] == target){
                return new int[] {first+1, second + 1};
            }
            else if(numbers[first] + numbers[second] < target){
                first++;
            }else{
                second--;
            }
        }
        return new int[] {first+1, second+1};
    }
}
```

