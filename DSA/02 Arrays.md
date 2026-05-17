# LeetCode

[[#2D Arrays / Matrix Questions]]

[1848. Minimum Distance to the Target Element](https://leetcode.com/problems/minimum-distance-to-the-target-element/)
#array #easy #twopointer 
```java
class Solution {
    public int getMinDistance(int[] nums, int target, int start) {
        int left = start;
        int right = start;

        if(nums[start] == target) return 0;

        while(left >= 0 || right <= nums.length -1){
            if(left > 0){
                left--;
            }
            if(right < nums.length-1){
                right++;
            }
            if(nums[left] == target){
                return start - left;
            }else if(nums[right] == target){
                return right - start;
            }
        }
        return -1;
    }
}
```

[3379. Transformed Array](https://leetcode.com/problems/transformed-array/)
#easy #array
```java
class Solution {
    public int[] constructTransformedArray(int[] nums) {
        int n = nums.length;
        int[] result = new int[n];
        for(int i =0; i<n; i++){
            int newIndex = 0;
            if(nums[i] >= 0){
                newIndex = (nums[i] + i)%n;;
            }
            else{
                newIndex = (((nums[i] + i )%n) + n)%n;
            }
            result[i] = nums[newIndex];
        }

        return result;
    }
}
```

[2022. Convert 1D Array Into 2D Array](https://leetcode.com/problems/convert-1d-array-into-2d-array/)
#easy #matrix #array
```java
class Solution {
    public int[][] construct2DArray(int[] original, int m, int n) {
        int[][] ans = new int[m][n];
        if(m*n != original.length){
            return new int[][] {}; // return empty array if they size if different
        }
        int row;
        int col;
        for(int i = 0; i<original.length; i++){
            row = i/n;   // row = original array index / number of columns
            col = i %n;	 // column = original array index % number of columns 
            ans[row][col] = original[i];
        }
        return ans;
    }
}
```

[1855. Maximum Distance Between a Pair of Values](https://leetcode.com/problems/maximum-distance-between-a-pair-of-values/)
#easy #twopointer #array 
```java
class Solution {
    public int maxDistance(int[] nums1, int[] nums2) {
        int i = 0; 
        int j = 0;
        int ans = 0;
        
        while(i < nums1.length && j < nums2.length){
            if(nums1[i] > nums2[j]){
                i++;
            }
            else{
                ans = Math.max(ans, j - i);
                j++;
            }
        }

        return ans;
    }
}
```

[2553. Separate the Digits in an Array](https://leetcode.com/problems/separate-the-digits-in-an-array/)
#easy #array 
```java
class Solution {
    public int[] separateDigits(int[] nums) {
        ArrayList<Integer> digits = new ArrayList<>();

        for(int i = nums.length -1 ; i>= 0; i--){
            int num = nums[i];
            while(num != 0){
                int rem = num % 10;
                digits.add(rem);
                num = num / 10;
            }
        }

        int[] ans = new int[digits.size()];
        for(int i = 0; i < ans.length; i++){
            ans[i] = digits.get(digits.size() - 1 - i);
        }
        return ans;
    }
}
```

[2515. Shortest Distance to Target String in a Circular Array](https://leetcode.com/problems/shortest-distance-to-target-string-in-a-circular-array/)
#array #easy #twopointer 
```java
class Solution {
    public int closestTarget(String[] words, String target, int startIndex) {

        int left = startIndex;
        int right = startIndex;

        int steps = 0;

        while(steps < words.length){
            int l = (left % words.length + words.length) % words.length;
            if(target.equals(words[l])){
                return steps;
            }

            int r = right % words.length;
            if(target.equals(words[r])){
                return steps;
            }

            left--;
            right++;
            steps++;
        }
        return -1;
    }
}
```

[2441. Largest Positive Integer That Exists With Its Negative](https://leetcode.com/problems/largest-positive-integer-that-exists-with-its-negative/)
#easy #array #twopointer #sorting 
```java
class Solution {
    public int findMaxK(int[] nums) {
        Arrays.sort(nums);
        int start = 0;
        int end = nums.length-1;

        while(start < end){
            if(nums[start] == -1 * nums[end]){
                return nums[end];
            }
            if(nums[start] >  -1 * nums[end]){
                end--;
            }else{
                start++;
            }
        }
        return -1;
    }
}
```

[3487. Maximum Unique Subarray Sum After Deletion](https://leetcode.com/problems/maximum-unique-subarray-sum-after-deletion/)
Catch : you might have only negative numbers too
```java
class Solution {
    public int maxSum(int[] nums) {
        int maxElement = Integer.MIN_VALUE;
        int positiveSum = 0;
        Set<Integer> seen = new HashSet<>();

        for (int num : nums) {
            if (!seen.contains(num)) {
                seen.add(num);
                if (num > 0) {
                    positiveSum += num;
                }
                maxElement = Math.max(maxElement, num);
            }
        }

        return positiveSum > 0 ? positiveSum : maxElement;
    }
}

```

Move Zeros to End
[283. Move Zeroes](https://leetcode.com/problems/move-zeroes/)
#easy #array #twopointer 
```java
class Solution {
    public void moveZeroes(int[] nums) {
       int n = nums.length;
        int ans = 0;
        
        for (int num : nums) {
            if (num != 0) {
                nums[ans++] = num;
            }
        }
        while (ans < n) {
            nums[ans++] = 0;
        }
    }
}
```


[349. Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/)
#easy #twopointer #array #hashs
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

[153. Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)
#medium #array #sorting #binarysearch 
```java
class Solution {
    public int findMin(int[] nums) {
        int l = 0;
        int h = nums.length -1;
        int ans = Integer.MAX_VALUE;
        while(l <= h){
            int m = l + (h-l)/2;
            if(nums[l] <= nums[m]){
                ans = Math.min(ans, nums[l]);
                l = m + 1;
            }
            else{
                ans = Math.min(ans, nums[m]);
                h = m - 1;
            }
        }
        return ans;
    }
}
```

[1894. Find the Student that Will Replace the Chalk](https://leetcode.com/problems/find-the-student-that-will-replace-the-chalk/)
#medium #array 
```java
class Solution {
    public int chalkReplacer(int[] chalk, int k) {
        long sum = 0;
        for(int i = 0; i<chalk.length; i++){
            sum += chalk[i];
        }

        while(sum < k){
            k -= sum;
        }

        for(int i = 0; i<chalk.length; i++){
            if(k < chalk[i]){
                return i;
            }

            k -= chalk[i];

        }
        return 0;
    }
}
```

[1306. Jump Game III](https://leetcode.com/problems/jump-game-iii/)
#medium #array 
```java
class Solution {
    public boolean canReach(int[] arr, int start) {
        boolean[] vis = new boolean[arr.length];
        return helper(start, arr, vis);
    }

    public boolean helper(int i, int[] arr, boolean[] vis){
        if(i < 0 || i >= arr.length){
            return false;
        }

        if(arr[i] == 0){
            return true;
        }
        if(vis[i]){
            return false;
        }
        vis[i] = true;
        return helper(i - arr[i], arr, vis) || helper(i + arr[i], arr, vis);
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

[3741. Minimum Distance Between Three Equal Elements II](https://leetcode.com/problems/minimum-distance-between-three-equal-elements-ii/)
#medium #array #hash #slidingwindow 
```java
class Solution {
    public int minimumDistance(int[] nums) {
        HashMap<Integer, ArrayList<Integer>> map = new HashMap<>();

        for(int i =0; i<nums.length; i++){
            if(!map.containsKey(nums[i])){
                map.put(nums[i], new ArrayList<>());
            }
            map.get(nums[i]).add(i);
        }

        int ans = Integer.MAX_VALUE;

        for(int k : map.keySet()){
            int size = map.get(k).size();
            for(int i = 2; i<size; i++){
                ans = Math.min(ans, 2 * (map.get(k).get(i) - map.get(k).get(i-2)) );
            }
        }

        if(ans == Integer.MAX_VALUE)return -1;

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

[238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)
#medium #prefixsum #array
```java
public static int[] productExceptSelf(int[] nums) {
    int n = nums.length;
    int[] result = new int[n];
    for (int i = 0; i < n; i++) {
        result[i] = 1;
    }
    int product = 1;
    for (int i = 0; i < n; i++) {
        result[i] = product;
        product *= nums[i];
    }
    product = 1;
    for (int i = n - 1; i >= 0; i--) {
        result[i] *= product;
        product *= nums[i];
    }

    return result;
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

[334. Increasing Triplet Subsequence](https://leetcode.com/problems/increasing-triplet-subsequence/)
#medium #array #greedy
```java
// My Not so optimised solution 
class Solution {
    public boolean increasingTriplet(int[] nums){
		boolean[] prefix = new boolean[nums.length];
		boolean[] postfix = new boolean[nums.length];
		int min = nums[0];

		for(int i = 1; i<nums.length; i++){
			
			if(min <  nums[i]){
				prefix[i] = true;
			}
			else{
				min = nums[i];
				prefix[i] = false;
			}
		}
		int max = nums[nums.length -1];
		for(int i = nums.length -1; i >= 0; i--){
			if(max > nums[i]){
				postfix[i] = true;
			}
			else{
				max = nums[i];
				postfix[i] = false;
			}
		}
		for(int i = 0; i < nums.length; i++){
			if(prefix[i] && postfix[i]){
				return true;
			}
		}
		return false;
	}
}
```
Optimised Solution
```java
class Solution {
    public boolean increasingTriplet(int[] nums) {
        if(nums == null || nums.length < 3){
            return false;
        }

        int first = Integer.MAX_VALUE;
        int second = Integer. MAX_VALUE;

        for(int num : nums){
            if(num <= first){
                first = num;
            }else if(num <= second){
                second = num;
            }else{
                return true;
            }
        }
        return false;
    }
}
```

[396. Rotate Function](https://leetcode.com/problems/rotate-function/)
#medium #array #maths 
```java
class Solution {
    public int maxRotateFunction(int[] nums) {
        int sum = 0;
        int curr = 0;

        for(int i = 0 ; i < nums.length; i++){
            sum += nums[i];
            curr += nums[i] * i;
        }

        int ans = curr;

        for(int i = 1; i< nums.length; i++){
            curr = curr - sum + (nums.length * nums[i-1]);
            ans = Math.max(ans, curr);
        }

        return ans;
    }
}
```

[1356. Sort Integers by The Number of 1 Bits](https://leetcode.com/problems/sort-integers-by-the-number-of-1-bits/)
#bitmanipulation #sorting #array 
```java
class Solution {
    public int[] sortByBits(int[] arr) {
        int[] ans = new int[arr.length];
        HashMap<Integer, ArrayList<Integer>> map = new HashMap<>();

        // sort based on number of 1s
        for(int i = 0; i<arr.length; i++){
            int n = Integer.bitCount(arr[i]);
            if(!map.containsKey(n)){
                map.put(n, new ArrayList<>());
            }
            map.get(n).add(arr[i]);
        }

        List<Integer> keys = new ArrayList<>(map.keySet());
        Collections.sort(keys);

        // sort individual arrayLists
        for(int key : map.keySet()){
            ArrayList<Integer> list = map.get(key);
            Collections.sort(list);
        }

        // combine all lists
        int k =0;;
        for(int key : map.keySet()){
            ArrayList<Integer> list = map.get(key);
            for(int i = 0; i<list.size() ; i++){
                ans[k] = list.get(i);
                k++;
            }
        }

        return ans;
    }
}
```
Optimised code : 
```java
import java.util.*;

class Solution {
    public int[] sortByBits(int[] arr) {
        Integer[] temp = new Integer[arr.length];
        
        for(int i = 0; i < arr.length; i++) {
            temp[i] = arr[i];
        }

        Arrays.sort(temp, (a, b) -> {
            int countA = Integer.bitCount(a);
            int countB = Integer.bitCount(b);

            if(countA == countB) {
                return a - b;
            }
            return countA - countB;
        });

        for(int i = 0; i < arr.length; i++) {
            arr[i] = temp[i];
        }

        return arr;
    }
}
```

[118. Pascal's Triangle](https://leetcode.com/problems/pascals-triangle/)
#easy #array #dp 
```java
class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> ans = new ArrayList<>();

        for(int i = 0; i<numRows; i++){
            ans.add(new ArrayList<Integer>());
            for(int j = 0; j<i+1; j++){
                ans.get(i).add(1);
            }
        }

        for(int i = 2; i<numRows; i++){
            for(int j = 1; j < i; j++){
                int top = ans.get(i-1).get(j-1);
                int left = ans.get(i-1).get(j);
                ans.get(i).set(j, top+left);
            }
        }

        return ans;
    }
}
```


[3477. Fruits Into Baskets II](https://leetcode.com/problems/fruits-into-baskets-ii/)
```java
class Solution {
    public int numOfUnplacedFruits(int[] fruits, int[] baskets) {
        int unplaced = 0;
        boolean unp = true;
        for(int i = 0; i<fruits.length; i++){
            for(int j = 0; j< baskets.length; j++){
                if(baskets[j] >= fruits[i]){
                    baskets[j] = 0;
                    unp = false;
                    break;
                }
            }
            if(unp){
                unplaced++;
            }
            unp = true;
        }
        return unplaced;
    }
}
```


[2348. Number of Zero-Filled Subarrays](https://leetcode.com/problems/number-of-zero-filled-subarrays/)
```java
class Solution {
    public long zeroFilledSubarray(int[] nums) {
        long total = 0;
        long zeroCount = 0;
        for(int i = 0; i<nums.length+1; i++){
            if(i < nums.length && nums[i] == 0){
                zeroCount++;
            }
            else{
                if(zeroCount > 0){
                    total += calculateCombinations(zeroCount);
                    zeroCount = 0;
                }
            }
        }
        return total;
    }

    public long calculateCombinations(long count){
        return count * (count + 1)/2;
    }
}
```


[2154. Keep Multiplying Found Values by Two](https://leetcode.com/problems/keep-multiplying-found-values-by-two/)
```java
class Solution {
    public int findFinalValue(int[] nums, int original) {
        Set<Integer> numSet = new HashSet<>();
        for (int num : nums) {
            numSet.add(num);
        }
        
        while (numSet.contains(original)) {
            original *= 2;
        }
        
        return original;
    }
}
```


[1200. Minimum Absolute Difference](https://leetcode.com/problems/minimum-absolute-difference/)
```java
class Solution {
    public List<List<Integer>> minimumAbsDifference(int[] arr) {
        Arrays.sort(arr);
        int min = Integer.MAX_VALUE;
        for(int i = 1 ; i < arr.length;i++){
            if(arr[i] - arr[i-1] < min){
                min = arr[i] - arr[i-1];
            }
        }

        List<List<Integer>> ans = new ArrayList<>();

        for(int i = 1; i< arr.length ;i++){
            if(arr[i] - arr[i-1] == min){
                ArrayList<Integer> temp = new ArrayList<>();
                temp.add(arr[i-1]);
                temp.add(arr[i]);
                ans.add(temp);
            }
        }
        return ans;
    }
}
```

[3010. Divide an Array Into Subarrays With Minimum Cost I](https://leetcode.com/problems/divide-an-array-into-subarrays-with-minimum-cost-i/)
```java
class Solution {
    public int minimumCost(int[] nums) {
        int first = Integer.MAX_VALUE;
        int second = Integer.MAX_VALUE;

        for (int i = 1; i < nums.length; i++) {
            int x = nums[i];
            if (x < first) {
                second = first;
                first = x;
            } else if (x < second) {
                second = x;
            }
        }
        
        return nums[0] + first + second;
    }
}
```

[3637. Trionic Array I](https://leetcode.com/problems/trionic-array-i/)
```java
class Solution {
    public boolean isTrionic(int[] nums) {
        int i =1;
        while(i < nums.length && nums[i] > nums[i-1]){
            i++;
        }
        if(i == 1 || i == nums.length) return false;
        while(i < nums.length && nums[i] < nums[i-1]){
            i++;
        }
        if(i == nums.length) return false;
        while(i < nums.length && nums[i] > nums[i-1]){
            i++;
        }
        if(i == nums.length){
            return true;
        }
        return false;
    }
}
```

[3660. Jump Game IX](https://leetcode.com/problems/jump-game-ix/)
#array #dp #prefixsum 
```java
class Solution {
    public int[] maxValue(int[] nums) {
        int n = nums.length;

        int[] maxPrefix = new int[n];
        int[] minSuffix = new int[n];

        maxPrefix[0] = nums[0];
        minSuffix[n-1] = nums[n-1];

        for(int i = 1; i < n; i++){
            maxPrefix[i] = Math.max(nums[i], maxPrefix[i-1]);
        }

        for(int i = n -2; i >= 0 ; i--){
            minSuffix[i] = Math.min(nums[i], minSuffix[i+1]);
        }

        int ans[] = new int[n];
        ans[n-1] = maxPrefix[n-1];

        for(int i = n -2; i >= 0 ; i--){
            if(maxPrefix[i] > minSuffix[i+1]){
                ans[i] = ans[i+1];
            }else{
                ans[i] = maxPrefix[i];
            }
        }

        return ans;
    }
}
```


### 2D Arrays / Matrix Questions

[3643. Flip Square Submatrix Vertically](https://leetcode.com/problems/flip-square-submatrix-vertically/)
#matrix #twopointer #easy 
```java
class Solution {
    public int[][] reverseSubmatrix(int[][] grid, int x, int y, int k) {
        int top = x;
        int bottom = x + k -1;

        while(top < bottom){
            for(int i = y; i < y + k; i++){
                int temp = grid[top][i];
                grid[top][i] = grid[bottom][i];
                grid[bottom][i] = temp;
            }
            top++;
            bottom--;
        }

        return grid;
    }
}
```


[1582. Special Positions in a Binary Matrix](https://leetcode.com/problems/special-positions-in-a-binary-matrix/)
#matrix #easy 
```java
class Solution {
    public int numSpecial(int[][] mat) {
        int ans = 0;
        boolean isValid = false;
        for(int i = 0; i< mat.length; i++){
            isValid = false;
            for(int j = 0; j < mat[0].length; j++){
                if(mat[i][j] == 1){
                    mat[i][j] = 0;
                    if(check(mat, i, j)){
                        ans++;
                        isValid = true;
                    }
                    mat[i][j] = 1;
                }
            }
            if(isValid){continue;}
        }
        return ans;
    }
    public boolean check(int[][] mat, int row, int col){
        for(int i =0; i<mat.length; i++){
            if(mat[i][col] == 1){
                return false;
            }
        }
        for(int j = 0; j< mat[0].length; j++){
            if(mat[row][j] == 1){
                return false;
            }
        }
        return true;
    }
}
```

[2946. Matrix Similarity After Cyclic Shifts](https://leetcode.com/problems/matrix-similarity-after-cyclic-shifts/)
#easy #matrix #twopointer 
```java
class Solution {
    public boolean areSimilar(int[][] mat, int k) {

        int n = mat[0].length;

        int evenStart = ((-k % n) + n) % n;
        int oddStart = (k % n);

        for(int i = 0; i<mat.length; i++){
            int even = evenStart;
            int odd = oddStart;
            for(int j = 0; j < n; j++){
                if(i%2 == 0){
                    if(mat[i][j] != mat[i][even % n]){
                        return false;
                    } 
                    even++;
                }else{
                    if(mat[i][j] != mat[i][odd % n]){
                        return false;
                    } 
                    odd++;
                }
            }
        }
        return true;
    }
}
```

[2033. Minimum Operations to Make a Uni-Value Grid](https://leetcode.com/problems/minimum-operations-to-make-a-uni-value-grid/)
#medium #array #matrix #sorting 
```java
class Solution {
    public int minOperations(int[][] grid, int x) {
        int[] arr = new int[grid.length * grid[0].length];
        int k =0;

        for(int i =0 ; i< grid.length; i++){
            for(int j = 0; j < grid[0].length; j++){
                arr[k] = grid[i][j];
                k++;
            }
        }

        Arrays.sort(arr);

        int medianIndex = arr.length / 2;
        int sum = 0;
        
        for(int i =0 ; i<arr.length; i++){
            int temp = Math.abs(arr[medianIndex] - arr[i]);
            if(temp % x != 0){
                return -1;
            }
            sum += temp / x;
        }

        return sum;
    }
}

```

[74. Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)
#medium #array #matrix #binarysearch 
```java
public class LeetCode {
	public static boolean searchMatrix(int[][] matrix, int target){
		int row = 0;
		int col = matrix[0].length -1;
		while(row < matrix.length && col >= 0 ){
			if(matrix[row][col] == target){
				return true;
			}
			else if(matrix[row][col] > target){
				col--;
			}
			else{
				row++;
			}
		}
		return false;
	}
}
```

Max Array in Array
```java
public class MaxArrayInArray {
    public static void main(String[] args){
        int[][] arr = {
                {9,9,8,1},
                {5,6,2,6},
                {8,2,6,4},
                {6,2,2,2}};
        int[][] ans = largestLocal(arr);
        for(int i = 0; i<ans.length; i++){
            for(int j = 0; j < ans.length; j++){
                System.out.print(ans[i][j]);
            }
            System.out.println();
        }
    }
    public static int[][] largestLocal(int[][] grid) {
        int m = grid.length;
        int x = 0;
        int y = 0;
        int[][] arr = new int[m-2][m-2];
        for(int i = 0; i <= m-3; i++){ // row till-1 for index -2 for size n-2
            y = 0;
            for(int j = 0;j <= m-3; j++){ // col
                arr[x][y] = findMax(grid, i,i+2, j, j+2);
                y++;
            }
            x++;
        }
        return arr;
    }
    public static int findMax(int[][] grid, int srow,int erow, int scol, int ecol ){
        int max = 0;
        for(int i = srow; i<= erow; i++){
            for(int j = scol; j <= ecol; j++){
                if(max < grid[i][j]){
                    max = grid[i][j];
                }
            }
        }
        return max;
    }
}

```


[1536. Minimum Swaps to Arrange a Binary Grid](https://leetcode.com/problems/minimum-swaps-to-arrange-a-binary-grid/)
#array #matrix #greedy #medium 
```java
class Solution {
    public int minSwaps(int[][] grid) {
        int[] zeroCount = new int[grid.length];

        for(int i = 0; i< grid.length; i++){
            int j = grid[0].length - 1;
            int count = 0;
            while(j >= 0 && grid[i][j] == 0){
                count++;
                j--;
            }
            zeroCount[i] = count;
        }

        
        int swapCount = 0;

        for(int i = 0; i< grid.length; i++){
            int required = grid.length - i -1;

            if(zeroCount[i] >= required){
                continue;
            }

            int j = i+1;
            while(j < grid.length && zeroCount[j] < required){
                j++;
            }

            if(j == grid.length) {
                return -1;
            }

            // swap
            while( j > i){
                int temp = zeroCount[j];
                zeroCount[j] = zeroCount[ j - 1];
                zeroCount[j - 1] = temp;
                j--;
                swapCount++;
            }
        }

        return swapCount;

    }
}
```

[3567. Minimum Absolute Difference in Sliding Submatrix](https://leetcode.com/problems/minimum-absolute-difference-in-sliding-submatrix/)
#medium #matrix #sorting 
```java
class Solution {
    public int[][] minAbsDiff(int[][] grid, int k) {
        int m = grid.length;
        int n = grid[0].length;

        int[][] answer = new int[m-k+1][n-k+1];

        if(k == 1){
                    return answer;
                }

        for(int i = 0; i <= m - k; i++){
            for(int j = 0; j<= n- k; j++){

                

                ArrayList<Integer> list = new ArrayList<>();

                // bruteforce
                for(int x = i; x < i + k; x++){
                    for(int y = j; y < j + k; y++){
                        list.add(grid[x][y]);
                    }
                }
                Collections.sort(list);

                int minDiff = Integer.MAX_VALUE;

                for(int temp = 1; temp < list.size(); temp++){

                    if(!list.get(temp).equals(list.get(temp - 1))){
                        minDiff = Math.min(minDiff, Math.abs(list.get(temp) - list.get(temp - 1)));
                    }
                }
                if(minDiff == Integer.MAX_VALUE)
                    minDiff = 0;

                answer[i][j] = minDiff;
                
            }
        }
        return answer;
    }
}
```

[1861. Rotating the Box](https://leetcode.com/problems/rotating-the-box/)
#medium #matrix #twopointer 
we first set the values in the original matrix
then copy them to the result rotated matrix
```java
class Solution {
    public char[][] rotateTheBox(char[][] boxGrid) {
        int m = boxGrid.length;
        int n = boxGrid[0].length;

        for(int i = 0; i< m; i++){
            int j = 0;
            while(j < n){
                int rockCount = 0;
                while(j < n && boxGrid[i][j] != '*'){
                    if(boxGrid[i][j] == '#'){
                        rockCount++;
                        boxGrid[i][j] = '.';
                    }
                    j++;
                }

                int left = j - rockCount;
                while(left < j){
                    boxGrid[i][left] = '#';
                    left++;
                }
                j++;
            }
        }

        char[][] ans = new char[n][m]; 
        int row = m-1;
        int col = 0;

        for(int i = 0; i< n; i++){
            row = m-1;
            for(int j = 0; j < m; j++){
                ans[i][j] = boxGrid[row][col];
                row--;
            }
            col++;
        }


        return ans;
    }
}
```

[1886. Determine Whether Matrix Can Be Obtained By Rotation](https://leetcode.com/problems/determine-whether-matrix-can-be-obtained-by-rotation/)
#easy #matrix #array 
```java
class Solution {
    public boolean findRotation(int[][] mat, int[][] target) {
        int n = mat.length - 1;
        boolean orientation = true; // true -> hori / false- > vert
        // start i, start j, end i, end j, i change, j change
        return traverse(0, 0, 1, 1,true, mat, target) || 
        traverse(0, n, 1,-1, false, mat, target) ||
        traverse(n, n, -1, -1, true, mat, target) ||
        traverse(n, 0, -1, 1, false, mat, target);
    }

    public boolean traverse(int starti, int startj, int ichange, int jchange, boolean orientation, int[][] mat, int[][] target){
        int tempI = starti;
        int tempJ = startj;
        if(orientation){ // move horizontally
            for(int i = 0; i<mat.length; i++){
                startj = tempJ;
                for(int j = 0; j < mat.length; j++){
                    if(mat[i][j] != target[starti][startj]){
                        return false;
                    }
                    startj += jchange;
                }
                starti += ichange; 
            }
        }else{
            for(int i = 0; i < mat.length; i++){
                starti = tempI;
                for(int j = 0; j< mat.length; j++){
                    if(mat[i][j] != target[starti][startj]){
                        return false;
                    }
                    starti += ichange;
                }
                startj += jchange;
            }
        }
        return true;
    }
}
```

[36. Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)
```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        int i =0;
        int j = 0;

        HashSet<Character> set = new HashSet<>();

        // check rows
        for(i = 0; i< 9; i++){
            for(j = 0; j < 9; j++){
                if(board[i][j] != '.'){
                    if(set.contains(board[i][j])){
                        return false;
                    }
                    set.add(board[i][j]);
                }
            }
            set.clear();
        }


        // check columns
        for(i = 0; i<9; i++){
            for(j = 0; j< 9; j++){
                if(board[j][i] != '.'){
                    if(set.contains(board[j][i])){
                        return false;
                    }
                    set.add(board[j][i]);
                }
            }
            set.clear();
        }

        i = 0;
        j = 0;

        


        // check grids
        for(i =0; i<9; i+=3){
            for(j = 0; j <9;j+=3){
                boolean check = checkGrid(board, i, j, set);
                if(check == false){
                    return false;
                }
                set.clear();
            }
        }

        return true;
    }

    public boolean checkGrid(char[][] board, int r, int c, HashSet<Character> set){
        for(int i = r; i<r+3; i++){
            for(int j = c; j< c+3; j++){
                if(board[i][j] != '.'){
                    if(set.contains(board[i][j])){
                        return false;
                    }
                    set.add(board[i][j]);
                }
            }
        }
        return true;
    }
}
```


[48. Rotate Image](https://leetcode.com/problems/rotate-image/)
#medium #maths #array 
logic : transpose and reversing the matrix also simulates the roatation
```java
class Solution {
    public void rotate(int[][] matrix) {
        // transpose the matrix
        for(int i =0; i <matrix.length; i++){
            for(int j =0; j< matrix[0].length; j++){
                if(j > i){
                    int temp = matrix[i][j];
                    matrix[i][j] = matrix[j][i];
                    matrix[j][i] = temp;
                }
            }
        }

        // reverse rows 
        for(int i =0 ; i < matrix.length; i++){
            for(int j = 0; j < matrix.length / 2; j++){
                int temp = matrix[i][j];
                matrix[i][j] = matrix[i][matrix[i].length - 1 - j];
                matrix[i][matrix[i].length- 1 - j] = temp;
            }
        }
    }
}
```


[2326. Spiral Matrix IV](https://leetcode.com/problems/spiral-matrix-iv/)
```java

/*
Input: m = 3, n = 5, head = [3,0,2,6,8,1,7,9,4,2,5,5,0]
Output: [[3,0,2,6,8],[5,0,-1,-1,1],[5,2,4,9,7]]
*/

class Solution {
    public int[][] spiralMatrix(int m, int n, ListNode head) {
        int[][] ans = new int[m][n];

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                ans[i][j] = -1;
            }
        }

        int topRow = 0;
        int leftCol = 0;
        int rightCol = n - 1;
        int bottomRow = m - 1;

        int row = 0;
        int col = 0;

        int rowSum = 0;
        int colSum = 1;

        while (head != null) {
            if (colSum == 1) {
                while (head != null && col != rightCol) {
                    ans[row][col] = head.val;
                    head = head.next;

                    col++;
                }

                topRow++;
                rowSum = 1;
                colSum = 0;

            }
            if (rowSum == 1) {
                while (head != null &&row != bottomRow) {
                    ans[row][col] = head.val;
                    head = head.next;

                    row++;
                }

                rightCol--;
                rowSum = 0;
                colSum = -1;

            }
            if (colSum == -1) {
                while (head != null &&col != leftCol) {
                    ans[row][col] = head.val;
                    head = head.next;

                    col--;
                }
                bottomRow--;
                rowSum = -1;
                colSum = 0;

            }
            if (rowSum == -1) {
                while (head != null &&row != topRow) {
                    ans[row][col] = head.val;
                    head = head.next;
                    row--;
                }
                leftCol++;
                colSum = 1;
                rowSum = 0;

            }
        }
        return ans;
    }
}
```


[2373. Largest Local Values in a Matrix](https://leetcode.com/problems/largest-local-values-in-a-matrix/)
```java
class Solution {
    public int[][] largestLocal(int[][] grid) {
        int m = grid.length;
        int x = 0;
        int y = 0;
        int[][] arr = new int[m-2][m-2];
        for(int i = 0; i <= m-3; i++){ // row till-1 for index -2 for size n-2
            y = 0;
            for(int j = 0;j <= m-3; j++){ // col
                arr[x][y] = findMax(grid, i,i+2, j, j+2);
                y++;
            }
            x++;
        }
        return arr;
    }
    public int findMax(int[][] grid, int srow,int erow, int scol, int ecol ){
        int max = 0;
        for(int i = srow; i<= erow; i++){
            for(int j = scol; j <= ecol; j++){
                if(max < grid[i][j]){
                    max = grid[i][j];
                }
            }
        }
        return max;
    }
}
```

[3417. Zigzag Grid Traversal With Skip](https://leetcode.com/problems/zigzag-grid-traversal-with-skip/)
```java
class Solution {
    public List<Integer> zigzagTraversal(int[][] grid) {
        ArrayList<Integer> list = new ArrayList<>();
        for(int i = 0 ;i<grid.length ;i++){
            if(i%2 == 0){
                helper(grid,i, 0, grid[0].length , true, list);
            }
            else{
                helper(grid,i, grid[0].length -1, -1, false, list);
            }
        }
        return list;
    }

    public void helper(int[][] grid,int i, int start, int end, boolean left, ArrayList<Integer> list){ // left -> left to right
        for(int j = start;; ){
            if((i+j)%2 == 0){
                list.add(grid[i][j]);
            }
            if(left){
                j++;
                if(j >= end){
                    return;
                }
            }else{
                j--;
                if(j <= end){
                    return;
                }
            }
        }
    }
}
```

[1594. Maximum Non Negative Product in a Matrix](https://leetcode.com/problems/maximum-non-negative-product-in-a-matrix/)
#medium #matrix #dp #greedy 
```java
class Solution {
    public int maxProductPath(int[][] grid) {
        long[][] maxDP = new long[grid.length][grid[0].length];
        long[][] minDP = new long[grid.length][grid[0].length];
        maxDP[0][0] = grid[0][0];
        minDP[0][0] = grid[0][0];

        for(int i =0; i < grid.length; i++){
            for(int j = 0; j < grid[0].length ; j++){
                if(i == 0 && j == 0) continue;
                long tMax = Integer.MIN_VALUE;
                long tMin = Integer.MAX_VALUE;
                if(i > 0){
                    tMax = Math.max(tMax, maxDP[i-1][j] * grid[i][j]);
                    tMax = Math.max(tMax, minDP[i-1][j] * grid[i][j]);
                    
                    tMin = Math.min(tMin, maxDP[i-1][j] * grid[i][j]);
                    tMin = Math.min(tMin, minDP[i-1][j] * grid[i][j]);
                }if(j > 0){
                    tMax = Math.max(tMax, maxDP[i][j-1] * grid[i][j]);
                    tMax = Math.max(tMax, minDP[i][j-1] * grid[i][j]);
                    
                    tMin = Math.min(tMin, maxDP[i][j-1] * grid[i][j]);
                    tMin = Math.min(tMin, minDP[i][j-1] * grid[i][j]);
                }

                maxDP[i][j] = tMax;
                minDP[i][j]= tMin;
            }
        }

        long ans = maxDP[grid.length-1][grid[0].length - 1];
        if(ans < 0){
            return -1;
        }
        return (int)(ans % 1000000007);
    }
}
```