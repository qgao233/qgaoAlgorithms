# 图

## **所有可能路径**
力扣第 797 题

输入的图是无环的，不需要visited数组辅助

```
// 记录所有路径  
List<List<Integer>> res = new LinkedList<>();  
  
public List<List<Integer>> allPathsSourceTarget(int[][] graph) {  
    LinkedList<Integer> path = new LinkedList<>();  
    traverse(graph, 0, path);  
    return res;  
}  
  
/* 图的遍历框架 */  
void traverse(int[][] graph, int s, LinkedList<Integer> path) {  
  
    // 添加节点 s 到路径  
    path.addLast(s);  
  
    int n = graph.length;  
    if (s == n - 1) {  
	// 到达终点  
	res.add(new LinkedList<>(path));  
	path.removeLast();  
	return;  
    }  
  
    // 递归每个相邻节点  
    for (int v : graph[s]) {  
	traverse(graph, v, path);  
    }  
  
    // 从路径移出节点 s  
    path.removeLast();  
}  
```
 
## **课程表（有向图中是否存在环）**
力扣第 207 题：判断是否能完成所有课程

- visited数组是用来剪枝的，在代码中是全局变量，并且只赋值true，而没有像onPath一样有回溯操作（即onPath【s】=false），举个例值，有a，b两个节点都有一条到c的路径，那么a判断之后被标记了visited【s】=true，b再去遍历的时候可以直接返回。
- onPath是记录回溯的路径的，是检查环是否存在的重要标志！

经过测试，只有OnPath没有visited，在100个节点的时候就会超时。

```
// 记录一次 traverse 递归经过的节点  
boolean[] onPath;  
// 记录遍历过的节点，防止走回头路  
boolean[] visited;  
// 记录图中是否有环  
boolean hasCycle = false;  
  
boolean canFinish(int numCourses, int[][] prerequisites) {  
    List<Integer>[] graph = buildGraph(numCourses, prerequisites);  
  
    visited = new boolean[numCourses];  
    onPath = new boolean[numCourses];  
  
    for (int i = 0; i < numCourses; i++) {  
	// 遍历图中的所有节点  
	traverse(graph, i);  
    }  
    // 只要没有循环依赖可以完成所有课程  
    return !hasCycle;  
}  
  
void traverse(List<Integer>[] graph, int s) {  
    if (onPath[s]) {  
	// 出现环  
	hasCycle = true;  
    }  
  
    if (visited[s] || hasCycle) {  
	// 如果之前已经遍历过了，或找到了环，也不用再遍历了  
	return;  
    }  
    // 前序遍历代码位置  
    visited[s] = true;  
    onPath[s] = true;  
    for (int t : graph[s]) {  
	traverse(graph, t);  
    }  
    // 后序遍历代码位置  
    onPath[s] = false;  
}  
List<Integer>[] buildGraph(int numCourses, int[][] prerequisites) {  
    // 图中共有 numCourses 个节点  
    List<Integer>[] graph = new LinkedList[numCourses];  
    for (int i = 0; i < numCourses; i++) {  
	graph[i] = new LinkedList<>();  
    }  
    for (int[] edge : prerequisites) {  
	int from = edge[1];  
	int to = edge[0];  
	// 修完课程 from 才能修课程 to  
	// 在图中添加一条从 from 指向 to 的有向边  
	graph[from].add(to);  
    }  
    return graph;  
}  
```

## **课程表 II（拓扑排序）**
力扣第 210 题：完成所有课程完排的学习顺序

**后序遍历的结果进行反转，就是拓扑排序的结果**

```
boolean[] visited;  
// 记录后序遍历结果，存的是to->from，因此要反转  
List<Integer> postorder = new ArrayList<>();  
  
int[] findOrder(int numCourses, int[][] prerequisites) {  
    // 先保证图中无环  
    if (!canFinish(numCourses, prerequisites)) {  
	return new int[]{};  
    }  
    // 建图  
    List<Integer>[] graph = buildGraph(numCourses, prerequisites);  
    // 进行 DFS 遍历  
    visited = new boolean[numCourses];  
    for (int i = 0; i < numCourses; i++) {  
	traverse(graph, i);  
    }  
    // 将后序遍历结果反转，转化成 int[] 类型  
    Collections.reverse(postorder);  
    int[] res = new int[numCourses];  
    for (int i = 0; i < numCourses; i++) {  
	res[i] = postorder.get(i);  
    }  
    return res;  
}  
  
void traverse(List<Integer>[] graph, int s) {  
    if (visited[s]) {  
	return;  
    }  
  
    visited[s] = true;  
    for (int t : graph[s]) {  
	traverse(graph, t);  
    }  
    // 后序遍历位置  
    postorder.add(s);  
}  
  
// 参考上一题的解法  
boolean canFinish(int numCourses, int[][] prerequisites);  
  
// 参考前文代码  
List<Integer>[] buildGraph(int numCourses, int[][] prerequisites);  
```

## **名流问题**
力扣第 277 题

所谓「名人」的定义：

1、所有其他人都认识名人。

2、名人不认识任何其他人。

```
int findCelebrity(int n) {  
    // 先假设 cand 是名人  
    int cand = 0;  
    // 循环一次，两两进行比较，淘汰不可能是名人的人  
    for (int other = 1; other < n; other++) {  
	if (!knows(other, cand) || knows(cand, other)) {  
	    // cand 不可能是名人，排除  
	    // 假设 other 是名人  
	    cand = other;  
	} else {  
	    // other 不可能是名人，排除  
	    // 什么都不用做，继续假设 cand 是名人  
	}  
    }  
  
    // 现在的 cand 是排除的最后结果，但不能保证一定是名人  
    for (int other = 0; other < n; other++) {  
	if (cand == other) continue;  
	// 需要保证其他人都认识 cand，且 cand 不认识任何其他人  
	if (!knows(other, cand) || knows(cand, other)) {  
	    return -1;  
	}  
    }  
  
    return cand;  
}  
```