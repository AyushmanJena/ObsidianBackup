# LeetCode

[696. Count Binary Substrings](https://leetcode.com/problems/count-binary-substrings/)
#twopointer #strings
```java
class Solution {
    public int countBinarySubstrings(String s) {
        int i = 0;
        int j = 0;
        int ans = 0;
        while(i < s.length()){
            char firstChar = s.charAt(i);
            int iCount = 0;
            int jCount = 0;
            while( i < s.length() && s.charAt(i) == firstChar){
                i++;
                iCount++;
            }
            j = i;
            while(i < s.length() && s.charAt(i) != firstChar){
                i++;
                jCount++;
            } 
            ans += Math.min(iCount, jCount);
            i = j;
        }
        return ans;
    }
}
```

[868. Binary Gap](https://leetcode.com/problems/binary-gap/)
#bitmanipulation #easy #twopointer 
```java
class Solution {
    public int binaryGap(int n) {
        int maxGap = 0;
        int slow = 0;
        int fast = 0;

        String num = Integer.toBinaryString(n);

        while(fast < num.length() && num.charAt(fast) != '1'){
            fast++;
        }

        while(fast < num.length() && slow < num.length()){
            maxGap = Math.max(maxGap, fast - slow);
            slow = fast;
            fast++;
            while(fast < num.length() && num.charAt(fast) != '1'){
                fast++;
            }
        }

        return maxGap;
    }
}
```

[2486. Append Characters to String to Make Subsequence](https://leetcode.com/problems/append-characters-to-string-to-make-subsequence/)
```java
public class Solution{
	public static void main(String[] args){
		String s = "coaching";
		String t = "coding";
		System.out.println(appendCharToMakeSubsequence(s,t));
	}

    public static int appendCharToMakeSubsequence(String s, String t) {
    	int i = 0, j = 0;
        while(i < s.length() && j < t.length()){
        	if(s.charAt(i) == t.charAt(j)){
        		i++;
        		j++;
        	}
        	else{
        		i++;
        	}
        }
        return t.length() - j;
    }
}
```

[12. Integer to Roman](https://leetcode.com/problems/integer-to-roman/)
```java
class Solution {
    public String intToRoman(int num) {
        StringBuilder ans = new StringBuilder();
        while(num != 0){
            if(num >= 1000){
                num = num - 1000;
                ans.append('M');
            }
            else if(num < 1000 && num >= 500){
                if(num >= 900){
                    num = num -900;
                    ans.append("CM");
                    continue;
                }
                num = num- 500;
                ans.append('D');
            }
            else if(num <500 && num >=100){
                if(num >= 400){
                    num = num - 400;
                    ans.append("CD");
                    continue;
                }
                num = num-100;
                ans.append('C');
            }
            else if(num < 100 && num >=50){
                if(num >= 90){
                    num = num -90;
                    ans.append("XC");
                    continue;
                }
                num = num-50;
                ans.append('L');
            }
            else if(num <50 && num >=10){
                if(num >= 40){
                    num = num -40;
                    ans.append("XL");
                    continue;
                }
                num = num -10;
                ans.append('X');
            }
            else if(num <10 && num >=5){
                if(num >= 9){
                    num = num -9;
                    ans.append("IX");
                    continue;
                }
                num = num - 5;
                ans.append('V');
            }
            else if(num <5 && num > 0){
                if(num >= 4){
                    num = num -4;
                    ans.append("IV");
                    continue;
                }
                num = num -1;
                ans.append('I');
            }
        }
        return ans.toString();
    }
}
```


[13. Roman to Integer](https://leetcode.com/problems/roman-to-integer/)
```java
class RomanToInt {
    public static void main(String[] args) {
        System.out.println(romanToInt("MCMXCIV"));
    }
    public static int romanToInt(String s) {
        int ans = 0;
        for(int i = 0; i<s.length(); i++){
            char ch = s.charAt(i);
            switch(ch){
                case 'M':
                    ans += 1000;
                    break;
                case 'D':
                    ans += 500;
                    break;
                case 'C':
                    if(i != s.length()-1){
                        ans += addOrSub('C', s.charAt(i+1));
                    }else{
                        ans += 100;
                    }
                    break;
                case 'L':
                    ans += 50;
                    break;
                case 'X':
                    if(i != s.length()-1){
                        ans += addOrSub('X', s.charAt(i+1));
                    }else{
                        ans += 10;
                    }
                    break;
                case 'V':
                    ans += 5;
                    break;
                case 'I':
                    if(i != s.length()-1){
                        ans += addOrSub('I', s.charAt(i+1));
                    }else{
                        ans += 1;
                    }
                    break;
            }
        }
        return ans;
    }

    static int addOrSub(char a, char b){
        if(a =='I'){
            if(b == 'V' || b =='X'){
                return -1;
            }
            else{
                return 1;
            }
        }
        if(a =='X'){
            if(b == 'L' || b =='C'){
                return -10;
            }
            else{
                return 10;
            }
        }
        if(a =='C'){
            if(b == 'D' || b =='M'){
                return -100;
            }
            else{
                return 100;
            }
        }
        return 0;
    }
}

```


[345. Reverse Vowels of a String](https://leetcode.com/problems/reverse-vowels-of-a-string/)
```java
class Solution {
    public String reverseVowels(String s) {
        int i = 0;
        int j = s.length() - 1;
        StringBuilder st = new StringBuilder(s);
        char ch;

        while (i < j) {
            while (i < j && !isVowel(s.charAt(i))) {
                i++;
            }
            while (i < j && !isVowel(s.charAt(j))) {
                j--;
            }
            if (i < j) {
                ch = st.charAt(i);
                st.setCharAt(i, st.charAt(j));
                st.setCharAt(j, ch);
                i++;
                j--;
            }
        }
        return st.toString();
    }

    private boolean isVowel(char c) {
        return "aeiouAEIOU".indexOf(c) != -1;
    }
}
```

[67. Add Binary](https://leetcode.com/problems/add-binary/)
```java
class Solution {
    public String addBinary(String a, String b) {
        int carry = 0;
        int size = Math.max(a.length(), b.length());
        StringBuilder sb = new StringBuilder();

        for(int i = 0; i < size; i++){
            char aValue = '0';
            char bValue = '0';
            if(a.length() - i -1 >= 0){
                aValue = a.charAt(a.length() - i - 1);
            }
            if(b.length() - i -1 >= 0){
                bValue = b.charAt(b.length() - i - 1);
            }
            if(aValue == '1' && bValue == '1'){
                if(carry == 1){ // 1+1+1
                    sb.append('1');
                }else{ // 1+1+0
                    sb.append('0');
                }
                carry = 1;
            }else if(aValue== '0' && bValue == '0'){
                if(carry == 1){ // 0 + 0 + 1
                    sb.append('1');
                }else{ // 0+ 0+0
                    sb.append('0');
                }
                carry = 0;
            }else{
                if(carry == 1){ // 1+0+1 / 0+1+1
                    sb.append('0');
                    carry = 1;
                }else{ // 1+0+0 / 0+1+0
                    sb.append('1');
                    carry = 0;
                }
            }
        }
        if(carry == 1){ // extra 1 if gets carried
            sb.append('1');
        }

        sb.reverse();
        return sb.toString();
    }
}
```

[1945. Sum of Digits of String After Convert](https://leetcode.com/problems/sum-of-digits-of-string-after-convert/)
```java

/*
Input: s = "leetcode", k = 2
Output: 6
Explanation: The operations are as follows:
- Convert: "leetcode" ➝ "(12)(5)(5)(20)(3)(15)(4)(5)" ➝ "12552031545" ➝ 12552031545
- Transform #1: 12552031545 ➝ 1 + 2 + 5 + 5 + 2 + 0 + 3 + 1 + 5 + 4 + 5 ➝ 33
- Transform #2: 33 ➝ 3 + 3 ➝ 6
Thus the resulting integer is 6.

*/


class Solution {
   public int getLucky(String s, int k) {
        int sum =0;
        for(int i = 0; i<s.length(); i++){
            sum = sum + getDigitsSum(s.charAt(i) - 96);
        }
        k--;
        while(k > 0){
            sum = getDigitsSum(sum);
            k--;
        }
        return sum;
    }
    public int getDigitsSum(int num){
        int sum = 0;
        int rem = 0;
        while(num != 0 ){
            rem = num % 10;
            num = num /10;
            sum += rem;
        }
        return sum;
    }
}
```

[3713. Longest Balanced Substring I](https://leetcode.com/problems/longest-balanced-substring-i/)
```java
class Solution {
    public int longestBalanced(String s) {
        HashMap<Character, Integer> map = new HashMap<>();
        int maxLen = 0;

        for(int i = 0; i< s.length() ; i++){
            for(int j = i; j<s.length() ;j++){
                if(!map.containsKey(s.charAt(j))){
                    map.put(s.charAt(j), 1);
                }else{
                map.put(s.charAt(j), map.get(s.charAt(j)) + 1);

                }

                int count = map.get(s.charAt(j));
                boolean isValid = true;
                if((j-i + 1) >= maxLen){
                    for(Integer k : map.values()){
                        if(k != count){
                            isValid = false;
                        }
                    }
                }
                if(isValid == true){
                    maxLen = Math.max(maxLen, j-i+1);
                }
            }
            map = new HashMap<>();            
        }
        return maxLen;
    }
}
```

[1071. Greatest Common Divisor of Strings](https://leetcode.com/problems/greatest-common-divisor-of-strings/)
```java
class Solution {
    public String gcdOfStrings(String str1, String str2) {
        if (str2.length() > str1.length()) {
            String temp = str1;
            str1 = str2;
            str2 = temp;
        }

        for (int i = str2.length(); i > 0; i--) {
            if (str1.length() % i == 0 && str2.length() % i == 0) {
                String sub = str2.substring(0, i);
                if (checkDiv(sub, str1) && checkDiv(sub, str2)) {
                    return sub;
                }
            }
        }
        return "";
    }

    public boolean checkDiv(String sub, String str) {
        for (int i = 0; i < str.length(); i += sub.length()) {
            for (int j = 0; j < sub.length(); j++) {
                if (sub.charAt(j) != str.charAt(i + j)) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

[1002. Find Common Characters](https://leetcode.com/problems/find-common-characters/)
```java
class Solution {
    public List<String> commonChars(String[] words) {
        ArrayList<String> ans = new ArrayList<>();
        int[] first = new int[26];
        int[] current = new int[26];
        for (char c : words[0].toCharArray()) {
            first[c - 'a']++;
        }
        
        for (int i = 1; i < words.length; i++) {
            Arrays.fill(current, 0);
            for (char c : words[i].toCharArray()) {
                current[c - 'a']++;
            }
            for (int j = 0; j < 26; j++) {
                first[j] = Math.min(first[j], current[j]);
            }
        }

        for (int i = 0; i < 26; i++) {
            while (first[i] > 0) {
                ans.add(String.valueOf((char) (i + 'a')));
                first[i]--;
            }
        }
        
        return ans;
    }
}
```


[761. Special Binary String](https://leetcode.com/problems/special-binary-string/)
#hard #strings #sorting #recursion
```java
class Solution {
    public String makeLargestSpecial(String s) {
        return helper(s);
    }

    public String helper(String s){
        if(s.length() <= 2){ // 10 smallest possible valid
            return s;
        }

        int i = 0;
        int j = 0;
        int sum = 0;
        ArrayList<String> lst = new ArrayList<>();
        while(i < s.length()){           // break into multiple valids and run recursion on them
            if(s.charAt(i) == '1'){
                sum += 1;
            }
            else if(s.charAt(i) == '0'){
                sum -= 1;
            }
            
            if(sum == 0){ // recurrence on valid parts 
                String st = s.substring(j+1, i); 
                String temp = helper(st);
                lst.add("1" + temp + "0");
                j = i+1;
            }
            i++;
        }

        lst.sort((a, b) -> b.compareTo(a));
        return String.join("", lst);
    }
}
```