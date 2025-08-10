
Apna College Questions 
### Majority Element : 
- Given an integer array of size n, 
- find all elements that appear more than floor(n/3) times
- Ex : input : `[1, 3, 2, 5, 1, 3, 1, 5, 1]` output : `[1]`
```java
public static void majorityElement(int[] arr){
	HashMap<Integer, Integer> map = new HashMap<>();

	int n = arr.length;
	for(int i = 0; i<n; i++){
		if(map.containsKey(arr[i])){
			map.put(arr[i], map.get(arr[i]) + 1);
		}else{
			map.put(arr[i], 1);
		}
	}

	for(int key : map.keySet()){
		if(map.get(key) > n/3){
			System.out.println(key);
		}
	}
}
```

### Set Union of Two arrays
- Given two arrays return another array with all elements of both the arrays
```java
public HashSet<Integer> setUnion(int[] arr1, int[] arr2){
	HashSet<Integer> set = new HashSet<>();
	for(int i = 0; i<arr1.length; i++){
		set.add(arr1[i]);
	}
	for(int j = 0; j < arr2.length; j++){
		set.add(arr2[j]);
	}
	return set;
}
```

### Set Intersection of Two Arrays
```java
public ArrayList<Integer> setIntersection(int[] arr1, int[] arr2){
	HashSet<Integer> set = new HashSet<>();
	int count = 0;
	ArrayList<Integer> res = new ArrayList<>();

	for(int i = 0; i<arr1.length; i++){
		set.add(arr1[i]);
	}

	for(int j = 0; j< arr2.length; j++){
		if(set.contains(arr2[i])){
			count++;
			res.add(arr2[j]);
			set.remove(arr2[j]);
		}
	}

	return res;
}
```

### SubArray Sum equal to k
- Can be solved using Prefix sum
- But solving using HashMaps :
- Returns number of possible subarrays
```java
public static int sumArray(int[] arr, int k){
	HashMap<Integer, Integer> map = new HashMap<>(); // <sum, frequency>
	map.put(0,1);

	int ans = 0;
	int sum = 0;

	for(int j = 0; j < arr.length; j++){
		sum += arr[j];
		if(map.containsKey(sum - k)){
			ans += map.get(sum - k);
		}
		if(map.containsKey(sum)){
			map.put(sum, map.get(sum) + 1);
		}else{
			map.put(sum, 1);
		}
	}

	return ans;
}
```

### Karp Robin String Matching Algorithm 
- Kunal Kushwah
```java
package HashMaps;

public class KarpRabinAlgo {
    private static final int PRIME = 101;

    private static double calculateHash(String str){
        double hash = 0;

        for(int i = 0;i<str.length(); i++){
            hash = hash + str.charAt(i) * Math.pow(PRIME, i);
        }
        return hash;
    }

    private static double updateHash(double prevHash, char oldChar, char newChar, int patternLength){
        double newHash = (prevHash - oldChar) / PRIME; // we dont need to calculate the whole hash again
        // ex 'APPOR'V -> A'PPORV' => -A +V

        newHash = newHash + newChar * Math.pow(PRIME, patternLength-1);
        return newHash;
    }

    public static void search(String text, String pattern){
        int patternLength = pattern.length();
        double patternHash = calculateHash(pattern);
        double textHash = calculateHash(text.substring(0, patternLength));

        for(int i = 0; i<text.length()-patternLength; i++){
            if(textHash == patternHash){
                if(text.substring(i, i+patternLength).equals(pattern)){
                    System.out.println("Pattern Found at index" + i);
                }
            }

            if(i<text.length() - patternLength){
                textHash = updateHash(textHash, text.charAt(i), text.charAt(i+patternLength), patternLength);
            }
        }

    }

    public static void main(String[] args) {
        search("apporvKunalRahul", "Kunal");
    }
}

```