# 回溯算法

## 1. 2n皇后

```
问题描述
　　给定一个n*n的棋盘，棋盘中有一些位置不能放皇后。现在要向棋盘中放入n个黑皇后和n个白皇后，
使任意的两个黑皇后都不在同一行、同一列或同一条对角线上，任意的两个白皇后都不在同一行、同一列或同一条对角线上。
问总共有多少种放法？n小于等于8。
输入格式
　　输入的第一行为一个整数n，表示棋盘的大小。
　　接下来n行，每行n个0或1的整数，如果一个整数为1，表示对应的位置可以放皇后，如果一个整数为0，表示对应的位置不可以放皇后。
输出格式
　　输出一个整数，表示总共有多少种放法。
样例输入
4
1 1 1 1
1 1 1 1
1 1 1 1
1 1 1 1
样例输出
2
样例输入
4
1 0 1 1
1 1 1 1
1 1 1 1
1 1 1 1
样例输出
0
```

```
package 回溯算法;

import java.util.*;

/**
 * 单皇后解法
 * 
 * @author Bermuda
 *
 */
class SingleQueen implements Runnable {

	private static int n = 4;
	private int[] array;

	public SingleQueen() {
		array = new int[n];
	}

	public SingleQueen(int[] array) {
		SingleQueen.n = array.length;
		this.array = new int[n];
	}

	@Override
	public void run() {
		queen(array);
	}

	public void queen(int[] array) {
		Arrays.fill(array, -1);
		int row = 0;
		int count = 0;
		while (row >= 0 && row < n) {
			array[row]++;
			while (array[row] < n && check(row, array)) {
				array[row]++;
			}
			if (array[row] < n && row == n - 1) {
				count++;
				for (int i = 0; i < n; i++) {
					System.out.println((i + 1) + "行：" + (array[i] + 1) + "列");
				}
				System.out.println("===============");
				if (array[0] < n - 1) {
					row = 0;
					for (int i = 1; i < n; i++) {
						array[i] = -1;
					}
					continue;
				}
				break;
			}
			if (array[row] < n && row < n - 1) {
				row += 1;
			} else {
				array[row--] = -1;
			}
		}
		System.out.println("单皇后一共有" + count + "种解！");
	}

	public boolean check(int row, int[] array) {
		for (int perRow = 0; perRow < row; perRow++) {
			if (array[perRow] == array[row] || Math.abs(perRow - row) == Math.abs(array[perRow] - array[row])) {
				return true;
			}
		}
		return false;
	}

}

class DoubleQueen implements Runnable {

	private static int n = 4;
	private int count = 0;
	private int[][] chessBoard;
	private int[] whiteQueen;
	private int[] blackQueen;

	public DoubleQueen() {
		chessBoard = new int[n][n];
		for (int i = 0; i < n; i++) {
			Arrays.fill(chessBoard[i], 1);
		}
		whiteQueen = new int[n];
		blackQueen = new int[n];
	}

	public DoubleQueen(int n, int[][] chessBoard) {
		DoubleQueen.n = n;
		this.chessBoard = chessBoard;
		this.whiteQueen = new int[n];
		this.blackQueen = new int[n];
	}

	@Override
	public void run() {
		whiteQueen(whiteQueen);
	}

	public void whiteQueen(int[] array) {
		Arrays.fill(array, -1);
		int row = 0;
		while (row >= 0 && row < n && array[row] < n) {
			array[row]++;
			while (array[row] < n && check(row, array)) {
				array[row]++;
				if (array[row] >= n) {
					break;
				} else if (chessBoard[row][array[row]] == 0) {
					array[row]++;
				}
			}
			if (array[row] >= n) {
			} else if (chessBoard[row][array[row]] == 0) {
				continue;
			}
			if (array[row] < n && row == n - 1) {
				if (!blackQueen()) {
					break;
				}
				for (int i = 0; i < n; i++) {
					System.out.println((i + 1) + "行：" + (array[i] + 1) + "列");
				}

				System.out.println("*********************以上是白");
				if (array[0] < n - 1) {
					row = 0;
					for (int i = 1; i < n; i++) {
						array[i] = -1;
					}
					continue;
				}
				break;
			}
			if (array[row] < n && row < n - 1) {
				row += 1;
			} else {
				array[row--] = -1;
			}
		}
		System.out.println("2n皇后一共有" + count + "种解！");
		return;
	}

	public boolean blackQueen() {
		int[] array = blackQueen;
		Arrays.fill(array, -1);
		int row = 0;
		while (row >= 0 && row < n) {
			array[row]++;
			while (array[row] < n && check(row, array) || array[row] == whiteQueen[row]) {
				array[row]++;
				if (array[row] >= n) {
					break;
				} else if (chessBoard[row][array[row]] == 0) {
					array[row]++;
				}
			}
			if (array[row] >= n) {
			} else if (chessBoard[row][array[row]] == 0) {
				continue;
			}
			if (array[row] < n && row == n - 1) {
				count++;
				for (int i = 0; i < n; i++) {
					System.out.println((i + 1) + "行：" + (array[i] + 1) + "列");
				}
				System.out.println("===============以上是黑");
				if (array[0] < n - 1) {
					row = 0;
					for (int i = 1; i < n; i++) {
						array[i] = -1;
					}
					continue;
				}
				break;
			}
			if (array[row] < n && row < n - 1) {
				row += 1;
			} else {
				array[row--] = -1;
			}
		}
		return count > 0 ? true : false;
	}

	public boolean check(int row, int[] array) {
		for (int perRow = 0; perRow < row; perRow++) {
			if (array[perRow] == array[row] || Math.abs(perRow - row) == Math.abs(array[perRow] - array[row])) {
				return true;
			}
		}
		return false;
	}

}

public class Step1 {

	private static int n = 0;

	public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);
		n = scanner.nextInt();
		scanner.nextLine();
		int[][] chessBoard = new int[n][n];
		for (int i = 0; i < n; i++) {
			for (int j = 0; j < n; j++) {
				chessBoard[i][j] = scanner.nextInt();
			}
		}
		// scanner.close();
		// int[] array = new int[n];
		// SingleQueen singleQueen = new SingleQueen();
		// new Thread(singleQueen).start();
		DoubleQueen doubleQueen = new DoubleQueen(n, chessBoard);
		new Thread(doubleQueen).start();
	}

}
```