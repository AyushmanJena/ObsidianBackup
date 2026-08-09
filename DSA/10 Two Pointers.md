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


[647. Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/)
```java
class Solution {
    public int countSubstrings(String s) {
        int l = 0;
        int r = 0;
        int ans = 0;
        for(int i = 0; i < s.length(); i++){
            l = i;
            r = i;

            while(l>=0 && r <= s.length()-1){
                if(s.charAt(l) != s.charAt(r)){
                    break;
                }
                ans++;
                l--;
                r++;
            }
        }

        for(int i = 0; i<s.length(); i++){
            l=i;
            r=i+1;

            while(l >= 0 && r <= s.length() -1){
                if(s.charAt(l) != s.charAt(r)){
                    break;
                }
                ans++;
                l--;
                r++;
            }
        }

        return ans;
    }
}
```

[15. 3Sum](https://leetcode.com/problems/3sum/) 🌚
```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> result = new ArrayList<>();
        
        for(int i = 0; i<nums.length ; i++){
            if(i >0 && nums[i] == nums[i-1]){
                continue;
            }

            int left = i+1;
            int right = nums.length - 1;

            while(left  < right){
                int sum = nums[i] + nums[left] + nums[right];
                if(sum == 0){
                    result.add(Arrays.asList(nums[i], nums[left], nums[right]));

                    while(left < right && nums[left] == nums[left+1])left++;
                    while(right >=  0 && left < right && nums[right] == nums[right -1])right--;

                    left++;
                    right--;
                }
                else if(sum < 0){
                    left++;
                }
                else{
                    right--;
                }
            }
        }
        return result;
    }
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

[3634. Minimum Removals to Balance Array](https://leetcode.com/problems/minimum-removals-to-balance-array/)
```java
class Solution {
    public int minRemoval(int[] nums, int k) {
        if(nums.length == 1) return 0;
        
        int l = 0;
        int maxWindow = 0;

        Arrays.sort(nums);
        
        for(int r = 0; r < nums.length ; r++){
            while((long)nums[r] > (long)nums[l] * k){
                l++;
            }
            maxWindow = Math.max(maxWindow, r-l + 1);
        }

        return nums.length - maxWindow;
    } 
}
```

[556. Next Greater Element III](https://leetcode.com/problems/next-greater-element-iii/)
#revise 
```java
public class Solution {
    public int nextGreaterElement(int n) {
        char[] number = (n + "").toCharArray();
        
        int i, j;
        // I) Start from the right most digit and 
        // find the first digit that is
        // smaller than the digit next to it.
        for (i = number.length-1; i > 0; i--)
            if (number[i-1] < number[i])
               break;

        // If no such digit is found, its the edge case 1.
        if (i == 0)
            return -1;
            
         // II) Find the smallest digit on right side of (i-1)'th 
         // digit that is greater than number[i-1]
        int x = number[i-1], smallest = i;
        for (j = i+1; j < number.length; j++)
            if (number[j] > x && number[j] <= number[smallest])
                smallest = j;
        
        // III) Swap the above found smallest digit with 
        // number[i-1]
        char temp = number[i-1];
        number[i-1] = number[smallest];
        number[smallest] = temp;
        
        // IV) Sort the digits after (i-1) in ascending order
        Arrays.sort(number, i, number.length);
        
        long val = Long.parseLong(new String(number));
        return (val <= Integer.MAX_VALUE) ? (int) val : -1;
    }
}
```


