1. Remove Dups: Write code to remove duplicates from an unsorted linked list. How would you solve this problem if a temporary buffer
is not allowed?
* 基本解法
```
void deleteDups(LinkedListNode n){
    HashSet<Integer> set = new HashSet<Integer>();
    LinkedListNode previous = null;
    while(n!=null){
        if(set.contatins(n.data)){
            previous.next = n.next;
        }else{
            set.add(n.data);
            previous = n;
        }
        n = n.next;
    }
}
```
* Follow Up
> If we don't have a buffer, we can iterate with two pointers:Current which iterates through the linked list, and runner
> which checks all subsequent nodes for duplicates.

```
void deleteDups(LinkedListNode head){
    LinkedListNode current = head;
    while(current != null){
        /* Remove all future nodes that have the same value */
        LinkedListNode runner = current;
        while(runner.next!=null){
            if(runner.next.data == current.data){
                runner.next = runner.next.next;
            }else{
                runner = runner.next;
            }
        }
        current = current.next;
    }
}
```

2. Return Kth to Last: Implement an algorithm to find the kth to last element of a single linked list.
```
LinkedListNode nthToLast(LinkedListNode head, int k){
    LinkedListNode p1 = head;
    LinkedListNode p2 = head;
    
    /* Move p1 k nodes into the list. */
    for(int i=0; i < k ; i++){
        if( p1 == null) return null; // Out of bounds
        p1 = p1.next;
    }

    while(p1 != null){
        p1 = p1.next;
        p2 = p2.next;
    }
    return p2;
}
```

3. Delete Middle Node: Implement an algorithm to delete a node in the middle (i.e., any node but
the first and last node, not necessarily the exact middle) of a singly linked list, given only access to that node.
```
boolean deleteNode(LinkedListNode n){
    if( n == null || n.next == null){
        return false;
    }
    LinkedListNode next = n.next;
    n.data = next.data;
    n.next = next.next;
    return true;
}
```

4. Partition(leetcode 86): Write code to partition a linked list around a value x, such that
all nodes less than x come before all nodes greater than or equal to x. If x in contained within the list, the values of x only need to be after the elements less than x (see below).
* Example:
> Input: 3->5->8->5->10->2->1 partition = 5
> Output: 3->1->2->10->5->5->8

```
    public static ListNode partition(ListNode head, int x) {
            ListNode newhead = new ListNode(0);
            newhead.next = head;
            ListNode p = head;
            ListNode pre = newhead;
            while( p!=null && p.val<x ){
                pre = p;
                p= p.next;
            }
            if(p!=null){ /* p是大于等于x的结点 */
                ListNode q = p.next;
                ListNode pre_q = p;
                while(q!=null){
                    if(q.val<x){  /* q指向的结点值比x小 */
                        /*插入*/
                        ListNode tmp = new ListNode(q.val);
                        pre.next = tmp;
                        tmp.next = p;
                        pre = tmp;

                        /*删除*/
                        if(q.next!=null){
                            q.val = q.next.val;
                            q.next = q.next.next;
                        }
                        else{
                            pre_q.next = null;
                            break;
                        }
                    }else {
                        pre_q = q;
                        q = q.next;
                    }
                }
            }
            return newhead.next;
    }
``` 

5. Sum Lists: You have two numbers represented by a linked list, where each node contains
a single digit. The digits are stored in reverse order, such that the 1's digit is at the head of
the list.

```
   public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        int carry = 0,tmp,val;
        ListNode head = new ListNode(0);
        ListNode p = head;
        ListNode p1 = l1,p2 = l2;
        while(p1!=null || p2!=null){
            tmp = carry;
            if(p1!=null){
                tmp += p1.val;
            }
            if(p2!=null){
                tmp += p2.val;
            }
            carry = tmp / 10;
            tmp %= 10;
            p.next = new ListNode(tmp);
            p = p.next;
            if(p1!=null){p1 = p1.next;}
            if(p2!=null){p2 = p2.next;}
        }
        if(carry!=0){
            p.next = new ListNode(carry);
        }
        return head.next;
    }
```

6. Palindrome: Implement a function to check if a linked list is a palindrome.
```
boolean isPalindrome(LinkedListNode head){
    LinkedListNode fast = head;
    LinkedListNode slow = head;

    Stack<Integer> stack = new Stack<Integer>();
    /* Push elements from first half of linked list onto stack. When fast runner
    (which is moving at 2x speed) reaches the end of the linked list, then we know
    we're at the middele */

    while( fast!=null && fast.next !=null ){
        stack.push(slow.data);
        slow = slow.next;
        fast = fast.next;
    }

    /* Has odd number of elements, so skip the middel element */
    if(fast!=null){
        slow = slow.next;
    }

    while( slow!= null){
        int top = stack.pop().intValue();
        
        /*  If values are different, then it's not a palindrome */
        if( top!= slow.data){
            return false;
        }
        slow = slow.next;
    }
    return true;
}
```

7. Intersection: Given two (singly) linked lists, determine if the two lists intersect. Return the intersecting node.
```
public static ListNode Intersection(ListNode t1, ListNode t2){
        ListNode p1=t1,p2=t2;
        int len1 = 1,len2 = 1;
        while(p1.next!=null){
            p1 = p1.next;
            len1 ++;
        }
        while(p2.next!=null){
            p2 = p2.next;
            len2 ++;
        }
        if(p1!=p2){
            return null;
        }
        int diff = len1>len2? len1-len2:len2-len1;
        ListNode longer = len1>len2? t1:t2;
        ListNode shorter = len1>len2? t2:t1;
        int step = 0;
        while(step<diff){
            longer = longer.next;
            step ++;
        }
        while(shorter!=longer){
            shorter = shorter.next;
            longer = longer.next;
        }
        return shorter;
    }
```

8. Loop Detection: Given a circular linked list, implement an algorithm that returns the node
at the beginning of the loop.



