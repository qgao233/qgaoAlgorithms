# 双指针

## 87. 合并两个有序的数组

```
import java.util.*;
public class Solution {
    public void merge(int A[], int m, int B[], int n) {
        //src – 源数组。
        //srcPos – 源数组中的起始位置。
        //dest – 目标数组。
        //destPos – 目标数据中的起始位置。
        //length – 要复制的数组元素的数量
        System.arraycopy(B, 0, A, m, n);
        //排序算法是 Vladimir Yaroslavskiy、Jon Bentley 和 Joshua Bloch 的双枢轴快速排序
        //a – 要排序的数组
        Arrays.sort(A);
    }
}
```

## 88. 判断是否为回文字符串

```
import java.util.*;


public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     * 
     * @param str string字符串 待判断的字符串
     * @return bool布尔型
     */
    public boolean judge (String str) {
        // write code here
        if (str.length() == 0)
            return true;
        //两个指针，一个从左边开始，一个从右边开始，每次两个
        //指针都同时往中间挪，只要两个指针指向的字符不一样就返回false
        int left = 0;
        int right = str.length() - 1;
        while (left < right) {
            if (str.charAt(left++) != str.charAt(right--))
                return false;
        }
        return true;
    }
}
```

## 89. 合并区间

```
import java.util.*;
/**
 * Definition for an interval.
 * public class Interval {
 *     int start;
 *     int end;
 *     Interval() { start = 0; end = 0; }
 *     Interval(int s, int e) { start = s; end = e; }
 * }
 */
public class Solution {
    //先升序,再模拟
    public ArrayList<Interval> merge(ArrayList<Interval> intervals) {
        Collections.sort(intervals, (v1, v2)->v1.start - v2.start);
        ArrayList<Interval> res = new ArrayList<Interval>();
        int idx = -1;
        for(Interval interval : intervals){
            if(idx == -1 || interval.start > res.get(idx).end){ //若数组为空，或当前区间的起始位置小于结果list中最后区间的终止位置
                //不合并，直接将当前区间加入结果list
                res.add(interval);
                idx++;
            }else{    //合并，选择较大的数作为最后区间的终止位置
                res.get(idx).end = Math.max(interval.end, res.get(idx).end);
            }
        }
        return res;
    }
}
```

## 90. 最小覆盖子串

给出两个字符串 s 和 t，要求在 s 中找出最短的包含 t 中所有字符的连续子串。

数据范围：0 \le |S|,|T| \le100000≤∣S∣,∣T∣≤10000，保证s和t字符串中仅包含大小写英文字母

要求：进阶：空间复杂度 O(n)O(n) ， 时间复杂度 O(n)O(n)

例如：
```
S ="XDOYEZODEYXNZ"S="XDOYEZODEYXNZ"
T ="XYZ"T="XYZ"
找出的最短子串为"YXNZ""YXNZ".
```

注意：
* 如果 s 中没有包含 t 中所有字符的子串，返回空字符串 “”；
* 满足条件的子串可能有很多，但是题目保证满足条件的最短的子串唯一。

滑动窗口

```
import java.util.*;


public class Solution {
    /**
     * 
     * @param S string字符串 
     * @param T string字符串 
     * @return string字符串
     */
    public String minWindow (String s, String t) {
        // write code here
        // l(left): 左边界
        // r(right): 右边界
        int l = 0, r = 0;
        // size=r-l+1: 滑动窗口的大小，默认值Integer.MAX_VALUE方便值交换
        int size = Integer.MAX_VALUE;
        // needCount: 当前遍历下，满足t还需要的元素个数，默认值、最大值是t.length，为0时表示滑动窗口内容覆盖了t中所有元素
        int needCount = t.length();
        // start: 记录有效滑动窗口的起始位置(左边界)，方便后续查找对应的字串
        int start = 0;
        // 字典need: 记录滑动窗口中需要t中各个元素的数量
        // ASCII方式存储 [A-Z]：65-90  [a-z]：97-122
        // need[97]=2 表示t中需要2个a
        // t中对应字符的need[]必须>=0
        // 若need[]<0表示是个多余的字符
        int[] need = new int[128];
        // 根据t的内容，向字典need添加元素
        for (int i = 0; i < t.length(); i++)
            need[t.charAt(i)]++;
        // 循环条件，右边界不能溢出
        while (r < s.length()) {
            // 获取当前遍历的元素字符
            char c = s.charAt(r);
            // 若当前遍历字符是t中的内容，needCount需要-1
            if (need[c] > 0)
                needCount--;
            // 无论c在不在t中，都要在need中-1
            // 目的：利用正负来区分字符是否多余的还是有用的
            need[c]--;
            // needCount==0表示当前窗口满足t中所有元素
            if (needCount == 0) {
                // 判断左边界是否可以收缩
                // 如果l对应字符的need[]<0，表示是个多余的字符
                while (l < r && need[s.charAt(l)] < 0) {
                    // 在need[]中维护更新这个字符
                    need[s.charAt(l)]++;
                    // 左边界向右移，移除这个多余字符
                    l++;
                }
                // 判断是否更新有效滑动窗口的大小
                if (r - l + 1 < size) {
                    // 更新
                    size = r - l + 1;
                    // 记录有效滑动窗口的起始位置(左边界)，方便查找对应的字串
                    start = l;
                }
                // 左边界对应的字符计数需要+1
                need[s.charAt(l)]++;
                // 重新维护左边界
                l++;
                // 重新维护当前的needCount
                needCount++;
            }
            // 右边界向右移，寻找下一满足条件的有效滑动窗口
            r++;
        }
        return size == Integer.MAX_VALUE ? "" : s.substring(start, start + size);
    }
}
```

## 91. 反转字符串

```
import java.util.*;


public class Solution {
    /**
     * 反转字符串
     * @param str string字符串 
     * @return string字符串
     */
    public String solve (String str) {
        // write code here
        return new StringBuilder(str).reverse().toString();
    }
}
```

## 92. 最长无重复子数组

```
import java.util.*;


public class Solution {
    /**
     * 
     * @param arr int整型一维数组 the array
     * @return int整型
     */
    public int maxLength (int[] arr) {
        // write code here
        Queue<Integer> queue = new LinkedList<>();
        int res = 0;
        for (int c : arr) {
            while (queue.contains(c)) {
                //如果有重复的，队头出队
                queue.poll();
            }
            //添加到队尾
            queue.add(c);
            res = Math.max(res, queue.size());
        }
        return res;
    }
}
```

## 93. 盛水最多的容器

总是移动短的那边的指针，从统计上看不会丢失最大面积。

```
import java.util.*;


public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param height int整型一维数组 
     * @return int整型
     */
    public int maxArea (int[] height) {
        // write code here
        int l = 0, r = height.length-1;
        int ans = 0;
        while(l<r){
            int area = Math.min(height[l], height[r]) * (r-l);
            ans = Math.max(area, ans);
            if(height[l] <= height[r]){  //左侧为短边，移动短边
                l++;
            } else{  //右侧为短边，移动短边
                r--;
            }
             
        }
        return ans;
    }
}
```

## 94. 接雨水问题

双指针求解,一个指向最左边，一个指向最右边。

最开始的时候如果左边柱子从左往右是递增的，那么这些柱子是不能盛水的，同理最开始的时候如果右边的柱子从右往左是递增的，也是不能盛水的。

确定left和right的值之后，在left和right之间相当于构成了一个桶，桶的高度是最矮的那根柱子。然后我们从两边往中间逐个查找，如果查找的柱子高度小于桶的高度，那么盛水量就是桶的高度减去我们查找的柱子高度，如果查找的柱子大于桶的高度，我们要更新桶的高度。

```
import java.util.*;


public class Solution {
    /**
     * max water
     * @param arr int整型一维数组 the array
     * @return long长整型
     */
    public long maxWater (int[] arr) {
        // write code here
        if (arr.length <= 2)
            return 0;
        long water = 0;
        int left = 0, right = arr.length - 1;
        //最开始的时候确定left和right的边界，这里的left和right是
        //柱子的下标，不是柱子的高度
        while (left < right && arr[left] <= arr[left + 1])
            left++;
        while (left < right && arr[right] <= arr[right - 1])
            right--;

        while (left < right) {
            int leftValue = arr[left];
            int rightValue = arr[right];
            //在left和right两根柱子之间计算盛水量
            if (leftValue <= rightValue) {
                //如果左边柱子高度小于等于右边柱子的高度，根据木桶原理，
                // 桶的高度就是左边柱子的高度
                while (left < right && leftValue >= arr[++left]) {
                    water += leftValue - arr[left];
                }
            } else {
                //如果左边柱子高度大于右边柱子的高度，根据木桶原理，
                // 桶的高度就是右边柱子的高度
                while (left < right && arr[--right] <= rightValue) {
                    water += rightValue - arr[right];
                }
            }
        }
        return water;
    }
}
```