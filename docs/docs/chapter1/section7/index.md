# 链表

## **递归反转链表**

```
ListNode reverse(ListNode head) {  
    if (head.next == null) return head;  
    ListNode headAfterReverse = reverse(head.next);  
    head.next.next = head;  
    head.next = null;  
    return headAfterReverse;  
}  
```

反转一部分：

```
ListNode successor = null; // 后驱节点  
  
// 反转以 head 为起点的 n 个节点，返回新的头结点  
ListNode reverseN(ListNode head, int n) {  
    if (n == 1) {   
	// 记录第 n + 1 个节点  
	successor = head.next;  
	return head;  
    }  
    // 以 head.next 为起点，需要反转前 n - 1 个节点  
    ListNode last = reverseN(head.next, n - 1);  
  
    head.next.next = head;  
    // 让反转之后的 head 节点和后面的节点连起来  
    head.next = successor;  
    return last;  
}    
ListNode reverseBetween(ListNode head, int m, int n) {  
    // base case  
    if (m == 1) {  
        return reverseN(head, n);  
    }  
    // 前进到反转的起点触发 base case  
    head.next = reverseBetween(head.next, m - 1, n - 1);  
    return head;  
}  
```

## **k个一组反转链表**

```
ListNode reverseKGroup(ListNode head, int k) {  
    if (head == null) return null;  
    // 区间 [a, b) 包含 k 个待反转元素  
    ListNode a, b;  
    a = b = head;  
    for (int i = 0; i < k; i++) {  
        // 不足 k 个，不需要反转，base case  
        if (b == null) return head;  
        b = b.next;  
    }  
    // 反转前 k 个元素  
    ListNode newHead = reverse(a, b);  
    // 递归反转后续链表并连接起来  
    a.next = reverseKGroup(b, k);  
    return newHead;  
}
```

## **判断回文单链表**
第一种方法：递归

```
// 左侧指针  
ListNode left;  
  
boolean isPalindrome(ListNode head) {  
    left = head;  
    return traverse(head);  
}  
  
boolean traverse(ListNode right) {  
    if (right == null) return true;  
    boolean res = traverse(right.next);  
    // 后序遍历代码  
    res = res && (right.val == left.val);  
    left = left.next;  
    return res;  
}  
```

第二种方法：快慢指针找中点，然后反转中点后的链表，再进行比较

```
boolean isPalindrome(ListNode head) {  
    ListNode left = head;  
    ListNode right = reverse(slow);  
    while(right != null) {  
	if(left.val != right.val) return false;  
	left = left.next;  
	right = right.next;  
    }  
    return true;  
}  
ListNode reverse(ListNode head) {  
    ListNode pre = null, cur = head;  
    while (cur != null) {  
	ListNode next = cur.next;  
	cur.next = pre;  
	pre = cur;  
	cur = next;  
    }  
    return pre;  
}  
```