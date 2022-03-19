# 递归/回溯

## 55. 没有重复项数字的全排列

```
import java.util.*;

public class Solution {
    
    private ArrayList<ArrayList<Integer>> resList;
    private boolean[] isVisited;
    
    public ArrayList<ArrayList<Integer>> permute(int[] num) {
        resList = new ArrayList<>();
        isVisited = new boolean[num.length];
        traverse(num,new ArrayList<>());
        return resList;
    }
    
    private void traverse(int[] num, ArrayList<Integer> res){
        if(res.size() == num.length){
            resList.add(new ArrayList<Integer>(res));
            return;
        }
        
        for(int i = 0; i < num.length; i++){
            if(isVisited[i]) continue;
            
            isVisited[i] = true;
            res.add(num[i]);
            traverse(num,res);
            isVisited[i] = false;
            res.remove(res.size()-1);    //删除最后一个
        }
    }
}
```

## 56. 有重复项数字的全排列

```
import java.util.*;

public class Solution {
    
    private ArrayList<ArrayList<Integer>> resList;
    private boolean[] isVisited;
    
    public ArrayList<ArrayList<Integer>> permuteUnique(int[] num) {
        resList = new ArrayList<>();
        isVisited = new boolean[num.length];
        
        Arrays.sort(num);   //新增
        traverse(num,new ArrayList<>());
        return resList;
    }
    
    private void traverse(int[] num, ArrayList<Integer> res){
        if(res.size() == num.length){
            resList.add(new ArrayList<Integer>(res));
            return;
        }
        
        for(int i = 0; i < num.length; i++){
            if(isVisited[i]) continue;
            //新增
            if(i>0 && !isVisited[i] && !isVisited[i-1] && num[i] == num[i-1]) continue; 
            isVisited[i] = true;
            res.add(num[i]);
            traverse(num,res);
            isVisited[i] = false;
            res.remove(res.size()-1);    //删除最后一个
        }
    }
}
```

## 57. 岛屿数量

```
import java.util.*;


public class Solution {
    /**
     * 判断岛屿数量
     * @param grid char字符型二维数组 
     * @return int整型
     */
    //上下左右
    private int[][] direction = new int[][]{{-1,0},{1,0},{0,-1},{0,1}};
    
    public int solve (char[][] grid) {
        // write code here
        int res = 0;

        for(int i = 0; i < grid.length; i++){
            for(int j = 0; j < grid[0].length; j++){
                if(grid[i][j] == '1'){
                    res++;
                    dfs(grid,i,j);
                }
            }
        }
        return res;
    }
    
    public void dfs(char[][] grid, int i, int j){
        if(i < 0 || j < 0 || i >= grid.length || j >= grid[0].length) return;
        if(grid[i][j] == '0') return;

        grid[i][j] = '0';
        for(int[] x : direction){
            int ni = i + x[0];
            int nj = j + x[1];
            dfs(grid, ni, nj);
        }
    }
}
```

## 58. 字符串的排列

```
import java.util.*;
public class Solution {
    
    private ArrayList<String> resList;
    private boolean[] isVisited;
    
    public ArrayList<String> Permutation(String str) {
        char[] charArray = str.toCharArray();
        Arrays.sort(charArray);
        resList = new ArrayList<>();
        isVisited = new boolean[charArray.length];
        traverse(charArray,new StringBuilder());
        return resList;
    }
    
    private void traverse(char[] charArray, StringBuilder sb){
        if(sb.length() == charArray.length){
            resList.add(sb.toString());
            return;
        }
        
        for(int i = 0; i < charArray.length; i++){
            if(isVisited[i]) continue;
            if(i>0 && !isVisited[i] && !isVisited[i-1] && charArray[i] == charArray[i-1]) continue; 
            isVisited[i] = true;
            sb.append(charArray[i]);
            traverse(charArray,sb);
            isVisited[i] = false;
            sb.deleteCharAt(sb.length() - 1);    //删除最后一个
        }
    }
}
```

## 59. N皇后问题

```
import java.util.*;


public class Solution {
    /**
     * 
     * @param n int整型 the n
     * @return int整型
     */
    private int res = 0;
    
    public int Nqueen (int n) {
        // write code here
        int[][] queen = new int[n][n];
        for(int i = 0; i < n; i++) Arrays.fill(queen[i],0);
        traverse(queen,0);
        return res;
    }
    
    private void traverse(int[][] queen, int row){
        if(row == queen.length){
            res++;
            return;
        }
        int n = queen[row].length;
        for(int i = 0; i < n; i++){
            if(!isValid(queen,row,i)) continue;
            queen[row][i] = 1;
            traverse(queen,row+1);
            queen[row][i] = 0;
        }
    }
    
    private boolean isValid(int[][] queen, int row, int col){
        //检查列
        int totalRow = queen.length;
        for(int i = 0; i < totalRow; i++){
            if(queen[i][col] == 1) return false;
        }
        //检查左上方
        for(int i = row - 1, j = col - 1;
           i >= 0 && j >= 0; i--, j--){
            if(queen[i][j] == 1) return false;
        }
        //检查右上方
        int totalCol = queen[row].length;
        for(int i = row - 1, j = col + 1;
           i >= 0 && j < totalCol; i--, j++){
            if(queen[i][j] == 1) return false;
        }
        return true;
    }
}
```

## 60. 括号生成

```
import java.util.*;


public class Solution {
    /**
     * 
     * @param n int整型 
     * @return string字符串ArrayList
     */
    //使用括号时，总是先左括号，后右括号，
    //也就是说使用过程中，左括号数量要大于右括号的使用数量
    //反过来说就是未使用的左括号数量要小于右括号的未使用数量
    //如果未使用的左括号的数量大于了右括号未使用数量，那就赶紧用左括号
    //否则使用右括号
    private ArrayList<String> res = new ArrayList<>();
    
    public ArrayList<String> generateParenthesis (int n) {
        // write code here
        traverse(new StringBuilder(),n,n);
        return res;
    }
    
    private void traverse(StringBuilder sb, int left, int right){
        if (left > right) return;
        if (left < 0 || right < 0) return;
        if(left == 0 && right == 0) res.add(sb.toString());
        
        sb.append("(");
        traverse(sb,left-1, right);
        sb.deleteCharAt(sb.length()-1);
        sb.append(")");
        traverse(sb,left, right-1);
        sb.deleteCharAt(sb.length()-1);
    }
}
```

## 61. 矩阵最长递增路径

```
import java.util.*;


public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     * 递增路径的最大长度
     * @param matrix int整型二维数组 描述矩阵的每个数
     * @return int整型
     */
    private int[][] direction = new int[][]{{-1,0},{1,0},{0,-1},{0,1}};
    public int maxLen = 0;
    private boolean[][] isVisited;
    
    public int solve (int[][] matrix) {
        // write code here
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return 0;
        }
        
        int rows = matrix.length;
        int cols = matrix[0].length;
        isVisited = new boolean[rows][cols];
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                traverse(matrix, i, j, -1, 0);
            }
        }
        return maxLen;
    }
    
    private void traverse(int[][] matrix, int i, int j, int lastVal, int step){
        if(i < 0 || i >= matrix.length
          || j < 0 || j >= matrix[0].length
          || isVisited[i][j] == true
          || lastVal >= matrix[i][j]) return;
        
        maxLen = Math.max(maxLen, ++step);
        isVisited[i][j] = true;
        for(int[] x : direction){
            int ni = i + x[0];
            int nj = j + x[1];
            traverse(matrix, ni, nj, matrix[i][j], step);
        }
        isVisited[i][j] = false;
    }
}
```