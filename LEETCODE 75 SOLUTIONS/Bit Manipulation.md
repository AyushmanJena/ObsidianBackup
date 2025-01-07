[338. Counting Bits](https://leetcode.com/problems/counting-bits/)
```java
class Solution {
    public int[] countBits(int n) {
        int[] ans = new int[n+1];
        int count = 0;
        for(int i = 0; i<=n; i++){
            int num = i;
            while(num != 0){
                count += num % 2;
                num = num >> 1;
            }
            ans[i] = count;
            count = 0;
        }
        return ans;
    }
}
```