---
lastSync: Sun Oct 06 2024 20:45:40 GMT+0530 (India Standard Time)
---
[2215. Find the Difference of Two Arrays](https://leetcode.com/problems/find-the-difference-of-two-arrays/)
```java
```java
class Solution {
    private List<Integer> onlyInFirstSet(int[] first, int[] second) {
        Set<Integer> onlyInFirst = new HashSet<>();
        
        for(int i=0; i<first.length; i++) {
            onlyInFirst.add(first[i]);
        }
        
        for(int i=0; i<second.length; i++) {
            int val = second[i];
            if(onlyInFirst.contains(val)) {
                onlyInFirst.remove(val);
            }
        }
        return new ArrayList<>(onlyInFirst);
    }
    
    public List<List<Integer>> findDifference(int[] nums1, int[] nums2) {
        return Arrays.asList(
            onlyInFirstSet(nums1, nums2),
            onlyInFirstSet(nums2, nums1)
        );
    }
}
```


[1207. Unique Number of Occurrences](https://leetcode.com/problems/unique-number-of-occurrences/)
```java
class Solution {
    public boolean uniqueOccurrences(int[] arr) {
        HashMap<Integer, Integer> map = new HashMap<>();

        for(Integer x : arr){
            if(map.containsKey(x)){
                map.put(x, map.get(x)+1);
            }
            else{
                map.put(x, 1);
            }
        }

        HashSet<Integer> set = new HashSet<>();
        
        for(Integer x : map.keySet()){
            if(set.contains(map.get(x))){
                return false;
            }
            else{
                set.add(map.get(x));
            }
        }
        
        return true;
    }
}
```


[1657. Determine if Two Strings Are Close](https://leetcode.com/problems/determine-if-two-strings-are-close/)
```java
class Solution {
    public boolean closeStrings(String word1, String word2) {

        if (word1.length() != word2.length()) {
            return false;
        }

        HashMap<Character, Integer> map1 = new HashMap<>();
        HashMap<Character, Integer> map2 = new HashMap<>();

        for (int i = 0; i < word1.length(); i++) {
            if (map1.containsKey(word1.charAt(i))) {
                map1.put(word1.charAt(i), map1.get(word1.charAt(i)) + 1);
            } else {
                map1.put(word1.charAt(i), 1);
            }
        }

        for (int i = 0; i < word2.length(); i++) {
            if (map2.containsKey(word2.charAt(i))) {
                map2.put(word2.charAt(i), map2.get(word2.charAt(i)) + 1);
            } else {
                map2.put(word2.charAt(i), 1);
            }
        }

        for (char ch : map1.keySet()) {
            if (!map2.containsKey(ch)) {
                return false;
            }
        }

        int[] list1 = new int[map1.size()];
        int[] list2 = new int[map2.size()];
        int i = 0;
        for (char ch : map1.keySet()) {
            list1[i++] = map1.get(ch);
        }
        i = 0;
        for (char ch : map2.keySet()) {
            list2[i++] = map2.get(ch);
        }

        Arrays.sort(list1);
        Arrays.sort(list2);

        return Arrays.equals(list1, list2);
    }
}
```
More optimized solution : 
```java
class Solution {
    public boolean closeStrings(String word1, String word2) {
        int n=word1.length(),m=word2.length(),i,j;
        int[] f1 = new int[26];
        int[] f2 = new int[26];
        if(n!=m)
            return false;
        if(word1.equals(word2))
            return true;
        for(i=0;i<n;i++)
        {
            f1[word1.charAt(i)-'a']++;
        }     
        for(j=0;j<m;j++)
        {
            f2[word2.charAt(j)-'a']++;
        }
        for(i = 0; i < 26; ++i) {
            if((f1[i]==0 && f2[i]!=0) || (f1[i]!=0 && f2[i]==0)) {
                return false;
            }
        }
        Arrays.sort(f1);
        Arrays.sort(f2);
        for(i=25;i>=0;i--)
        {
            if(f1[i]!=f2[i])
            {   
                return false;
            }
        }
        return true;
    }
}
```


[2352. Equal Row and Column Pairs](https://leetcode.com/problems/equal-row-and-column-pairs/)
```java
class Solution {
    public int equalPairs(int[][] grid) {
        int n = grid.length;
        HashMap<String, Integer> map = new HashMap<>();

        for(int i = 0; i<n; i++){
            String rowString = Arrays.toString(grid[i]);
            if(map.containsKey(rowString)){
                map.put(rowString, map.get(rowString)+1);
            }
            else{
                map.put(rowString, 1);
            }
        }

        int count = 0;

        for(int i = 0; i<n; i++){
            int[] colArray = new int[n];
            for(int j = 0; j<n; j++){
                colArray[j] = grid[j][i];
            }
            String rowString = Arrays.toString(colArray);
            if(map.containsKey(rowString)){
                count += map.get(rowString);
            }
        }

        return count;
    }
}
```