##### by Kunal Kushwaha

### 1. Binary Search
```java
static in binarySearch(int[] arr, int target){
	int start = 0;
	int end = arr.length -1;
	while(start <= end){
		int mid = start + (end- start)/2; // to limit within integer range
		if(target < arr[mid]){
			end = mid-1;
		}
		else if(target > arr[mid]){
			start = mid + 1;
		}
		else{
			return mid;
		}
	}
	return -1;
}
```

### 2.  Binary Search in Sorted Matrix (2d sorted)
- Each row is sorted and is greater than the previous row value
		int[][] arr = {{10, 20, 30, 40},
                {15, 25, 35, 45},
                {28, 29, 37, 49},
                {33, 34, 38, 50}
        };
```java
public int[] search(int[][] arr, int target){
	int row = 0;
	int col = arr.length - 1;
	while(row < arr.length && col > 0){
		if(arr[row][col] == target)
			return new int[] {row, col};
		if(arr[row][col] > target)
			col--;
		else
			row++;
	}
	return new int[] {-1, -1};
}
```

### 3.  Binary Search in Sorted Matrix
		int[][] arr = {
                {1, 2, 3},
                {4, 5, 6},
                {7, 8, 9}
        };
- Vulcan bhai solution
```java
public static int[] search2(int[][] arr, int target){ // self made
        int prevRow = 0;
        int currRow = arr.length - 1;
        int col = arr[0].length - 1;
        if(arr[currRow][col] < target || arr[0][0] > target){
            return new int[] {-1, -1};        
        }

        while(currRow > 0){ // or prevRow == 0
            prevRow = currRow - 1;

            if(arr[currRow][col] >= target && arr[prevRow][col] < target){
                return binarySearch(arr, currRow, 0, col, target);
            }
            else{ //(arr[prevRow][col] < target){
                currRow--;
            }
        }
        return binarySearch(arr, currRow, 0, col, target);
    }

    // search in the row provided between the cols provided
    static int[] binarySearch(int[][] matrix, int row, int cStart, int cEnd, int target) {
        while (cStart <= cEnd) {
            int mid = cStart + (cEnd - cStart) / 2;
            if (matrix[row][mid] == target) {
                return new int[]{row, mid};
            }
            if (matrix[row][mid] < target) {
                cStart = mid + 1;
            } else {
                cEnd = mid - 1;
            }
        }
        return new int[]{-1, -1};
    }
```


- Kunal Kushwaha Solution
```java
// search in the row provided between the cols provided
static int[] binarySearch(int[][] matrix, int row, int cStart, int cEnd, int target) {
    while (cStart <= cEnd) {
        int mid = cStart + (cEnd - cStart) / 2;
        if (matrix[row][mid] == target) {
            return new int[]{row, mid};
        }
        if (matrix[row][mid] < target) {
            cStart = mid + 1;
        } else {
            cEnd = mid - 1;
        }
    }
    return new int[]{-1, -1};
}

static int[] search(int[][] matrix, int target) {
    int rows = matrix.length;
    int cols = matrix[0].length;
    if (cols == 0){ // matrix may be empty
        return new int[] {-1,-1};
    }
    if (rows == 1) { // only one row
        return binarySearch(matrix,0, 0, cols-1, target);
    }

    int rStart = 0;
    int rEnd = rows - 1;
    int cMid = cols / 2;

    // run the loop till 2 rows are remaining
    while (rStart < (rEnd - 1)) { // while this is true it will have more than 2 rows
        int mid = rStart + (rEnd - rStart) / 2;
        if (matrix[mid][cMid] == target) {
            return new int[]{mid, cMid};
        }
        if (matrix[mid][cMid] < target) {
            rStart = mid;
        } else {
            rEnd = mid;
        }
    }

    // now we have two rows
    // check whether the target is in the col of 2 rows
    if (matrix[rStart][cMid] == target) {
        return new int[]{rStart, cMid};
    }
    if (matrix[rStart + 1][cMid] == target) {
        return new int[]{rStart + 1, cMid};
    }

    // search in 1st half
    if (target <= matrix[rStart][cMid - 1]) {
        return binarySearch(matrix, rStart, 0, cMid-1, target);
    }
    // search in 2nd half
    if (target >= matrix[rStart][cMid + 1] && target <= matrix[rStart][cols - 1]) {
        return binarySearch(matrix, rStart, cMid + 1, cols - 1, target);
    }
    // search in 3rd half
    if (target <= matrix[rStart + 1][cMid - 1]) {
        return binarySearch(matrix, rStart + 1, 0, cMid-1, target);
    } else {
        return binarySearch(matrix, rStart + 1, cMid + 1, cols - 1, target);
    }
}
```

### 4. Search in Infinite sized array 
```java
public static int infiniteArray(int[] arr, int target) {
    int start = 0;
    int end = 1;

    while(target > arr[end]){
        int temp = end + 1;
        end = end + (end - start +1) * 2;
        start = temp;
    }
    int ans = search(arr, target, start, end);
    return ans;
}
```

### 5. Find peak index in Mountain Array or Bitonic array
`int[] arr = {0, 1, 2, 5, 19, 24, 36, 12, 2};`
```java
public static int mountainArray(int[] arr) {
    int start = 0;
    int end = arr.length - 1;

    while(start < end) {
        int mid = start + (end - start)/2;

        if(arr[mid] > arr[mid+1]) {
            end = mid;
        }

        else {
            start = mid +1;
        }
    }

    return start;
}
```

### 5. Find pivot element of a rotated array without duplicates
```java
// this will not work in duplicate values
static int findPivot(int[] arr) {
    int start = 0;
    int end = arr.length - 1;
    while (start <= end) {
        int mid = start + (end - start) / 2;
        // 4 cases over here
        if (mid < end && arr[mid] > arr[mid + 1]) {
            return mid;
        }
        if (mid > start && arr[mid] < arr[mid - 1]) {
            return mid-1;
        }
        if (arr[mid] <= arr[start]) {
            end = mid - 1;
        } else {
            start = mid + 1;
        }
    }
    return -1;
}
```

### 6. Find pivot element of rotated array with duplicates
```java
static int findPivotWithDuplicates(int[] arr) {
    int start = 0;
    int end = arr.length - 1;
    while (start <= end) {
        int mid = start + (end - start) / 2;
        // 4 cases over here
        if (mid < end && arr[mid] > arr[mid + 1]) {
            return mid;
        }
        if (mid > start && arr[mid] < arr[mid - 1]) {
            return mid-1;
        }

        // if elements at middle, start, end are equal then just skip the duplicates
        if (arr[mid] == arr[start] && arr[mid] == arr[end]) {
            // skip the duplicates
            // NOTE: what if these elements at start and end were the pivot?
			// check if start is pivot
            if (start < end && arr[start] > arr[start + 1]) {
                return start;
            }
            start++;

            // check whether end is pivot
            if (end > start && arr[end] < arr[end - 1]) {
                return end - 1;
            }
            end--;
        }
        // left side is sorted, so pivot should be in right
        else if(arr[start] < arr[mid] || (arr[start] == arr[mid] && arr[mid] > arr[end])) {
            start = mid + 1;
        } else {
            end = mid - 1;
        }
    }
    return -1;
}
```

--- 
