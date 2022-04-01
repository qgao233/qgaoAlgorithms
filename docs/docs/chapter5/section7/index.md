# 精度算法

## 1. 大数加法

```
题目：
输入两个整数a和b，输出这两个整数的和。a和b都不超过100位。
算法描述
　　由于a和b都比较大，所以不能直接使用语言中的标准数据类型来存储。对于这种问题，一般使用数组来处理。
　　定义一个数组A，A[0]用于存储a的个位，A[1]用于存储a的十位，依此类推。同样可以用一个数组B来存储b。
　　计算c = a + b的时候，首先将A[0]与B[0]相加，如果有进位产生，则把进位（即和的十位数）存入r，把和的个位数存入C[0]，即C[0]等于(A[0]+B[0])%10。然后计算A[1]与B[1]相加，这时还应将低位进上来的值r也加起来，即C[1]应该是A[1]、B[1]和r三个数的和．如果又有进位产生，则仍可将新的进位存入到r中，和的个位存到C[1]中。依此类推，即可求出C的所有位。
　　最后将C输出即可。
输入格式
　　输入包括两行，第一行为一个非负整数a，第二行为一个非负整数b。两个整数都不超过100位，两数的最高位都不是0。
输出格式
　　输出一行，表示a + b的值。
样例输入
20100122201001221234567890
2010012220100122
样例输出
20100122203011233454668012
```

```
package 精度算法;

import java.util.Scanner;

public class Step1 {
	
	private static final int MAX_SIZE = 1000;
	
	public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);
		String a = scanner.nextLine();
		String b = scanner.nextLine();
		scanner.close();
		add(stringToInts(a), stringToInts(b));
	}
	
	public static void add(int[] a, int[] b) {
		int[] result = new int[MAX_SIZE];
		int forward = 0;
		for(int i = 0;i<MAX_SIZE;i++) {
			result[i] = (a[i] + b[i] + forward)%10;
			forward = (a[i] + b[i] + forward)/10;
		}
		int i;
		for(i = MAX_SIZE-1;i>=0;i--) {
			if(result[i] != 0) {
				break;
			}
		}
		for(; i>= 0 ;i--) {
			System.out.print(result[i]);
		}
	}

	public static int[] stringToInts(String s) {
		int[] n = new int[MAX_SIZE];
		int i = 0;
		int j = s.length();
		for (; i < j; i++) {
//			n[i] = Integer.parseInt(s.substring(i, i + 1)); 顺取
			n[i] = Integer.parseInt(s.substring(j-i-1, j-i)); //倒取
		}
		for(; i < MAX_SIZE;i++) {
			n[i] = 0;
		}
		return n;
	}
}
```

## 2. 大数乘法

```
package 精度算法;

import java.util.Arrays;
import java.util.Scanner;

public class Step2 {
	
	private static final int MAX_SIZE = 1000;
	
	public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);
		String a = scanner.nextLine();
		String b = scanner.nextLine();
		scanner.close();
		multiply(stringToInts(a), stringToInts(b),b.length());
	}
	
	public static void multiply(int[] a,int[] b,int bLength) {
		if(null == a || null == b) {
			return;
		}
		int[] result = new int[MAX_SIZE];
		Arrays.fill(result, 0);
		int temp;
		for(int i = 0;i<bLength;i++) {
			temp = i;
			for(int j = 0;j<MAX_SIZE && temp < MAX_SIZE;j++) {
				result[temp] += a[j] * b[i];
				temp++;
			}
		}
		int foward = 0;
		for(int i = 0;i < MAX_SIZE;i++) {
			temp = result[i] + foward;
			result[i] = temp%10;
			foward = temp/10;
		}
		output(result);
	}
	
	public static void output(int[] result) {
		int i;
		for(i = MAX_SIZE-1;i>=0;i--) {
			if(result[i] != 0) {
				break;
			}
		}
		for(; i>= 0 ;i--) {
			System.out.print(result[i]);
		}
	}
	
	public static int[] stringToInts(String s) {
		if(null == s) {
			return null;
		}
		int[] n = new int[MAX_SIZE];
		int i = 0;
		int j = s.length();
		for (; i < j; i++) {
//			n[i] = Integer.parseInt(s.substring(i, i + 1)); 顺取
			n[i] = Integer.parseInt(s.substring(j-i-1, j-i)); //倒取
		}
		for(; i < MAX_SIZE;i++) {
			n[i] = 0;
		}
		return n;
	}
}
```

## 3. 大数阶乘

```
问题描述
　　输入一个正整数n，输出n!的值。
　　其中n!=1*2*3*…*n。
算法描述
　　n!可能很大，而计算机能表示的整数范围有限，需要使用高精度计算的方法。使用一个数组A来表示一个大整数a，A[0]表示a的个位，A[1]表示a的十位，依次类推。
　　将a乘以一个整数k变为将数组A的每一个元素都乘以k，请注意处理相应的进位。
　　首先将a设为1，然后乘2，乘3，当乘到n时，即得到了n!的值。
输入格式
　　输入包含一个正整数n，n<=1000。
输出格式
　　输出n!的准确值。
样例输入
10
样例输出
3628800
```

```
package 精度算法;

import java.util.Scanner;

public class Step3 {
	
	private static final int MAX_SIZE = 1000;
	
	public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);
		int n = scanner.nextInt();
		scanner.close();
		factorial(n);
	}
	
	public static void factorial(int n) {
		int[] a = stringToInts("1");
		int forward;
		int temp;
		for(int i=2;i<=n;i++) {
			for(int j=0;j<MAX_SIZE;j++) {
				a[j] *= i;
			}
			forward = 0;
			temp = 0;
			for(int j=0;j<MAX_SIZE;j++) {
				temp = a[j]+forward;
				a[j] = temp%10;
				forward = temp/10;
			}
		}
		output(a);
	}
	
	public static void output(int[] result) {
		int i;
		for(i = MAX_SIZE-1;i>=0;i--) {
			if(result[i] != 0) {
				break;
			}
		}
		for(; i>= 0 ;i--) {
			System.out.print(result[i]);
		}
	}
	
	public static int[] stringToInts(String s) {
		int[] n = new int[MAX_SIZE];
		int i = 0;
		int j = s.length();
		for (; i < j; i++) {
//			n[i] = Integer.parseInt(s.substring(i, i + 1)); 顺取
			n[i] = Integer.parseInt(s.substring(j-i-1, j-i)); //倒取
		}
		for(; i < MAX_SIZE;i++) {
			n[i] = 0;
		}
		return n;
	}
}
```