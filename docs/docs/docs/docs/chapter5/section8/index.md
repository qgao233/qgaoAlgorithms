# 贪心算法

## 1. 完美的代价

```
问题描述
　　回文串，是一种特殊的字符串，它从左往右读和从右往左读是一样的。小龙龙认为回文串才是完美的。现在给你一个串，它不一定是回文的，请你计算最少的交换次数使得该串变成一个完美的回文串。
　　交换的定义是：交换两个相邻的字符
　　例如mamad问题描述
　　回文串，是一种特殊的字符串，它从左往右读和从右往左读是一样的。小龙龙认为回文串才是完美的。现在给你一个串，它不一定是回文的，请你计算最少的交换次数使得该串变成一个完美的回文串。
　　交换的定义是：交换两个相邻的字符
　　例如mamad
　　第一次交换 ad : mamda
　　第二次交换 md : madma
　　第三次交换 ma : madam (回文！完美！)
输入格式
　　第一行是一个整数N，表示接下来的字符串的长度(N <= 8000)
　　第二行是一个字符串，长度为N.只包含小写字母
输出格式
　　如果可能，输出最少的交换次数。
　　否则输出Impossible
样例输入
5
mamad
样例输出
3
```

```
package 贪心算法;

import java.util.Scanner;

class Task implements Runnable{

	private char[] cString = new char[] {'m','a','m','a','d'};
	
	public Task() {
	}

	public Task(char[] cString) {
		this.cString = cString;
	}

	@Override
	public synchronized void run() {
		//总步数
		int sum = 0;
		//输入字符串不能达到目标要求的标志
		int flag = 0;
		//不止一个无相同的字符
		int single = 0;
		//代码优化
		int n = cString.length;
		//尾指针指向
		int point = n-1;
		for (int i = 0;i < n;i++) {
			for(int j = point;j >= i;j--) {
				if(i == j) {
					if(n % 2 == 0 || single != 0) {
						flag = 1;
						break;
					}
					sum += n/2 - i;
					single = 1;
					break;
				}
				if(cString[i] == cString[j]) {
					for(int k = j;k < point;k++) {
						cString[k] = cString[k+1];
					}
					cString[point] = cString[i];
					sum += point - j;
					point--;
					break;
				}
			}
			if(flag == 1) {
				break;
			}
		}
		System.out.println(flag == 1?"impossible":"最少交换次数："+sum);
	}
	
}

public class Step1 {
	public static void main(String[] args) {
//		Scanner scanner = new Scanner(System.in);
//		int n = scanner.nextInt();
//		scanner.nextLine();//清除nextInt在缓冲区留下的“\r”
//		String string = scanner.nextLine();
//		char[] cs = string.toCharArray();
//		Task task = new Task(cs);
		Task task = new Task();
		new Thread(task).start();
	}
}
```