# 滑动窗口

## **最小覆盖子串**
LeetCode 76 题

```
string minWindow(string s, string t) {  
    unordered_map<char, int> need, window;  
    for (char c : t) need[c]++;  
  
    int left = 0, right = 0;  
    int valid = 0;  
    // 记录最小覆盖子串的起始索引及长度  
    int start = 0, len = INT_MAX;  
    while (right < s.size()) {  
	// c 是将移入窗口的字符  
	char c = s[right];  
	// 右移窗口  
	right++;  
	// 进行窗口内数据的一系列更新  
	if (need.count(c)) {  
	    window[c]++;  
	    if (window[c] == need[c])  
		valid++;  
	}  
  
	// 判断左侧窗口是否要收缩  
	while (valid == need.size()) {  
	    // 在这里更新最小覆盖子串  
	    if (right - left < len) {  
		start = left;  
		len = right - left;  
	    }  
	    // d 是将移出窗口的字符  
	    char d = s[left];  
	    // 左移窗口  
	    left++;  
	    // 进行窗口内数据的一系列更新  
	    if (need.count(d)) {  
		if (window[d] == need[d])  
		    valid--;  
		window[d]--;  
	    }                      
	}  
    }  
    // 返回最小覆盖子串  
    return len == INT_MAX ?  
	"" : s.substr(start, len);  
}  
```

## **字符串排列**
LeetCode 567 题

```
// 判断 s 中是否存在 t 的排列  
bool checkInclusion(string t, string s) {  
    unordered_map<char, int> need, window;  
    for (char c : t) need[c]++;  
  
    int left = 0, right = 0;  
    int valid = 0;  
    while (right < s.size()) {  
	char c = s[right];  
	right++;  
	// 进行窗口内数据的一系列更新  
	if (need.count(c)) {  
	    window[c]++;  
	    if (window[c] == need[c])  
		valid++;  
	}  
  
	// 判断左侧窗口是否要收缩  
	while (right - left >= t.size()) {  
	    // 在这里判断是否找到了合法的子串  
	    if (valid == need.size())  
		return true;  
	    char d = s[left];  
	    left++;  
	    // 进行窗口内数据的一系列更新  
	    if (need.count(d)) {  
		if (window[d] == need[d])  
		    valid--;  
		window[d]--;  
	    }  
	}  
    }  
    // 未找到符合条件的子串  
    return false;  
}  
```

## **找所有字母异位词**
LeetCode 第 438 题

```
vector<int> findAnagrams(string s, string t) {  
    unordered_map<char, int> need, window;  
    for (char c : t) need[c]++;  
  
    int left = 0, right = 0;  
    int valid = 0;  
    vector<int> res; // 记录结果  
    while (right < s.size()) {  
	char c = s[right];  
	right++;  
	// 进行窗口内数据的一系列更新  
	if (need.count(c)) {  
	    window[c]++;  
	    if (window[c] == need[c])   
		valid++;  
	}  
	// 判断左侧窗口是否要收缩  
	while (right - left >= t.size()) {  
	    // 当窗口符合条件时，把起始索引加入 res  
	    if (valid == need.size())  
		res.push_back(left);  
	    char d = s[left];  
	    left++;  
	    // 进行窗口内数据的一系列更新  
	    if (need.count(d)) {  
		if (window[d] == need[d])  
		    valid--;  
		window[d]--;  
	    }  
	}  
    }  
    return res;  
} 
```

## **最长无重复子串**
LeetCode 第 3 题

```
int lengthOfLongestSubstring(string s) {  
    unordered_map<char, int> window;  
  
    int left = 0, right = 0;  
    int res = 0; // 记录结果  
    while (right < s.size()) {  
	char c = s[right];  
	right++;  
	// 进行窗口内数据的一系列更新  
	window[c]++;  
	// 判断左侧窗口是否要收缩  
	while (window[c] > 1) {  
	    char d = s[left];  
	    left++;  
	    // 进行窗口内数据的一系列更新  
	    window[d]--;  
	}  
	// 在这里更新答案  
	res = max(res, right - left);  
    }  
    return res;  
}  
```