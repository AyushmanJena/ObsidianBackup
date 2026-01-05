[62. Unique Paths](https://leetcode.com/problems/unique-paths/)
```java
class Solution {
    public int uniquePaths(int m, int n) {
        // going from top to bottom for easier traversal
        int[][] dp = new int[m][n];
        for(int i =0; i<m; i++){
            for(int j = 0; j<n; j++){
                if(i == 0 || j == 0){
                    dp[i][j] = 1;
                }
                else{
                    dp[i][j] = dp[i-1][j] + dp[i][j-1];
                }
            }
        }
        return dp[m-1][n-1];
    }
}
```

[1143. Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)
```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int[][] dp = new int[text1.length()][text2.length()];
        for(int i = 0; i<text1.length(); i++){
            for(int j = 0; j < text2.length(); j++){
                if(text1.charAt(i) == text2.charAt(j)){
                    if(i > 0 && j > 0){
                        dp[i][j] = dp[i-1][j-1] + 1;
                    }else{
                        dp[i][j] = 1;
                    }
                }
                else{
                    if(i <= 0 && j<= 0){
                            dp[i][j] = 0;
                        }
                    else if(i <=0 ){
                        dp[i][j] = dp[i][j-1];
                    }else if(j <= 0){
                        dp[i][j] = dp[i-1][j];
                    
                        
                    }else{
                        dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
                    }
                }
            }
        }
        return dp[text1.length()-1][text2.length()-1];
    }
}
```

