# 二叉树

## 23. 二叉树的前序遍历

```
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 *   public TreeNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param root TreeNode类 
     * @return int整型一维数组
     */
    public int[] preorderTraversal (TreeNode root) {
        // write code here
        List<Integer> res = new ArrayList<>();
        preOrder(root,res);
        int[] resArray = new int[res.size()];
        int j = 0;
        for(Integer i : res) resArray[j++] = i;
        return resArray;
        
    }
    
    private void preOrder(TreeNode root, List<Integer> res){
        if(root == null) return;
        
        res.add(root.val);
        preOrder(root.left,res);
        preOrder(root.right,res);
    }
}
```

## 24. 二叉树的中序遍历

```
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 *   public TreeNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param root TreeNode类 
     * @return int整型一维数组
     */
    public int[] inorderTraversal (TreeNode root) {
        // write code here
        List<Integer> res = new ArrayList<>();
        preOrder(root,res);
        int[] resArray = new int[res.size()];
        int j = 0;
        for(Integer i : res) resArray[j++] = i;
        return resArray;
        
    }
    
    private void preOrder(TreeNode root, List<Integer> res){
        if(root == null) return;
        
        
        preOrder(root.left,res);
        res.add(root.val);
        preOrder(root.right,res);
    }
}
```

## 25. 二叉树的后序遍历

```
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 *   public TreeNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param root TreeNode类 
     * @return int整型一维数组
     */
    public int[] postorderTraversal (TreeNode root) {
        List<Integer> res = new ArrayList<>();
        preOrder(root,res);
        int[] resArray = new int[res.size()];
        int j = 0;
        for(Integer i : res) resArray[j++] = i;
        return resArray;
        
    }
    
    private void preOrder(TreeNode root, List<Integer> res){
        if(root == null) return;
        
        
        preOrder(root.left,res);
        preOrder(root.right,res);
        res.add(root.val);
        
    }
}
```

## 26. 求二叉树的层序遍历

```
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 * }
 */

public class Solution {
    /**
     * 
     * @param root TreeNode类 
     * @return int整型ArrayList<ArrayList<>>
     */
    public ArrayList<ArrayList<Integer>> levelOrder (TreeNode root) {
        // write code here
        if(root == null) return new ArrayList<>(0);
        ArrayList<ArrayList<Integer>> res = new ArrayList<>();
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            ArrayList<Integer> levelList = new ArrayList<>();
            int len = q.size();
            for(int i = 0; i < len; i++){
                TreeNode node = q.poll();
                levelList.add(node.val);
                if(node.left != null){
                    q.offer(node.left);
                }
                if(node.right != null){
                    q.offer(node.right);
                }
            }
            res.add(levelList);
        }
        return res;
    }
}
```

## 27. 按之字形顺序打印二叉树

```
import java.util.*;
/*
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
public class Solution {
    public ArrayList<ArrayList<Integer> > Print(TreeNode root) {
        if(root == null) return new ArrayList<>(0);
        ArrayList<ArrayList<Integer>> res = new ArrayList<>();
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        boolean odd = true;
        while(!q.isEmpty()){
            ArrayList<Integer> levelList = new ArrayList<>();
            int len = q.size();
            Deque<TreeNode> nextQueue = new LinkedList<>();
            for(int i = 0; i < len; i++){
                TreeNode node = q.poll();
                levelList.add(node.val);
                if(odd){
                    if(node.left != null){
                        nextQueue.addFirst(node.left);
                    }
                    if(node.right != null){
                        nextQueue.addFirst(node.right);
                    }
                }else{
                    if(node.right != null){
                        nextQueue.addFirst(node.right);
                    }
                    if(node.left != null){
                        nextQueue.addFirst(node.left);
                    }
                    
                }
                
            }
            res.add(levelList);
            odd = !odd;
            q.addAll(nextQueue);
        }
        return res;
    }

}
```

## 28. 二叉树的最大深度

```
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 * }
 */

public class Solution {
    /**
     * 
     * @param root TreeNode类 
     * @return int整型
     */
    public int maxDepth (TreeNode root) {
        // write code here
        if(root == null) return 0;
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        int level = 0;
        while(!q.isEmpty()){
            int len = q.size();
            for(int i = 0; i < len; i++){
                TreeNode node = q.poll();
                if(node.left != null){
                    q.offer(node.left);
                }
                if(node.right != null){
                    q.offer(node.right);
                }
            }
            level++;
        }
        return level;
    }
}
```

## 29. 二叉树中和为某一值的路径(一)

```
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 * }
 */

public class Solution {
    /**
     * 
     * @param root TreeNode类 
     * @param sum int整型 
     * @return bool布尔型
     */
    
    public boolean hasPathSum (TreeNode root, int sum) {
        // write code here
        if(root == null) return false;
        
        sum -= root.val;
        if(0 == sum && root.left == null && root.right == null) return true;
        return hasPathSum(root.left,sum) || hasPathSum(root.right,sum);
    }
}
```

## 30. 二叉搜索树与双向链表

```
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
public class Solution {
    
    TreeNode head = null;
    TreeNode pre =null;
    
    public TreeNode Convert(TreeNode pRootOfTree) {
        process(pRootOfTree);
        
        return head;
    }
    
    public void process(TreeNode root) {
        if (root == null) {
            return;
        }
        process(root.left);
        if(head==null)head=root;
        else{
            root.left=pre;
            pre.right=root;
        }
        pre=root;
        process(root.right);
    }
}
```

## 31. 对称的二叉树

```
/*
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
public class Solution {
    boolean isSymmetrical(TreeNode pRoot) {
        if(pRoot==null)return true;
        return calcRoot(pRoot.left,pRoot.right);
    }
    boolean calcRoot(TreeNode left, TreeNode right){
        if(left==null && right==null) return true;
        if(left==null && right != null || left!=null && right==null) return false;
        if(left.val != right.val) return false;
        return calcRoot(left.right,right.left) && calcRoot(left.left,right.right);
    }
}
```

## 32. 合并二叉树

```
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 * }
 */

public class Solution {
    /**
     * 
     * @param t1 TreeNode类 
     * @param t2 TreeNode类 
     * @return TreeNode类
     */
    public TreeNode mergeTrees (TreeNode t1, TreeNode t2) {
        if(t1==null) return t2;
        if(t2==null) return t1;
        TreeNode root=new TreeNode(t1.val+t2.val);
        root.left=mergeTrees(t1.left,t2.left);
        root.right=mergeTrees(t1.right,t2.right);
        return root;
    }
    
}
```

## 33. 二叉树的镜像

```
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 *   public TreeNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param pRoot TreeNode类 
     * @return TreeNode类
     */
    public TreeNode Mirror (TreeNode pRoot) {
        // write code here
        if(pRoot == null) return null;
        TreeNode tmp = pRoot.left;
        pRoot.left = pRoot.right;
        pRoot.right = tmp;
        Mirror(pRoot.right);
        Mirror(pRoot.left);
        return pRoot;
        
    }
}
```

## 34. 判断是不是二叉搜索树

```
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 *   public TreeNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param root TreeNode类 
     * @return bool布尔型
     */
    int min = Integer.MIN_VALUE;
    public boolean isValidBST (TreeNode root) {
        // write code here
        return dfs(root,Integer.MIN_VALUE,Integer.MAX_VALUE);
    }
    boolean dfs(TreeNode root,int l,int r){
        if(root==null){
            return true;
        }
//         如果当前节点的值小于左区间或者大于右区间，则返回false
        if(root.val<l||root.val>r){
            return false;
        }
        //递归左儿子，并将左儿子的右区间修改为父节点的值
        //递归右儿子，并将右儿子的左区间修改为父节点的值
        return dfs(root.left,l,root.val) && dfs(root.right,root.val,r);
    }
}
```

## 35. 判断是不是完全二叉树

```
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 *   public TreeNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param root TreeNode类 
     * @return bool布尔型
     */
    public boolean isCompleteTree (TreeNode root) {
        // write code here
        if(root == null) return true;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        TreeNode cur;
        boolean flag = false;
        while(!queue.isEmpty()){
            cur = queue.poll();
            if(cur == null){
                flag = true;
                continue;
            }
            if(flag) return false;
            queue.offer(cur.left);
            queue.offer(cur.right);
        }
        return true;
    }
}
```

## 36. 判断是不是平衡二叉树

```
public class Solution {
    public boolean IsBalanced_Solution(TreeNode root){
        // 边界值：空树返回true
        if(root==null)
            return true;
        // 最大深度-最小深度=高度差, 并且从每个节点看进来， 左右子树都是平衡二叉树
        return IsBalanced_Solution(root.left) && IsBalanced_Solution(root.right) && Math.abs(maxDepth(root.left)-maxDepth(root.right))<=1;
         
    }
     
    public int maxDepth(TreeNode root){
        // 判空：null时返回0作为兜底
        if(root==null)
            return 0;
        // 左右递归
        return 1+ Math.max(maxDepth(root.left),maxDepth(root.right));
    }
}
```

## 37. 二叉搜索树的最近公共祖先

```
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 *   public TreeNode(int val) {
 *     this.val = val;
 *   }
 * }
 */

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param root TreeNode类 
     * @param p int整型 
     * @param q int整型 
     * @return int整型
     */
    public int lowestCommonAncestor (TreeNode root, int p, int q) {
        // write code here
        if(root == null) return -1;
        if(root.val == p || root.val == q) return root.val;
        
        int left = lowestCommonAncestor(root.left, p, q);
        int right = lowestCommonAncestor(root.right, p, q);
        if(left != -1 && right != -1) return root.val;
        if(left == -1 && right == -1) return -1;
        return left != -1 ? left : right;
    }
}
```

## 38. 在二叉树中找到两个节点的最近公共祖先

```
import java.util.*;

/*
 * public class TreeNode {
 *   int val = 0;
 *   TreeNode left = null;
 *   TreeNode right = null;
 * }
 */

public class Solution {
    /**
     * 
     * @param root TreeNode类 
     * @param o1 int整型 
     * @param o2 int整型 
     * @return int整型
     */
    public int lowestCommonAncestor (TreeNode root, int p, int q) {
        // write code here
        if(root == null) return -1;
        if(root.val == p || root.val == q) return root.val;
        
        int left = lowestCommonAncestor(root.left, p, q);
        int right = lowestCommonAncestor(root.right, p, q);
        if(left != -1 && right != -1) return root.val;
        if(left == -1 && right == -1) return -1;
        return left != -1 ? left : right;
    }
}
```

## 39. 序列化二叉树

```
/*
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
import java.util.*;
public class Solution {
    
    String Serialize(TreeNode root) {
        //遇到null怎么办
        if(root==null){
            return "X,";
        }
        String left=Serialize(root.left);
        String right=Serialize(root.right);
        return root.val+","+left+right;
         
    }
    TreeNode Deserialize(String str) {
        String[] nodes=str.split(",");
        //将String数组转换为list
        Deque<String> deque=new LinkedList<>(Arrays.asList(nodes));
        return buildTree(deque);
    }
    TreeNode buildTree(Deque<String> dq){
        String var=dq.poll();
        //若为X，说明为空节点
        if(var.equals("X")){
            return null;
        }
        TreeNode root=new TreeNode(Integer.parseInt(var));
        root.left=buildTree(dq);
        root.right=buildTree(dq);
        return root;
    }
}
```

## 40. 重建二叉树

```
import java.util.*;
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public int preindex = 0;
   public TreeNode reConstructBinaryTreeCore(int[] pre,int[] vin,int inbegin,int inend){
        if(inbegin > inend){
            return null;
        }
       TreeNode root = new TreeNode(pre[preindex]);
       int rootindex = findRoot(vin,inbegin,inend,pre[preindex]);
       preindex++;
       root.left = reConstructBinaryTreeCore(pre,vin,inbegin,rootindex - 1);
       root.right = reConstructBinaryTreeCore(pre,vin,rootindex + 1,inend);
       return root;
    }
    public int findRoot(int[] vin,int start,int end,int key){
        if(start > end){
            return -1;
        }
        for(int i = start;i <= end;i++){
            if(vin[i] == key){
                return i;
            }
        }
        return -1;
    }
    public TreeNode reConstructBinaryTree(int [] pre,int [] vin) {
        if(pre == null || vin == null){
            return null;
        }
       return reConstructBinaryTreeCore(pre,vin,0,vin.length - 1);
    }
}
```

## 41. 输出二叉树的右视图

```
import java.util.*;


public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     * 求二叉树的右视图
     * @param xianxu int整型一维数组 先序遍历
     * @param zhongxu int整型一维数组 中序遍历
     * @return int整型一维数组
     */
    public int[] solve (int[] xianxu, int[] zhongxu) {
        List<Integer> res = new ArrayList<>();
        //先确定二叉树
        TreeNode root = reconstrution(xianxu, 0, xianxu.length - 1, zhongxu, 0, zhongxu.length - 1);
        //使用队列，对树进行层次遍历
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while(!queue.isEmpty()) {
            int size = queue.size();
            for(int i = 0; i < size; i++) {
                if(i == size - 1) {
                    res.add(queue.peek().val);
                }
                TreeNode node = queue.poll();
                if(node.left != null) {
                    queue.offer(node.left);
                }
                if(node.right != null) {
                    queue.offer(node.right);
                }
            }
        }
        return res.stream().mapToInt(x -> x).toArray();
    }
    //递归，类似回溯：路径、选择列表、结束条件
    //但是回溯要移除选择，而递归无需移除选择
    public TreeNode reconstrution(int[] xianxu, int preStart, int preEnd, int[] zhongxu, int inStart, int inEnd) {
        if(preStart > preEnd || inStart > inEnd) {
            return null;
        }
        TreeNode root = new TreeNode(xianxu[preStart]);
        //找到中序的位置
        int i = 0;
        for(i = inStart; i <= inEnd; i++) {
            if(zhongxu[i] == xianxu[preStart]) {
                break;
            }
        }
        root.left = reconstrution(xianxu, preStart + 1, preStart + (i - inStart), zhongxu, inStart, i - 1);
        root.right = reconstrution(xianxu, preStart + (i - inStart) + 1, preEnd, zhongxu, i + 1, inEnd);
        return root;
    }
}
```