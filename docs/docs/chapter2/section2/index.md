# 二分查找/排序

## 17. 二分查找-I

```
import java.util.*;


public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param nums int整型一维数组 
     * @param target int整型 
     * @return int整型
     */
    public int search (int[] nums, int target) {
        // write code here
        int left = 0, right = nums.length - 1;
        while(left <= right){
            int mid = left + (right - left) / 2;
            if(target < nums[mid]){
                right = mid - 1;
            }else if(target > nums[mid]){
                left = mid + 1;
            }else{
                right = mid - 1;
            }
        }
        
        if(left >= nums.length || nums[left] != target) return -1;
        return left;
    }
}
```

## 18. 二维数组中的查找

```
public class Solution {
    public boolean Find(int target, int [][] array) {
        if(array.length == 0)  return false;
        int r = array.length; //行
        int l = array[0].length; //列
        int left = 0, down = r - 1;
        while(left<l && down>=0)
        {
            int tmp = array[down][left];
            if( tmp == target) return true;
            else if(tmp < target) left++;
            else down--;
        }
        return false;
    }
}
```

## 19. 寻找峰值

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
    public int findPeakElement(int[] nums){
        int left = 0, right = nums.length - 1;

        while (left <= right){
            int mid = left + (right - left) / 2;
            if(compareNeighbor(nums,mid-1,mid) < 0 && compareNeighbor(nums,mid,mid+1) > 0){
                return mid;
            }
            if(compareNeighbor(nums,mid-1,mid) < 0){
                left = mid+1;
            }else{
                right = mid-1;
            }
        }
        return -1;
    }

    public int[] get(int[] nums, int index){
        if(index == -1 || index == nums.length){
            return new int[]{0,0};
        }
        return new int[]{1,nums[index]};
    }

    public int compareNeighbor(int[] nums, int k1, int k2){
        int[] a = get(nums,k1);
        int[] b = get(nums,k2);

        //表示到边界的对比了
        if(a[0] != b[0]) return a[0] > b[0] ? 1 : -1;
        //正常对比
        if(a[1] == b[1]) return 0;
        return a[1] > b[1] ? 1 : -1;
    }
}
```

## 20. 数组中的逆序对

归并典型用法

```
public class Solution {
    private int count;
    public int InversePairs(int [] array) {
        if(array.length != 0){
            divide(array,0,array.length - 1);
        }
        return count;
    }
    
    private void divide(int[] array, int start, int end){
        if(start >= end) return;
        
        int mid = start + (end - start) / 2;
        divide(array,start,mid);
        divide(array,mid+1,end);
        
        merge(array,start,mid,end);
    }
    
    private void merge(int[] array, int start, int mid, int end){
        int[] res = new int[end-start+1];
        
        int i = start, j = mid + 1, k = 0;
        while(i <= mid && j <= end ){
            if(array[i] <= array[j]) res[k++] = array[i++];
            else{
                res[k++] = array[j++];
                count += mid - i + 1;
                count %= 1000000007;
            }
        }
        
        while(i <= mid) res[k++] = array[i++];
        while(j <= end) res[k++] = array[j++];
        
        for(i = 0; i < res.length; i++){
            array[start+i] = res[i];
        }
    }
}
```

## 21. 旋转数组的最小数字

```
import java.util.ArrayList;
public class Solution {
    public int minNumberInRotateArray(int [] nums) {
    int left = 0, right = nums.length - 1;
        while(left < right){
            int mid = left + (right - left) / 2;
            if(nums[mid] < nums[right]){
                right = mid;
            }else if(nums[mid] > nums[right]){
                left = mid + 1;
            }else {
                right--;
            }
        }
        return nums[left];
    }
}
```

## 22. 比较版本号

```
import java.util.*;
import java.math.*;

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 比较版本号
     * @param version1 string字符串 
     * @param version2 string字符串 
     * @return int整型
     */
    public int compare (String version1, String version2) {
        // write code here
        String[] s1 = version1.split("\\.");
        String[] s2 = version2.split("\\.");
        int n = Math.max(s1.length, s2.length);
        for (int i = 0; i < n; i++) {
            BigInteger c1 = i < s1.length ? new BigInteger(s1[i]) : BigInteger.ZERO;
            BigInteger c2 = i < s2.length ? new BigInteger(s2[i]) : BigInteger.ZERO;
            if (c1.compareTo(c2) > 0) return 1;
            else if (c1.compareTo(c2) < 0) return -1;
        }
        return 0;
    }
}
```