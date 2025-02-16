---
lastSync: Mon Oct 14 2024 21:22:32 GMT+0530 (India Standard Time)
---
[2390. Removing Stars From a String](https://leetcode.com/problems/removing-stars-from-a-string/)
```java
class Solution {
    public String removeStars(String s) {
        StringBuilder str = new StringBuilder();

        if(s.length() == 0){
            return "";
        }

        Stack<Character> st = new Stack<>();
        for(int i = 0; i<s.length(); i++){
            st.push(s.charAt(i));
        }
        int stCount = 0;

        while(!st.isEmpty()){
            if(st.peek() == '*'){
                stCount++;
                st.pop();
            }
            else{
                if(stCount != 0){
                    st.pop();
                    stCount--;
                }
                else{
                    str.append(st.pop());
                }
            }
        }

        return str.reverse().toString();
    }
}
```

 [735. Asteroid Collision](https://leetcode.com/problems/asteroid-collision/) 
#todo
```java
class Solution {
    public int[] asteroidCollision(int[] asteroids) {
        Stack<Integer> st = new Stack();
        for(int i = 0; i<asteroids.length; i++){
            if(st.isEmpty() || asteroids[i] > 0){
                st.push(asteroids[i]);
            }
            else{
                while(true){
                    int peek = st.peek();
                    if(peek <0){
                        st.push(asteroids[i]);
                        break;
                    }
                    else if(peek == -asteroids[i]){
                        st.pop();
                        break;
                    }
                    else if(peek > -asteroids[i]){
                        break;
                    }
                    else{
                        st.pop();
                        if(st.isEmpty()){
                            st.push(asteroids[i]);
                            break;
                        }
                    }
                }
            }
        }

        int[] ans = new int[st.size()];
        for(int i = st.size()-1 ; i>=0; i--){
            ans[i] = st.pop();
        }
        return ans;
    }
}
```

[394. Decode String](https://leetcode.com/problems/decode-string/)
```java
class Solution {
    public String decodeString(String s) {
        Stack<Integer> st = new Stack<>();
        Stack<StringBuilder> st1 = new Stack<>();
        StringBuilder sb = new StringBuilder();
        int n = 0;

        for(char c : s.toCharArray()){
            if(Character.isDigit(c)){
                n = n* 10+(c-'0');
            }
            else if(c == '['){
                st.push(n);
                n = 0;
                st1.push(sb);
                sb = new StringBuilder();
            }
            else if(c == ']'){
                int k = st.pop();
                StringBuilder temp = sb;
                sb = st1.pop();
                while(k > 0){
                    sb.append(temp);
                    k--;
                }
            }
            else{
                sb.append(c);
            }
        }

        return sb.toString();
    }
}
```