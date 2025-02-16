---
lastSync: Tue Oct 08 2024 20:36:43 GMT+0530 (India Standard Time)
---
[2095. Delete the Middle Node of a Linked List](https://leetcode.com/problems/delete-the-middle-node-of-a-linked-list/)
```java
class Solution {
    public ListNode deleteMiddle(ListNode head) {
        ListNode slow = head;
        ListNode fast = head;
        ListNode prev = new ListNode(0, head);

        if(slow.next == null){
            return null;
        }

        while(fast != null && fast.next != null){
            fast = fast.next.next;
            slow = slow.next;
            prev = prev.next;
        }

        prev.next = slow.next;

        return head;
    }
}
```

[328. Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/)
```java
public ListNode oddEvenList(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }

        ListNode a = new ListNode(0, head);
        ListNode b = head;
        ListNode c = head.next;
        ListNode p = a;

        while(b.next != null){
            if(c.next != null){
                b.next = c.next;
                a.next = c;
                a = a.next;
                b = a.next;
            }
            else{
                b.next = null;
                a.next = c;
            }

            if(b.next!= null){
                c = b.next;
            }
            else{
                c.next = null;
            }
        }

        b.next = p.next;
        return head;
        
    }
```

[206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)
```java
public ListNode reverseList(ListNode head) {
        if(head == null || head.next == null){
            return head;
        }
        
        ListNode a = null;
        ListNode b = head;
        ListNode c;
        
        while(b!= null){
            c = b.next;
            b.next = a;
            a = b;
            b = c;
        }
        return a;
    }
```

[2130. Maximum Twin Sum of a Linked List](https://leetcode.com/problems/maximum-twin-sum-of-a-linked-list/)
```java
public int pairSum(ListNode head) {
        Stack<Integer> st = new Stack<>();
        int max = 0;

        ListNode slow = head;
        ListNode fast = head;
        while(fast != null && fast.next != null){
            st.push(slow.val);
            slow = slow.next;
            fast = fast.next.next;
        }
        int temp;
        while(!st.isEmpty()){
            temp = st.pop() + slow.val;
            if(temp > max){
                max = temp;
            }
            slow = slow.next;
        }

        return max;
    }
```