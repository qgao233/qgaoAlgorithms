# 基础练习

## 1. 和为n的回文数

```
package gq.ExerciseOfFoundation;

import java.util.Scanner;

/**
 * 123321是一个非常特殊的数，它从左边读和从右边读是一样的。
 * 输入一个正整数n， 编程求所有这样的五位和六位十进制数，满足各位数字之和等于n 。
 * @author Bermuda
 *
 */
public class Palindrome {

	public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);
		int sum = scanner.nextInt();
		for(int i = 10000; i < 1000000; i++){
			String orgString = String.valueOf(i);
			String newString = new StringBuffer(orgString).reverse().toString();
			if(orgString.equals(newString)){
				char[] tmp = orgString.toCharArray();
				int tmp1 = 0;
				for(int j = 0; j < tmp.length; j++){
					tmp1 += Character.getNumericValue(tmp[j]);
				}
				if(tmp1 == sum){
					System.out.println(orgString);
				}
			}
		}
		
	}
}
```

## 2. 乘法算式

```
package gq.test;

public class 乘法算式 {

	static int[] a = new int[10];
	
	public static void main(String[] args) {
		for(int i = 100;i<1000;i++){
			for(int j = 100;j<1000;j++){
				count(i);
				count(j);
				count(i * (j % 10));
				count(i * (j / 10 % 10));
				count(i * (j / 100));
				count(i * j);
				if(validate()){
					System.out.println(i*j);
				}
			}
		}
		
	}
	
	public static void count(int key){
		while(key > 0){
			a[key%10] += 1;		//核心代码,计算每个数字出现了多少次
			key /= 10;
		}
	}
	
	public static boolean validate(){
		for(int i = 0; i < a.length; i++){
			if(a[i] != 2){
				for(int j = 0; j < a.length; j++){
					a[j] = 0;
				}
				return false;
			}
		}
		return true;
	}
}
```

## 3. 还款计算

```
银行贷款的等额本息还款方法是：
每月还固定的金额，在约定的期数内正好还完（最后一个月可能会有微小的零头出入）。

比如说小明在银行贷款1万元。贷款年化利率为5%，贷款期限为24个月。
则银行会在每个月进行结算：
结算方法是：计算本金在本月产生的利息： 本金 x (年利率/12)
则本月本金结余为：本金 + 利息 - 每月固定还款额 = 还欠银行多少钱
计算结果会四舍五入到“分”。

经计算，此种情况下，固定还款额应为：438.71

这样，第一月结算时的本金余额是：
9602.96
第二个月结算：
9204.26
第三个月结算：
8803.9
....
最后一个月如果仍按固定额还款，则最后仍有0.11元的本金余额，
但如果调整固定还款额为438.72, 则最后一个月会多还了银行0.14元。
银行会选择最后本金结算绝对值最小的情况来设定 每月的固定还款额度。
如果有两种情况最后本金绝对值相同，则选择还款较少的那个方案。

本题的任务是已知年化利率，还款期数，求每月的固定还款额度。

假设小明贷款为1万元，即：初始本金=1万元。
年化利率的单位是百分之多少。
期数的单位为多少个月。

输入为2行，
第一行为一个小数r，表示年率是百分之几。(0<r<30)
第二行为一个整数n，表示还款期限。 (6<=n<=120)

要求输出为一个整数，表示每月还款额（单位是：分）

例如：
输入：
4.01
24

程序应该输出：
43429

再比如：
输入：
6.85
36

程序应该输出：
30809
```

```
package gq.test;

import java.util.Scanner;

public class 还款计算 {

	public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);
		double rate = scanner.nextDouble()/100/12;
		int deadline = scanner.nextInt();
		int total = 10000;
		double perMin = total/deadline;
		double min = 0x7fffffff;
//		long a = 0x7fffffff;
		double perReturn = 0;
		for(double i = perMin;;i+=0.01){
			double money = total;
			for(int j = 0;j<deadline;j++){
				money = money*(rate+1)-i;	//核心代码
			}
			if(Math.abs(min) > Math.abs(money)){	//核心判断
				min = money;
				perReturn = i;
			}else break;
		}
		System.out.println(getInt(perReturn));
	}
	
	public static int getInt(double money){
		return (int)(money * 100 + 0.5);//四舍五入“+0.5”
	}
}
```

## 4. 滑动解锁

```
滑动解锁是智能手机一项常用的功能。你需要在3x3的点阵上，从任意一个点开始，反复移动到一个尚未经过的"相邻"的点。这些划过的点所组成的有向折线，如果与预设的折线在图案、方向上都一致，那么手机将解锁。

所谓两个点“相邻”：当且仅当以这两个点为端点的线段上不存在尚未经过的点。

此外，许多手机都约定：这条折线还需要至少经过4个点。

为了描述方便，我们给这9个点从上到下、从左到右依次编号1-9。即如下排列：

1 2 3
4 5 6
7 8 9

那么1->2->3是非法的，因为长度不足。
1->3->2->4也是非法的，因为1->3穿过了尚未经过的点2。
2->4->1->3->6是合法的，因为1->3时点2已经被划过了。

某大神已经算出：一共有389112种不同的解锁方案。没有任何线索时，要想暴力解锁确实很难。
不过小Hi很好奇，他希望知道，当已经瞥视到一部分折线的情况下，有多少种不同的方案。
遗憾的是，小Hi看到的部分折线既不一定是连续的，也不知道方向。

例如看到1-2-3和4-5-6，
那么1->2->3->4->5->6，1->2->3->6->5->4, 3->2->1->6->5->4->8->9等都是可能的方案。


你的任务是编写程序，根据已经瞥到的零碎线段，求可能解锁方案的数目。

输入：
每个测试数据第一行是一个整数N(0 <= N <= 8)，代表小Hi看到的折线段数目。
以下N行每行包含两个整数 X 和 Y (1 <= X, Y <= 9)，代表小Hi看到点X和点Y是直接相连的。

输出：
对于每组数据输出合法的解锁方案数目。


例如：
输入：
8
1 2
2 3
3 4
4 5
5 6
6 7
7 8
8 9

程序应该输出：
2

再例如：
输入：
4
2 4
2 5
8 5
8 6

程序应该输出：
258
```

```
package gq.test;

import java.util.Scanner;

public class 滑动解锁 {

	private static boolean f[] = new boolean[10];
    private static int N;
    private static int[][] path;
    static int count = 0;
    public static boolean isOK(int a,int b){
        int[][] dig = { {1,3,2},{1,7,4},{1,9,5},{2,8,5},{3,7,5},{3,9,6},{4,6,5},{7,9,8} };
        for(int i = 0;i<8;i++){
            if(dig[i][0]==a&&dig[i][1]==b||dig[i][1]==a&&dig[i][0]==b){
                if(!f[dig[i][2]])return false;
            }
        }
        return true;
    }
    public static void dfs(int number,int step,int[] process){
        if(step>=2){//当process数组里面至少有2个数时开始判断，代表我要选取最近划中的2个数字是否合法
            int a = process[step-2];
            int b = process[step-1];
            if(!isOK(a,b))return;
        }
        if(step==number){//判断是否存在此数组，存在则继续，不存在直接返回代表不符合
            for(int i = 0;i<N;i++){
                int a = path[i][0];
                int b = path[i][1];
                for(int j = 0;j<step-1;j++){
                    if(a==process[j]&&b==process[j+1]||a==process[j+1]&&b==process[j])break;
                    if(j==step-2)return;//不存在此条路线
                }
            }
            count++;
            return;
        }else if(step>number)return;
        for(int i = 1;i<=9;i++){
            if(!f[i]){
                f[i] = true;
                process[step] = i;
                dfs(number,step+1,process);
                f[i] = false;
            }
        }
    }
    public static void main(String[] args) {
        Scanner sn = new Scanner(System.in);
        N = sn.nextInt();
        path = new int[N+1][2];
        int[] process = new int[10];
        for(int i = 0;i<N;i++){
            path[i][0] = sn.nextInt();
            path[i][1] = sn.nextInt();
        }
        for(int i = N>4?N:4;i<=9;i++){//枚举划中的数字个数，最少4个数字
            dfs(i,0,process);
        }
        System.out.println(count);
        sn.close();
    }
}
```

## 5. 排列序数

```
X星系的某次考古活动发现了史前智能痕迹。
这是一些用来计数的符号，经过分析它的计数规律如下：
（为了表示方便，我们把这些奇怪的符号用a~q代替）

abcdefghijklmnopq 表示0
abcdefghijklmnoqp 表示1
abcdefghijklmnpoq 表示2
abcdefghijklmnpqo 表示3
abcdefghijklmnqop 表示4
abcdefghijklmnqpo 表示5
abcdefghijklmonpq 表示6
abcdefghijklmonqp 表示7
.....

在一处石头上刻的符号是：
bckfqlajhemgiodnp

请你计算出它表示的数字是多少？295797692
```

```
package gq.test;

import java.util.Arrays;
import java.util.Scanner;

public class 排列序数 {

	public static String letterString;
	public static char[] s;
	public static boolean[] visited;
	public static int count;
	
	public static void main(String[] args) {
		int[] que = new int[18];
		
		que[0] = 1;
		for(int i=1; i < que.length; i++){
			que[i] = i * que[i-1];	//i的阶乘
		}
		
		int k = 0;
		int line = 0;
		Scanner scanner = new Scanner(System.in);
		letterString = scanner.nextLine();
		s = new char[letterString.length()];
		visited = new boolean[letterString.length()];
//		long start = System.currentTimeMillis();
//		System.out.println(start);
		for(int i = 0; i < letterString.length(); i++){
			k = 0;
			for(int j = 0 ; j < i ; j++){
				if(letterString.charAt(j) < letterString.charAt(i)){	//核心判断
					k++;
				}
			}
			
			line += que[letterString.length()-i-1]*(letterString.charAt(i)-'a'-k);	//核心代码
		}
		System.out.println(line);
//		System.out.println(System.currentTimeMillis());
//		System.out.println(System.currentTimeMillis()-start);
//		start = System.currentTimeMillis();System.out.println(start);
		dfs(0);
//		System.out.println(System.currentTimeMillis());
//		System.out.println(System.currentTimeMillis()-start);
	}
	
	public static void dfs(int level){
		if(level == letterString.length()){
			
			String aString = String.valueOf(s);
//			System.out.println(aString);
//			for(char c:s){
//				System.out.print(c+"");
//			}
//			System.out.println();
			if(aString.equals(letterString)){
				System.out.println(count);
				return;
			}
			count++;
		}else{
			for(int i=0;i<letterString.length();i++)
				if(visited[i] == false){
					visited[i] = true;
					s[level] = (char) ('a'+i);
					dfs(level+1);
					visited[i] = false;
				}
		}
	}
}
```

## 6. 字符串比较

```
我们需要一个新的字符串比较函数compare(s1, s2).
对这个函数要求是：
1. 它返回一个整数，表示比较的结果。
2. 结果为正值，则前一个串大，为负值，后一个串大，否则，相同。
3. 结果的绝对值表示：在第几个字母处发现了两个串不等。

下面是代码实现。对题面的数据，结果为：
-3
2
5
```

```
package gq.test;

public class 字符串比较 {

	static int compare(String s1, String s2)
	{
		if(s1==null && s2==null) return 0;
		if(s1==null) return -1;
		if(s2==null) return 1;
		
		if(s1.isEmpty() && s2.isEmpty()) return 0;
		if(s1.isEmpty()) return -1;
		if(s2.isEmpty()) return 1;
		
		char x = s1.charAt(0);
		char y = s2.charAt(0);
		
		if(x<y) return -1;
		if(x>y) return 1;
		
		int t = compare(s1.substring(1),s2.substring(1));
		if(t==0) return 0;
		
		return t!=0?(t<0?--t:++t):0 ; //填空位置
	}

	public static void main(String[] args)
	{
		System.out.println(compare("abc", "abk"));
		System.out.println(compare("abc", "a"));
		System.out.println(compare("abcde", "abcda"));			
	}
}
```