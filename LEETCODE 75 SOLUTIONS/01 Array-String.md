[1768. Merge Strings Alternately](https://leetcode.com/problems/merge-strings-alternately/)
```java
class Solution {
    public String mergeAlternately(String word1, String word2) {
        StringBuilder st = new StringBuilder();

        int i = 0;
        int j = 0;
        int k = 0;
        while(i < word1.length() && j < word2.length()){
            if(k%2== 0){
                st.append(word1.charAt(i));
                i++;
            }else{
                st.append(word2.charAt(j));
                j++;
            }
            k++;
        }
        if(i < word1.length()){
            st.append(word1.substring(i, word1.length()));
        }
        if(j < word2.length()){
            st.append(word2.substring(j, word2.length()));
        }
        return st.toString();
    }
}
```

[1071. Greatest Common Divisor of Strings](https://leetcode.com/problems/greatest-common-divisor-of-strings/)
```java
class Solution {
    public String gcdOfStrings(String str1, String str2) {
        StringBuilder min;
        if(str1.length() > str2.length()){
            min = new StringBuilder(str2);
        }else{
            min = new StringBuilder(str1);
        }

        while(min.length() > 0){
            if(isGCD(min.toString(), str1)  && isGCD(min.toString(), str2)){
                return min.toString();
            }
            min.deleteCharAt(min.length()-1);
        }
        return "";
    }
    public boolean isGCD(String st, String str){
        int i = 0;
        if (str.length() % st.length() != 0){
            return false;
        }
        while(i + st.length() <= str.length()){
            if(!st.equals(str.substring(i, i+ st.length()))){
                return false;
            }
            i += st.length();
        }
        return true;
    }
}
```
BETTER 
```java
class Solution {
    public String gcdOfStrings(String str1, String str2) {
        if (!(str1 + str2).equals(str2 + str1)) {
            return "";
        }

        int gcdResult = gcd(str1.length(), str2.length());

        return str1.substring(0, gcdResult);
    }

    private int gcd(int a, int b) {
        while (b != 0) {
            int temp = b;
            b = a % b;
            a = temp;
        }
        return a;
    }
}
```

[1431. Kids With the Greatest Number of Candies](https://leetcode.com/problems/kids-with-the-greatest-number-of-candies/)
```java
class Solution {
    public List<Boolean> kidsWithCandies(int[] candies, int extraCandies) {
        int max = candies[0];
        List<Boolean> list = new ArrayList<>();
        for(int i = 0; i<candies.length; i++){
            if(max < candies[i]){
                max = candies[i];
            }
        }

        for(int i = 0; i<candies.length; i++){
            if(candies[i] + extraCandies >= max){
                list.add(true);
            }
            else{
                list.add(false);
            }
        }
        return list;
    }
}
```

[605. Can Place Flowers](https://leetcode.com/problems/can-place-flowers/)
```java
class Solution {
    public boolean canPlaceFlowers(int[] flowerbed, int n) {
        int count = 0;
        for(int i =0; i<flowerbed.length; i++){
            if(flowerbed[i] == 0){
                int prev = ((i == 0) || (flowerbed[i-1] == 0)) ? 0 : 1;
                int next = (i == flowerbed.length -1 || flowerbed[i + 1] == 0) ? 0 : 1;

                if(prev == 0 && next == 0){
                    flowerbed[i] = 1;
                    count++;
                }
            }
        }
        return count >= n;
    }
}
```

[345. Reverse Vowels of a String](https://leetcode.com/problems/reverse-vowels-of-a-string/)
```java
class Solution {
    public String reverseVowels(String s) {
        int l = 0;
        int r = s.length()-1;
        StringBuilder str = new StringBuilder(s);

        while(l < r){
            while(l <= s.length() - 1 && !isAVowel(str, l)){
                l++;
            }
            while(r >= 0 && !isAVowel(str, r)){
                r--;
            }
            if(l < r){
                char temp = str.charAt(r);
                str.setCharAt(r, str.charAt(l)); 
                str.setCharAt(l, temp);
                l++;
                r--;
            }
        }
        return str.toString();
    }
    public boolean isAVowel(StringBuilder str , int l){
        char ch = str.charAt(l);
        if(ch == 'a' || ch == 'e' || ch == 'i' || ch == 'o' || ch == 'u' || 
        ch == 'A' || ch == 'E' || ch == 'I' || ch == 'O' || ch == 'U'){
            return true;
        }
        return false;
    }
}
```

[151. Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/) #hard 
```java
class Solution {
    public String reverseWords(String s) {
        StringBuilder word = new StringBuilder();
        ArrayList<String> words = new ArrayList<>();
        int i = 0;

        while(i < s.length()){
            while(i < s.length() && s.charAt(i) == ' '){
                i++;
            }
            while(i < s.length() && s.charAt(i) != ' '){
                word.append(s.charAt(i));
                i++;
            }
            if(word.length() > 0){
                words.add(word.toString());
                word = new StringBuilder();
            }
        }
        
        StringBuilder sentence = new StringBuilder();

        for(int k = words.size() -1; k >= 0; k--){
            sentence.append(words.get(k));
            if(k > 0){
                sentence.append(" ");
            }
        }

        return sentence.toString();
    }
}
```

[238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)
```java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int left = 1;
        int right = 1;
        int[] ans = new int[nums.length];
        
        ans[0] = 1;
        for(int i = 1; i<nums.length; i++){
            ans[i] = ans[i-1]  * nums[i-1];
        }
        
        int prefixProduct = 1;
        for(int i = nums.length - 1; i >= 0; i--){
            ans[i] = ans[i] * prefixProduct;
            prefixProduct = prefixProduct * nums[i];
        }
        return ans;
    }
}
```

[334. Increasing Triplet Subsequence](https://leetcode.com/problems/increasing-triplet-subsequence/)
```java
class Solution {
    public boolean increasingTriplet(int[] nums) {
        int firstNum = Integer.MAX_VALUE;
        int secondNum = Integer.MAX_VALUE; 

        for(int i = 0; i<nums.length; i++){
            if(nums[i] <= firstNum){
                firstNum = nums[i];
            }
            else if(nums[i] <= secondNum){
                secondNum = nums[i];
            }
            else{
                return true;
            }
        }
        return false;
    }
}
```


[443. String Compression](https://leetcode.com/problems/string-compression/)
```java
class Solution {
    public int compress(char[] chars) {
        int j = 0;
        char ch = chars[0];
        int i = 1;
        int count = 1;

        if(chars.length == 1){
            return 1;
        }

        while (i < chars.length) {
            while (i < chars.length && chars[i] == ch) {
                count++;
                i++;
            }

            if (count == 1) {
                chars[j++] = ch;
            } else {
                chars[j++] = ch;
                j = storeNumInArr(chars, j, count);
            }
            
            if (i == chars.length) {
                return j;
            }

            ch = chars[i];
            count = 0;
        }
        return j;
    }

    public int storeNumInArr(char[] chars, int j, int count){
        String s = count+"";
        char i = 0;
        while(j < chars.length && i < s.length()){
            chars[j] = s.charAt(i);
            i++;
            j++;
        }
        return j;
    }
}
```