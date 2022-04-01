# 随机

## **实现O(1)随机集合**
增、删、随机获得一个值，都是O(1)

```
class RandomizedSet {  
public:  
    // 存储元素的值  
    vector<int> nums;  
    // 记录每个元素对应在 nums 中的索引  
    unordered_map<int,int> valToIndex;  
  
    bool insert(int val) {  
	// 若 val 已存在，不用再插入  
	if (valToIndex.count(val)) {  
	    return false;  
	}  
	// 若 val 不存在，插入到 nums 尾部，  
	// 并记录 val 对应的索引值  
	valToIndex[val] = nums.size();  
	nums.push_back(val);  
	return true;  
    }  
  
    bool remove(int val) {  
	// 若 val 不存在，不用再删除  
	if (!valToIndex.count(val)) {  
	    return false;  
	}  
	// 先拿到 val 的索引  
	int index = valToIndex[val];  
	// 将最后一个元素对应的索引修改为 index  
	valToIndex[nums.back()] = index;  
	// 交换 val 和最后一个元素  
	swap(nums[index], nums.back());  
	// 在数组中删除元素 val  
	nums.pop_back();  
	// 删除元素 val 对应的索引  
	valToIndex.erase(val);  
	return true;  
    }  
  
    int getRandom() {  
	// 随机获取 nums 中的一个元素  
	return nums[rand() % nums.size()];  
    }  
}; 
```

## **避开黑名单的随机数**
将黑名单的索引映射到白名单中，这样就算获得了黑名单的索引，其实引用的是白名单的。

```
class Solution {  
public:  
    int sz;  
    unordered_map<int, int> mapping;  
  
    Solution(int N, vector<int>& blacklist) {  
	sz = N - blacklist.size();  
	for (int b : blacklist) {  
	    mapping[b] = 666;  
	}  
  
	int last = N - 1;  
	for (int b : blacklist) {  
	    // 如果 b 已经在区间 [sz, N)  
	    // 可以直接忽略  
	    if (b >= sz) {  
		continue;  
	    }  
	    //防止映射过后的索引还是黑名单的索引，  
	    //必须将黑名单的索引映射到白名单的索引  
	    while (mapping.count(last)) {  
		last--;  
	    }  
	    mapping[b] = last;  
	    last--;  
	}  
    }  
  
    int pick() {  
	// 随机选取一个索引  
	int index = rand() % sz;  
	// 这个索引命中了黑名单，  
	// 需要被映射到其他位置  
	if (mapping.count(index)) {  
	    return mapping[index];  
	}  
	// 若没命中黑名单，则直接返回  
	return index;  
    }  
};  
```