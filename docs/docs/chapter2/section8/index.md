# 字符串

## 83. 字符串变形

```
import java.util.*;

public class Solution {
    public String trans(String s, int n) {
        // write code here
        String[] temp=s.split(" ",-1);
        StringBuilder sb=new StringBuilder();
        for(int i=temp.length-1;i>=0;i--){
            char[] str=temp[i].toCharArray();
            for(int j=0;j<str.length;j++){
                if(str[j]>=97){
                    str[j]=(char)(str[j]-32);
                }else if(str[j]>=65){
                     str[j]=(char)(str[j]+32);
                }
                sb.append(str[j]);
                 
            }
            sb.append(i==0?"":" ");
        }
        return sb.toString();
    }
}
```

## 84. 最长公共前缀

纵向扫描法：按列扫描，先验证所有字符串的第一个元素

```
import java.util.*;


public class Solution {
    /**
     * 
     * @param strs string字符串一维数组 
     * @return string字符串
     */
    public String longestCommonPrefix (String[] strs) {
        // write code here
        if (strs == null || strs.length == 0) {
            return "";
        }
        int length = strs[0].length();
        int count = strs.length;
        for (int i = 0; i < length; i++) {
            char c = strs[0].charAt(i);
            for (int j = 1; j < count; j++) {
                if (i == strs[j].length() || strs[j].charAt(i) != c) {
                    return strs[0].substring(0, i);
                }
            }
        }
        return strs[0];
    }
}
```

## 85. 验证IP地址

```
import java.util.*;


public class Solution {
    /**
     * 验证IP地址
     * @param IP string字符串 一个IP地址字符串
     * @return string字符串
     */
    public String solve (String IP) {
        // write code here
        return validIPv4(IP) ? "IPv4" : (validIPv6(IP) ? "IPv6" : "Neither");
    }
    
    private boolean validIPv4(String IP) {
        //使用limit = -1的split函数，使得字符串末尾或开头有一个'.'或':'也能分割出空的字符串
        String[] strs = IP.split("\\.", -1);
        if (strs.length != 4) {
            return false;
        }

        for (String str : strs) {
            if (str.length() > 1 && str.startsWith("0")) {
                return false;
            }
            try {
                int val = Integer.parseInt(str);
                if (!(val >= 0 && val <= 255)) {
                    return false;
                }
            } catch (NumberFormatException numberFormatException) {
                return false;
            }
        }
        return true;
    }

    private boolean validIPv6(String IP) {
        String[] strs = IP.split(":", -1);
        if (strs.length != 8) {
            return false;
        }

        for (String str : strs) {
            if (str.length() > 4 || str.length() == 0) {
                return false;
            }
            try {
                int val = Integer.parseInt(str, 16);//转换16进制为十进制
            } catch (NumberFormatException numberFormatException) {
                return false;
            }
        }
        return true;
    }
}
```

## 86. 大数加法

```
import java.util.*;


public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     * 计算两个数之和
     * @param s string字符串 表示第一个整数
     * @param t string字符串 表示第二个整数
     * @return string字符串
     */
    public String solve (String s, String t) {
        // write code here
        if(s==null||"".equals(s)){return t;}
        if(t==null||"".equals(t)){return s;}
        int a1,a2,carry=0;
        int ss=s.length()-1;
        int tt=t.length()-1;
        StringBuilder str = new StringBuilder();
        while (ss>=0||tt>=0) {
            a1=ss>=0?(s.charAt(ss) - '0'):0;//减去'0'会自动进行ascii码运算
            a2=tt>=0?(t.charAt(tt)-'0'):0;
            int sum=a1+a2+carry;
            carry=sum/10;
            str.append(String.valueOf(sum%10));//转为ascii码又转为对应的字符
            ss--;
            tt--;
        }
        if(carry!=0){str.append('1');}
        str.reverse();
        return str.toString();
    }
}
```