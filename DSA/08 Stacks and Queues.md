Implement Queue using 2 stacks : 
1. Remove efficient stack
```java
class RemoveEfficient{
	Stack<Integer> first;
	Stac<Integer>  second;

	public RemoveEfficient(){
		first = new Stack<>();
		second = new Stack<>();
	}

	public void insert(int data){
		while(!first.isEmpty()){
			second.push(first.pop());
		}
		first.push(data);
		while(!second.isEmpty()){
			first.push(second.pop());
		}
	}
	public bolean isEmpty(){
		return first.isEmpty();
	}
	public int remove(){
		return first.pop();
	}
	public int peek(){
		return first.peek();
	}
}
```
2. Insert Efficient Stack
```java
class InsertEfficient{
	Stack<Integer> first;
	Stack<Integer> second;
	public InsertEfficient(){
		first = new Stack<>();
		second = new Stack<>();
	}

	public void inset(int data){
		first.push(data);
	}
	public int remove(){
		while(!first.isEmpty()){
			second.push(first.pop());
		}
		int removed = second.pop();
		while(!second.isEmpty()){
			first.push(second.pop());
		}
		return removed;
	}
	public boolean isEmpty(){
		return first.isEmpty();
	}
	public int peek(){
		while(!first.isEmpty()){
			second.push(first.pop());
		}
		int peeked = second.peek();
		while(!second.isEmpty()){
			first.push(second.pop());
		}
		return peeked;
	}
}
```

TWO STACKS :
- You are given two stacks of integers
- `[4, 2, 4, 6, 1] & [2, 1, 8, 5]` and target = 10
- at a time you can pop from any one stack until the sum of numbers popped < target and count how many maximum possible steps you can make
- Answer uses recursion :
```java
static int twoStacks(int x, int [] a, int[] b){
	return twoStacks(x, a, b, 0, 0) -1;
}
private static int twoStacks(int x, int[] a, int[] b, int sum, int count){
	if(sum > x){
		return count;
	}
	if(a.length == 0 || b.length == 0){
		return count;
	}
	
	int ans1 = twoStacks(x, Arrays.copyOfRange(a, 1, a.length), b, sum + a[0], count + 1);
	int ans2 = twoStacks(x, a, Arrays.copyOfRange(b, 1, b.length), sum + b[0], count + 1);
	
	return Math.max(ans1, ans2);
}
```

Check if the parenthesis are valid in a string expression  consisting of only parenthesis: 
```java
public static boolean validParenthesis(String s) {
    Stack<Character> stack = new Stack<>();

    for (char ch : s.toCharArray()) {
        if (ch == '(' || ch == '{' || ch == '[') {
            stack.push(ch);
        } 
        else if (ch == ')') {
            if (stack.isEmpty() || stack.pop() != '(') return false;
        } 
        else if (ch == '}') {
            if (stack.isEmpty() || stack.pop() != '{') return false;
        } 
        else if (ch == ']') {
            if (stack.isEmpty() || stack.pop() != '[') return false;
        }
    }
    return stack.isEmpty();
}

```

Minimum parenthesis needed to make the string valid
```java
public static int minAddToMakeValid(String s){
	Stack<Character> stack = new Stack<>();
	for(char ch : s.toCharArray<>()){
		if(ch == ')'){
			if(!stack.isEmpty() && stack.peek() == '('){
				stack.pop();
			}else{
				stack.push(ch);
			}
		} else { // if ch == '('
			stack.push(ch);
		}
	}
	return stack.size();
}
```

Largest Area Histogram 
[84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)
```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        Stack<Integer> stack = new Stack<>();
        int[] prevSmallElement = new int[heights.length];
        int[] nextSmallElement = new int[heights.length];
        
        for(int i= 0; i<heights.length; i++){
            while(!stack.isEmpty() && heights[stack.peek()] >= heights[i]){
                stack.pop();
            }
            if(!stack.isEmpty()){
                prevSmallElement[i] = stack.peek();
            }else{
                prevSmallElement[i] = -1;
            }
            stack.push(i);
        }

        stack = new Stack<>();
        for(int i = heights.length - 1; i>=0; i--){
            while(!stack.isEmpty() && heights[stack.peek()] >= heights[i]){
                stack.pop();
            }
            if(!stack.isEmpty()){
                nextSmallElement[i] = stack.peek();
            }else{
                nextSmallElement[i] = heights.length;
            }
            stack.push(i);
        }

        int max = 0;
        for(int i = 0; i<heights.length; i++){
            int currMax = (nextSmallElement[i] - prevSmallElement[i] - 1) * heights[i];
            max = Math.max(currMax, max);
        }

        return max;
    }
}
```

[71. Simplify Path](https://leetcode.com/problems/simplify-path/)
```java
class Solution {
    public String simplifyPath(String path){
        StringBuilder str = new StringBuilder();
        Stack<String> st = new Stack<>();
        for(int i = 1; i<path.length(); i++){
            str = new StringBuilder();
            while(i < path.length() && path.charAt(i) != '/'){
                str.append(path.charAt(i));
                i++;
            }
            if(str.toString().equals(".") || str.isEmpty()){
                continue;
            }else if(str.toString().equals("..")){
                if(!st.isEmpty()){
                    st.pop();
                }
            }else {
                st.push(str.toString());
            }
        }
        str = new StringBuilder();

        boolean isFirst = true;

        while(!st.isEmpty()){
            String string = st.pop();
            if(isFirst){
                    str.insert(0, string);
                    isFirst = false;
                    continue;
                }
                str.insert(0, string + "/");
        }
        return "/" + str.toString();
    }
}
```

# LEETCODE QUESTIONS

[1717. Maximum Score From Removing Substrings](https://leetcode.com/problems/maximum-score-from-removing-substrings/)
Uses Stacks and Greedy Algorithm
```java
class Solution {
    public int maximumGain(String s, int x, int y) {
        if(x > y){
            return calculate(s, 'a', 'b', x, y);
        }else{
            return calculate(s, 'b', 'a', y, x);
        }
    }

    public int calculate(String s, char first, char second, int firstValue, int secondValue){
        Stack<Character> stack = new Stack<>();
        int score = 0;

        for(char c : s.toCharArray()){
            if(!stack.isEmpty() && stack.peek() == first && c == second){
                stack.pop();
                score += firstValue;
            }
            else{
                stack.push(c);
            }
        }

        Stack<Character> stack2 = new Stack<>();
        while(!stack.isEmpty()){
            char c = stack.pop();
            if(!stack2.isEmpty() && c == second  && stack2.peek() == first){
                stack2.pop();
                score += secondValue;
            }
            else{
                stack2.push(c);
            }
        }

        return score;
    }
}
```


[143. Reorder List](https://leetcode.com/problems/reorder-list/)
```java
class Solution {
    public void reorderList(ListNode head) {
        // find mid
        ListNode fast = head;
        ListNode slow = head;
        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode dummy = slow;
        
        // push second half into stack
        Stack<ListNode> stack = new Stack<>();
        while(slow != null){
            stack.push(slow);
            slow = slow.next;
        }

        // actual megic
        ListNode curr = head;
        ListNode next = head.next;

        while(!stack.isEmpty()){
            if(curr != null && curr.next != dummy){
                next = curr.next;
            }else if(curr.next == dummy){
                while(!stack.isEmpty()){
                    curr.next  = stack.pop();
                    if(curr != null)curr = curr.next;
                }
                if(curr != null){
                    curr.next = null;
                }
                return;
            }
            curr.next = stack.pop();
            curr.next.next = next;
            curr = next;
            
            
        }
    }
}
```

[503. Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/)
```java
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];
        Arrays.fill(ans, -1);

        Stack<Integer> stack = new Stack<>();

        for(int i =0 ;i < 2* n; i++){
            int idx = i % n;

            while(!stack.isEmpty() && nums[stack.peek()] < nums[idx]){
                ans[stack.pop()] = nums[idx];
            }

            if(i < n){
                stack.push(idx);
            }
        }

        return ans;
    }
}
```


[316. Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters/)
```java
class Solution {
    public String removeDuplicateLetters(String s) {
        HashMap<Character, Integer> lastIndex = new HashMap<>();
        HashSet<Character> seen = new HashSet<>();
        Stack<Character> stack = new Stack<>();

        for(int i = 0; i< s.length(); i++){
            lastIndex.put(s.charAt(i), i);
        }
        stack.push(s.charAt(0));
        seen.add(s.charAt(0));

        for(int i = 1; i<s.length(); i++){
            if(seen.contains(s.charAt(i))){
                        continue;
                    }
            if(!stack.isEmpty()){
                while(!stack.isEmpty() && 
		                (stack.peek() > s.charAt(i) && 
		                i < lastIndex.get(stack.peek()))){
                    seen.remove(stack.pop());
                }
                stack.push(s.charAt(i));
                seen.add(s.charAt(i));
            }
        }

        StringBuilder st = new StringBuilder();
        while(!stack.isEmpty()){
            st.append(stack.pop());
        }

        st.reverse();

        return st.toString();
    }
}
```

[445. Add Two Numbers II](https://leetcode.com/problems/add-two-numbers-ii/)
```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        Stack<Integer> s1 = new Stack<>();
        Stack<Integer> s2 = new Stack<>();
        Stack<Integer> ans = new Stack<>();

        ListNode node1 = l1;
        ListNode node2 = l2;

        while(l1 != null){
            s1.push(l1.val);
            l1 = l1.next;
        }
        while(l2 != null){
            s2.push(l2.val);
            l2 = l2.next;
        }
        int rem = 0;

        while(!s1.isEmpty() || !s2.isEmpty()){

            int sum = rem;
            if(!s1.isEmpty()){
                sum += s1.pop();
            }
            if(!s2.isEmpty()){
                sum += s2.pop();
            }

            ans.push(sum % 10);
            rem = sum / 10;
        }

        if(rem > 0){
            ans.push(rem);
        }

        ListNode result = new ListNode(ans.pop(), null);
        ListNode head = result;

        while(!ans.isEmpty()){
            result.next = new ListNode(ans.pop(), null);
            result = result.next;
        }

        return head;
    }
}
```

[402. Remove K Digits](https://leetcode.com/problems/remove-k-digits/)
```java
class Solution {
    public String removeKdigits(String num, int k) {
        Stack<Character> stack = new Stack<>();

        if(k == num.length()){
            return "0";
        }

        for(int i = 0; i< num.length(); i++){
            while(!stack.isEmpty() && k > 0){
                if(num.charAt(i) < stack.peek()){
                    stack.pop();
                    k--;
                }else{
                    break;
                }
            }
            stack.push(num.charAt(i));
        }

        while(k > 0 && !stack.isEmpty()){
            stack.pop();
            k--;
        }

        StringBuilder ans = new StringBuilder();

        while(!stack.isEmpty()){
            ans.append(stack.pop());
        }
        ans.reverse();

        // trim initial 0's
        while(ans.length() > 1 && ans.charAt(0) == '0'){
            ans.deleteCharAt(0);
        }

        return ans.toString();
    }
}
```

[897. Increasing Order Search Tree](https://leetcode.com/problems/increasing-order-search-tree/)
```java
class Solution {
    public TreeNode increasingBST(TreeNode root) {
        Stack<TreeNode> stack = new Stack<>();
        helper(root, stack);

        TreeNode head = stack.pop();
        head.left = null;
        TreeNode tempHead = head;

        while(!stack.isEmpty()){
            head.right = stack.pop();
            head = head.right;
            head.left = null;
        }
        head.right = null;

        return tempHead;
    }

    public void helper(TreeNode node, Stack<TreeNode> stack){
        if(node == null)return;

        if(node.right != null){
            helper(node.right, stack);
        }
        stack.push(node);
        if(node.left != null){
            helper(node.left, stack);
        }
    }
}
```

[1700. Number of Students Unable to Eat Lunch](https://leetcode.com/problems/number-of-students-unable-to-eat-lunch/)
```java
class Solution {
    public int countStudents(int[] students, int[] sandwiches) {
        Queue<Integer> queue = new LinkedList<>();

        for(int i = 0; i< students.length; i++){
            queue.offer(students[i]);
        }

        for(int i = 0; i < sandwiches.length; i++){
            int size = queue.size();
            boolean eaten = false;

            for(int j = 0; j < size ; j++){
                if(sandwiches[i] == queue.peek()){
                    queue.poll();
                    eaten = true;
                    break;
                }else{
                    queue.offer(queue.poll());
                }
            }
            if(!eaten){
                break;
            }
        }

        return queue.size();
    }
}
```