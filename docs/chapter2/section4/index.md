# 堆/栈/队列

## 42. 用两个栈实现队列

```
import java.util.Stack;

public class Solution {
    Stack<Integer> stack1 = new Stack<Integer>();
    Stack<Integer> stack2 = new Stack<Integer>();
     
    public void push(int node) {
        stack1.push(node);
    }
     
    public int pop() {
        if(stack1.empty()&&stack2.empty()){
            throw new RuntimeException("Queue is empty!");
        }
        if(stack2.empty()){
            while(!stack1.empty()){
                stack2.push(stack1.pop());
            }
        }
        return stack2.pop();
    }
}
```

## 43. 包含min函数的栈

```
import java.util.Stack;

public class Solution {

    Stack<Integer> stack1 = new Stack<Integer>();
    Stack<Integer> stack2 = new Stack<Integer>();
    public void push(int value) {
        stack1.push(value);
        if(stack2.empty() || value<=stack2.peek()) stack2.push(value);
        else stack2.push(stack2.peek());
        
    }
    
    public void pop() {
        stack1.pop();
        stack2.pop();
    }
    
    public int top() {
        return stack1.peek();
    }
    
    public int min() {
        return stack2.peek();
    }
}
```

## 44. 有效括号序列

```
import java.util.*;


public class Solution {
    /**
     * 
     * @param s string字符串 
     * @return bool布尔型
     */
    public boolean isValid (String s) {
         // write code here
        char[] c=s.toCharArray();
        Stack<Character> stack=new Stack<>();
        for(Character cha:c){
            if(cha=='(' || cha=='{' || cha=='[') {
                stack.push(cha);
            }else{
                if(stack.isEmpty()) return false;
                Character k=stack.pop();
                if(k=='(' && cha!=')' || k=='{' && cha!='}' || k=='[' && cha!=']') return false;
            }
  }
         return stack.isEmpty();
    }
}
```

## 45. 滑动窗口的最大值

```
import java.util.*;
public class Solution {
    public ArrayList<Integer> maxInWindows(int[] nums, int size) {
        ArrayList<Integer> res = new ArrayList<>();
        if(size == 0 || size > nums.length) return res;
        Deque<Integer> dq = new LinkedList<>();
        int i = 0;
        for(; i < size; i++){
            while(!dq.isEmpty() && nums[dq.peekLast()] <= nums[i]){
                dq.pollLast();
            }
            dq.offerLast(i);
        }
        res.add(nums[dq.peekFirst()]);
        for(; i < nums.length; i++){
            while(!dq.isEmpty() && nums[dq.peekLast()] <= nums[i]){
                dq.pollLast();
            }
            dq.offerLast(i);
            while(dq.peekFirst() <= i-size) dq.pollFirst();
            res.add(nums[dq.peekFirst()]);
        }
        return res;
    }
}
```

## 46. 最小的K个数

```
import java.util.ArrayList;
import java.util.*;
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        if(k==0) return new ArrayList<>();
        PriorityQueue<Integer> bigHeap = new PriorityQueue<>(k,(a,b)->(b-a));
        int len = input.length;
        for(int i=0;i<len;i++){
            if(bigHeap.size() < k){
                bigHeap.offer(input[i]);
            }else if(bigHeap.peek() > input[i]){
                bigHeap.poll();
                bigHeap.offer(input[i]);
            }
        }
        
        return new ArrayList<Integer>(bigHeap);
           
    }
}
```

## 47. 寻找第K大

```
import java.util.*;

public class Solution {
    public int findKth(int[] a, int n, int k) {
        // write code here
        quickSort(a, 0, n-1, k);
        return a[k-1];
    }
    
    private void quickSort(int[] a, int start, int end, int k){
        if(start >= end) return ;
        int index = divide(a, start, end);
        
        quickSort(a, start, index-1, k);
        quickSort(a, index+1,end,k);
    }
    
    private int divide(int[] a, int start, int end){
        int tmp = a[end];
        while(start < end){
            while(start < end && a[start] >= tmp) start++;
            a[end] = a[start];
            while(start < end && a[end] <= tmp) end--;
            a[start] = a[end];
        }
        a[end] = tmp;
        return end;
    }
}
```

## 48. 数据流中的中位数

```
import java.util.*;
public class Solution {

    private PriorityQueue<Integer> big = new PriorityQueue<>((a,b)->{
        return (int)b-a;
    });
    private PriorityQueue<Integer> small = new PriorityQueue<>();
    
    public void Insert(Integer num) {
        if(big.size() > small.size()){
            big.offer(num);
            small.offer(big.poll());
        }else{
            small.offer(num);
            big.offer(small.poll());
        }
    }

    public Double GetMedian() {
        if(big.size() > small.size()){
            return Double.parseDouble(big.peek().toString());
        }else if(big.size() < small.size()){
            return Double.parseDouble(small.peek().toString());
        }
        return big.peek()/2.0 + small.peek()/2.0;
    }
}
```

## 49. 表达式求值

```
import java.util.*;


public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     * 返回表达式的值
     * @param s string字符串 待计算的表达式
     * @return int整型
     */
    //中缀转后缀，再求值后缀
    public int solve (String s) {
        // write code here
        s.replaceAll(" ","");
        
        Map<Character, Integer> priority = new HashMap<>();
        priority.put('+',1);
        priority.put('-',1);
        priority.put('*',2);
        
        Stack<Integer> numStack = new Stack<>();
        Stack<Character> opStack = new Stack<>();
        int len = s.length();
        for(int i = 0; i < len; i++){
            //如果是数字
            if(s.charAt(i) >= '0' && s.charAt(i) <= '9'){
                if(i == 0){
                    numStack.push(s.charAt(i) - '0');
                }else{
                    if(s.charAt(i-1) >= '0' && s.charAt(i-1) <= '9'){
                        numStack.push(numStack.pop() * 10 + (s.charAt(i) - '0'));
                    }else{
                        numStack.push(s.charAt(i) - '0');
                    }
                }
            }
            //如果是符号
            else{
                if(s.charAt(i) == '(') opStack.push('(');
                else if(s.charAt(i) == ')'){
                    while(!opStack.isEmpty()){
                        if(opStack.peek() != '('){
                            int b = numStack.pop();
                            int a = numStack.pop();
                            numStack.push(calc(a,b,opStack.pop()));
                        }else{
                            opStack.pop();
                            break;
                        }
                    }
                }else{
                    while(!opStack.isEmpty() 
                          && opStack.peek() != '('
                         && priority.get(s.charAt(i)) <= priority.get(opStack.peek())){
                        int b = numStack.pop();
                        int a = numStack.pop();
                        numStack.push(calc(a,b,opStack.pop()));
                    }
                    opStack.push(s.charAt(i));
                }
            }
        }
        while(!opStack.isEmpty()){
            int b = numStack.pop();
            int a = numStack.pop();
            numStack.push(calc(a,b,opStack.pop()));
        }
        return numStack.pop();
    }
    
    private int calc(int a, int b, char op){
        switch(op){
            case '+': return a+b;
            case '-': return a-b;
            case '*': return a*b;
            default: return -1;
        }
    }
}
```