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

[714. Best Time to Buy and Sell Stock with Transaction Fee](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)
#revise 
```java
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int cash = 0;
        int hold = -prices[0];

        for(int i =1; i< prices.length; i++){
            cash = Math.max(cash, hold+prices[i] - fee);
            hold = Math.max(hold, cash - prices[i]);
        }

        return cash;
    }
}
```

[72. Edit Distance](https://leetcode.com/problems/edit-distance/)
![[Pasted image 20260107121304.png]]

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length();
        int n = word2.length();

        int[][] dp = new int[m + 1][n + 1];

        // setup the base case values
        for(int i = 0, j = m; i<= m; i++){
            dp[i][n] = j;
            j--;
        }
        for(int i = n, j = 0; j<= n; j++){
            dp[m][j]= i;
            i--;
        }        

        //actual magic of bottom up dp
        for(int i = m-1; i >= 0; i--){
            for(int j = n-1; j >= 0; j--){
                
                if(word1.charAt(i) != word2.charAt(j)){
                    dp[i][j] = 1 + Math.min(dp[i+1][j+1], Math.min(dp[i][j+1], dp[i+1][j]));
                    
                }else{
                    dp[i][j] = dp[i+1][j+1];
                }
            }
        }
        return dp[0][0];
    }
}
```