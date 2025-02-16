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