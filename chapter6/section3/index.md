# 进阶练习

## 1. 抢金块

```
抢金块(dp入门)
【问题描述】 
地面上有一些格子，每个格子上面都有金块，但不同格子上的金块有不同的价值，你一次可以跳S至T步(2≤S＜T≤10)。例如S=2，T=4，你就可以跳2步、3步或4步。你从第一个格子起跳，必须跳到最后一个格子上，请你输出最多可以获得的金块的总价值。 
【输入格式】 
第一行是格子个数n(n＜1000)；第二行是S和T，保证T大于S(2≤S＜T≤10)； 
第三行是每个格子上的金块价值Pi(Pi＜10000)。 
【输出格式】 
输出最多可以获得的金块的总价值。 
【输入样例】 
10 
2 3 
4 5 8 2 8 3 6 7 2 9 
【输出样例】 
36 
样例说明：跳1、3、5、8、10，总价值：4+8+8+7+9=36
```

```
package 练习;

public class 抢金块 {

	public static void main(String[] args) {
		//c语言
//		int n,S,T,v[1001],dp[1001]={0};
//	    scanf("%d%d%d",&n,&S,&T);
//
//	    for(int i = 1;i<=n;i++){
//	        scanf("%d",&v[i]);
//	    }
//	    dp[1] = v[1];
//	    for(int i = 2;i<=n;i++){
//	        for(int j = S;j<=T;j++){
//	            if(i-j>0){
//	                dp[i] = max(dp[i],dp[i-j]+v[i]);
//	            }
//	        }
//	    }
//	    printf("%d",dp[n]);
//	    return 0;
	}
}
```

## 2. 银行家算法

```
package 练习;

import java.util.Scanner;

public class 银行家算法 {

	static int[][] Max;// 进程需求资源的最大数目
	static int[] Available;// 系统现有资源
	static int[][] Allocation;// 进程现有资源数目
	static int[][] Need;// 进程还需求的资源数
	static int[][] Request;// 第i个进程请求j类资源的数目
	static int[] SafeQueen;// 安全序列
	static int N, M;// n个进程m种资源

	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		System.out.println("进程数量：");
		N = in.nextInt();
		System.out.println("资源种类：");
		M = in.nextInt();
		Max = new int[N][M];
		Available = new int[M];
		Allocation = new int[N][M];
		Need = new int[N][M];
		Request = new int[N][M];
		SafeQueen = new int[N];
		init();
		bank();
	}

	public static void init() {
		Scanner in = new Scanner(System.in);
		// Max
		for (int i = 0; i < N; i++) {
			System.out.println("请输入第" + (i + 1) + "个进程最多需要的各种资源数目[Max]，一行输入空格隔开");
			for (int j = 0; j < M; j++) {
				Max[i][j] = in.nextInt();
			}
		}
		// Available
		System.out.println("输入系统现有各资源的数量[Available]，一行输入空格隔开");
		for (int j = 0; j < M; j++) {
			Available[j] = in.nextInt();
		}
		// Allocation Need
		for (int i = 0; i < N; i++) {
			System.out.println("请输入第" + (i + 1) + "个进程现有的各种资源数目[Allocation]，一行输入空格隔开");
			for (int j = 0; j < M; j++) {
				int tmp = in.nextInt();
				if (tmp > Max[i][j]) {
					System.out.println("资源的数量输入不合法");
					j = 0;
					continue;
				}
				Allocation[i][j] = tmp;
				Need[i][j] = Max[i][j] - tmp;
			}
		}
	}

	private static void bank() {
		System.out.println("---------------------------------");
		System.out.print("第几个进程要请求资源：");
		Scanner in = new Scanner(System.in);
		int index = in.nextInt() - 1;
		System.out.println("输入请求各资源的数目，一行输入空格隔开");
		int flagA = 0, flagB = 0;
		for (int j = 0; j < M; j++) {
			Request[index][j] = in.nextInt();
			if (Request[index][j] > Need[index][j])
				flagA = 1;
			if (Request[index][j] > Available[j])
				flagB = 1;
		}
		if (flagA == 1 || flagB == 1) {
			System.out.println("申请资源失败");
			return;
		}
		for (int j = 0; j < M; j++) {
			Available[j] -= Request[index][j];
			Need[index][j] -= Request[index][j];
			Allocation[index][j] += Request[index][j];
		}

		if (safe(index)) {
			System.out.println("系统是安全的，安全序列：");
			for (int i = 0; i < N - 1; i++)
				System.out.print(SafeQueen[i] + 1 + "->");
			System.out.print(SafeQueen[N - 1] + 1);
			System.out.println("完成该申请后的资源分配表如下：");
			showData();
			System.out.println("-----------------------");
			System.out.println("继续申请请求请输入 1");
			String s = in.next();
			if (s.equals("1"))
				bank();
		} else {
			for (int j = 0; j < M; j++) {
				Available[j] += Request[index][j];
				Need[index][j] += Request[index][j];
				Allocation[index][j] -= Request[index][j];
			}
			System.out.println("资源分配表如下：");
			showData();
			System.out.println("-------------------");
			System.out.println("系统不安全，请求失败，重新申请请求请输入 1");
			String s = in.next();
			if (s.equals("1"))
				bank();
		}
	}

	public static boolean safe(int index) {
		int[] work = new int[M];
		Boolean[] Finish = new Boolean[N];
		int count = 0;
		int flag = 0;
		for (int i = 0; i < N; i++)
			Finish[i] = false;

		Boolean fanren = false;
		for (int j = 0; j < M; j++) {
			if (Need[index][j] != 0) {
				fanren = true;
				break;
			}
		}
		if (fanren == false) {
			Finish[index] = true;
			count++;
			SafeQueen[0] = index;
			for (int j = 0; j < M; j++) {
				work[j] = Available[j] + Allocation[index][j];
			}
		} else {
			for (int j = 0; j < M; j++) {
				work[j] = Available[j];
			}
		}

		while (true) {
			for (int i = 0; i < N; i++) {
				if (Finish[i] == true)
					continue;
				int fffflag = 0;
				for (int j = 0; j < M; j++) {
					if (Need[i][j] > work[j])
						fffflag = 1;
				}
				if (fffflag == 1)
					continue;
				Finish[i] = true;
				count++;
				SafeQueen[count - 1] = i;
				for (int j = 0; j < M; j++) {
					work[j] = work[j] + Allocation[i][j];
				}
			}
			flag++;
			if (flag > N)
				break;
		}
		if (count == N) {
			for (int j = 0; j < M; j++) {
				Available[j] += Allocation[index][j];
				Allocation[index][j] = 0;
			}
			return true;
		} else
			return false;
	}

	private static void showData() {
		System.out.print("     [Available]");
		for (int j = 0; j < M; j++)
			System.out.print(Available[j] + " ");
		System.out.println();
		for (int i = 0; i < N; i++) {
			System.out.print("第" + (i + 1) + "个进程:[Max]");
			for (int j = 0; j < M; j++)
				System.out.print(Max[i][j] + " ");
			System.out.print("[Allocation]");
			for (int j = 0; j < M; j++)
				System.out.print(Allocation[i][j] + " ");
			System.out.print("[Need]");
			for (int j = 0; j < M; j++)
				System.out.print(Need[i][j] + " ");
			System.out.println();
		}
	}
}
```

## 3. 硬币问题

```
package 练习;

import java.util.Scanner;

public class 硬币问题 {

	public static void main(String[] args) {
		int num,N,coin[]=new int[10],mins=0x7fffffff;
	    num = 3;
	    Scanner scanner = new Scanner(System.in);
	    for(int i = 0;i<num;i++){
	        coin[i] = scanner.nextInt();
	        mins = coin[i]>mins?mins:coin[i];
	    }
	    N=5;
	    int[] dp = new int[10];
	    for(int i = mins ;i<=N;i++){
	        int min = 0x7fffffff;
	        for(int j = 0;j<num;j++){
	            if(i-coin[j]>=0){
	                if(i==coin[j]){min=1;break;}//如果刚好等于面值，那么这肯定是最佳的
	                if(dp[i-coin[j]]>0)min = dp[i-coin[j]]+1<min?dp[i-coin[j]]+1:min;//i-coin[j]表示，我先选中coin[j]面值，那么之前的方法数dp[i-coin[j]]再加上选中的coin[j]的方法数为1,即dp[i-coin[j]]+1
	            }
	        }
	        dp[i] = min==0x7fffffff?0:min;
	    }
	    for(int i = 2;i<=N;i++){
	        System.out.println(dp[i]);
	    }

	}

}
```

## 4. 冰雹数

```
任意给定一个正整数N，
如果是偶数，执行： N / 2
如果是奇数，执行： N * 3 + 1

生成的新的数字再执行同样的动作，循环往复。

通过观察发现，这个数字会一会儿上升到很高，
一会儿又降落下来。
就这样起起落落的，但最终必会落到“1”
这有点像小冰雹粒子在冰雹云中翻滚增长的样子。

比如N=9
9,28,14,7,22,11,34,17,52,26,13,40,20,10,5,16,8,4,2,1
可以看到，N=9的时候，这个“小冰雹”最高冲到了52这个高度。

输入格式：
一个正整数N（N<1000000）
输出格式：
一个正整数，表示不大于N的数字，经过冰雹数变换过程中，最高冲到了多少。

例如，输入：
10
程序应该输出：
52

再例如，输入：
100
程序应该输出：
9232
```

```
package 七届JavaC组蓝桥杯;

import java.util.Scanner;

public class 冰雹数 {

	public static void main(String[] args) {
		int maxNum,maxHeight = -1;
		Scanner scanner = new Scanner(System.in);
		maxNum = scanner.nextInt();
//		for(int i=1;i<=maxNum;i++){
			int n = maxNum;
//			System.out.print(n+"#,");
			while(n!=1){
				System.out.print(n+",");
				if(n > maxHeight){
					maxHeight = n;
				}
				n = transform(n);
			}
//		}
		System.out.print(n+"\n");
		System.out.println(maxHeight);
		
	}

	public static int transform(int num){
		if(num % 2 == 0){
			return num/2;
		}else{
			return num*3 + 1;
		}
	}
}
```

## 5. 凑算式

```
凑算式

     B      DEF
A + --- + ------- = 10
     C      GHI
     
（如果显示有问题，可以参见【图1.jpg】）
	 
	 
这个算式中A~I代表1~9的数字，不同的字母代表不同的数字。

比如：
6+8/3+952/714 就是一种解法，
5+3/1+972/486 是另一种解法。

这个算式一共有多少种解法？
```

```
package 七届JavaC组蓝桥杯;

public class 凑算式 {

	public static int num = 0;
	public static boolean visited[] = new boolean[10];
	public static int permutation[] = new int[10];
	
	public static void main(String[] args) {
		getPermutation(1);
		System.out.println(num);
	}

	public static void getPermutation(int level){
		if(level == 10){
			check();
			return;
		}
		for(int i=1;i<10;i++){
			if(visited[i] == false){
				visited[i] = true;
				permutation[level] = i;
				getPermutation(level+1);
				visited[i] = false;
			}
		}
	}
	
	public static void check(){
		if((permutation[1]
				+(double)permutation[2]/(double)permutation[3]
				+((double)permutation[4]*100+(double)permutation[5]*10+(double)permutation[6])
						/((double)permutation[7]*100+(double)permutation[8]*10+(double)permutation[9]))
				== (double)10){
			num++;
		}
	}
}
```

## 6. 搭积木

```
小明最近喜欢搭数字积木，
一共有10块积木，每个积木上有一个数字，0~9。

搭积木规则：
每个积木放到其它两个积木的上面，并且一定比下面的两个积木数字小。
最后搭成4层的金字塔形，必须用完所有的积木。

下面是两种合格的搭法：

   0
  1 2
 3 4 5
6 7 8 9

   0
  3 1
 7 5 2
9 8 6 4    

请你计算这样的搭法一共有多少种？
```

```
package 七届JavaC组蓝桥杯;

public class 搭积木 {

	public static int num = 0;
	public static boolean visited[] = new boolean[10];
	public static int permutation[] = new int[10];
	
	public static int n[]={1,2,2,3,3,3};
	
	public static void main(String[] args) {
		getPermutation(1);
		System.out.println(num);
	}

	public static void getPermutation(int level){
		if(level == 10){
			check();
			return;
		}
		for(int i=1;i<10;i++){
			if(visited[i] == false){
				visited[i] = true;
				permutation[level] = i;
				getPermutation(level+1);
				visited[i] = false;
			}
		}
	}
	
	public static void check(){
		for(int i=1;i<6;i++){
			if(permutation[i] > permutation[i+n[i]] || permutation[i] > permutation[i+n[i]+1])
				return;
		}
		num++;
	}

}
```

## 7. 分小组

```
9名运动员参加比赛，需要分3组进行预赛。
有哪些分组的方案呢？

我们标记运动员为 A,B,C,... I
下面的程序列出了所有的分组方法。

该程序的正常输出为：
ABC DEF GHI
ABC DEG FHI
ABC DEH FGI
ABC DEI FGH
ABC DFG EHI
..... (以下省略，总共560行)。
```

```
package 七届JavaC组蓝桥杯;

public class 分小组 {
	
	public static int total;
	
	public static void main(String[] args) {
		int[] a = new int[9];
		a[0] = 1;
		for(int b=1;b<a.length;b++){
			a[b] = 1;
			for(int c=b+1;c<a.length;c++){
				a[c] = 1;
				String s = "A"+(char)(b+'A')+(char)(c+'A');
				dfs(s,a);
				a[c] = 0;
			}
			a[b] = 0;
		}
		System.out.println(total+"行");
	}
	
	public static void dfs(String s,int[] a){
		for(int i=0;i<a.length;i++){
			if(a[i] == 1)
				continue;
			a[i] = 1;
			for(int j = i+1;j<a.length;j++){
				if(a[j] == 1)
					continue;
				a[j] = 1;
				for(int k=j+1;k<a.length;k++){
					if(a[k] == 1)
						continue;
					a[k] = 1;
					System.out.println(s+""+(char)(i+'A')+(char)(j+'A')+(char)(k+'A')+""+remain(a));
					total++;a[k] = 0;
				}
				a[j] = 0;
			}
			a[i] = 0;
		}
	}
	
	public static String remain(int[] a){
		String s = "";
		for(int i=0;i<a.length;i++){
			if(a[i] == 0){
				s += (char)(i+'A');
			}
		}
		return s;
	}

}
```

## 8. 密码脱落

```
X星球的考古学家发现了一批古代留下来的密码。
这些密码是由A、B、C、D 四种植物的种子串成的序列。
仔细分析发现，这些密码串当初应该是前后对称的（也就是我们说的镜像串）。
由于年代久远，其中许多种子脱落了，因而可能会失去镜像的特征。

你的任务是：
给定一个现在看到的密码串，计算一下从当初的状态，它要至少脱落多少个种子，才可能会变成现在的样子。

输入一行，表示现在看到的密码串（长度不大于1000）
要求输出一个正整数，表示至少脱落了多少个种子。

例如，输入：
ABCBA
则程序应该输出：
0

再例如，输入：
ABDCDCBABC
则程序应该输出：
3
```

```
package 七届JavaC组蓝桥杯;

import java.util.Scanner;

/**
 * 此题可以用动态规划与最长公共子序列得出答案，
 * 也可以用普通的递归遍历或者加上贪心算法，使程序的时间复杂度缩小到O(n^2)
 * 这里选择普通的递归遍历
 * @author Bermuda
 *
 */
public class 密码脱落 {

	public static int length;
	public static char[] s;
	public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);
		s = scanner.nextLine().toCharArray();
		length = s.length;
		recursion(0, length-1, 0);
		System.out.println(length);
	}

	public static void recursion(int start,int end,int total){
		if(start >= end){
			length = length < total?length:total;
		}
		else{
			if(s[start] == s[end]){
				recursion(start+1, end-1, total);
			}else{
				recursion(start+1, end, total+1);
				recursion(start, end-1, total+1);
			}
		}
	}
}
```

## 9. 平方怪圈

```
如果把一个正整数的每一位都平方后再求和，得到一个新的正整数。
对新产生的正整数再做同样的处理。

如此一来，你会发现，不管开始取的是什么数字，
最终如果不是落入1，就是落入同一个循环圈。

请写出这个循环圈中最大的那个数字。
```

```
package 七届JavaC组蓝桥杯;

public class 平方怪圈 {

	public static void main(String[] args) {
		int num = 60;
		for(int i=0;i<100;i++){
			System.out.print(num+"#");
			num = transform(num);
		}
	}
	
	public static int transform(int num){
		char[] array = String.valueOf(num).toCharArray();
		int result = 0;
		for(char s:array){
			result += (s-'0')*(s-'0');
		}
		return result;
	}

}
```

## 10. 有奖猜谜

```
小明很喜欢猜谜语。
最近，他被邀请参加了X星球的猜谜活动。

每位选手开始的时候都被发给777个电子币。
规则是：猜对了，手里的电子币数目翻倍，
猜错了，扣除555个电子币, 扣完为止。

小明一共猜了15条谜语。
战果为：vxvxvxvxvxvxvvx
其中v表示猜对了，x表示猜错了。

请你计算一下，小明最后手里的电子币数目是多少。
```

```
package 七届JavaC组蓝桥杯;

public class 有奖猜谜 {

	public static void main(String[] args) {
		int init = 777;
		char[] key = "vxvxvxvxvxvxvvx".toCharArray();
		for(int i = 0;i<15;i++){
			if(key[i] == 'v'){
				init *= 2;
			}else{
				init -= 555;
			}
		}
		System.out.println(init);
	}

}
```