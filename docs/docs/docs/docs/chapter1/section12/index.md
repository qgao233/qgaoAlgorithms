# 去重

## **有序数组去重（快慢指针）**
力扣第26

找到重复的，跳

```
int removeDuplicates(int[] nums) {  
    int n = nums.length;  
    if (n == 0) return 0;  
    int slow = 0, fast = 1;  
    while (fast < n) {  
        if (nums[fast] != nums[slow]) {  
            slow++；  
            //维护nums[0. .slow]无重复   
            nums[slow] = nums[fast];  
        }  
        fast++;  
    }  
    //长度为索引+ 1  
    return slow + 1;  
}  
```

## **有序链表去重（快慢指针）**
力扣第83

```
ListNode deleteDuplicates(ListNode head) {  
    if (head == null) return null;  
    ListNode slow = head, fast = head.next;  
    while (fast != null) {  
	if (fast.val != slow.val) {  
	    // nums[slow]= nums[fast];   
	    slow.next = fast;  
	    // slow++；   
	    slow = slow.next;  
	}  
	// fast++   
	fast = fast.next;  
    }  
    //断开与后面重复元素的连接   
    slow.next = null;  
    return head;  
}  
```

## **去除重复字母**
力扣第 316 题、第 1081 题

字典序最小的作为最终结果：比如说输入字符串s = "babc"，去重且符合相对位置的字符串有两个，分别是"bac"和"abc"，但是我们的算法得返回"abc"，因为它的字典序更小。

``` 
String removeDuplicateLetters(String s) {  
    Stack<Character> stk = new Stack<>();  
  
    // 维护一个计数器记录字符串中字符的数量  
    // 因为输入为 ASCII 字符，大小 256 够用了  
    int[] count = new int[256];  
    for (int i = 0; i < s.length(); i++) {  
	count[s.charAt(i)]++;  
    }  
  
    boolean[] inStack = new boolean[256];  
    for (char c : s.toCharArray()) {  
	// 每遍历过一个字符，都将对应的计数减一  
	count[c]--;  
  
	if (inStack[c]) continue;  
	//单调栈  
	while (!stk.isEmpty() && stk.peek() > c) {  
	    // 若之后不存在栈顶元素了，则停止 pop  
	    if (count[stk.peek()] == 0) {  
		break;  
	    }  
	    // 若之后还有，则可以 pop  
	    inStack[stk.pop()] = false;  
	}  
	stk.push(c);  
	inStack[c] = true;  
    }  
  
    StringBuilder sb = new StringBuilder();  
    while (!stk.empty()) {  
	sb.append(stk.pop());  
    }  
    //反转一次才是最终结果  
    return sb.reverse().toString();  
}  
```