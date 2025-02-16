[208. Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)
```java
class Trie {

    class Node{
        Node[] links = new Node[26];
        boolean flag = false;
    }
    Node root;

    public Trie() {
        root = new Node();
    }
    
    public void insert(String word) {
        Node node = root;
        for(int i = 0; i < word.length(); i++){
            if(node.links[word.charAt(i)- 'a'] == null){
                node.links[word.charAt(i) - 'a'] = new Node();
            }
            node = node.links[word.charAt(i)- 'a'];
        }
        node.flag = true;
    }
    
    public boolean search(String word) {
        Node node = root;
        for(int i = 0; i<word.length(); i++){
            if(node.links[word.charAt(i)- 'a'] == null){
                return false;
            }
            node = node.links[word.charAt(i)- 'a'];
        }
        return node.flag;
    }
    
    public boolean startsWith(String prefix) {
        Node node = root;
        for(int i = 0; i<prefix.length(); i++){
            if(node.links[prefix.charAt(i)- 'a'] == null){
                return false;
            }
            node = node.links[prefix.charAt(i)- 'a'];
        }
        return true;
    }
}
```

[1268. Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/) #hard 
```java
class Solution {
    class Trie {
        Trie[] sub = new Trie[26];
        LinkedList<String> suggestions = new LinkedList<>();
    }

    public List<List<String>> suggestedProducts(String[] products, String searchWord) {
        Trie root = new Trie();
        for(String p : products){
            insert(p, root);
        }        
        return search(searchWord, root);
    }

    private void insert(String p, Trie root){
        Trie t = root;
        for(char c : p.toCharArray()){
            if(t.sub[c-'a'] == null){
                t.sub[c-'a'] = new Trie();
            }
            t = t.sub[c-'a'];
            t.suggestions.offer(p);
            Collections.sort(t.suggestions);
            if(t.suggestions.size() > 3){
                t.suggestions.pollLast();
            }
        }
    }

    private List<List<String>> search(String searchWord, Trie root){
        List<List<String>> ans = new ArrayList<>();
        for(char c : searchWord.toCharArray()){
            if(root != null){
                root = root.sub[c-'a'];
            }
            ans.add(root == null ? Arrays.asList() : root.suggestions);
        }
        return ans;
    }

    
}
```