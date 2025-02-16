[435. Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)
```java
class Solution {
    public int eraseOverlapIntervals(int[][] intervals) {
        Arrays.sort(intervals,(a,b) -> Integer.compare(a[1], b[1]));
        int count = 1;
        int endTime = intervals[0][1];
        
        for(int i = 1; i<intervals.length; i++){
            if(intervals[i][0] >= endTime){
                endTime = intervals[i][1];
                count++;
            }
        }
        
        return intervals.length-count;
    }
}
```
(refer striver greedy algorithms for more)

[452. Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)
```java
class Solution {
    public int findMinArrowShots(int[][] points) {
        Arrays.sort(points, (a,b)->Integer.compare(a[0], b[0])); 
        // sort based on starting point 

        // take initial ans = n and decrease everytime there is an overlapping
        int count = points.length;
         // interval refers to the overlapping part
        int intervalRight = points[0][1];

        for(int i = 1; i< points.length; i++){
            if(points[i][0] <= intervalRight){ // if next element overlaps with interval 
                count--;
                intervalRight = Math.min(points[i][1], intervalRight);
            }
            else{ // if next element does not overlap with interval itself becomes the new interval
                intervalRight = points[i][1];
            }
        }
        return count;
    }
}
```