TRIE Implementation
```java
public class TrieImpl {
    static class Node {
        Node[] links = new Node[26];
        boolean flag = false;

        boolean containsKey(char ch) {
            return links[ch - 'a'] != null;
        }

        void put(char ch, Node node) {
            links[ch - 'a'] = node;
        }
        Node get(char ch) {
            return links[ch - 'a'];
        }

        void setEnd() {
            flag = true;
        }

        boolean isEnd() {
            return flag;
        }
    }

    private Node root;

    public Trie() {
        root = new Node();
    }

    // Inserts a word into the Trie
    // Time Complexity O(len), where len
    // is the length of the word
    public void insert(String word) {
        Node node = root;
        for (int i = 0; i < word.length(); i++) {
            if (!node.containsKey(word.charAt(i))) {
                // Create a new node for letter if not present
                node.put(word.charAt(i), new Node());
            }
            // Move to the next node
            node = node.get(word.charAt(i));
        }
        // Mark the end of the word
        node.setEnd();
    }

    public boolean search(String word) {
        Node node = root;
        for (int i = 0; i < word.length(); i++) {
            if (!node.containsKey(word.charAt(i))) {
                return false;
            }
            node = node.get(word.charAt(i));
        }
        // Check if the last node marks the end of a word
        return node.isEnd();
    }

    // Returns if there is any word in the trie that starts with the given prefix
    public boolean startsWith(String prefix) {
        Node node = root;
        for (int i = 0; i < prefix.length(); i++) {
            if (!node.containsKey(prefix.charAt(i))) {
                return false;
            }
            node = node.get(prefix.charAt(i));
        }
        // The prefix is found in the Trie
        return true;
    }

    public static void main(String[] args) {
        Trie trie = new Trie();
        System.out.println("Inserting words: Striver, Striving, String, Strike");
        trie.insert("striver");
        trie.insert("striving");
        trie.insert("string");
        trie.insert("strike");

        System.out.println("Search if Strawberry exists in trie: " +
                (trie.search("strawberry") ? "True" : "False"));

        System.out.println("Search if Strike exists in trie: " +
                (trie.search("strike") ? "True" : "False"));

        System.out.println("If words in Trie start with Stri: " +
                (trie.startsWith("stri") ? "True" : "False"));
    }
}
```

TRIE Implementation 2 
Keep a count of how many words exist 
```java
class TrieImpl2 {
    static class Node {
        Node[] links;
        // Counter for number of words that end at this node
        int cntEndWith;
        // Counter for number of words that have this node as a prefix
        int cntPrefix;

        Node() {
            links = new Node[26];
            cntEndWith = 0;
            cntPrefix = 0;
        }

        boolean containsKey(char ch) {
            return links[ch - 'a'] != null;
        }

        Node get(char ch) {
            return links[ch - 'a'];
        }

        void put(char ch, Node node) {
            links[ch - 'a'] = node;
        }

        void increaseEnd() {
            cntEndWith++;
        }

        void increasePrefix() {
            cntPrefix++;
        }

        void deleteEnd() {
            cntEndWith--;
        }

        void reducePrefix() {
            cntPrefix--;
        }
    }

    private Node root;

    Trie() {
        root = new Node();
    }

    void insert(String word) {
        Node node = root;
        for (int i = 0; i < word.length(); i++) {
            if (!node.containsKey(word.charAt(i))) {
                node.put(word.charAt(i), new Node());
            }
            node = node.get(word.charAt(i));
            node.increasePrefix();
        }
        node.increaseEnd();
    }

    int countWordsEqualTo(String word) {
        Node node = root;
        for (int i = 0; i < word.length(); i++) {
            if (node.containsKey(word.charAt(i))) {
                node = node.get(word.charAt(i));
            } else {
                return 0;
            }
        }
        return node.cntEndWith;
    }

    int countWordsStartingWith(String word) {
        Node node = root;
        for (int i = 0; i < word.length(); i++) {
            if (node.containsKey(word.charAt(i))) {
                node = node.get(word.charAt(i));
            } else {
                // Return 0 if the character is not found
                return 0;
            }
        }
        return node.cntPrefix;
    }

    void erase(String word) {
        Node node = root;
        for (int i = 0; i < word.length(); i++) {
            if (node.containsKey(word.charAt(i))) {
                node = node.get(word.charAt(i));
                node.reducePrefix();
            } else {
                return;
            }
        }
        node.deleteEnd();
    }
}

public class Main {
    public static void main(String[] args) {
        Trie trie = new Trie();
        trie.insert("apple");
        trie.insert("app");
        System.out.println("Inserting strings 'apple', 'app' into Trie");
        System.out.print("Count Words Equal to 'apple': ");
        System.out.println(trie.countWordsEqualTo("apple"));
        System.out.print("Count Words Starting With 'app': ");
        System.out.println(trie.countWordsStartingWith("app"));
        System.out.println("Erasing word 'app' from trie");
        trie.erase("app");
        System.out.print("Count Words Equal to 'apple': ");
        System.out.println(trie.countWordsEqualTo("apple"));
        System.out.print("Count Words Starting With 'apple': ");
        System.out.println(trie.countWordsStartingWith("app"));
    }
}
                            
                        
```


Complete String
Given array of strings
`['n', 'ninja', 'ninj', 'ni', 'nin', 'ninga']`

Find the Longest Word which has all prefixes available in the string
```java
class Node{
    Node links[] = new Node[26];
    boolean flag = false;
    
    public Node(){
        
    }
    boolean containsKey(char ch)
    {
        return (links[ch -'a'] != null);
    }
    
    void put(char ch, Node node)
    {
        links[ch-'a'] = node;
    }
    Node get(char ch)
    {
        return (links[ch-'a']); // ch -'a' gives corresponding ascii of the particular alphabet
    }
    void setEnd()
    {
        flag = true;
    }
    boolean isEnd()
    {
        return flag;
    }
}
class Trie{
     private static Node root;
     //whenever we require object of this trie we know we'll be needing a new trie 
     Trie(){
         root = new Node();
     }
     public static void insert(String word)
     {
         Node node = root;
        for(int i = 0;i<word.length();i++) 
        {
            if(!node.containsKey(word.charAt(i))) {
                node.put(word.charAt(i), new Node()); 
            }
            node = node.get(word.charAt(i)); 
        }
        node.setEnd(); 
     }
      boolean checkIfAllPrefixExists(String word) 
      {
        Node node = root;
        boolean flag = true; 
        for(int i = 0;i<word.length() && flag;i++) {
            if(node.containsKey(word.charAt(i))) {
                node = node.get(word.charAt(i)); 
                flag = flag && node.isEnd(); 
            }
            else {
                return false; 
            } 
        }
        return flag; 
    }
 }
public class completeString {

  public static String completeString(int n, String[] a) {
    Trie obj = new Trie();
      for(int i =0; i<n; i++)
      {
          obj.insert(a[i]);
      }
      String longest = "";
      for(int i= 0; i<n; i++)
      {
          if(obj.checkIfAllPrefixExists(a[i])) // checks if all the prefixes of the string exist
          {
              if(a[i].length() > longest.length()) // the longer string
              {
                  longest = a[i]; 
              }
               else if(a[i].length() == longest.length() && a[i].compareTo(longest) < 0) // compare if same length
               {
                longest = a[i]; 
               }
          }
      }
      if(longest == "") return "None"; 
    return longest;

  }
}

```

Number of Distinct Substrings
abab -> a, ab, aba, abab, b, ba, bab (8)
```java
import java.util.HashMap;

class Node {
    Node[] links;  
    boolean flag;  

    public boolean containsKey(char ch) {  
        return links[ch - 'a'] != null;
    }

    public Node get(char ch) {  
        return links[ch - 'a'];
    }

    public void put(char ch, Node node) {  
        links[ch - 'a'] = node;
    }

    public void setEnd() {  
        flag = true;
    }
    public boolean isEnd() {  
        return flag;
    }
}

public class NumberOfDistinceSubStrings {  
    public static int countDistinctSubstrings(String s) {  
        Node root = new Node();  
        int cnt = 0;  
        int n = s.length();  

        for (int i = 0; i < n; i++) {  
            Node node = root;  
            for (int j = i; j < n; j++) {  
                if (!node.containsKey(s.charAt(j))) {
                    node.put(s.charAt(j), new Node());  
                    cnt++;  
                }
                node = node.get(s.charAt(j));  // move to next char
            }
        }
        return cnt + 1;  // +1 for empty string
    }

    public static void main(String[] args) {  
        String s = "striver";  
        System.out.println("Current String: " + s);
        System.out.println("Number of distinct substrings: " + countDistinctSubstrings(s));  
    }
}                         
```


Maximum XOR of 2 Numbers in 2 given arrays
`[6, 8] and [7, 8, 2]`
8 xor 7 gives 15

```java
import java.util.ArrayList;

// Node class
// for the Trie
class Node {
    // Array to store links
    // to child nodes (0 and 1)
    Node[] links;
    
    // Constructor
    Node() {
        links = new Node[2];
    }

    // Method to check if a specific
    // bit key is present in the child nodes
    boolean containsKey(int bit) {
        // Returns true if the link at
        // index 'bit' is not null
        return links[bit] != null;
    }

    // Method to get the child node
    // corresponding to a specific bit
    Node get(int bit) {
        // Returns the child
        // node at index 'bit'
        return links[bit];
    }

    // Method to set a child node at a
    // specific index in the links array
    void put(int bit, Node node) {
        // Sets the child node at index
        // 'bit' to the provided node
        links[bit] = node;
    }
}

// Trie class
class Trie {
    // Root node of the Trie
    Node root;

    // Constructor to initialize
    // the Trie with a root node
    Trie() {
        // Creates a new root
        // node for the Trie
        root = new Node();
    }

    // Method to insert a number into the Trie
    void insert(int num) {
        // Start from the root node
        Node node = root;
        // Iterate through each bit of the
        // number (from left to right)
        for (int i = 31; i >= 0; i--) {
            // Extract the i-th bit of the number
            int bit = (num >> i) & 1;

            // If the current node doesn't have a
            // child node with the current bit
            if (!node.containsKey(bit)) {

                // Create a new child node
                // with the current bit
                node.put(bit, new Node());
            }

            // Move to the child node
            // corresponding to the current bit
            node = node.get(bit);
        }
    }

    // Method to find the maximum
    // XOR value for a given number
    int getMax(int num) {
        // Start from the root node
        Node node = root;

        // Initialize the maximum XOR value
        int maxNum = 0;

        // Iterate through each bit of
        // the number (from left to right)
        for (int i = 31; i >= 0; i--) {

            // Extract the i-th
            // bit of the number
            int bit = (num >> i) & 1;

            // If the complement of the current
            // bit exists in the Trie
            if (node.containsKey(1 - bit)) {

                // Update the maximum XOR
                // value with the current bit
                maxNum |= (1 << i);

                // Move to the child node corresponding
                // to the complement of the current bit
                node = node.get(1 - bit);
            } else {

                // Move to the child node
                // corresponding to the current bit
                node = node.get(bit);
            }
        }

        // Return the maximum XOR value
        return maxNum;
    }
}

public class MaximumXorOfTwoNoInTwoArrays {
    // Function to find the maximum XOR
    // value between two sets of numbers
    static int maxXOR(int n, int m, ArrayList<Integer> arr1, ArrayList<Integer> arr2) {
        // Create a Trie object
        Trie trie = new Trie();
        // Insert each number from
        // the first set into the Trie
        for (int it : arr1) {
            trie.insert(it);
        }

        // Initialize the maximum XOR value
        int maxi = 0;

        // Iterate through each
        // number in the second set
        for (int it : arr2) {
            // Update the maximum XOR value
            // with the result from the Trie
            maxi = Math.max(maxi, trie.getMax(it));
        }
        // Return the
        // maximum XOR value
        return maxi;
    }

    // Function to print the
    // Input Arrays
    static void printArr(ArrayList<Integer> arr) {
        for (int it : arr) {
            System.out.print(it + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        ArrayList<Integer> arr1 = new ArrayList<>(java.util.Arrays.asList(3, 10, 5, 25, 2));
        ArrayList<Integer> arr2 = new ArrayList<>(java.util.Arrays.asList(8, 1, 2, 12, 7));
        int n = arr1.size();
        int m = arr2.size();

        System.out.print("Arr1: ");
        printArr(arr1);
        System.out.print("Arr2: ");
        printArr(arr2);

        int result = maxXOR(n, m, arr1, arr2);
        System.out.println("Maximum XOR value: " + result);
    }
}
```