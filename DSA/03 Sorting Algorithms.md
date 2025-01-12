### 1. Bubble sort
- Time Complexity : O(n<sup>2</sup>)
```java
public static void bubbleSort(int[] arr){
	boolean swapped;
	for(int i = 0; i < arr.length; i++){
		swapped = false;
		for(int j = 1; j<arr.length - i ; j++){ 
		// (-i) -> biggest numbers are already sorted
			if(arr[j] < arr[j-1]){
				int temp = arr[j];
				arr[j] = arr[j-1];
				arr[j-1] = temp;
				swapped = true;
			}
		}
		if(swapped = false){ // if not swapped already sorted
			return;
		}
	}
}
```

### 2. Selection Sort
- Pick the largest element and place it at the end
- Time Complexity : O(n<sup>2</sup>)
```java
private static void selectionSort(int[] arr) {
    for(int i = 0; i < arr.length ; i++) {
        int last = arr.length - i - 1;
        int max = getMaxIndex(arr, 0, last);
        swap(arr, max, last);
    }
}

private static int getMaxIndex(int[] arr, int start, int end) {
    int max = start;
    for(int i = start; i <= end ; i++) {
        if(arr[max] < arr[i]){
            max = i;
        }
    }
    return max;
}
```

### 3. Insertion Sort
- Pick a element such that all elements before it are smaller than it and all elements after it are larger than it
- Start from 1 
- Instead of comparing with all elements compare the curr element with the prev element only and place the larger element on right and smaller element on left
- Time Complexity : O(n<sup>2</sup>)
```java
private static void insertionSort(int[] arr) {
    for(int i = 0; i < arr.length - 1; i++) {
        for(int j = i+1; j> 0; j--) {
            if(arr[j] < arr[j-1]) {
                int temp = arr[j];
                arr[j] = arr[j-1];
                arr[j-1] = temp;
            }
            else {
                break;
            }
        }
    }
}
```

### 4. Cyclic Sort
- When given numbers are in range from 1 to N
```java
static void cyclicSort(int[] arr) {
    int i = 0;
    while(i < arr.length) {
        int correctIndex = arr[i] - 1; // Correct Index
        if(arr[correctIndex] == arr[i]){
            i++;
        }
        else {
            swap(arr, i, correctIndex);
        }
    }
}
```

### 5. Count Sort
```java
public static void countSortHash(int[]  arr){
    if(arr == null || arr.length <= 1){
        return;
    }

    int max = Arrays.stream(arr).max().getAsInt();
    int min = Arrays.stream(arr).min().getAsInt();

    Map<Integer, Integer> countMap = new HashMap<>();
    for(int num : arr){
        countMap.put(num, countMap.getOrDefault(num, 0) + 1);
    }

    int index = 0;
    for(int i = min; i<max; i++){
        int count = countMap.getOrDefault(i, 0);
        for(int j = 0; j<count; j++){
            arr[index] = i;
            index++;
        }
    }
}
```


## Some Cyclic Sort Questions
[268. Missing Number](https://leetcode.com/problems/missing-number/)
```java
public int missingNumber(int[] nums) {
    for(int i = 0; i<nums.length; ){
        int correctIndex = nums[i] - 1;
        
        if(correctIndex ==  -1 || correctIndex == i ){
            i++;
        }else{
            int temp = nums[correctIndex];
            nums[correctIndex] = nums[i];
            nums[i] = temp;
        }
    }
    for(int i = 0; i<nums.length; i++){
        if(nums[i] != i+1){
            return i+1;
        }
    }
    return 0;
}
```

[442. Find All Duplicates in an Array](https://leetcode.com/problems/find-all-duplicates-in-an-array/)
```java
public List<Integer> findDuplicates(int[] nums) {
    for(int i = 0; i<nums.length;){
        int correctIndex = nums[i] -1;
        if(nums[correctIndex] == nums[i]){
            i++;
        }else{
            int temp = nums[correctIndex];
            nums[correctIndex] = nums[i];
            nums[i] = temp;
        }
    }

    ArrayList<Integer> list = new ArrayList<>();

    for(int i = 0; i<nums.length; i++){
        if(nums[i] != i+1){
            list.add(nums[i]);
        }
    }
    return list;
}
```

# Recursive Sorting Algorithms

Merge Sort : 
```java
public static int[] mergeSort(int[] arr){
    if(arr.length == 1){
        return arr;
    }
    int mid = arr.length/2;
    int[] first = mergeSort(Arrays.copyOfRange(arr,0,mid));
    int[] second = mergeSort(Arrays.copyOfRange(arr, mid, arr.length));

    return merge(first, second);
}

public static int[] merge(int[] first, int[] second){
    int i= 0;
    int j = 0;
    int k = 0;
    int[] ans = new int[first.length + second.length];
    while(i != first.length && j != second.length){
        if(first[i] < second[j]){
            ans[k] = first[i];
            i++;
        }
        else{
            ans[k] = second[j];
            j++;
        }
        k++;
    }

    while(i < first.length){
        ans[k] = first[i];
        i++;
        k++;
    }
    while(j < second.length){
        ans[k] = second[j];
        j++;
        k++;
    }
    return ans;
}
```

Merge Sort in Place
```java
static void mergeSortInPlace(int[] arr, int s, int e) {
    if (e - s == 1) {
        return;
    }

    int mid = (s + e) / 2;

    mergeSortInPlace(arr, s, mid);
    mergeSortInPlace(arr, mid, e);

    mergeInPlace(arr, s, mid, e);
}

private static void mergeInPlace(int[] arr, int s, int m, int e) {
    int[] mix = new int[e - s];

    int i = s;
    int j = m;
    int k = 0;

    while (i < m && j < e) {
        if (arr[i] < arr[j]) {
            mix[k] = arr[i];
            i++;
        } else {
            mix[k] = arr[j];
            j++;
        }
        k++;
    }

    while (i < m) {
        mix[k] = arr[i];
        i++;
        k++;
    }

    while (j < e) {
        mix[k] = arr[j];
        j++;
        k++;
    }

    for (int l = 0; l < mix.length; l++) {
        arr[s+l] = mix[l];
    }
}
```

Quick Sort : 
```java
// sort(arr, 0, arr.length-1);
public static void sort(int[] arr, int low, int high){ // 
    if(low >= high){
        return;
    }
    int s = low;
    int e = high;
    int m = s + (e-s)/2;
    int pivot = arr[m];

    while(s <= e){
        //also a reason why if its already sorted it will not swap
        while(arr[s] < pivot){
            s++;
        }
        while(arr[e] > pivot){
            e--;
        }
        // the original condition might be violated
        if(s <= e){
            int temp = arr[s];
            arr[s] = arr[e];
            arr[e] = temp;
            s++;
            e--;
        }
    }

    // now my pivot is at correct index, sort other two halves now
    sort(arr, low, e);
    sort(arr, s, high);
}
```