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