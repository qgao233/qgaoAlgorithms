# 递归算法

## 1. FJ的字符串

```
问题描述
　　FJ在沙盘上写了这样一些字符串：
　　A1 = “A”
　　A2 = “ABA”
　　A3 = “ABACABA”
　　A4 = “ABACABADABACABA”
　　… …
　　你能找出其中的规律并写所有的数列AN吗？
输入格式
　　仅有一个数：N ≤ 26。
输出格式
　　请输出相应的字符串AN，以一个换行符结束。输出中不得含有多余的空格或换行、回车符。
样例输入
3 
样例输出
ABACABA
```

```
package 递归算法;

import java.util.Scanner;

public class Step1
{
	public static void main(String[] args){
		Scanner scanner = new Scanner(System.in);
		int n = scanner.nextInt();
		scanner.close();
		System.out.println(fun(n));
	}

	public static String fun(int n){
		if(1 == n){
			return "A";
		}
		return fun(n-1)+(char)('A'+n-1)+fun(n-1);
	}
}
```

## 2. Sine之舞

```
问题描述
　　最近FJ为他的奶牛们开设了数学分析课，FJ知道若要学好这门课，必须有一个好的三角函数基本功。所以他准备和奶牛们做一个“Sine之舞”的游戏，寓教于乐，提高奶牛们的计算能力。
　　不妨设
　　An=sin(1–sin(2+sin(3–sin(4+...sin(n))...)
　　Sn=(...(A1+n)A2+n-1)A3+...+2)An+1
　　FJ想让奶牛们计算Sn的值，请你帮助FJ打印出Sn的完整表达式，以方便奶牛们做题。
输入格式
　　仅有一个数：N<201。
输出格式
　　请输出相应的表达式Sn，以一个换行符结束。输出中不得含有多余的空格或换行、回车符。
样例输入
3
样例输出
((sin(1)+3)sin(1–sin(2))+2)sin(1–sin(2+sin(3)))+1
```

```
package 递归算法;

import java.util.Scanner;

public class Step2
{
	public static void main(String[] args){
		Scanner scanner = new Scanner(System.in);
		int n = scanner.nextInt();
		scanner.close();
		System.out.println(a(n));
	}

	public static String s(int n){
		int count = n;
		StringBuilder sb = new StringBuilder();
		for(int i = 1;i<=n-1;i++){
			System.out.print("(");
		}
		for(int i = 1;i<=n;i++){
			if(i == 1){
				sb.append(a(i)+"+"+count--);
			}else{
				sb.append(")"+a(i)+"+"+count--);
			}
		}
		return sb.toString();
	}

	public static String a(int n){
		if(1 == n){
			return "sin(1";
		}else if(0 == n%2){
			return a(n-1)+"-sin("+n;
		}else{
			return a(n-1)+"+sin("+n;
		}
		
	}
}
```

```
package 递推算法;

import java.util.Scanner;

public class Step1
{
	public static void main(String[] args){
		Scanner scanner = new Scanner(System.in);
		int n = scanner.nextInt();
		scanner.close();
		System.out.println(s(n));
	}

	public static String s(int n){
		int count = n;
		StringBuilder sb = new StringBuilder();
		for(int i = 1;i<=n-1;i++){
			System.out.print("(");
		}
		for(int i = 1;i<=n;i++){
			if(i == 1){
				sb.append(a(i)+"+"+count--);
			}else{
				sb.append(")"+a(i)+"+"+count--);
			}
		}
		return sb.toString();
	}

	public static String a(int n){
		StringBuilder sb = new StringBuilder();
		for(int i=1;i<=n;i++){
			if(i == 1){
				sb.append("sin("+i);
				continue;
			}else if(i % 2 == 0){
				sb.append("-sin("+i);
			}else{
				sb.append("+sin("+i);
			}
		}
		for(int i=1;i<=n;i++){
			sb.append(")");
		}
		return sb.toString();
	}
}
```

## 3. 最长三角形

动态规划

```
在上面的数字三角形中寻找一条从顶部到底边的路径，使得路径上所经过的数字之和最大。路径上的每一步都只能往左下或 右下走。
只需要求出这个最大和即可，不必给出具体路径。 三角形的行数大于1小于等于100，数字为 0 - 99
    输入格式：
    5      //表示三角形的行数    接下来输入三角形
    7
    3   8
    8   1   0
    2   7   4   4
    4   5   2   6   5
    要求输出最大和
```

```
package 递归算法;

import java.util.Scanner;

/*
 * 从递归开始：Step1
 */

public class Step3 {
	
	private int[][] array ;
	static int n;
	
	public static void main(String[] args) {
		Step3 step1 = new Step3();
		step1.start();
		System.out.println(step1.MaxSum(1, 1));
	}
	
	public int MaxSum(int i,int j) {
		if(i == n) {
			return array[i][j];
		}
		return Math.max(MaxSum(i+1, j), MaxSum(i+1, j+1))+array[i][j];
	}
	
	public void start() {
		Scanner scanner = new Scanner(System.in);
		n = scanner.nextInt();
		array = new int[n+1][n+1];
		for(int i = 1; i <= n;i++) {
			for(int j = 1; j <= i ;j++) {
				array[i][j] = scanner.nextInt();
			}
		}
		scanner.close();
	}
}
```