---
lastSync: Tue Oct 15 2024 20:00:22 GMT+0530 (India Standard Time)
---
[933. Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/) (No idea what the question wants us to do)
```java
class RecentCounter {
    Queue<Integer> queue;
    public RecentCounter() {
        queue = new LinkedList();
    }

    public int ping(int t) {
        queue.add(t);
        while(queue.peek() <t - 3000){
            queue.poll();
        }
        return queue.size();
    }
}
```

[649. Dota2 Senate](https://leetcode.com/problems/dota2-senate/)
```java
class Solution {
    public String predictPartyVictory(String senate) {
        int n = senate.length();
        int i = 0;
        int j = 0;

        Queue<Integer> R = new LinkedList();        
        Queue<Integer> D = new LinkedList();
        for(int k = 0; k<n; k++){
            if (senate.charAt(k) == 'D'){
                D.add(k);
            }
            else{
                R.add(k);
            }
        }

        while(!D.isEmpty() && !R.isEmpty()){

            int t1 = R.remove();
            int t2 = D.remove();

            if(t1 > t2){
                D.add(n++);
            }
            else{
                R.add(n++);
            }
            i++;
            j++;
        }

        if(R.isEmpty()){
            return "Dire";
        }
        else{
            return "Radiant";
        }
    }
}
```