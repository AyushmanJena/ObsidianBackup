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