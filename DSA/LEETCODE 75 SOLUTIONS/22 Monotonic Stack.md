[739. Daily Temperatures](https://leetcode.com/problems/daily-temperatures/) #todo 
```java
class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        int[] ans = new int[temperatures.length];
        Stack<Integer> st = new Stack<>();

        for(int i = 0; i<temperatures.length; i++){
            while(!st.isEmpty() && temperatures[st.peek()] < temperatures[i]){
                ans[st.peek()] = i - st.pop();
            }
            st.push(i);
        }
        return ans;
    }
}
```

[901. Online Stock Span](https://leetcode.com/problems/online-stock-span/) #todo 
```java
class StockSpanner {

    Stack<int[]> st;

    public StockSpanner() {
        st = new Stack<>();
    }
    
    public int next(int price) {
        int ans = 1;
        while(!st.isEmpty() && st.peek()[0] <= price){
            ans += st.pop()[1];
        }
        st.push(new int[]{price, ans});
        return ans;
    }
}
```