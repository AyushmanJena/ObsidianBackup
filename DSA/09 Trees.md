Populate a Binary Search Tree from a given unsorted array : 
**Populate Sorted**
```java
	public void populate(int[] nums){
		for(int i = 0; i<nums.length; i++){
			this.insert(nums[i]);
		}
	}

	public void insert(int value){
		root = insert(value, root);
	}
	public Node insert(int value, Node node){
		if(node == null){
			node = new Node(value);
			return node;
		}
		if(value < node.value){
			node.left = insert(value, node.left);
		}
		if(value > node.value){
			node.right = insert(value, node.right);
		}
		return node;
	}
```

Check if a Binary tree is Balanced : 
```java
public boolean balanced(){
	return balanced(root);
}

public boolean balanced(Node node){
	if(node == null){
		return true;
	}
	return Math.abs(height(node.left) - height(node.right)) <= 1 && balanced(node.right) && balanced(node.left);
}

publiv int height(Node node){
	if(node == null){
		return 0;
	}
	return 1 + Math.max(height(node.left), height(node.right));
}
```


AVL Tree : (Rotating Binary Tree) #revise 
AVL -> Adelson Velskil and Landis
- Self Balancing Binary Tree
```java
public void populate(int value){
	root = insert(value, root);
}
publiv Node insert(int value, Node node){
	if(node == null){
		node = new Node(value);
		return node;
	}
	if(value < node.value){
		node.left = insert(value, node.left);
	}
	if(valuse > node.value){
		node.right = insert(value, node.right);
	}
	
	node.height = Math.max(height(node.left), height(node.right)) + 1;
	return rotate(node);
}

private Node rotate(Node node){
	// left heavy (case l-l or l-r)
	if((height(node.left) - height(node.right)) > 1){ 
		// left-left case
		if((height(node.left.left) - height(node.left.right)) > 0){
			return rightRotate(node);
		}
		// left-right case
		if((height(node.left.left) - height(node.left.right)) < 0) {
			node.left = leftRotate(node.left);
			return rightRotate(node);
		}
	}

	// right heavy case (r-l or r-r)
	if((height(node.left) - height(node.right)) < -1){
		// right-right case
		if((height(node.right.left) - height(node.right.right)) < 0){
			return leftRotate(node);
		}
		if((height(node.right.left) - height(node.right.right)) > 0){
			node.right = rightRotate(node.right);
			return leftRotate(node);
		}
	}
	return node;
}

private Node rightRotate(Node p){
	Node c = p.left;
	Node t = c.right; // t3; t1 and t2 remain unchanged

	c.right = p;
	p.left = t;

	// update height
	p.height = Math.max(height(p.left), height(p.right)) + 1;
	c.height = Math.max(height(c.left), height(c.right)) + 1;
	return c;
}

private Node leftRotate(Node c){
	Node p = c.right;
	Node t = p.left; // t2; t1 and t3 remain unchanged

	p.left = c;
	c.right = t;

	// update height
	p.height = Math.max(height(p.left), height(p.right)) + 1;
	c.height = Math.max(height(c.left), height(c.right)) + 1;
	return p;
}
```


Segment Tree 
- Find sum of indices from given start till end in an array
- segment : perform query on a range
- can be sum or some other operations as well
```java
public SegmentTree(int[] arr){
	this.root = constructTree(arr, 0, arr.length -1);
}
private Node constructTree(int[] arr,  int start, int end){
	if(start == end){
		// we are at leaf node (single element)
		Node leaf = new Node(start, end);
		leaf.data = arr[start];
		return leaf;
	}
	// create new node with index you are at
	Node node = new Node(start, end);
	int mid = (start + end)/2;

	node.left = this.constructTree(arr, start, mid);
	node.right = this.constructtree(arr, mid+1, end);

	node.data = node.left.data + node.right.data;
	return node;
}

// Querying a segment tree (sum in this case with query start index and end index)
public int query(int qsi, int qei){
	return this.query(this.root, qsi, qei);
}
private int query(Node node, int qsi, int qei){
	// case 1
	if(node.startInterval >= qsi && node.endInterval <= qei){
		// node is lyiing inside the query
		return node.data;
	}
	// case 2
	else if(node.startInterval > qei || node.endInterval < qsi){
		// completely outside the query range
		return 0;
	}
	// case 3
	else{
		return this.query(node.left, qsi, qei) + this.query(node.right, qsi, qei);
	}
}

// Updating index of a segment tree
public void update(int index, int value){
	this.root.data = update(this.root, index, value);
}

private int update(Node node , int index, int value){
	if(index >= node.startInterval && index <= node.endInterval){
		if(index == node.startInterval && index == endInterval){
			node.data = value;
			return node.data;
		}else{
			int leftAns = update(node.left, index, value);
			int rightAns = update(node.right, index, value);
			node.data = leftAns + rightAns;
			return node.data;
		}
	}
	return node.data;
}
```


# LeetCode Questions


[1367. Linked List in Binary Tree](https://leetcode.com/problems/linked-list-in-binary-tree/)
```java
class Solution {
    public boolean isSubPath(ListNode head, TreeNode root) {
        if (root == null)
            return false;
        return solve(head, root) || isSubPath(head, root.left) || isSubPath(head, root.right);
    }

    private boolean solve(ListNode head, TreeNode root) {
        if (head == null)
            return true;
        if (root == null)
            return false;
        return head.val == root.val && (solve(head.next, root.left) || (solve(head.next, root.right)));
    }
}
```

[226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)
```java
class Solution {
    public TreeNode invertTree(TreeNode node) {
        if(node == null){
            return null;
        }
        TreeNode left = invertTree(node.left);
        TreeNode right = invertTree(node.right);
        node.left = right;
        node.right = left;
        return node;
    }
}
```


[1325. Delete Leaves With a Given Value](https://leetcode.com/problems/delete-leaves-with-a-given-value/)
```java
class Solution {
    public TreeNode removeLeafNodes(TreeNode node, int target) {
        if(node == null){
            return null;
        }
        node.left = removeLeafNodes(node.left, target);
        node.right = removeLeafNodes(node.right, target);
        if(node.left == null && node.right == null){//leaf
            if(node.val == target){
                return null;
            }
            return node;
        }
        return node;
    }
}
```

[404. Sum of Left Leaves](https://leetcode.com/problems/sum-of-left-leaves/)
```java
class Solution {
    public int sumOfLeftLeaves(TreeNode root) {
        return sumOfLeftLeaves(root.left, true) + sumOfLeftLeaves(root.right, false);
    }
    public int sumOfLeftLeaves(TreeNode node, boolean isLeft){
        if(node == null){
            return 0;
        }
        if((node.left == null && node.right == null) && isLeft){
            return node.val;
        }

        int leftVal = sumOfLeftLeaves(node.left, true);
        int rightVal = sumOfLeftLeaves(node.right, false);
        return leftVal + rightVal;
    }
}
```


[230. Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

```java
class Solution {
    public int kthSmallest(TreeNode root, int k) {
        int n = 0;
        Stack<TreeNode> stack = new Stack<>();
        TreeNode curr = root;

        while(curr != null || !stack.isEmpty()){
            while(curr != null){
                stack.push(curr);
                curr = curr.left;
            }
            curr = stack.pop();
            n += 1;
            if(n == k){
                return curr.val;
            }

            curr = curr.right; 
        }

        return curr.val;
    }
}
```

[94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)
```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        helper(root, list);
        return list;
    }

    public void helper(TreeNode node, List<Integer> list){
        if(node == null){
            return;
        }
        if(node.left == null && node.right == null){
            list.add(node.val);
            return;
        }
        if(node.left != null){
            helper(node.left, list);
        }
        list.add(node.val);
        if(node.right != null){
            helper(node.right, list);
        }
    }
}
```

[114. Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)
#revise 
```java
class Solution {
    public void flatten(TreeNode root) {
        helper(root);
    }

    public TreeNode helper(TreeNode curr){
        if(curr == null){
            return null;
        }
        TreeNode leftTail = helper(curr.left);
        TreeNode rightTail = helper(curr.right);

        if(leftTail != null){
            leftTail.right = curr.right;
            curr.right = curr.left;
            curr.left = null;
        }

        if(rightTail != null)return rightTail; // if right subtree exists
        if(leftTail != null) return leftTail;  // if right subtree does not exist but left does
        return curr; // if leaf node
    }
}
```

[106. Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

```java
class Solution {
    int postIndex;
    HashMap<Integer, Integer> inorderIndex;
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        postIndex = postorder.length -1;
        inorderIndex = new HashMap<>();

        for(int i =0 ; i< inorder.length; i++){
            inorderIndex.put(inorder[i] , i);
        }

        return helper(postorder, 0, inorder.length - 1);
    }
    private TreeNode helper(int[] postorder, int left, int right){
        if(left > right) return null;

        int rootVal = postorder[postIndex--];
        TreeNode root = new TreeNode(rootVal);

        int idx = inorderIndex.get(rootVal);

        root.right = helper(postorder, idx+1, right);
        root.left = helper(postorder, left, idx-1);

        return root;
    }
}
```

[897. Increasing Order Search Tree](https://leetcode.com/problems/increasing-order-search-tree/)
```java
class Solution {
    TreeNode prev = null;
    TreeNode head = null;
    public TreeNode increasingBST(TreeNode root) {
        helper(root);
        return head;
    }

    public void helper(TreeNode node){
        if(node == null){
            return;
        }

        helper(node.left);

        if(prev == null){
            head = node;
        }
        else{
            prev.right = node;
        }
        node.left = null;
        prev = node;

        helper(node.right);
    }
}
```

solved using stack : 
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
```