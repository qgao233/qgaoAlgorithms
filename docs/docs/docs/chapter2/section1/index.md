# 链表

## 1. 反转链表

```
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode ReverseList(ListNode head) {
        if(head == null || head.next == null) return head;
        ListNode next = head.next;
        ListNode reverse = ReverseList(next);
        
        next.next = head;
        head.next = null;
        return reverse;
    }
}
```

## 2. 链表内指定区间反转

```
import java.util.*;

/*
 * public class ListNode {
 *   int val;
 *   ListNode next = null;
 * }
 */

public class Solution {
    /**
     * 
     * @param head ListNode类 
     * @param m int整型 
     * @param n int整型 
     * @return ListNode类
     */
    public ListNode reverseBetween (ListNode head, int m, int n) {
        // write code here
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        
        ListNode pre = dummy;
        for(int i = 0; i < m-1; i++){
            pre = pre.next;
        }
        
        ListNode right = pre;
        for(int i = 0; i < n-m+1; i++){
            right = right.next;
        }
        
        ListNode left = pre.next;
        ListNode tail = right.next;
        
        right.next = null;
        
        pre.next = reverseList(left);
        left.next = tail;
        
        return dummy.next;
        
    }
    
    private ListNode reverseList(ListNode head){
        if(head == null || head.next==null) return head;
        
        ListNode next = head.next;
        ListNode reverse = reverseList(next);
        next.next = head;
        head.next = null;
        return reverse;
    }
}
```

## 3. 链表中的节点每k个一组翻转

```
import java.util.*;

/*
 * public class ListNode {
 *   int val;
 *   ListNode next = null;
 * }
 */

public class Solution {
    /**
     * 
     * @param head ListNode类 
     * @param k int整型 
     * @return ListNode类
     */
    public ListNode reverseKGroup (ListNode head, int k) {
        // write code here
        ListNode node = head;
        for(int i = 0; i < k; i++){
            if(node == null) return head;
            node = node.next;
        }
        ListNode res = reverse(head, node);
        head.next = reverseKGroup(node,k);
        return res;
        
    }
    
    
    
    private ListNode reverse(ListNode left, ListNode right){
        ListNode pre = right;
        while(left != right){
            ListNode node = left.next;
            left.next = pre;
            pre = left;
            left = node;
        }
        return pre;
    }
}
```

## 4. 合并两个排序的链表

```
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode Merge(ListNode list1,ListNode list2) {
        ListNode dummy = new ListNode(-1);
        ListNode cur = dummy;
        while(list1 != null && list2 != null){
            if(list1.val <= list2.val){
                cur.next = list1;
                list1 = list1.next;
            }else{
                cur.next = list2;
                list2 = list2.next;
            }
            cur = cur.next;
        }
        cur.next = list1 != null ? list1 : list2;
        return dummy.next;
    }
}
```

## 5. 合并k个已排序的链表

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
import java.util.*;
public class Solution {
    public ListNode mergeKLists(ArrayList<ListNode> lists) {
        if(lists.size() == 0) return null;
        PriorityQueue<ListNode> minHeap = new PriorityQueue<>(lists.size(),(a,b)->{
            return a.val-b.val;
        });
        for(ListNode node : lists){
            if(node != null)
                minHeap.add(node);
        }
        
        return merge(minHeap);
    }
    
    public ListNode merge(PriorityQueue<ListNode> queue){
        if(queue.isEmpty()) return null;
        ListNode node = queue.poll();
        if(node.next!=null) queue.add(node.next);
        node.next = merge(queue);
        return node;
    }
}
```

## 6. 判断链表中是否有环

```
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        if(head == null) return false;
        ListNode slow = head, fast = head;
        while(slow != null && fast != null){
            slow = slow.next;
            if(fast.next != null){
                fast = fast.next.next;
            }else return false;
            if(slow == fast) return true;
        }
        return false;
    }
}
```

## 7. 链表中环的入口结点

```
/*
 public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {

    public ListNode EntryNodeOfLoop(ListNode pHead) {
        if(pHead == null) return pHead;
        ListNode meetPoint = findMeet(pHead);
        if(meetPoint == null) return null;
        
        ListNode one = pHead, two = meetPoint;
        if(one == two) return one;
        while(one != null && two != null){
            one = one.next;
            two = two.next;
            if(one == two) return one;
        }
        return null;
    }
    
    private ListNode findMeet(ListNode head){
        ListNode slow = head, fast = head;
        while(slow != null && fast != null){
            slow = slow.next;
            if(fast.next != null){
                fast = fast.next.next;
            }
            if(fast == null) return null;
            if(fast == slow) return slow;
        }
        return null;
    }
}
```

## 8. 链表中倒数最后k个结点

```
import java.util.*;

/*
 * public class ListNode {
 *   int val;
 *   ListNode next = null;
 *   public ListNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param pHead ListNode类 
     * @param k int整型 
     * @return ListNode类
     */
    public ListNode FindKthToTail (ListNode pHead, int k) {
        // write code here
        ListNode slow = pHead, fast = pHead;
        int step = 0;
        while(fast != null){
            fast = fast.next;
            if(step >= k) {
                slow = slow.next;
            }
            step++;
        }
        if(step < k){
            return null;
        }
        return slow;
    }
}
```

## 9. 删除链表的倒数第n个节点

```
import java.util.*;

/*
 * public class ListNode {
 *   int val;
 *   ListNode next = null;
 * }
 */

public class Solution {
    /**
     * 
     * @param head ListNode类 
     * @param n int整型 
     * @return ListNode类
     */
    public ListNode removeNthFromEnd (ListNode head, int n) {
        // write code here
        if(n==0){
            return head;
        }
        ListNode tail = head;
        ListNode predict = head;
        for (int i = 0; i < n; i++) {
            tail = tail.next;
        }
        if (tail==null){
            return head.next;
        }
        while (tail.next!=null){
            tail = tail.next;
            predict = predict.next;
        }
        predict.next = predict.next.next;
        return head;
    }
}
```

## 10. 两个链表的第一个公共结点

```
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) {
 
        if(pHead1 == null || pHead2 == null) return null;
        ListNode cur1 = pHead1, cur2 = pHead2;
        while(cur1 != cur2){
            if(cur1 == null) cur1 = pHead2;
            else cur1 = cur1.next;
            if(cur2 == null) cur2 = pHead1;
            else cur2 = cur2.next;
        }
        return cur1;
    }
}
```

## 11. 链表相加(二)

```
import java.util.*;

/*
 * public class ListNode {
 *   int val;
 *   ListNode next = null;
 * }
 */

public class Solution {
    /**
     * 
     * @param head1 ListNode类 
     * @param head2 ListNode类 
     * @return ListNode类
     */
    public ListNode addInList (ListNode head1, ListNode head2) {
        // 进行判空处理
        if(head1 == null)
            return head2;
        if(head2 == null){
            return head1;
        }
        // 反转h1链表
        head1 = reverse(head1);
        // 反转h2链表
        head2 = reverse(head2);
        // 创建新的链表头节点
        ListNode head = new ListNode(-1);
        ListNode nHead = head;
        // 记录进位的数值
        int tmp = 0;
        while(head1 != null || head2 != null){
            // val用来累加此时的数值（加数+加数+上一位的进位=当前总的数值）
            int val = tmp;
            // 当节点不为空的时候，则需要加上当前节点的值
            if (head1 != null) {
                val += head1.val;
                head1 = head1.next;
            }
            // 当节点不为空的时候，则需要加上当前节点的值
            if (head2 != null) {
                val += head2.val;
                head2 = head2.next;
            }
            // 求出进位
            tmp = val/10;
            // 进位后剩下的数值即为当前节点的数值
            nHead.next = new ListNode(val%10);
            // 下一个节点
            nHead = nHead.next;
 
        }
        // 最后当两条链表都加完的时候，进位不为0的时候，则需要再加上这个进位
        if(tmp > 0){
            nHead.next = new ListNode(tmp);
        }
        // 重新反转回来返回
        return reverse(head.next);
    }
 
    // 反转链表
    ListNode reverse(ListNode head){
        if(head == null)
            return head;
        ListNode cur = head;
        ListNode node = null;
        while(cur != null){
            ListNode tail = cur.next;
            cur.next = node;
            node = cur;
            cur = tail;
        }
        return node;
    }
}
```

## 12. 单链表的排序

```
import java.util.*;

/*
 * public class ListNode {
 *   int val;
 *   ListNode next = null;
 * }
 */

public class Solution {
    /**
     * 
     * @param head ListNode类 the head node
     * @return ListNode类
     */
    public ListNode sortInList (ListNode head) {
        // write code here
        if(head == null || head.next == null){
            return head;
        }
        ListNode slow = head, fast = head.next;
        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }
        
        ListNode newList = slow.next;
        slow.next = null;
        
        ListNode left = sortInList(head);
        ListNode right = sortInList(newList);
        
        ListNode res = new ListNode(-1);
        ListNode cur = res;
        while(left != null && right != null){
            if(left.val < right.val){
                cur.next = left;
                left = left.next;
            }else{
                cur.next = right;
                right = right.next;
            }
            cur = cur.next;
        }
        cur.next = left != null ? left : right;
        return res.next;
    }
}
```

## 13. 判断一个链表是否为回文结构

```
import java.util.*;

/*
 * public class ListNode {
 *   int val;
 *   ListNode next = null;
 * }
 */

public class Solution {
    /**
     * 
     * @param head ListNode类 the head
     * @return bool布尔型
     */
    
    ListNode left;
    
    public boolean isPail (ListNode head) {
        // write code here
        left = head;
        return traverse(head);
    }
    
    private boolean traverse(ListNode right){
        if(right == null) return true;
        boolean flag = traverse(right.next);
        boolean res = flag && left.val == right.val;
        left = left.next;
        return res;
        
    }
}
```

## 14. 链表的奇偶重排

```
import java.util.*;

/*
 * public class ListNode {
 *   int val;
 *   ListNode next = null;
 *   public ListNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param head ListNode类 
     * @return ListNode类
     */
    public ListNode oddEvenList (ListNode head) {
        // write code here
        
        if(head == null) return null;
        
        ListNode odd = head;
        ListNode even = head.next;
        ListNode evenHead = even;
        
        while(even != null && even.next != null){
            odd.next = odd.next.next;
            even.next = even.next.next;
            
            odd = odd.next;
            even = even.next;
        }
        odd.next = evenHead;
        return head;
    }
}
```

## 15. 删除有序链表中重复的元素-I

```
import java.util.*;

/*
 * public class ListNode {
 *   int val;
 *   ListNode next = null;
 * }
 */

public class Solution {
    /**
     * 
     * @param head ListNode类 
     * @return ListNode类
     */
    public ListNode deleteDuplicates (ListNode head) {
        // write code here
        if(head == null) return null;
        
        ListNode slow = head, fast = head.next;
        
        while(fast != null){
            if(slow.val != fast.val){
                slow.next = fast;
                slow = slow.next;
            }
            fast = fast.next;
        }
        slow.next = null;
        return head;
    }
}
```

## 16. 删除有序链表中重复的元素-II

```
import java.util.*;

/*
 * public class ListNode {
 *   int val;
 *   ListNode next = null;
 * }
 */

public class Solution {
    /**
     * 
     * @param head ListNode类 
     * @return ListNode类
     */
    public ListNode deleteDuplicates (ListNode head) {
        // write code here
        if(head == null) return null;
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode pre = dummy;
        
        while(head != null && head.next != null){
            if(head.val != head.next.val){
                pre = head;
                head = head.next;
            }else{
                int repeatVal = head.val;
                head = head.next;
                while(head != null && head.val == repeatVal){
                    head = head.next;
                }
                pre.next = head;
            }
        }
        return dummy.next;
        
    }
}
```

