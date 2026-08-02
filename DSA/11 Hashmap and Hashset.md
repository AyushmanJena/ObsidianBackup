
Apna College Questions 
### Majority Element : 
- Given an integer array of size n, 
- find all elements that appear more than floor(n/3) times
- Ex : input : `[1, 3, 2, 5, 1, 3, 1, 5, 1]` output : `[1]`
```java
public static void majorityElement(int[] arr){
	HashMap<Integer, Integer> map = new HashMap<>();

	int n = arr.length;
	for(int i = 0; i<n; i++){
		if(map.containsKey(arr[i])){
			map.put(arr[i], map.get(arr[i]) + 1);
		}else{
			map.put(arr[i], 1);
		}
	}

	for(int key : map.keySet()){
		if(map.get(key) > n/3){
			System.out.println(key);
		}
	}
}
```

### Set Union of Two arrays
- Given two arrays return another array with all elements of both the arrays
```java
public HashSet<Integer> setUnion(int[] arr1, int[] arr2){
	HashSet<Integer> set = new HashSet<>();
	for(int i = 0; i<arr1.length; i++){
		set.add(arr1[i]);
	}
	for(int j = 0; j < arr2.length; j++){
		set.add(arr2[j]);
	}
	return set;
}
```

### Set Intersection of Two Arrays
```java
public ArrayList<Integer> setIntersection(int[] arr1, int[] arr2){
	HashSet<Integer> set = new HashSet<>();
	int count = 0;
	ArrayList<Integer> res = new ArrayList<>();

	for(int i = 0; i<arr1.length; i++){
		set.add(arr1[i]);
	}

	for(int j = 0; j< arr2.length; j++){
		if(set.contains(arr2[i])){
			count++;
			res.add(arr2[j]);
			set.remove(arr2[j]);
		}
	}

	return res;
}
```

[3016. Minimum Number of Pushes to Type Word II](https://leetcode.com/problems/minimum-number-of-pushes-to-type-word-ii/)
#medium #hash #array #strings #sorting 
```java
class Solution {
    public int minimumPushes(String word) {

        int[] arr = new int[26];

        for(int i = 0 ;i < word.length(); i++){
            char ch = word.charAt(i);
            arr[ch - 'a']++;
        }

        Arrays.sort(arr); // sort in asc order and traverse from back

        int repeats = 1;
        int count = 0;
        int ans = 0;

        for(int i = 25; i >= 0 ; i--){
            ans += arr[i] * repeats;
            count++;

            if(count == 8){
                count = 0;
                repeats++;
            }
        }
        return ans;
    }
}
```


### SubArray Sum equal to k
- Can be solved using Prefix sum
- But solving using HashMaps :
- Returns number of possible subarrays
```java
public static int sumArray(int[] arr, int k){
	HashMap<Integer, Integer> map = new HashMap<>(); // <sum, frequency>
	map.put(0,1);

	int ans = 0;
	int sum = 0;

	for(int j = 0; j < arr.length; j++){
		sum += arr[j];
		if(map.containsKey(sum - k)){
			ans += map.get(sum - k);
		}
		if(map.containsKey(sum)){
			map.put(sum, map.get(sum) + 1);
		}else{
			map.put(sum, 1);
		}
	}

	return ans;
}
```

### Karp Robin String Matching Algorithm 
- Kunal Kushwah
```java
package HashMaps;

public class KarpRabinAlgo {
    private static final int PRIME = 101;

    private static double calculateHash(String str){
        double hash = 0;

        for(int i = 0;i<str.length(); i++){
            hash = hash + str.charAt(i) * Math.pow(PRIME, i);
        }
        return hash;
    }

    private static double updateHash(double prevHash, char oldChar, char newChar, int patternLength){
        double newHash = (prevHash - oldChar) / PRIME; // we dont need to calculate the whole hash again
        // ex 'APPOR'V -> A'PPORV' => -A +V

        newHash = newHash + newChar * Math.pow(PRIME, patternLength-1);
        return newHash;
    }

    public static void search(String text, String pattern){
        int patternLength = pattern.length();
        double patternHash = calculateHash(pattern);
        double textHash = calculateHash(text.substring(0, patternLength));

        for(int i = 0; i<text.length()-patternLength; i++){
            if(textHash == patternHash){
                if(text.substring(i, i+patternLength).equals(pattern)){
                    System.out.println("Pattern Found at index" + i);
                }
            }

            if(i<text.length() - patternLength){
                textHash = updateHash(textHash, text.charAt(i), text.charAt(i+patternLength), patternLength);
            }
        }

    }

    public static void main(String[] args) {
        search("apporvKunalRahul", "Kunal");
    }
}

```

# LEETCODE


[3740. Minimum Distance Between Three Equal Elements I](https://leetcode.com/problems/minimum-distance-between-three-equal-elements-i/)
#array #hash #easy 
for the numbers being sorted : 
abs(a - b) = b - a  
abs(b - c) = c - b  
abs(c - a) = c - a  
Now, let's add them together: Distance = (b - a) + (c - b) + (c - a)  
The b and -b terms cancel each other out: Distance = -a + c + c - a Distance = 2 * (c - a)  
This is a crucial simplification. The distance of any good tuple is just 2 times the difference between its largest and smallest index. The middle index doesn't affect the final distance at all.
```java
class Solution {
    public int minimumDistance(int[] nums) {
        int ans = Integer.MAX_VALUE;
        HashSet<Integer> set = new HashSet<>();
        for(int i =0 ; i<nums.length; i++){
            int k = nums[i];
            if(set.contains(k)){
                continue;
            }
            set.add(k);
            int j = i;
            ArrayList<Integer> list = new ArrayList<>();
            while(j < nums.length){
                if(nums[j] == k){
                    list.add(j);
                }
                j++;
            }
            if(list.size() <= 2) continue;
            for(int l = 2; l < list.size(); l++){
                ans = Math.min(ans, 2 * (list.get(l) - list.get(l-2)));
            }
        }
        if(ans == Integer.MAX_VALUE) return -1;
        return ans;
    }
}
```


[3120. Count the Number of Special Characters I](https://leetcode.com/problems/count-the-number-of-special-characters-i/)
#strings #hash #easy 
```java
class Solution {
    public int numberOfSpecialChars(String word) {
        HashSet<Integer> set = new HashSet<>();
        for(int i = 0 ; i < word.length() ; i++){
            char ch = word.charAt(i);
            
            if((ch >= 'a' && ch <= 'z') && !set.contains(ch+0)){
                set.add(ch+0);
            }
        }
        int ans = 0;
        System.out.println(set);

        for(int i = 0; i < word.length(); i++){
            char ch = word.charAt(i);
            System.out.println(ch-32);
            if((ch >= 'A' && ch <= 'Z') && set.contains(ch + 32)){
                ans++;
                set.remove(ch+32);
            }
        }

        return ans;
    }
}
```


[349. Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/)
#easy #twopointer #array #hash
```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        Arrays.sort(nums1);
        Arrays.sort(nums2);
        int i = 0;
        int j = 0;
        HashSet<Integer> set = new HashSet<>();

        while(i < nums1.length && j < nums2.length){
            if(nums1[i] == nums2[j]){
                set.add(nums1[i]);
                i++;
                j++;
            }
            else if(nums1[i] > nums2[j]){
                j++;
            }else{
                i++;
            }
        }
        int[] ans = new int[set.size()];
        int k = 0;
        for(int n : set){
            ans[k++] = n;
        }
        return ans;
    }
}
```

[2615. Sum of Distances](https://leetcode.com/problems/sum-of-distances/)
#array #hash #prefixsum #medium 
make a frequency map of number and its occurrences
then perform prefix sum on each index of every number array
```java
class Solution {
    public long[] distance(int[] nums) {
        HashMap<Integer, ArrayList<Integer>> map = new HashMap<>();
        long[] ans = new long[nums.length];

        for(int i = 0; i< nums.length; i++){
            if(!map.containsKey(nums[i])){
                map.put(nums[i], new ArrayList<>());
            }
            map.get(nums[i]).add(i);
        }

        for(ArrayList<Integer> list : map.values()){
            int size = list.size();

            long[] prefix = new long[size + 1];

            for(int j = 0; j < size; j++){
                prefix[j + 1] = prefix[j] + list.get(j);
            }

            for(int j = 0; j < size; j++){
                long currIndex = list.get(j);
                long left = j * currIndex - prefix[j];
                long right = (prefix[size] - prefix[j + 1]) - (size - j - 1) * currIndex;

                ans[(int) currIndex] = left + right;
            }
        }

        return ans;
    }
}
```


[3761. Minimum Absolute Distance Between Mirror Pairs](https://leetcode.com/problems/minimum-absolute-distance-between-mirror-pairs/)
#medium #array #hash 
Traverse the array and keep reversing numbers, and compare the current number if there was the same number in the past (since past number is reversed)
```java
class Solution {
    public int minMirrorPairDistance(int[] nums) {
        HashMap<Integer, Integer> map = new HashMap<>();
        int ans = Integer.MAX_VALUE;
        for(int i = 0; i < nums.length; i++){
            int num = nums[i];

            if(map.containsKey(num)){
                ans = Math.min(ans, i - map.get(num));
            }

            int reversed = 0;
            int n = 0;
            while(num > 0){
                reversed = reversed * 10 + (num % 10) ;
                num = num/10;
            }
            
            map.put(reversed, i);
        }
        if(ans == Integer.MAX_VALUE) return -1;
        return ans;
    }
}
```


[3020. Find the Maximum Number of Elements in Subset](https://leetcode.com/problems/find-the-maximum-number-of-elements-in-subset/)
#medium #hash 
```java
class Solution {
    public int maximumLength(int[] nums) {
        HashMap<Integer, Integer> map = new HashMap<>();
        int ans = 0;

        for(int i = 0; i < nums.length; i++){
            if(!map.containsKey(nums[i])){
                map.put(nums[i], 1);
            }else{
                map.put(nums[i], map.get(nums[i] ) +  1);
            }
        }

        if(map.containsKey(1)){
            ans = Math.max(ans, (map.get(1) % 2 == 0)? map.get(1) - 1 : map.get(1));
        }

        for(int num : map.keySet()){
            if(num == 1){
                continue;
            }

            int temp = num;
            int max = 0;
            while(map.containsKey(temp) && map.get(temp) >= 2){
                max += 2;
                temp = temp * temp;
            }
            if(map.containsKey(temp)){
                max++;
            }else{
                max--;
            }
            ans = Math.max(ans, max);
        }

        return ans;
    }
}
```


[3488. Closest Equal Element Queries](https://leetcode.com/problems/closest-equal-element-queries/)
#medium #hash #array #binarysearch
make a frequency map of number and its all occurrences
then for each index, either its previous or next index is closest
```java
class Solution {
    public List<Integer> solveQueries(int[] nums, int[] queries) {
        HashMap<Integer, ArrayList<Integer>> map = new HashMap<>();

        for(int i = 0; i<nums.length; i++){
            if(!map.containsKey(nums[i])){
                map.put(nums[i], new ArrayList<>());
            }
            map.get(nums[i]).add(i);
        }

        List<Integer> ans = new ArrayList<>();
        for(int i = 0;i< queries.length; i++){
            int num = nums[queries[i]];

            ArrayList<Integer> list = map.get(num);

            if(list.size() == 1){
                ans.add(-1);
                continue;
            }
            
            int curr = Collections.binarySearch(list, queries[i]); //IMP for TLE

            int left = curr - 1;
            int right = curr + 1;
            if(left == -1) left = list.size() - 1;
            if(right == list.size()) right = 0;

            int n = nums.length;
            int leftDist = Math.abs(list.get(curr) - list.get(left));
            leftDist = Math.min(leftDist, n - leftDist);

            int rightDist = Math.abs(list.get(curr) - list.get(right));
            rightDist = Math.min(rightDist, n - rightDist);

            int temp = Math.min(leftDist, rightDist);

            ans.add(temp);
        }

        return ans;
    }
}
```

[3043. Find the Length of the Longest Common Prefix](https://leetcode.com/problems/find-the-length-of-the-longest-common-prefix/)
#medium #array #hash 
```java
class Solution {
    public int longestCommonPrefix(int[] arr1, int[] arr2) {
        HashSet<Integer> set = new HashSet<>();
        for(int i = 0; i < arr1.length ;i++){
            int n = arr1[i];
            while(n > 0){
                set.add(n);
                n = n /10;
            }
        }
        System.out.println("temp");
        int ans = 0;

        for(int i = 0; i< arr2.length; i++){
            int n = arr2[i];
            while(n > 0){
                if(set.contains(n)){
                    String s = Integer.toString(n);
                    ans = Math.max(ans, s.length());
                }
                n = n/10;
            }
        }

        return ans;
    }
}
```
