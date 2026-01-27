### BASIC OPERATIONS 

Insert node using recursion
```java
private Node insertRec(int val, int index, Node node){
	if(index == 0){
		Node temp = new Node(val, node);
		size++;
		return temp;
	}
	node.next = insertRec(val, index-1, node.next);
	return node;
}
```

Remove Duplicates in a sorted linkedlist
```java
public void removeDuplicates(){
	Node node = head;
	while(node.next != null){
		if(node.value == node.next.value){
			node.next = node.next.next;
			size--;
		}else{
			node = node.next;
		}
	}
	tail = node;
	tail.next = null;
}
```


Reverse Linked List
```java
public Node reverseList(Node head){
	if(head == null) {
		return head;
	}
	Node prev = null;
	Node present = head;
	Node next = present.next;

	while(present != null){
		present.next = prev;
		prev = present;
		present = next;
		if(next != null){
			next = next.next;
		}
	}
	return prev;
}
```


Palindrome Check
```java
public static boolean isPalindrome(Node head) {
	Node mid = middleNode(head);
	Node headSecond = reverseList(mid);
	Node rereverseHead = headSecond;

	while(head != null && headSecond != null){
		if(head.data != headSecond.data){
			break;
		}
		head = head.next;
		headSecond = headSecond.next;
	}
	reverseList(rereverseHead);
	return head == null || headSecond == null;
}
```

### Two pointer Questions 
Check if a LinkedList has a cycle or not
```java
public boolean hasCycle(Node head){
	Node fast = head;
	Node slow = head;

	while(fast != null && fast.next != null){
		fast = fast.next.next;
		slow = slow.next;
		if(slow == fast){
			return true;
		}
	}
	return false;
}
```

Find the length of the cycle in the linkedlist
```java
private int lengthCycle(Node head){
	Node fast = head;
	Node slow = head;

	while(fast != null && fast.next != null){
		fast = fast.next.next;
		slow = slow.next;
		if(fast == slow){
			// calculate length
			Node temp = slow;
			int length = 0;
			do{
				temp = temp.next;
				length++;
			}while(temp != slow);
			return length; 
		}
	}
	return 0;
}
```

Find the Node where the cycle starts
```java
public ListNode detectCycle(ListNode head) {
    ListNode fast = head;
    ListNode slow = head;
    Boolean cycle = false;

    while(fast != null && fast.next != null){
        fast = fast.next.next;
        slow = slow.next;
        if(fast == slow){
            cycle = true;
            break;
        }
    }
    if(!cycle){
        return null;
    }

    while(head != slow){
        slow = slow.next;
        head = head.next;
    }
    return head;
}
```

Remove nth node from the end of the linked list 
[19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)
```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0, head);
        ListNode l = dummy;
        ListNode r = head;

        while(n > 0 && r != null){
            r = r.next;
            n--;
        }

        while(r != null){
            l = l.next;
            r = r.next;
        }

        l.next = l.next.next;
        return dummy.next;
    }
}
```
# SORTING

Bubble Sort
```java
public static void bubbleSort(int row, int col){ // (n,0)
	if(row == 0){
		return;
	}
	if(col < row -1){
		Node first = get(col);
		Node second = get(ol + 1);

		if(first.val > second.val){
			// swap nodes
			if(first == head){
				head = second;
			}else{
				Node prev = get(col - 1);
				prev.next = second;
			}

			Node temp = second.next;
			second.next = first;
			first.next = temp;

			bubbleSort(row, col+1);
		}else{
			bubbleSort(row, col+1);
		}
	}else {
		bubbleSort(row-1, 0);
	}
}
```

Merge Sort 
- Using dummy head approach
```java
public static ListNode mergeSort(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }

    ListNode mid = getMid(head);
    ListNode left = mergeSort(head);
    ListNode right = mergeSort(mid);

    return merge(left, right);
}

public static ListNode merge(ListNode list1, ListNode list2) {
    ListNode dummyHead = new ListNode();
    ListNode tail = dummyHead;
    while (list1 != null && list2 != null) {
        if (list1.val < list2.val) {
            tail.next = list1;
            list1 = list1.next;
            tail = tail.next;
        } else {
            tail.next = list2;
            list2 = list2.next;
            tail = tail.next;
        }
    }
    tail.next = (list1 != null) ? list1 : list2;
    return dummyHead.next;
}

static ListNode getMid(ListNode head) {
    ListNode midPrev = null;
    while (head != null && head.next != null) {
        midPrev = (midPrev == null) ? head : midPrev.next;
        head = head.next.next;
    }
    ListNode mid = midPrev.next;
    midPrev.next = null; 
    //important, because its not just pointing anymore but splitting
    return mid;
}
```

### LeetCode :

[725. Split Linked List in Parts](https://leetcode.com/problems/split-linked-list-in-parts/)
```java
class Solution {
    public ListNode[] splitListToParts(ListNode head, int k) {
        ListNode[] ans = new ListNode[k];
        int length = 0;
        ListNode temp = head;
        while(head != null){
            head = head.next;
            length++;
        }

        int size = length/k;
        int extra = length%k;
        for(int i = 0; i<k; i++){

            if(temp == null){
                break;
            }

            ans[i] = temp;
            ListNode prev = null;
            for(int j = 0; j<size; j++){
                prev = temp;
                temp = temp.next;
            }
            if(extra > 0){
                prev = temp;
                temp = temp.next;
                extra--;
            }
            prev.next = null;
        }

        return ans;
    }
}
```


[3217. Delete Nodes From Linked List Present in Array](https://leetcode.com/problems/delete-nodes-from-linked-list-present-in-array/)
```java
class Solution {
    public ListNode modifiedList(int[] nums, ListNode head) {
        HashSet<Integer> map = new HashSet<>();

        for(int num : nums){
            map.add(num);
        }

        ListNode temp = new ListNode(0, head);
        ListNode curr = head;
        ListNode prev = temp;

        while(curr != null){

            if(map.contains(curr.val)){
                prev.next = curr.next;
            }
            else{
                prev = curr;
            }
            curr = curr.next;
        }

        return temp.next;
    }
}
```

[2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)
```java
 class ListNode {
      int val;
      ListNode next;
      ListNode() {}
      ListNode(int val) { this.val = val; }
      ListNode(int val, ListNode next) { this.val = val; this.next = next; }
  }

public class AddTwoNumUsingLL {
    public static void main(String[] args) {
        // testcase 1
//        int[] arr1 = {2, 4, 3};
//        int[] arr2 = {5, 6, 4};

        // testcase 2
//        int[] arr1 = {0};
//        int[] arr2 = {0};

        // testcase 3
        int[] arr1 = {9,9,9,9,9,9,9};
        int[] arr2 = {9,9,9,9};

        ListNode l1 = arrayToLinkedList(arr1);
        display(l1);


        ListNode l2 = arrayToLinkedList(arr2);
        display(l2);

        ListNode ans = addTwoNumbers(l1, l2);
        display(ans);
    }

    public static ListNode arrayToLinkedList(int[] arr){
        ListNode tail = new ListNode(arr[arr.length-1], null);

        for(int i = arr.length-2; i >= 0; i--){
            ListNode newNode = new ListNode();
            newNode.val = arr[i];
            newNode.next = tail;
            tail = newNode;
            if(i == 0){
                return newNode;
            }
        }
        return tail;
    }

    private static void display(ListNode l1){
        while(l1 != null){
            System.out.print(l1.val+ " ");
            l1 = l1.next;
        }
        System.out.println();
    }

    private static ListNode reverseList(ListNode l1) {
        if(l1.next == null){
            return l1;
        }
        ListNode head = l1;
        ListNode prev = null;
        ListNode present = head;
        ListNode next = present.next;

        while(present != null){
            present.next = prev;
            prev = present;
            present = next;
            if(next != null){
                next = next.next;
            }
        }
        return prev;
    }

    private static ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        // [2,4,9] + [5,6,4,9] = [7,0,4,0,1]
        ListNode newNode = new ListNode();
        ListNode head = newNode;
        int rem = 0;

        while (l1 != null || l2 != null) {
            int val1 = (l1 != null) ? l1.val : 0;
            int val2 = (l2 != null) ? l2.val : 0;
            newNode.val = (val1 + val2 + rem) % 10;
            rem = (val1 + val2 + rem) / 10;

            if (l1 != null) l1 = l1.next;
            if (l2 != null) l2 = l2.next;

            if (l1 != null || l2 != null) {
                newNode.next = new ListNode();
                newNode = newNode.next;
            }
        }

        if (rem != 0) {
            newNode.next = new ListNode(rem);
        }

        return head;
    }

}

```


[82. Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)
```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        ListNode dummy = new ListNode(0, head);
        ListNode prev = dummy;
        while (head != null) {
            if (head.next != null && head.val == head.next.val) {
                while (head.next != null && head.val == head.next.val) {
                    head = head.next;
                }
                prev.next = head.next;
            } else {
                prev = prev.next;
            }
            head = head.next;
        }
        return dummy.next;
    }
}
```


[2816. Double a Number Represented as a Linked List](https://leetcode.com/problems/double-a-number-represented-as-a-linked-list/)
```java
class Solution {
    public ListNode doubleIt(ListNode head) {
        ListNode node = head;
        int ret = doubleItRec(node);
        if(ret != 0){
            ListNode newNode = new ListNode(1, head);
            head = newNode;
        }
        return head;
    }
    public int doubleItRec(ListNode node){
        if(node.next == null){
            int temp = node.val * 2;
            node.val = temp%10;
            return temp/10;
        }
        int temp = (node.val * 2)+ doubleItRec(node.next);
        node.val = temp % 10;
        return temp/10;
    }
}
```


[234. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/)
```java
public boolean isPalindrome(ListNode head){
	ListNode mid = middleNode(head);
	ListNode secondHead = reverse(mid);

	while(head != null && secondHead != null){

		if(head.val != secondHead.val){
			return false;
		}

		head = head.next;
		secondHead = secondHead.next;
	}
	return true;

}

public ListNode middleNode(ListNode head){
	ListNode s = head;
	ListNOde f = head;
	wile(f != null && f.next != null){
		f = f.next.next;
		s = s.next;
	}
	return s;
}
public ListNode reverse(ListNode head){
	if(head == null){
		return head;
	}
	ListNode prev = null;
	ListNode present = head;
	ListNode next = present.next;

	while(present != null){
		present.next = prev;
		prev = present;
		present = next;
		if(next != null){
			next = next.next;
		}
	}
	return prev;
}
```


[1290. Convert Binary Number in a Linked List to Integer](https://leetcode.com/problems/convert-binary-number-in-a-linked-list-to-integer/)
```java
public class Solution {
    public static void main(String[] args){
        int a = power(2);
        System.out.println(a);
    }
    public int getDecimalValue(ListNode head) {
        int n = 0;
        ListNode node = head;
        while(node.next != null){
            n++;
            node = node.next;
        }
        node = head;
        int num = 0;
        while(node.next != null){
            num = num + node.val * power(n); 
            n--;
            node = node.next;
        }
        return num;
    }
    public static int power(int n){
        int ans = 1;
        while(n != 0){
            ans = ans * 2;
            n--;
        }
        return ans;
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

[23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)
```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if(lists.length == 0){
            return null;
        }

        ListNode ans = lists[0];

        int i = 1;
        while(i<lists.length){
            ans = merge(ans, lists[i]);
            i++;
        }

        return ans;
    }

    public ListNode merge(ListNode first, ListNode second){
        if(first == null) return second;
        if(second == null) return first;
        ListNode head;
        if(first.val < second.val){
            head = new ListNode(first.val, null);
            first = first.next;
        }else{
            head = new ListNode(second.val, null);
            second = second.next;
        }

        ListNode temp = head;
        

        while(first!=null && second != null){
            if(first.val < second.val){
                temp.next = first;
                first = first.next;
            }
            else{
                temp.next = second;
                second = second.next;
            }
            temp = temp.next;

        }

        if(first != null){
            temp.next = first;
        }

        if(second != null){
            temp.next = second;
        }

        return head;
    }
}
```
More optimised solution by taking two pairs as a time :
```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if(lists == null || lists.length == 0) return null;

        while(lists.length > 1) {
            List<ListNode> mergedLists = new ArrayList<>();
            for(int i=0;i<lists.length;i=i+2) {
                ListNode l1 = lists[i];
                ListNode l2 = (i+1 < lists.length) ? lists[i+1] : null;
                mergedLists.add(merge(l1,l2));
            }
            lists = mergedLists.toArray(new ListNode[mergedLists.size()]);
        }

        return lists[0];
    }

    ListNode merge(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(-1);
        ListNode tail = dummy;

        while(l1 != null && l2 != null) {
            if(l1.val <= l2.val) {
                tail.next = new ListNode(l1.val);
                l1 = l1.next;
            } else {
                tail.next = new ListNode(l2.val);
                l2 = l2.next;
            }
            tail = tail.next;
        }

        while(l1 != null) {
            tail.next = new ListNode(l1.val);
            l1 = l1.next;
            tail = tail.next;
        }

        while(l2 != null) {
            tail.next = new ListNode(l2.val);
            l2 = l2.next;
            tail = tail.next;
        }

        return dummy.next;
    }
}
```

[25. Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/) #revise 
```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode dummy = new ListNode(0, head);
        ListNode groupPrev = dummy;

        while(true){
            ListNode kth = getKth(groupPrev, k);
            if(kth == null){
                break;
            }

            ListNode groupNext = kth.next;

            // reversing the group
            ListNode prev = kth.next;
            ListNode curr = groupPrev.next;

            while(curr != groupNext){
                ListNode temp = curr.next;
                curr.next = prev;
                prev = curr;
                curr = temp;
            }

            ListNode temp = groupPrev.next;
            groupPrev.next = kth;
            groupPrev = temp;

        }
        return dummy.next;
    }

    public ListNode getKth(ListNode curr, int k){
        while(curr != null && k > 0){
            curr = curr.next;
            k = k-1;
        }
        return curr;
    }
}
```