##### by STRIVER

> [!check]
> Green Diary : 23rd July

### 1. SJF -> shortest job first find the average waiting time
```java
public static int solution(int[] arr){
	Arrays.sort(arr);
	int t = 0;
	int wt = 0;
	for(int i = 0; i<arr.length; i++){
		wt = wt+ t;
		t = t+ arr[i];
	}
	return wt/arr.length;
}
```

### 2. Jump Game
- every element tells you how many places you can jump at max
- Check if you can reach the end index if you start at 0th index
```java
public static boolean solution(int[] arr){
	int maxReach = 0;
	for(int i = 0; i<arr.length; i++){
		if(i > maxReach){
			return false;
		}
		maxReach = Math.max(maxReach, i + arr[i]);
	}
	return true;
}
```

### 3. Jump Game II
- Similar to jump game except return the smallest number of steps taken to reach the end.
```java
public static int solution(int[] arr){
	int jumps = 0; 
	int l =0, r = 0;
	while(r < arr.length-1){
		int farthest = 0;
		for(int index = l ; index <= r; index++){
			farthest = Math.max(index + arr[index], farthest);
		}
		l = r+1;
		r = farthest;
		jumps++;
	}
	return jumps;
}
```

### 4. N Meetings in One Room
Given start[] = {0, 3, 1, 5, 5, 8} & end[] = {5, 4, 2, 9, 7, 9}
starting and ending times of meetings where i is a single meeting
You can follow any order and maximize the number of meetings performed
Ans  : 3, 2, 5, 6
```java
import java.util.*;
public class NMeetingsInOneRoom{
	public static void main(String[] args){
		int[] start = {0, 3, 1, 5, 5, 8};
		int[] end = {5, 4, 2, 9, 7, 9};
		System.out.println(solution(start, end));
	}
	public static class Data{
		int start;
		int end;
		int pos;
	}
	public static ArrayList<Integer> solution(int[] start, int[] end){
		Data[] arr = new Data[start.length];

		for(int i =0 ; i< start.length; i++){
			arr[i] = new Data();
			arr[i].start = start[i];
			arr[i].end = end[i];
			arr[i].pos = i + 1;
		}

		Arrays.sort(arr, (a, b) -> Integer.compare(a.end, b.end));

		int count = 1;
		int freeTime = arr[0].end;
		ArrayList<Integer> ans = new ArrayList<>();
		ans.add(arr[0].pos);

		for(int i = 0; i< arr.length; i++){
			if(arr[i].start > freeTime){
				count++;
				freeTime = arr[i].end;
				ans.add(arr[i].pos);
			}
		}
		return ans;
	}
}
```

### 5. Non overlapping Intervals 
- Remove some intervals so that there are no overlapping ( end exclusive)
- Ex : int[][] arr = { {0, 5}, {3, 4}, {1, 2}, {5, 9}, {5, 7}, {7, 9} };
- Ans : 2 -> remove 0, 5 and 5, 9
```java
public static int solution(int[][] arr){
		Arrays.sort(arr, (a, b) -> Integer.compare(a[1], b[1]));

		int count = 1;
		int endTime = arr[0][1];

		for(int i = 1; i<arr.length; i++){
			if(arr[i][0] >= endTime){
				endTime = arr[i][1];
				count++;
			}
		}

		return arr.length - count;
	}
```

### 6. Insert Interval 
- such that there are no overlapping sets
- Ex : intervals = {{1, 3}, {6, 9}} newInterval = {2, 5}
- after insertion since it overlaps with {1, 3} ans will be : {{1, 5}, {6, 9}}
- if there is no overlapping insert as usual 

```java
public static ArrayList<int[]> solution(int[][] arr, int[] newInterval){
	ArrayList<int[]> res = new ArrayList<>();
	int i = 0;
	int n = arr.length;
	//left
	while(i < n && arr[i][1] < newInterval[0]){
		res.add(arr[i]);
		i++;
	}
	//middle
	while(i < n && arr[i][0] <= newInterval[1]){
		newInterval[0] = Math.min(newInterval[0], arr[i][0]);
		newInterval[1] = Math.max(newInterval[1], arr[i][1]);
		i++;
	}
	res.add(newInterval);
	// right
	while(i < n){
		res.add(arr[i]);
		i++;
	}
	return res;
}
```


### 7. Minimum Number of Platforms Required for a railway
- given arrival and departure times. Find minimum number of platforms required such that each of the n trains can arrive and depart
- Ex : int[] arr = {900, 945, 955, 1100, 1500, 1800};
		int[] dep = {920, 1200, 1130, 1150, 1900, 2000};
		ans will be  : 3
```java
public static int solution(int[] arr, int[] dep){
	Arrays.sort(arr);
	Arrays.sort(dep);
	int i = 0, j = 0;
	int count = 0;
	int maxCount = 0;
	while(i < arr.length){
		if(arr[i] < dep[j]){
			count++;
			i++;
		}
		else{
			count--;
			j++;
		}
		maxCount = Math.max(maxCount, count);
	}
	return maxCount;
}
```

### 8. Valid Parenthesis String
Given string s with characters `'(', ')'` and `'*'`
the `'*'` can be replaced with `'(', ')'` or `' '` 
check if with any possible combination the string is valid.
```java
public static boolean solution(String s){
	int min = 0, max = 0;
	for(int i = 0; i<s.length() ; i++){
		if(s.charAt(i) == '('){
			min = min + 1;
			max = max + 1;
		}
		else if(s.charAt(i) == ')'){
			min = min - 1;
			max = max - 1;
		}
		else{
			min = min -1;
			max = max + 1;
		}
		if(min < 0){
			min = 0;
		}
		if(max < 0){
			return false;
		}
	}
	return (min == 0);
}
```

### 9. Children ad candies
 given : arr = {1, 3, 2, 1};  -> output  = 7
 each element is rating value of a children
 Distribute minimum candies such that : 
 - each children gets at least one candy
 - children with higher rating has more candies than their neighbours
```java
public static int solution(int[] ratings){
	int sum = 1;
	int i = 1;
	while(i < ratings.length){
		if(ratings[i] == ratings[i-1]){
			sum += 1;
			i++;
			continue;
		}
		int peak = 1;
		while(i < ratings.length && ratings[i] > ratings[i-1]){
			peak++; // here we increase peak first then add to sum
			sum += peak; // cause the actual peak is included already in the sum
			i++;
		}
		int down = 1;
		while(i < ratings.length && ratings[i] < ratings[i-1]){
			sum += down; // notice here we do sum first then increase down
			down++; // because last down is not included in the sum
			i++;
		}
		if(down > peak){
			sum = sum + down - peak;
		}
	}
	return sum;
}
```

### 10. Fractional Knapsack
given arr[] = int[][] arr = {{100, 20}, {60, 10}, {100, 50}, {200, 50}}; w = 90
{value, weight}
maximize the total value such that the total weight  <= w
you can take fraction of a item
output : 380
```java
public static double solution(int[][] arr, int w){
	Arrays.sort(arr, (a,b) -> -Integer.compare(a[0]/a[1], b[0]/b[1]));
	// compare per unit value and sort them in reverse order
	double totVal = 0;
	for(int i = 0; i<arr.length; i++){
		if(arr[i][1] <= w){
			totVal += arr[i][0];
			w -= arr[i][1];
		}
		else{
			totVal += ((double)(arr[i][0]/arr[i][1])) * w;
			break;
		}
	}
	return totVal;
}
```

--- 

