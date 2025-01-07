[374. Guess Number Higher or Lower](https://leetcode.com/problems/guess-number-higher-or-lower/)
```java
public class Solution extends GuessGame {
    public int guessNumber(int n) {
        int l = 0;
        int r = n;
        while(l <= r){
            int mid = l + (r-l)/2; 
            int y = guess(mid);
            if(y == 0){
                return mid;
            }  
            else if(y == 1){
                l = mid + 1;
            }
            else{
                r = mid - 1;
            }
        }
        return -1;
    }
}
```

[2300. Successful Pairs of Spells and Potions](https://leetcode.com/problems/successful-pairs-of-spells-and-potions/)
```java
class Solution {
    public int[] successfulPairs(int[] spells, int[] potions, long success) {
        Arrays.sort(potions);
        int[] ans = new int[spells.length];

        for(int i = 0 ; i < spells.length; i++){
            int s = 0; 
            int e = potions.length-1;
            while(s <= e){
                int mid = s + (e-s)/2;
                if((long)potions[mid] * (long)spells[i] >= success){
                    e = mid-1;
                }
                else{
                    s = mid + 1;
                }
            }
            ans[i] = potions.length - s;
        }

        return ans;
    }
}
```

[162. Find Peak Element](https://leetcode.com/problems/find-peak-element/)
```java
class Solution {
    public int findPeakElement(int[] nums) {
        int s = 0;
        int e = nums.length - 1;

        while(s < e){
            int mid = s + (e-s)/2;
            if(mid-1>-1 && nums[mid-1] > nums[mid]){
                e = mid - 1;
            }
            else if(mid + 1 < nums.length && nums[mid + 1] > nums[mid]){
                s = mid + 1;
            }
            else {
                return mid;
            }
        }
        return s;
    }
}
```

[875. Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)
```java
class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        int start = 1;
        int end = piles[0];
        for(int i = 0; i<piles.length; i++){
            if(piles[i] > end){
                end = piles[i];
            }
        }
        int ans = end;

        while(start < end){
            int mid = start + (end-start)/2;
            if(isValid(mid, piles, h)){
                end = mid;
            }
            else{
                start = mid+1;
            }
        }

        return start;
    }

    public boolean isValid(int mid, int[] piles, int h){
        int sum = 0;
        for(int i = 0; i<piles.length; i++){
            sum += (piles[i] + mid -1)/mid;

            if(sum > h){
                return false;
            }
        }
       return true;
    }
}
```