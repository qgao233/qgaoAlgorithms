# 兴业数金

## 1 

**问题**：给定一系列字符串，然后找到其中最长的字符串，并且该字符串由其它字符串组合而成。

如果有相同长度的字符串，按照字典序返回最小的那个。

**实现**：在做的时候，使用的是单调队列来找到最长的字符串，然后使用回溯来达到其它字符串的全排列，最后与找到字符串进行匹配，是否找到题目要求的字符串

**提交**：最后提交的代码，有两处错误没有修改：

* 一是单调队列排除中间的干扰项时，判断条件不应加=号，否则只能得到1个结果，若有相同长度的字符串，会被误删除
* 二是在遍历队列时，没使用isEmpty来判断队列是否为空，而是采用dq.size()来比对，导致在poll之后，遍历出现错误，导致获得不到相同长度的字符串。

**结束**：考完之后，修改了上面两个bug，但仍有疑虑，若是一系列字符串有相同的字符串出现，如"a,a,b,ab"，那么我写的程序就仍然是有问题的，难受！

```
package com.qgao.main.xingyeshujin;

import java.util.*;


public class Solution {

    ArrayList<ArrayList<String>> arrayList = new ArrayList<>();
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     *
     * @param words string字符串一维数组
     * @return string字符串
     */
    public String longestWord (String[] words) {

        words = new String[]{"apple","iOS","dog","nana","man","good","goodman","gooddog"};

        // write code here
        if(words.length == 0) return "";

        //先找出从大往小的序列
        Deque<String> dq = new LinkedList<>();
        for(int i = 0; i < words.length; i++){
            while(!dq.isEmpty() && dq.peekLast().length() < words[i].length()){
                dq.pollLast();
            }
            dq.offerLast(words[i]);
        }
        //然后取出相同长度的字符串
        int len = dq.iterator().next().length();
        List<String> strList = new ArrayList<>();
        while(!dq.isEmpty()){
            String str = dq.pollFirst();
            if(str.length() == len){
                strList.add(str);
            }
        }
        //按字典序排序
        Collections.sort(strList);



//        return strList.get(0);
        for(int i = 0; i < strList.size(); i++){
            String key = strList.get(i);
            ArrayList<String> wordList = new ArrayList<>();
            for(String word : words){
                if(key.contains(word) && !key.equals(word) && word.length()<key.length()){
                    wordList.add(word);
                }
            }
            boolean[] isVisited = new boolean[wordList.size()];
            //回溯：字符串全排列
            traverse(wordList, isVisited, 0,new ArrayList<>());


            for(ArrayList<String> alist : arrayList){
                StringBuilder sb = new StringBuilder();
                for(String a : alist){
                    sb.append(a);
                }
                if(key.equals(sb.toString())) return key;
            }
            arrayList = new ArrayList<>();
        }
        return "";
    }

    public void traverse(ArrayList<String> wordList, boolean[] isVisited, int step, ArrayList<String> res){
        if(step == wordList.size()) {
            arrayList.add(new ArrayList<>(res));
            return;
        }
        for(int i = 0; i < wordList.size(); i++){
            if(isVisited[i]) continue;

            isVisited[i] = true;
            res.add(wordList.get(i));
            traverse(wordList,isVisited,++step,res);
            step--;
            res.remove(res.size()-1);
            isVisited[i] = false;
        }
    }

    public static void main(String[] args) {
        new Solution().longestWord(null);
    }
}
```