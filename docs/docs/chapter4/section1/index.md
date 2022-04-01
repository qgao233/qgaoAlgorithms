# 双指针

## 713. 乘积小于K的子数组

```
public static int numSubarrayProductLessThanK(int[] nums, int k) {
    //if(k <= 1) return 0;

    int left = 0, right = 0;
    int size = nums.length;

    int result = 0;
    int product = 1;
    while(right < size){
        product *= nums[right];
        while(product >= k) product /= nums[left++];
        result += right - left + 1;
        right++;
    }
    return result;

}
```