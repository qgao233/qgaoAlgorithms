# 双指针技巧

## **移除元素（快慢指针）**
力扣第27

找到target，跳

```
int removeElement(int[] nums, int val) {  
    int fast = 0, slow = 0;  
    while (fast < nums.length) {  
	if (nums[fast] != val) {  
	    nums[slow] = nums[fast];  
	    slow++;  
	}  
	fast++;  
    }  
    return slow;  
}  
```

## **移动0（快慢指针）**
```
void moveZeroes(int[] nums) {  
    // 去除 nums 中的所有 0  
    // 返回去除 0 之后的数组长度  
    int p = removeElement(nums, 0);  
    // 将 p 之后的所有元素赋值为 0  
    for (; p < nums.length; p++) {  
	nums[p] = 0;  
    }  
}  
```