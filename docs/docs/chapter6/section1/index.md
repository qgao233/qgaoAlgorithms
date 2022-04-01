# 算法入门练习

## 1. 十字型的徽标

```
package gq.ever;

import java.util.Scanner;

/**
 * 小明为某机构设计了一个十字型的徽标（并非红十字会啊），如下所示：

..$$$$$$$$$$$$$..
..$...........$..
$$$.$$$$$$$$$.$$$
$...$.......$...$
$.$$$.$$$$$.$$$.$
$.$...$...$...$.$
$.$.$$$.$.$$$.$.$
$.$.$...$...$.$.$
$.$.$.$$$$$.$.$.$
$.$.$...$...$.$.$
$.$.$$$.$.$$$.$.$
$.$...$...$...$.$
$.$$$.$$$$$.$$$.$
$...$.......$...$
$$$.$$$$$$$$$.$$$
..$...........$..
..$$$$$$$$$$$$$..
对方同时也需要在电脑dos窗口中以字符的形式输出该标志，并能任意控制层数。
 * @author Bermuda
 *
 */
public class CrossGraph {

	public static void main(String[] args) {
		char cross[][] = new char[125][125];
		Scanner scanner = new Scanner(System.in);
		int floor = scanner.nextInt();
		int surround = 9;
		for(int i = 0; i < floor-1; i++){
			if(floor > 1){
				surround += 4;
			}
		}
		for(int i = 0; i < surround/2 + 1; i++){
			for(int j = 0; j < surround/2 + 1; j++){
				if(i < 2){
					if(j < 2){
						cross[i][j] = '.';
					}
					if(j == 2){
						cross[i][j] = '$';
					}
					if(j > 2 && i == 0){
						cross[i][j] = '$';
					}
					if(j > 2 && i == 1){
						cross[i][j] = '.';
					}
				}else if(i == 2){
					if(j < 3){
						cross[i][j] = '$';
					}else if(j == 3){
						cross[i][j] = '.';
					}else{
						cross[i][j] = '$';
					}
				}else{
					if((i+1) % 2 == 0){
						if(j < i-3+1){//重复上一行的多少位
							cross[i][j] = cross[i-1][j];
						}else if(j >= i-3+1 && j < i+1){
							cross[i][j] = '.';
						}else if(j == i+1){
							cross[i][j] = '$';
						}else{
							cross[i][j] = '.';
						}
					}else{
						if(j < i-3+1){//重复上一行的多少位
							cross[i][j] = cross[i-1][j];
						}else if(j >= i-3+1 && j < i+1){
							cross[i][j] = '$';
						}else if(j == i+1){
							cross[i][j] = '.';
						}else{
							cross[i][j] = '$';
						}
					}
				}
			}
		}
		for(int i = 0; i < surround/2+1; i++){
			for(int j = 0; j < surround/2+1; j++){
				cross[i][surround-j-1] = cross[i][j];
			}
		}
		for(int i = 0; i < surround/2+1; i++){
			for(int j = 0; j < surround; j++){
				cross[surround-i-1][j] = cross[i][j];
			}
		}
		for(int i = 0; i < surround; i++){
			for(int j = 0; j < surround; j++){
				System.out.print(cross[i][j]);
			}
			System.out.println();
		}
	}

}
```

## 2. 分割矩阵

```
package gq.ever;

/**
 * 如下图所示，3 x 3 的格子中填写了一些整数。

+--*--+--+
|10* 1|52|
+--****--+
|20|30* 1|
*******--+
| 1| 2| 3|
+--+--+--+
我们沿着图中的星号线剪开，得到两个部分，每个部分的数字和都是60。

本题的要求就是请你编程判定：对给定的m x n 的格子中的整数，是否可以分割为两个部分，使得这两个区域的数字和相等。

如果存在多种解答，请输出包含左上角格子的那个区域包含的格子的最小数目。

如果无法分割，则输出 0。
 * @author Bermuda
 *
 */
public class CutGrid {

	public static void main(String[] args) {
		
	}

}
```

## 3. 代分数表示

```
package gq.ever;

import java.util.Scanner;

/**
 * 100 可以表示为带分数的形式：100 = 3 + 69258 / 714。
 * 还可以表示为：100 = 82 + 3546 / 197。
 * 注意特征：带分数中，数字1~9分别出现且只出现一次（不包含0）。
 * 类似这样的带分数，100 有 11 种表示法。
 * @author Bermuda
 *
 */
public class MixedNumber {

	public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);
		int goal = scanner.nextInt();
		int count = 0;
		for(int i = 1; i < goal; i++){ //i:preNum
			String preChar = String.valueOf(i);
			int preLength = preChar.length();
			int flag = 1;
			for(int j = 0; j < preLength; j++){		//判断分数前的整数是否有重复
				for(int k = j+1; k < preLength; k++){
					if(preChar.charAt(j) == preChar.charAt(k)){
						flag = 0;
						break;
					}
				}
				if(flag == 0){
					break;
				}
			}
			if(flag == 0){
				continue;
			}
			int leftNum = goal - i;
			StringBuffer allBuffer;// = new StringBuffer();
			int numerator;		//分子
			for(int j = 2;;j++){// j:denominator 分母
				numerator = leftNum * j;
				int leftLength = String.valueOf(numerator).length()+String.valueOf(j).length();
				if(leftLength == 9-preChar.length()){
					allBuffer = new StringBuffer();
					allBuffer.append(preChar);
					allBuffer.append(String.valueOf(numerator));
					allBuffer.append(String.valueOf(j));
//					if(i<10)
//					System.out.println(allBuffer.toString()+"#");
					for(int m = 0; m < allBuffer.length(); m++){
						for(int n = m+1; n < allBuffer.length(); n++){
							//判断字符串中是否有重复，且是否含 0
							if(allBuffer.toString().charAt(m) == allBuffer.toString().charAt(n) || allBuffer.charAt(n) == '0'){
								flag = 0;//System.out.println(m+""+n+"break");
								break;
							}
						}
						if(flag == 0){
							break;
						}
						if(m == allBuffer.length()-1){
							count++;//System.out.println(allBuffer.toString()+"????");
						}
					}
					if(flag == 0){
						flag = 1;
						continue;
					}
				}else if(leftLength > 9-preChar.length()){
					break;
				}else{
					continue;
				}
			}
		}
		System.out.println(count);
	}

}
```

## 4. 三羊生瑞气

```
package gq.ever;

/**
 * 三羊生瑞气：祥瑞生辉+三羊献瑞=三羊生瑞气
 * @author Bermuda
 *
 */
public class ThreeSheep {

	public static void main(String[] args) {
		for(int a = 1; a < 10; a++){							//三
			for(int b = 0; b < 10; b++){						//羊
				if(b != a)
				for(int c = 0; c < 10; c++){					//生
					if(c != a && c != b)
					for(int d = 0; d < 10; d++){				//瑞
						if(d != a && d != b && d != c)
						for(int e = 0; e < 10; e++){			//气
							if(e != a && e != b && e != c && e != d)
							for(int f = 0; f < 10; f++){		//献
								if(f != a && f != b && f != c && f != d && f != e)
								for(int g = 1; g < 10; g++){	//祥
									if(g != a && g != b && g != c && g != d && g != e && g != f)
									for(int h = 0; h < 10; h++){//辉
										if(h != a && h != b && h != c && h != d && h != e && h != f && h != g)
											if((g*1000+d*100+c*10+h + a*1000+b*100+f*10+d) == (a*10000+b*1000+c*100+d*10+e)){
												System.out.println((g*1000+d*100+c*10+h)+"+"+(a*1000+b*100+f*10+d)+"="+(a*10000+b*1000+c*100+d*10+e));
											}
									}
								}
							}
						}
					}
				}
			}
		}
	}

}
```

## 5. 最小公倍数

```
package gq.ever;

import java.util.Scanner;

/**
 * 最小公倍数
 * 求最小公倍数算法：

最小公倍数=两整数的乘积÷最大公约数
 * @author Bermuda
 *
 */
public class LCM {

	public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);
		int[] a = new int[3];
		int i = 0;
		while(i < a.length){
			a[i] = scanner.nextInt();
			i++;
		}
		for(int j = 0; j<a.length;j++){
			for(int k = 0;k<a.length - j-1;k++){
				if(a[k] > a[k+1]){
					int tmp = a[k];
					a[k] = a[k+1];
					a[k+1] = tmp;
				}
			}
		}
		int tmp = a[2];
		while(true){
			if(a[2] % a[0] == 0 && a[2] % a[1] == 0){
				System.out.println(a[2]);
				break;
			}else{
				a[2] += tmp;
			}
		}
	}
}
```