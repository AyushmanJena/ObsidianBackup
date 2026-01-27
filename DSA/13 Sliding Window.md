---
lastSync: Mon Oct 21 2024 23:20:26 GMT+0530 (India Standard Time)
---
##### by STRIVER

> [!check]
> Green Diary : 4th July

# IMPORTANT FORMS : 
```JAVA
public int function(int[] nums, int goal){
        int l = 0;
        int r = 0; 
        int count = 0;
        int sum= 0;

        while(r < nums.length){
            sum = sum + nums[r];
            while(l < nums.length && sum > goal){
                sum = sum - nums[l];
                l++;
            }
            count = count + (r-l+1);
            r++;
        }
        return count;
    }
```


### 1. Consecutive Maximum Sum 
-  Maximum points you can obtain from cards 
- Pick consecutively from from or back or both
- Pick a total of **k** cards 
- Ex : `arr = [6, 2, 3, 4, 7, 2, 1, 7, 1], k = 4`
		allowed : (6, 2, 3, 4), (2, 1, 7, 1), (7, 1, 6, 2), (1, 6, 2, 3), etc...
```java
public static int solution(int[] arr, int k){
    int lSum = 0;
    int rSum = 0;
    int maxSum = 0;
    
    for(int i = 0; i< k; i++){// i-> right pointer
        lSum = lSum+ arr[i];
    }
    maxSum = lSum;
    
    int j = arr.length -1; // right pointer
    
    for(int i = k-1; i>= 0; i--){
        lSum = lSum - arr[i];
        rSum = rSum + arr[j];
        j = j-1;
        maxSum = Math.max(maxSum, lSum+rSum);
    }
    return maxSum;
}
```


### 2. Longest Non Repeating substring
- Return the length of the longest substring without repeating characters
- Ex : str = "cadbzabcd", output = 5 i.e. "cadbz"
```java
public static int solution(String str){
    int[] hash = new int[256];
    int n = str.length();
    int l = 0, r = 0, maxLen = 0;
    
    while(r < n){
        if(hash[str.charAt(r)] != 0){
            if(hash[str.charAt(r)] >= l){
                l = hash[str.charAt(r)]+1;
            }
        }
        maxLen = Math.max(maxLen, r-l+1);
        hash[str.charAt(r)] = r;
        r++;
    }
    return maxLen; 
}
```
Same question using hashmap #imp
```java
    public static int solution(String str){
        HashMap<Character, Integer> map = new HashMap<>();
        int n = str.length();
        int l = 0;
        int r = 0;
        int maxLen = 0;
        
        while(r < n){
            char ch = str.charAt(r);
            if(map.containsKey(ch)){
                if(map.get(ch) > l){
                    l = map.get(ch) + 1;
                }
            }
            maxLen = Math.max(maxLen, r-l + 1);
            map.put(ch, r);
            r++;
        }
        return maxLen;
    }
```

### 3. Fruits Into Baskets 
- Max length of subarray with at-most k types of numbers 
- At most 2 types of fruits(integers) can be placed  `k = 2`
- Ex :` [3, 3, 3, 1, 2, 1, 1, 2, 3, 3, 3, 4]` ANS : `1, 2, 1, 1, 2`
		one basket contains 1 and another contains 2 
```java
public static int solution(int[] arr, int k){
    int l = 0;
    int r = 0;
    int maxLen = 0;
    HashMap<Integer, Integer> map = new HashMap<>();
    
    while(r < arr.length){
        if(map.containsKey(arr[r])){
            map.put(arr[r], map.get(arr[r]) + 1);
        }else{
            map.put(arr[r], 1);
        }
        
        if(map.size() > k){
            map.put(arr[l], map.get(arr[l])- 1);
            if(map.get(arr[l]) == 0){
                map.remove(arr[l]);
            }
            l++;
        }
        
        if(map.size() <= k){
            maxLen = Math.max(maxLen, r-l+1);
        }
        r++;
    }
    return maxLen;
}
```

### 4. Number of Substrings containing all 3 characters
```java
function(String s){
	lastSeen[3] -> {-1, -1, -1};
	count = 0;
	for(int i = 0 -> n){
		lastSeen[s[i] - 'a'] = i;
		if(lastSeen[0] != -1 && lastSeen[1] != -1 && lastSeen[2] != -1){
			count = count + 1 + Math.min(lastSeen[0], lastSeen[1], lastSeen[2]);
		}
	}
	return count;
}
```

### 5. Longest Repeating character with k replacements
- s = "**AABAB**BA", k = 2
- changing 2 B's to A such that the longest series of the same character is possible i.e. 5 A's
- Any character can be converted to any other character (given all are uppercase)
```java
public static int solution(String s, int k){
	int l = 0, r = 0, maxLen = 0, maxFreq = 0;
	int[] hash = new int[26]; // alternate to regular hashing
	while(r < s.length()){
		hash[s.charAt(r) - 'A']++;
		maxFreq = Math.max(maxFreq, hash[s.charAt(r)- 'A']);
		if((r - l +1) - maxFreq > k){ //substr len-repeatn char = non-repeatn chars
			hash[s.charAt(l) - 'A']--;
			maxFreq = 0;
			for(int i = 0; i<= 25; i++){ // again find the max repeatin char
				maxFreq = Math.max(maxFreq, hash[i]);
			}
			l++;
		}
		if((r-l+1) - maxFreq <= k){
			maxLen = Math.max(maxLen, r-l + 1);
		}
		r++;
	}
	return maxLen;
}
```

### 6. Subarrays with K different Integers
- this code finds solution for <= k different integers
- use solution(arr, k) - solution(arr, k-1)
```java
public static int solution(int[] arr, int k){
	int l = 0, r = 0, count = 0;
	HashMap<Integer, Integer> map = new HashMap<>();
	while(r < arr.length){
		if(map.containsKey(arr[r])){
			map.put(arr[r], map.get(arr[r]) + 1);
		}else{
			map.put(arr[r], 1);
		}
		while(map.size() > k){
			map.put(arr[l], map.get(arr[l]) -1);
			if(map.get(arr[l]) == 0){
				map.remove(arr[l]);
			}
			l++;
		}
		count = count + (r-l+1);
		r++;
	}
	return count;
}
```

--- 

# LeetCode Questions

[3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int start = 0;
        char ch;
        int len = 0;
        int maxLen = 0;
        HashMap<Character, Integer> map = new HashMap<>();

        for(int i = 0; i<s.length() ; i++){
            ch = s.charAt(i);
            if(map.containsKey(ch)){
                if(map.get(ch) >= start){
                    start = map.get(ch) + 1;
                    
                }
            }
            map.put(ch, i);
            len = i - start + 1;
            maxLen = Math.max(len, maxLen);
        }
        return maxLen;
    }
}
```

[930. Binary Subarrays With Sum](https://leetcode.com/problems/binary-subarrays-with-sum/) #imp
```java
class Solution {
    public int numSubarraysWithSum(int[] nums, int goal) {
        int curr = numSubarraysWithSumLessOrEqual(nums, goal);
        System.out.println(curr);
        if(goal == 0){ // goal = -1 breaks the code
            return curr;
        }
        int prev = numSubarraysWithSumLessOrEqual(nums, goal-1);
        System.out.println(prev);
        return curr-prev;
    }
    public int numSubarraysWithSumLessOrEqual(int[] nums, int goal){
        int l = 0;
        int r = 0; 
        int count = 0;
        int sum= 0;

        while(r < nums.length){
            sum = sum + nums[r];
            while(l < nums.length && sum > goal){
                sum = sum - nums[l];
                l++;
            }
            count = count + (r-l+1);
            r++;
        }
        return count;
    }
}
```


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

[2264. Largest 3-Same-Digit Number in String](https://leetcode.com/problems/largest-3-same-digit-number-in-string/)
```java
class Solution {
    public String largestGoodInteger(String num) {
        char largestGood = ' ';

        for(int i = 0; i <=num.length() -3;){
            if(num.charAt(i) == num.charAt(i+1) && num.charAt(i+1) == num.charAt(i+2)){
                if(largestGood - '0' < num.charAt(i) - '0'){
                    largestGood = num.charAt(i);
                }
                i += 2;
            }else{
                i++;
            }
        }

        if(largestGood == ' '){
            return "";
        }

        return ""+largestGood + largestGood + largestGood;
    }
}
```