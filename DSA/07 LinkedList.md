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

Happy Number 
- Sum of square of digits at some point is 1
```java
public boolean isHappy(int n){
	int slow = n;
	int fast = n;

	do{
		slow = findSquareSum(slow);
		fast = findSquareSum(findSquareSum(fast));
	}while(slow != fast); // if we reach the same number again, its a loop and cannot become a happy number
	if(slow == 1) return true;
	else return false;
}
private int findSquareSum(int number){
	int ans = 0;
	while(number > 0){
		int rem = number % 10;
		ans += rem * rem;
		number /= 10;
	}
	return ans;
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

### Other Questions :

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