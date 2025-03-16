[88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)
```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int[] temp = new int[m];

        for(int i = 0 ; i<m; i++){
            temp[i] = nums1[i];
        }

        int i = 0;
        int j = 0;
        int k = 0;

        while(i < m && j < n){
            if(temp[i] < nums2[j]){
                nums1[k] = temp[i];
                i++;
            }else{
                nums1[k] = nums2[j];
                j++;
            }
            k++;
        }

        while(i < m){
            nums1[k] = temp[i];
            i++;
            k++;
        }

        while(j < n){
            nums1[k] = nums2[j];
            j++;
            k++;
        }
    }
}
```
## 27
## 26

[80. Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/)
```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int i = 0;
        int j = 0;

        while(i < nums.length){
            int count = 1;
            int currValue = nums[i];
            while(i < nums.length && currValue == nums[i]){
                if(count <= 2){
                    count++;
                    nums[j] = currValue;
                    j++;
                }
                i++;
            }
        }

        return j;
    }
}
```

## 169

[189. Rotate Array](https://leetcode.com/problems/rotate-array/)
```java
class Solution {
    public void rotate(int[] nums, int k) {
        int i = nums.length - (k % nums.length);
        int j = 0;
        int[] ans = new int[nums.length];

        while(i < nums.length){
            ans[j] = nums[i];
            i++;
            j++;
        }
        i = 0; 
        while(j < nums.length){
            ans[j] = nums[i];
            i++;
            j++;
        }

        for(int d = 0 ; d < nums.length; d++){
            nums[d] = ans[d];
        }
    }
}
```

[121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
```java
class Solution {
    public int maxProfit(int[] prices) {
        int minPrice = Integer.MAX_VALUE;
        int maxProfit = 0;

        for (int price : prices) {
            if (price < minPrice) {
                minPrice = price; 
            } else {
                maxProfit = Math.max(maxProfit, price - minPrice);
            }
        }
        
        return maxProfit;
    }
}

```

[122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)
```java
class Solution {
    public int maxProfit(int[] prices) {
        int profit = 0;
        for(int i =0 ;i< prices.length -1;i++){
            if(prices[i+1] > prices[i]){
                profit += prices[i+1] - prices[i];
            }
        }
        return profit;
    }
}
```

[55. Jump Game](https://leetcode.com/problems/jump-game/)
```java
class Solution {
    public boolean canJump(int[] nums) {
        int max = 0;
        for(int i =0;i<nums.length; i++){
            if(max >= nums.length-1){
                return true;
            }
            if(i == max && nums[max] == 0){
                return false;
            }
            max = Math.max(max, i+nums[i]);
        }
        return false;
    }
}
```

[45. Jump Game II](https://leetcode.com/problems/jump-game-ii/)
```java
class Solution {
    public int jump(int[] nums) {
        int ans = 0;
        int l = 0;
        int r = 0;
        while(r < nums.length -1){
            int max = 0;
            for(int i = l; i <= r; i++){
                max = Math.max(i + nums[i], max);
            }
            l = r+1;
            r = max;
            ans++;
        }
        return ans;
    }
}
```

[274. H-Index](https://leetcode.com/problems/h-index/)
```java
class Solution {
    public int hIndex(int[] citations) {
        // find max value in array
        int max = 0;
        for(int num : citations){
            if(max < num){
                max = num;
            }
        }

        int[] arr = new int[max+1];

        for(int num : citations){
            arr[num]++; 
        }

        // loop
        int sum = 0;
        for(int i = max; i>= 0; i--){
            sum = sum + arr[i];
            if(sum >= i){
                return i;
            }
        }
        return 0;
    }
}
```

[380. Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/) meh
```java
class RandomizedSet {
    HashMap<Integer, Integer> map;
    List<Integer> list;
    Random random = new Random();

    public RandomizedSet() {
        list = new ArrayList<>();
        map = new HashMap<Integer, Integer>();
    }
    
    public boolean insert(int val) {
        if(map.containsKey(val)){
            return false;
        }
        map.put(val, list.size());
        list.add(val);
        return true;
    }
    
    public boolean remove(int val) {
        if(!map.containsKey(val)){
            return false;
        }
        int pos = map.get(val);
        if(pos != (list.size() - 1)){
            int lastElement = list.get(list.size() - 1);
            list.set(pos, lastElement);
            map.put(lastElement, pos);
        }
        map.remove(val);
        list.remove(list.size() - 1);
        return true;
    }
    
    public int getRandom() {
        int randomInt = random.nextInt(list.size());
        return list.get(randomInt);
    }
}

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet obj = new RandomizedSet();
 * boolean param_1 = obj.insert(val);
 * boolean param_2 = obj.remove(val);
 * int param_3 = obj.getRandom();
 */
```

## 238

[134. Gas Station](https://leetcode.com/problems/gas-station/)
help : [yt](https://www.youtube.com/watch?v=3wUa7Lf1Xjk)
```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int sum = 0;
        int total = 0;
        int position = 0;

        for(int i = 0; i < gas.length; i++){
            sum += gas[i] - cost[i];
            if(sum < 0){
                total += sum;
                sum = 0;
                position = i + 1;
            }
        }
        total += sum;
        return total >= 0 ? position : -1;
    }
}
```

[135. Candy](https://leetcode.com/problems/candy/) #imp 
```java
class Solution {
    public int candy(int[] ratings) {
        int sum = 1;
        int i = 1;
        while(i < ratings.length){
            if(ratings[i] == ratings[i-1]){
                sum += 1;
                i++;
                continue;
            }

            int peak = 1;
            while(i < ratings.length && ratings[i] > ratings[i-1]){
                peak++;
                sum += peak;
                i++;
            }

            int down = 1;
            while(i < ratings.length && ratings[i] < ratings[i-1]){
                sum += down;
                down++;
                i++;
            }

            if(down > peak){
                sum = sum - peak + down;
            }
        }
        return sum;
    }
}
```

[42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)
```java
class Solution {
    public int trap(int[] height) {
        int[] left = new int[height.length];
        int[] right = new int[height.length];

        int leftGreatest = height[0];
        left[0] = 0;
        for(int i = 1; i<height.length; i++){
            left[i] = leftGreatest;
            if(height[i] > leftGreatest){
                leftGreatest = height[i];
            }
        }

        int rightGreatest = height[height.length-1];
        right[height.length-1] = 0;
        for(int i = height.length-2; i >= 0; i--){
            right[i] = rightGreatest;
            if(height[i] > rightGreatest){
                rightGreatest = height[i];
            }
        }

        int sum = 0;
        for(int i= 0; i<height.length; i++){
            int temp = Math.min(left[i], right[i]) - height[i];
            if(temp > 0){
                sum += temp;
            }
        }

        return sum;
    }
}
```

## 13
## 12

[58. Length of Last Word](https://leetcode.com/problems/length-of-last-word/)
```java
class Solution {
    public int lengthOfLastWord(String s) {
        int i = s.length() -1;
        int size = 0;

        while(s.charAt(i) == ' '){
            i--;
        }

        while(i >= 0){
            if(s.charAt(i) == ' '){
                return size;
            }
            size++;
            i--;
        }
        return size;
    }
}
```

[14. Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)
```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        int ans = strs[0].length(); // ans can be max as the first word
        char[] arr = new char[strs[0].length()];
        for(int i = 0 ; i < strs[0].length(); i++){
            arr[i] = strs[0].charAt(i);
        }

        for(int i = 1; i<strs.length; i++){
            for(int j = 0;j <= strs[i].length() && j < ans ; j++){
                if(j == strs[i].length() || strs[i].charAt(j) != arr[j]){
                    ans = j; // replace ans if curr char not match or ends there
                }
            }
        }
        return strs[0].substring(0, ans);
    }
}
```

## 151
## 6

[28. Find the Index of the First Occurrence in a String](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/)
```java
class Solution {
    public int strStr(String haystack, String needle) {
        int i = 0;
        if(needle.length() > haystack.length()){
            return -1;
        }
        while(i < haystack.length()){
            int j = 0;
            int temp = i;
            while(i < haystack.length() && j < needle.length() && haystack.charAt(i) == needle.charAt(j)){
                if(j == needle.length() -1){
                    return temp;
                }
                i++;
                j++;
            }
            i = temp + 1;
        }
        return -1;
    }
}
```

