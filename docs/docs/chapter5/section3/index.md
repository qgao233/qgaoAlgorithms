# 字符串

## 1. 正则匹配

```
public boolean isMatch(String s, String p) {
    /*
        'match' below including .
    f(i,j) means s where s.len=i matches p where p.len=j
    f(i,j) =
        if (p_j-1 != * ) f(i-1, j-1) and s_i-1 matches p_j-1
        if (p_j-1 == * )
            * matches zero times: f(i,j-2)
            or * matches at least one time: f(i-1,j) and s_i-1 matches p_j-2
     */

    if (!p.isEmpty() && p.charAt(0) == '*') {
        return false;   // invalid p
    }

    boolean[][] dp = new boolean[s.length() + 1][p.length() + 1];

    // initialize dp(0,0)
    dp[0][0] = true;

    // dp(k,0) and dp(0,2k-1) where k>=1 are false by default

    // initialize dp(0,2k) where p_2k-1 = * for any k>=1
    for (int j = 1; j < p.length(); j+=2) {
        if (p.charAt(j) == '*') {
            dp[0][j+1] = dp[0][j-1];
        }
    }

    for (int i = 1; i <= s.length(); i++) {
        for (int j = 1; j <= p.length(); j++) {
            if (p.charAt(j - 1) != '*') {
                dp[i][j] = dp[i - 1][j - 1] && isCharMatch(s.charAt(i - 1), p.charAt(j - 1));
            } else {
                dp[i][j] = dp[i][j - 2] || dp[i - 1][j] && isCharMatch(s.charAt(i - 1), p.charAt(j - 2));
            }
        }
    }

    return dp[s.length()][p.length()];
}

// no * in p
private boolean isCharMatch(char s, char p) {
    return p == '.' || s == p;
}
```

## 2. 最长不重复子串

```
/**
     * Given "abcabcbb", the answer is "abc", which the length is 3.

     Given "bbbbb", the answer is "b", with the length of 1
     * @param src
     * @return
     */
public int lengthOfLongestSubstring(String src) {
    if(src == null || src.length() <= 0){
        return -1;
    }

    int begin = 0,end = 0,strLength = src.length();
    Map<Character,Integer> charMap = new HashMap<Character, Integer>();
    int maxLength = 0;
    while (end < strLength){
        if(charMap.containsKey(src.charAt(end))){
            begin = Math.max(charMap.get(src.charAt(end)),begin);
        }
        maxLength = Math.max(maxLength,end - begin + 1);
        charMap.put(src.charAt(end),++end);
    }
    return maxLength;
}
```

## 3. kmp匹配算法

```
private int[] getNext(String pattern){
    if(pattern == null || pattern.length() <= 0){
        return null;
    }

    int strLength = pattern.length();
    int[] next = new int[strLength];
    int k = -1,j = 0;
    next[0] = -1;

    while (j < strLength -1){
        if(k == -1 || pattern.charAt(j) == pattern.charAt(k)){
            k++;
            j++;
            if(pattern.charAt(j) == pattern.charAt(k)){
                next[j] = next[k];
            }else {
                next[j] = k;
            }
        }
        else {
            k = next[k];
        }
    }
    return next;
}

public int kmpSearch(String source,String pattern){
    if(source == null || pattern == null){
        return -1;
    }
    if(pattern.length() > source.length()){
        return -1;
    }

    int srcPoint = 0,tarPoint = 0,srcLength = source.length(),ptnLength = pattern.length();
    int[] next = getNext(pattern);

    while (srcPoint < srcLength && tarPoint < ptnLength){
        if(tarPoint == -1 || source.charAt(srcPoint) == pattern.charAt(tarPoint)){
            srcPoint++;
            tarPoint++;
        }else {
            tarPoint = next[tarPoint];
        }
    }
    if(tarPoint == ptnLength){
        return srcPoint - ptnLength;
    }else {
        return -1;
    }
}
```

## 4. 最长回文子串

```
/**
     * Input: "babad"
     Output: "bab"

     Input: "cbbd"
     Output: "bb"
     * @param src
     * @return
     */
public String longestPalindrome(String src) {
    if(src == null || src.length() <= 0){
        return null;
    }

    char[] specialString = insertSymbol(src);
    int n = specialString.length;
    int[] p = new int[n];
    int center = 0,right = 0;

    for(int i = 1; i < n-1; i++){
        int iMirror = center - (i - center);
        p[i] = (right > i)?Math.min(right - i,p[iMirror]):0;

        while(specialString[i+1+p[i]] == specialString[i-1-p[i]]){
            p[i]++;
        }

        //更新center和right
        if(p[i] > right - i){
            center = i;
            right = i + p[i];
        }
    }

    int maxLength = 0,centerIndex = 0;
    for(int i = 1; i < n-1; i++){
        if(p[i] > maxLength){
            maxLength = p[i];
            centerIndex = i;
        }
    }

    System.out.println(centerIndex+"$"+maxLength);
    int temp = (centerIndex - 1 - maxLength) / 2;
    return src.substring(temp,temp+maxLength);
}
```