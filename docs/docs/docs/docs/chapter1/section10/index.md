# 二分搜索

## **吃香蕉**
力扣第 875 题

```
// 定义：速度为 x 时，需要 f(x) 小时吃完所有香蕉  
// f(x) 随着 x 的增加单调递减  
int f(int[] piles, int x) {  
    int hours = 0;  
    for (int i = 0; i < piles.length; i++) {  
	hours += piles[i] / x;  
	if (piles[i] % x > 0) {  
	    hours++;  
	}  
    }  
    return hours;  
}  
public int minEatingSpeed(int[] piles, int H) {  
    int left = 1;  
    //题目说了1 <= piles[i] <= 10^9，也就是速度最快可以一小时内吃10^9  
    //开区间，需要加1  
    int right = 1000000000 + 1;  
    while (left < right) {  
	int mid = left + (right - left) / 2;  
	if (f(piles, mid) == H) {  
	    // 搜索左侧边界，则需要收缩右侧边界  
	    right = mid;  
	} else if (f(piles, mid) < H) {  
	    // 需要让 f(x) 的返回值大一些  
	    right = mid;  
	} else if (f(piles, mid) > H) {  
	    // 需要让 f(x) 的返回值小一些  
	    left = mid + 1;  
	}  
    }  
    return left;  
}  
```


## **运送货物**
力扣第 1011 题

```
// 定义：当运载能力为 x 时，需要 f(x) 天运完所有货物  
// f(x) 随着 x 的增加单调递减  
int f(int[] weights, int x) {  
    int days = 0;  
    for (int i = 0; i < weights.length; ) {  
	// 尽可能多装货物  
	int cap = x;  
	while (i < weights.length) {  
	    if (cap < weights[i]) break; else cap -= weights[i];  
	    i++;  
	}  
	days++;  
    }  
    return days;  
}  
public int shipWithinDays(int[] weights, int days) {  
    int left = 0;  
    // 注意，right 是开区间，所以额外加一  
    //最大载重显然就是weights数组所有元素之和，也就是一次把所有货物都装走。  
    int right = 1;  
    for (int w : weights) {  
	left = Math.max(left, w);  
	right += w;  
    }  
    while (left < right) {  
	int mid = left + (right - left) / 2;  
	if (f(weights, mid) == days) {  
	    // 搜索左侧边界，则需要收缩右侧边界  
	    right = mid;  
	} else if (f(weights, mid) < days) {  
	    // 需要让 f(x) 的返回值大一些  
	    right = mid;  
	} else if (f(weights, mid) > days) {  
	    // 需要让 f(x) 的返回值小一些  
	    left = mid + 1;  
	}  
    }  
    return left;  
}  
```

## **分割数组的最大值**
力扣第 410 题

①回溯暴力穷举可以在哪几个地方进行分割的组合个数，根据穷举结果去计算每种方案的最大子数组和。

②

```
/* 辅助函数，若限制最大子数组和为 max， 
计算 nums 至少可以被分割成几个子数组 */  
int split(int[] nums, int max) {  
    // 至少可以分割的子数组数量  
    int count = 1;  
    // 记录每个子数组的元素和  
    int sum = 0;  
    for (int i = 0; i < nums.length; i++) {  
	if (sum + nums[i] > max) {  
	    // 如果当前子数组和大于 max 限制  
	    // 则这个子数组不能再添加元素了  
	    count++;  
	    sum = nums[i];  
	} else {  
	    // 当前子数组和还没达到 max 限制  
	    // 还可以添加元素  
	    sum += nums[i];  
	}  
    }  
    return count;  
}  
int splitArray(int[] nums, int m) {  
    // 一般搜索区间是左开右闭的，所以 hi 要额外加一  
    //最大子数组和max的取值范围显然是，子数组至少包含一个元素，或至多包含整个数组，  
    int lo = getMax(nums), hi = getSum(nums) + 1;  
    while (lo < hi) {  
	int mid = lo + (hi - lo) / 2;  
	// 根据分割子数组的个数收缩搜索区间  
	int n = split(nums, mid);  
	if (n == m) {  
	    // 收缩右边界，达到搜索左边界的目的  
	    hi = mid;  
	} else if (n < m) {  
	    // 最大子数组和上限高了，减小一些  
	    hi = mid;  
	} else if (n > m) {  
	    // 最大子数组和上限低了，增加一些  
	    lo = mid + 1;  
	}  
    }  
    return lo;  
}  
int getMax(int[] nums) {/* 计算数组中的最大值 */}  
int getSum(int[] nums) {/* 计算数组元素和 */}  
```