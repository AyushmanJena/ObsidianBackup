# Easy 

Print N to 1 and 1 to N
```java
public static void printNto1(int n){ // printNto1(5);
    if(n == 0){
        return;
    }
    System.out.println(n);
    printNto1(n-1);
}

public static void print1toN(int n){ // print1toN(5);
    if(n == 0){
        return;
    }
    print1toN(n-1);
    System.out.println(n);
}
```

Find number of steps to reduce a number to zero
```java
// If the current number is even, you have to divide it by 2, otherwise you have to subtract 1 from it
public static int countSteps(int num, int steps){ // countSteps(41, 0)
    if(num == 0){
        return steps;
    }
    if(num%2 == 0){
        return countSteps(num/2, steps+1);
    }
    return countSteps(num-1, steps +1);
}
```

CountZeros in a number
```java
public static int count(int n, int c){ // (10020, 0)
    if(n == 0){
        return c;
    }
    int rem = n%10;
    if(rem == 0){
        return count(n/10, c+1);
    }
    return count(n/10, c);
}
```

Factorial  Of a Number
```java
public static int factorial(int n){
    if(n == 1){
        return 1;
    }
    return n * factorial(n-1);
}
```

Sum Of Digits
```java
static int sumOfDig(int num){
    if(num < 10){
        return num;
    }
    return (num%10) + sumOfDig(num/10);
}
```

Reverse a Number : 
```java
public static int reverseNumber(int num, int reverse){ // (12345, 0)
    if(num == 0){
        return reverse;
    }
    int rem = num % 10;
    reverse = (reverse*10) + rem;
    return reverseNumber(num/10, reverse);
}
```

Fibonacci Series : 
```java
public static int fibo(int n) { //fibo(8)
    if(n == 0 || n == 1){
        return n;
    }
    return fibo(n-1) + fibo(n-2);
}
```

Finding Greatest Common Divisor (GCD)
```java
public static int gcd(int num1, int num2){
	if(num1 == 0) return num1;
	if(num2 == 0) return num2;

	if(num1 > num2){
		return gcd(num1 % num2, num2);
	}
	else{
		return gcd(num1, num2 % num1);
	}
}
```
# Array 
Linear Search 
```java
public static int find(int[] arr, int target, int index){ // find(arr, 3, 0)
	if(index == arr.length) return -1;
	
	if(arr[index] == target) return index;
	
	return findIndex(arr, target, index+1);
}
```
Binary Search
```java
// search(arr, 77, 0, arr.length -1);
private static int search(int[] arr, int target, int start, int end) {
    if(end < start){
        return -1;
    }

    int mid = start + (end - start) / 2;

    if(arr[mid] == target){
        return mid;
    }

    if(target < arr[mid]){
        return search(arr, target, start, mid-1);

    }

    return search(arr, target, mid+1, end);
}
```

Find all Occurrences of element in array 
1. Using ArrayList as parameter 
```java
static ArrayList<Integer> findAllIndex(int[] arr, int target, int index, ArrayList<Integer> list) {
	if(index == arr.length){
		return list;
	}
	if(arr[index] == target){
		list.add(inndex);
	}
	return findAllIndex(arr, target, index +1, list);
}
```
2. Taking ArrayList in the body of the method
```java
static ArrayList<Integer>  findAllIndex(int[] arr, int target, int index){
	ArrayList<Integer> list = new ArrayList<>();
	if(index == arr.length){
		return list;
	}
	// this list will contain the answer for that function call only 
	if(arr[index] == target){
		list.add(index);
	}
	ArrayList<Integer> ansFromBelowCalls = findAllIndex(arr, target, index +1);
	list.addAll(ansFromBelowCalls);
	return list;
}
```

Recursive Bubble Sort 
```java
void bubbleSort(int[] arr, int r, int c){ //bubble(arr, arr.length -1, 0)
	if(r == 0) return;

	if(c < r){
		if(arr[c] > arr[c+1]){
			swap(arr, c, c+1);
		}
		bubbleSort(arr, r, c+1);
	}
	bubbleSort(arr, r-1, 0);
}
```

Recursive Selection Sort 
```java
public void selectionSort(int[] arr, int r, int c, int max){ //selection(arr, arr.length, 0, 0)
	if(r == 0) return;

	if(r < c){
		if(arr[c] > arr[max]){
			selectionSort(arr, r, c+1, c);	
		}
		else{
			selectionSort(arr, r, c+1, max);
		}
	}
	else{
		swap(arr, r-1, max); // r-1 because we want to sort the last unsorted value
		selectionSort(arr, r-1, 0, 0);
	}
}
```

Recursive Rotated Binary Search : 
```java
static void search(int[] arr, int target, int s, int e){
	if(s > e) return -1;

	int m = s + (e-s)/2;
	if(arr[m] == target){
		return m;
	}

	if(arr[s] <= arr[m]){
		if(target >= arr[s] && target <= arr[m]){
			return search(arr, target, s, m-1);
		}else{
			return search(arr, target, m+1, e);
		}
	}

	if(target >= arr[m] && target <= arr[e]){
		return search(arr, target, m+1, e);
	}

	return search(arr, target, s, m-1);
}
```

# String Questions

Remove character from string 
```java
static String removeChar(String str , char ch){
	if(str.isEmpty()) return "";

	if(str.charAt(0) == ch){
		return removeChar(str.substring(1), ch);
	}
	return str.charAt(0) + removeChar(str.substring(1), ch);
}
```
More in subsets and subsequences