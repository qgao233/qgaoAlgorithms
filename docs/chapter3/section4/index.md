# 华为机试

## 1 分解质因子

1. 对 n 进行分解质因数，应先找到一个最小的质数 k，然后按下述步骤完成：
2. 如果这个质数恰等于 n，则说明分解质因数的过程已经结束，打印出即可。
3. 如果 n <> k，但 n 能被 k 整除，则应打印出 k 的值，并用 n 除以 k 的商,作为新的正整数，的 n,重复执行第一步
4. 如果 n 不能被 k 整除，则用 k+1 作为 k 的值,重复执行第一步。

```
package com.qgao.main.huawei;

import java.util.ArrayList;
import java.util.Scanner;

public class Fjzyz {


        public static void main(String[] args) {
            Scanner in = new Scanner(System.in);
            int x = 180;
            int i = 2;
            ArrayList<Integer> alist = new ArrayList<>();
            while(i <= x){
                if(i == x){
                    alist.add(i);
                    break;
                }else if(x % i == 0){
                    alist.add(i);
                    x /= i;
                }else{
                    i++;
                }
            }
            for (int a : alist){
                System.out.print(a+" ");
            }
        }
}

```

## 2 购物单

[购物单](https://www.nowcoder.com/practice/f9c6f980eeec43ef85be20755ddbeaf4?tpId=37&tags=&title=&difficulty=0&judgeStatus=0&rp=1&sourceUrl=%2Fexam%2Foj%2Fta%3FtpId%3D37)

本质上是0-1背包问题，但