# 数学

## 1. n的阶乘的数字有多少个0

```
public long countZeros(long n){
    long count = 0;
    while(n != 0){
        n = n / 5;
        count += n;
    }
    return count;
}
```

## 2. 通过位运算将两个数相加

```
public int add(int num1,int num2){
    int temp1 = 0,temp2 = 0;
    while(num2 != 0){
        temp1 = num1 ^ num2;
        temp2 = (num1 & num2) << 1;

        num1 = temp1;
        num2 = temp2;
    }
    return num1;
}
```

## 3. dijkstra算法

Graph.java

```
package com.bermuda.algorithms.dijkstra;

import java.util.ArrayList;
import java.util.List;
import java.util.PriorityQueue;
import java.util.Queue;

public class Graph {

    /*
     * 顶点
     */
    private List<Vertex> vertexs;

    /*
     * 边
     */
    private int[][] edges;

    /*
     * 没有访问的顶点
     */
    private Queue<Vertex> unVisited;

    public Graph(List<Vertex> vertexs, int[][] edges) {
        this.vertexs = vertexs;
        this.edges = edges;
        initUnVisited();
    }

    /*
     * 搜索各顶点最短路径
     */
    public void search(){
        while(!unVisited.isEmpty()){
            Vertex vertex = unVisited.element();
            //顶点已经计算出最短路径，设置为"已访问"
            vertex.setMarked(true);
            //获取所有"未访问"的邻居
            List<Vertex> neighbors = getNeighbors(vertex);
            //更新邻居的最短路径
            updatesDistance(vertex, neighbors);
            pop();
        }
        System.out.println("search over");
    }

    /*
     * 更新所有邻居的最短路径
     */
    private void updatesDistance(Vertex vertex, List<Vertex> neighbors){

        for(Vertex neighbor: neighbors){
            updateDistance(vertex, neighbor);
        }
    }

    /*
     * 更新邻居的最短路径
     */
    private void updateDistance(Vertex vertex, Vertex neighbor){
        int distance = getDistance(vertex, neighbor) + vertex.getPath();
        if(distance < neighbor.getPath()){
            neighbor.setPath(distance);
        }
    }

    /*
     * 初始化未访问顶点集合
     */
    private void initUnVisited() {
        unVisited = new PriorityQueue<Vertex>();
        for (Vertex v : vertexs) {
            unVisited.add(v);
        }
    }

    /*
     * 从未访问顶点集合中删除已找到最短路径的节点
     */
    private void pop() {
        unVisited.poll();
    }

    /*
     * 获取顶点到目标顶点的距离
     */
    private int getDistance(Vertex source, Vertex destination) {
        int sourceIndex = vertexs.indexOf(source);
        int destIndex = vertexs.indexOf(destination);
        return edges[sourceIndex][destIndex];
    }

    /*
     * 获取顶点所有(未访问的)邻居
     */
    private List<Vertex> getNeighbors(Vertex v) {
        List<Vertex> neighbors = new ArrayList<Vertex>();
        int position = vertexs.indexOf(v);
        Vertex neighbor = null;
        int distance;
        for (int i = 0; i < vertexs.size(); i++) {
            if (i == position) {
                //顶点本身，跳过
                continue;
            }
            distance = edges[position][i];    //到所有顶点的距离
            if (distance < Integer.MAX_VALUE) {
                //是邻居(有路径可达)
                neighbor = getVertex(i);
                if (!neighbor.isMarked()) {
                    //如果邻居没有访问过，则加入list;
                    neighbors.add(neighbor);
                }
            }
        }
        return neighbors;
    }

    /*
     * 根据顶点位置获取顶点
     */
    private Vertex getVertex(int index) {
        return vertexs.get(index);
    }

    /*
     * 打印图
     */
    public void printGraph() {
        int verNums = vertexs.size();
        for (int row = 0; row < verNums; row++) {
            for (int col = 0; col < verNums; col++) {
                if(Integer.MAX_VALUE == edges[row][col]){
                    System.out.print("X");
                    System.out.print(" ");
                    continue;
                }
                System.out.print(edges[row][col]);
                System.out.print(" ");
            }
            System.out.println();
        }
    }
}
```

Vertex.java

```
package com.bermuda.algorithms.dijkstra;

public class Vertex implements Comparable<Vertex>{

    /**
     * 节点名称(A,B,C,D)
     */
    private String name;

    /**
     * 最短路径长度
     */
    private int path;

    /**
     * 节点是否已经出列(是否已经处理完毕)
     */
    private boolean isMarked;

    public Vertex(String name){
        this.name = name;
        this.path = Integer.MAX_VALUE; //初始设置为无穷大
        this.setMarked(false);
    }

    public Vertex(String name, int path){
        this.name = name;
        this.path = path;
        this.setMarked(false);
    }

    @Override
    public int compareTo(Vertex o) {
        return o.path > path?-1:1;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getPath() {
        return path;
    }

    public void setPath(int path) {
        this.path = path;
    }

    public void setMarked(boolean marked) {
        isMarked = marked;
    }

    public boolean isMarked(){
        return isMarked;
    }
}
```

## 4. prim算法

```
package com.bermuda.algorithms.prim;

import java.util.ArrayList;
import java.util.List;

public class Prim {
    static int MAX = Integer.MAX_VALUE;

    public static void main(String[] args) {
        int[][] map = new int[][] {
                { 0, 10, MAX, MAX, MAX, 11, MAX, MAX, MAX },
                { 10, 0, 18, MAX, MAX, MAX, 16, MAX, 12 },
                { MAX, MAX, 0, 22, MAX, MAX, MAX, MAX, 8 },
                { MAX, MAX, 22, 0, 20, MAX, MAX, 16, 21 },
                { MAX, MAX, MAX, 20, 0, 26, MAX, 7, MAX },
                { 11, MAX, MAX, MAX, 26, 0, 17, MAX, MAX },
                { MAX, 16, MAX, MAX, MAX, 17, 0, 19, MAX },
                { MAX, MAX, MAX, 16, 7, MAX, 19, 0, MAX },
                { MAX, 12, 8, 21, MAX, MAX, MAX, MAX, 0 } };
        prim(map, map.length);
    }
    public static void prim(int[][] graph, int n){

        char[] c = new char[]{'A','B','C','D','E','F','G','E','F'};
        int[] lowcost = new int[n];  //到新集合的最小权
        int[] mid= new int[n];//存取前驱结点
        List<Character> list=new ArrayList<Character>();//用来存储加入结点的顺序
        int i, j, min, minid , sum = 0;
        //初始化辅助数组
        for(i=1;i<n;i++)
        {
            lowcost[i]=graph[0][i];
            mid[i]=0;
        }
        list.add(c[0]);
        //一共需要加入n-1个点
        for(i=1;i<n;i++)
        {
            min=MAX;
            minid=0;
            //每次找到距离集合最近的点
            for(j=1;j<n;j++)
            {
                if(lowcost[j]!=0&&lowcost[j]<min)
                {
                    min=lowcost[j];
                    minid=j;
                }
            }

            if(minid==0) return;
            list.add(c[minid]);
            lowcost[minid]=0;
            sum+=min;
            System.out.println(c[mid[minid]] + "到" + c[minid] + " 权值：" + min);
            //加入该点后，更新其它点到集合的距离
            for(j=1;j<n;j++)
            {
                if(lowcost[j]!=0&&lowcost[j]>graph[minid][j])
                {
                    lowcost[j]=graph[minid][j];
                    mid[j]=minid;
                }
            }
        }
        System.out.println("sum:" + sum);
        list.forEach(s-> System.out.println(s.toString()));

    }

}
```

## 5. 多矩阵相乘

动态规划

```
public void martixChainMultipy(int[] p,int[][] m,int[][] s){
    martixChain(p,m,s);
    traceback(s,0,p.length-1);
}

private void martixChain(int[] p,int[][] m,int[][] s){
    int n = p.length - 1;
    for(int i = 1;i <= n;i++) m[i][i] = 0;
    for(int r = 2;r <= n;r++){
        for(int i = 1;i <= n-r+1;i++){
            int j = i+r-1;
            m[i][j] = m[i+1][j] + p[i-1]*p[i]*p[j];
            s[i][j] = i;
            for(int k = i+1;k < j;k++){
                int t = m[i][k] + m[k+1][j] + p[i-1]*p[k]*p[j];
                if(t<m[i][j]){
                    m[i][j] = t;
                    s[i][j] = k;
                }
            }
        }
    }
}

private void traceback(int[][] s,int i,int j){
    if(i == j) return;
    traceback(s,i,s[i][j]);
    traceback(s,s[i][j]+1,j);
    System.out.println("Multiply A"+i+","+s[i][j]+"and A"+(s[i][j]+1)+","+j);
}
```

## 6. 两数之和

```
/**
    * Given nums = [2, 7, 11, 15], target = 9,

    Because nums[0] + nums[1] = 2 + 7 = 9,
    return [0, 1].
    * @param nums
    * @param target
    * @return
    */
public int[] twoSum(int[] nums,int target){
    Map<Integer,Integer> numsMap = new HashMap<Integer, Integer>();
    for(int step = 0;step < nums.length;step++){
        if(numsMap.containsKey(target - nums[step])){
            return new int[]{numsMap.get(target - nums[step]),step};
        }
        numsMap.put(nums[step],step);
    }
    return new int[]{0};
}
```

## 7. 找出两个排序数组中的中位数

```
/**
    * Example 1:
    nums1 = [1, 3]
    nums2 = [2]

    The median is 2.0

    Example 2:
    nums1 = [1, 2]
    nums2 = [3, 4]

    The median is (2 + 3)/2 = 2.5
    * @param nums1
    * @param nums2
    * @return
    */
public double findMedianSortedArrays(int[] nums1, int[] nums2) {
    if(nums1 == null || nums2 == null){
        return -1;
    }
    if(nums1.length == 0){
        return findMedianSortedArray(nums2);
    }
    if(nums2.length == 0){
        return findMedianSortedArray(nums1);
    }

    int m = nums1.length,n = nums2.length;
    int[] tempNums1 = nums1,tempNums2 = nums2;
    //保证tempNums1是最短的
    if(m > n){
        tempNums1 = nums2;
        tempNums2 = nums1;
        m = tempNums1.length;
        n = tempNums2.length;
    }

    int l1 = 0,l2 = 0,r1 = 0,r2 = 0,c1,c2,low = 0,high = 2*m;//2m+1
    while(low <= high){
        c1 = (low + high) / 2;
        c2 = m + n - c1;//(2m+2n)/2 - c1

        l1 = (c1 == 0)?Integer.MIN_VALUE:tempNums1[(c1 - 1)/2];
        r1 = (c1 == 2*m)?Integer.MAX_VALUE:tempNums1[c1/2];
        l2 = (c2 == 0)?Integer.MIN_VALUE:tempNums2[(c2 - 1)/2];
        r2 = (c2 == 2*n)?Integer.MAX_VALUE:tempNums2[c2/2];

        if(l1 > r2){
            high = c1 - 1;
        }else if(l2 > r1){
            low = c1 + 1;
        }else{
            break;
        }
    }

    return (Math.max(l1,l2) + Math.min(r1,r2))/2.0;


}

private double findMedianSortedArray(int[] nums){
    if(nums == null || nums.length <= 0){
        return -1;
    }
    return (nums[nums.length/2]+nums[(nums.length-1)/2])/2.0;
}
```