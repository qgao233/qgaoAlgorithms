# 贪心算法

## 95. 分糖果问题

一群孩子做游戏，现在请你根据游戏得分来发糖果，要求如下：

1. 每个孩子不管得分多少，起码分到一个糖果。
2. 任意两个相邻的孩子之间，得分较多的孩子必须拿多一些糖果。(若相同则无此限制)

给定一个数组 arrarr 代表得分数组，请返回最少需要多少糖果。

```
import java.util.*;


public class Solution {
    /**
     * pick candy
     * @param arr int整型一维数组 the array
     * @return int整型
     */
    public int candy (int[] arr) {
        // write code here
        int len = arr.length;
        if(len <= 1) return len;
        
        int[] candy = new int[len];
        Arrays.fill(candy, 1);
        
        //从左向右遍历，右边分数大于左边，则右边的糖果数加1
        for(int i = 0; i < len-1; i++){
            if(arr[i] < arr[i+1]) candy[i+1] = candy[i] + 1;
        }
        //从右往左遍历，左边分数大于右边，则左边的糖果数为它当前的糖果数和右边糖果数加1后中较大的一个
        for(int i = len-1; i > 0; i--){
            if(arr[i-1] > arr[i]) candy[i-1] = Math.max(candy[i-1],candy[i]+1);
        }
        int ans = 0;
        for(int x : candy) ans+=x;
        return ans;
    }
}
```

## 96. 主持人调度（二）

有 `n` 个活动即将举办，每个活动都有开始时间与活动的结束时间，第 `i` 个活动的开始时间是 `starti` ,第 `i` 个活动的结束时间是 `endi` ,举办某个活动就需要为该活动准备一个活动主持人。

一位活动主持人在同一时间只能参与一个活动。并且活动主持人需要全程参与活动，换句话说，一个主持人参与了第 `i` 个活动，那么该主持人在 (`starti`,`endi`) 这个时间段不能参与其他任何活动。求为了成功举办这 `n` 个活动，最少需要多少名主持人。

解题思路（排序+贪心）
* 首先建立两个数组分别存储开始时间（记为start）和结束时间（记为end）。
* 然后分别对start和end数组进行排序。
* 接着遍历start数组，判断当前开始时间是否大于等于最小的结束时间，如果是，则说明当前主持人就可以搞定（对应当前最小的结束时间的那个活动）；如果否，则需要新增一个主持人，并将end数组下标后移（表示对应的活动已经有人主持）。

```
import java.util.*;


public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     * 计算成功举办活动需要多少名主持人
     * @param n int整型 有n个活动
     * @param startEnd int整型二维数组 startEnd[i][0]用于表示第i个活动的开始时间，startEnd[i][1]表示第i个活动的结束时间
     * @return int整型
     */
    public int minmumNumberOfHost (int n, int[][] startEnd) {
        // write code here
        //初始化两个数组，分别记录开始时间和结束时间
        int[] start=new int[n];
        int[] end=new int[n];
 
        //将活动的开始和结束时间赋值道start和end数组
        for(int i=0;i<n;i++){
            start[i]=startEnd[i][0];
            end[i]=startEnd[i][1];
        }
 
        //按从小到大的顺序对start和end数组排序
        Arrays.sort(start);
        Arrays.sort(end);
 
        int res=0,index=0;
        for(int i=0;i<n;i++){
            //如果大于等于当前最小的结束时间，说明当前主持人可以搞定
            if(start[i]>=end[index]){
                index++;
            }
            //否则，需要新增主持人
            else{
                res++;
            }
        }
 
        return res;
    }
}
```