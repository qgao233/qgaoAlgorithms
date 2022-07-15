# 哈希

## 50. 两数之和

```
import java.util.*;


public class Solution {
    /**
     * 
     * @param numbers int整型一维数组 
     * @param target int整型 
     * @return int整型一维数组
     */
    public int[] twoSum (int[] numbers, int target) {
        // write code here
        //key为数组值，value为题目要求的下标，即数组下标+1
        HashMap<Integer,Integer> map = new HashMap<>();
         
        int[] result = new int[2];
         
        for(int i = 0;i<numbers.length;i++){
            //key+numebr[i]=target --> key=target-numbers[i]
            if(map.containsKey(target-numbers[i])){
                result[0] = map.get(target-numbers[i]);
                result[1] = i+1;
                return result;
            }else{
                map.put(numbers[i],i+1);
            }
        }
        return result;
    }
}
```

## 51. 数组中出现次数超过一半的数字

https://blog.csdn.net/qq_44688635/article/details/115638831

```
public class Solution {
    
    //摩尔投票法(候选法)
    //对拼消耗，剩下的一定是自己人
    public int MoreThanHalfNum_Solution(int [] array) {
        int candidate = 0, votes = 0;
        for(int cur : array){
            if(votes == 0) candidate = cur;
            if(candidate != cur) votes--;
            else votes++;
        }
        return candidate;
    }
}
```

## 52. 数组中只出现一次的两个数字

```
import java.util.*;


public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param array int整型一维数组 
     * @return int整型一维数组
     */
    public int[] FindNumsAppearOnce (int[] array) {
        // write code here
        int[] res = new int[2];
        // 创建一个哈希表
        HashSet<Integer> set = new HashSet<>();
        for(int i = 0; i < array.length; i++){
            // 如果已经被当作key了，那就直接remove掉
            if(set.contains(array[i])){
                set.remove(array[i]);
            }else{
                // 否则就添加进去
                set.add(array[i]);
            }
        }
        int i = 0;
        // 最后拿出来放进返回结果的数组中进行返回
        for(Integer num:set){
            res[i++] = num;
        }
        return res;
    }
}
```

## 53. 缺失的第一个正整数

https://www.cnblogs.com/zhanghongfeng/p/11696533.html

```
import java.util.*;


public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param nums int整型一维数组 
     * @return int整型
     */
    public int minNumberDisappeared (int[] nums) {
        // write code here
        int len = nums.length,tmp = -1;
        for(int i = 0; i < len; i++){
            //得用while循环
            while(nums[i] > 0 && nums[i] <= len
              && nums[nums[i] - 1] != nums[i]){
                tmp = nums[i];
                nums[i] = nums[nums[i] - 1];
                nums[tmp - 1] = tmp;
            }
        }
        for(int i = 0; i < len; i++){
            if(nums[i] != i+1){
                return i+1;
            }
        }
        return len+1;
    }
}
```

## 54. 三数之和

```
import java.util.*;
public class Solution {
    public ArrayList<ArrayList<Integer>> threeSum(int[] num) {
        Arrays.sort(num);
        ArrayList<ArrayList<Integer>> resList = new ArrayList<>();
        int i = 0, len = num.length - 2, leftVal = -1;
        boolean flag = false;
        while(i < len){
            flag = false;
            resList.addAll(twoSum(num, num[i], i+1, len+1));
            leftVal = num[i];
            while(i < len && leftVal == num[i]){
                i++;
                flag = true;
            }
            if(flag) continue;
            i++;
        }
        return resList;
    }
    
    public ArrayList<ArrayList<Integer>> twoSum(int[] num, int cur, int start, int end){
        int target = 0 - cur;
        int left = start, right = end;
        ArrayList<ArrayList<Integer>> resList = new ArrayList<>();
        while(left < right){
            int sum = num[left] + num[right];
            int leftVal = num[left], rightVal = num[right];
            if(sum < target){
                while(left < right && num[left] == leftVal) left++;
            }else if(sum > target){
                while(left < right && num[right] == rightVal) right--;
            }else{
                Integer[] res = new Integer[]{cur,num[left],num[right]};
                Arrays.sort(res);
                resList.add(new ArrayList<>(Arrays.asList(res)));
                while(left < right && num[left] == leftVal) left++;
                while(left < right && num[right] == rightVal) right--;
            }
        }
        return resList;
    }
}
```