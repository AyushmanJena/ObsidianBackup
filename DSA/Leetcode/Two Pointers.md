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

