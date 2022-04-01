# 动态规划

## 1. k好数

```
问题描述
如果一个自然数N的K进制表示中任意的相邻的两位都不是相邻的数字，那么我们就说这个数是K好数。求L位K进制数中K好数的数目。例如K = 4，L = 2的时候，所有K好数为11、13、20、22、30、31、33 共7个。由于这个数目很大，请你输出它对1000000007取模后的值。

输入格式
输入包含两个正整数，K和L。

输出格式
输出一个整数，表示答案对1000000007取模后的值。
样例输入
4 2
样例输出
7
数据规模与约定
对于30%的数据，K^L <= 106；

对于50%的数据，K <= 16， L <= 10；

对于100%的数据，1 <= K,L <= 100。
```

```
package 动态规划;

import java.util.Arrays;
import java.util.Scanner;


//D:\Program Files (x86)\eclipse for java\eclipse for storing\ExerciseOfCalculation\src\动态规划\kgood.png
public class Step2 {
	
	private static final int num = 1000000007;
	
	public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);
		int K = scanner.nextInt();
		int L = scanner.nextInt();
		scanner.close();
//		int L = 2;
//		int K = 4;
		
		int[][] dp = new int[101][101];
		Arrays.fill(dp[1], 1); 
		for(int i = 2;i<=L;i++) {
			int j = 0;
			if(i == 2) {//这里当i=2时，跳过了j=0，是因为要符合题目给的答案，答案中当计算是2位时，没有考虑最高位是0的情况。
				j = 1;
			}
			for(;j<K;j++) {
				for(int k = 0;k<K;k++) {
					if(Math.abs(k-j) != 1) {
						dp[i][j] += dp[i-1][k];
					}
				}
			}
		}
		int sum = 0;
		for(int i = 0;i<K;i++) {
			sum += dp[L][i];
		}
		System.out.println(sum%num);
	}
}
```