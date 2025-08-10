
[1717. Maximum Score From Removing Substrings](https://leetcode.com/problems/maximum-score-from-removing-substrings/)
Uses Stacks and Greedy Algorithm
```java
class Solution {
    public int maximumGain(String s, int x, int y) {
        if(x > y){
            return calculate(s, 'a', 'b', x, y);
        }else{
            return calculate(s, 'b', 'a', y, x);
        }
    }

    public int calculate(String s, char first, char second, int firstValue, int secondValue){
        Stack<Character> stack = new Stack<>();
        int score = 0;

        for(char c : s.toCharArray()){
            if(!stack.isEmpty() && stack.peek() == first && c == second){
                stack.pop();
                score += firstValue;
            }
            else{
                stack.push(c);
            }
        }

        Stack<Character> stack2 = new Stack<>();
        while(!stack.isEmpty()){
            char c = stack.pop();
            if(!stack2.isEmpty() && c == second  && stack2.peek() == first){
                stack2.pop();
                score += secondValue;
            }
            else{
                stack2.push(c);
            }
        }

        return score;
    }
}
```