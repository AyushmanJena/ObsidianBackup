[215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)
```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for(int i =0 ; i<k; i++){
            pq.add(nums[i]);
        }
        for(int i = k; i<nums.length; i++){
            if(pq.peek() < nums[i]){
                pq.remove();
                pq.add(nums[i]);
            }
        }
        return pq.peek();
    }
}
```

[2336. Smallest Number in Infinite Set](https://leetcode.com/problems/smallest-number-in-infinite-set/)
```java
class SmallestInfiniteSet {
    boolean[] arr; // false -> exists, true -> doesnot exist
    int smallest = 0;

    public SmallestInfiniteSet() {
        arr = new boolean[1000];
    }
    
    public int popSmallest() {
        for(int i = smallest; i<1000; i++){
            if(arr[i] != true){
                arr[i] = true;
                smallest = i+1;
                return i+1;
            }
        }
        return -1;
    }
    
    public void addBack(int num) {
        arr[num-1] = false;
        if(num < smallest+1){
            smallest = num-1;
        }
    }
}
```

[2542. Maximum Subsequence Score](https://leetcode.com/problems/maximum-subsequence-score/) #hard
```java
class Solution {
    public long maxScore(int[] nums1, int[] nums2, int k) {
        int n = nums1.length; 
        int[][] aug = new int[n][2]; 
        for (int i = 0; i < n; ++i) {
            aug[i][0] = nums1[i]; 
            aug[i][1] = nums2[i]; 
        }
        Arrays.sort(aug, new Comparator<int[]>(){
            public int compare(int[] lhs, int[] rhs) {
                return -Integer.compare(lhs[1], rhs[1]); 
            }
        }); 
        PriorityQueue<Integer> pq = new PriorityQueue(); 
        long ans = 0, total = 0; 
        for (int i = 0; i < aug.length; ++i) {
            total += aug[i][0]; 
            pq.add(aug[i][0]); 
            if (i >= k){
                total = total - pq.poll(); 
            }
            if (i >= k-1){
                ans = Math.max(ans, total * aug[i][1]); 
            } 
        }
        return ans; 
    }
}
```

[2462. Total Cost to Hire K Workers](https://leetcode.com/problems/total-cost-to-hire-k-workers/) #todo 
```java
class Solution {
    public long totalCost(int[] costs, int k, int candidates) {
        PriorityQueue<Integer> leftQueue = new PriorityQueue<>();
        PriorityQueue<Integer> rightQueue = new PriorityQueue<>();
        int n = costs.length;

        int l = 0, r = n - 1;
        for (int i = 0; i < candidates && l <= r; i++) {
            leftQueue.add(costs[l++]);
        }
        for (int i = 0; i < candidates && r >= l; i++) {
            rightQueue.add(costs[r--]);
        }

        long result = 0;

        while (k  > 0){
            k--;
            if (!leftQueue.isEmpty() && (rightQueue.isEmpty() || leftQueue.peek() <= rightQueue.peek())) {
                result += leftQueue.poll();
                if (l <= r) {
                    leftQueue.add(costs[l++]);
                }
            } else {
                result += rightQueue.poll();
                if (r >= l) {
                    rightQueue.add(costs[r--]);
                }
            }
        }
        return result;
    }
}
```