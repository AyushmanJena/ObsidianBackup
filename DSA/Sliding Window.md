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


## 1. Consecutive Maximum Sum 
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


## 2. Longest Non Repeating substring
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

## 3. Fruits Into Baskets 
- Max length of subarray with at-most k types of numbers 
- At most 2 types of fruits(integers) can be repeated 
- Ex :` [3, 3, 3, 1, 2, 1, 1, 2, 3, 3, 3, 4]` ANS : `1, 2, 1, 1, 2` 
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

# 4. Number of Substrings containing all 3 characters
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

# 5. Longest Repeating character with k replacements
- s = "AABABBA", k = 2
- changing 2 B's to A such that the longest series of the same character is possible i.e. 5 A's
- Any character can be converted to any other character (given all are uppercase)
```java
public static int solution(String s, int k){
	int l = 0, r = 0, maxLen = 0, maxFreq = 0;
	int[] hash = new int[26]; // alternate to regular hashing
	while(r < s.length()){
		hash[s.charAt(r) - 'A']++;
		maxFreq = Math.max(maxFreq, hash[s.charAt(r)- 'A']);
		if((r - l +1) - maxFreq > k){
			hash[s.charAt(l) - 'A']--;
			maxFreq = 0;
			for(int i = 0; i<=25; i++){
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

# 6. Subarrays with K different Integers
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

[930. Binary Subarrays With Sum](https://leetcode.com/problems/binary-subarrays-with-sum/)
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