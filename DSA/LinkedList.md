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
```java
public Node mergeSort(ListNode head){
	
}
```
