# DFS

## 目录

* [二叉树的镜像](#二叉树的镜像)
* [对称的二叉树](#对称的二叉树)
* [矩阵中的路径](#矩阵中的路径)
* [树的子结构](#树的子结构)
* [二叉搜索树与双向链表](#二叉搜索树与双向链表)
* [重建二叉树](#重建二叉树)
---
### 二叉树的镜像
Q:请完成一个函数，输入一个二叉树，该函数输出它的镜像。

A:采用深度优先遍历，先找到最后遍历的节点，进行交换操作，之后进行递归遍历。
```java
  public TreeNode mirrorTree(TreeNode root) {
        if(root==null) return null;
        //遍历当前节点的操作
        TreeNode temp=root.left;
        root.left=mirrorTree(root.right);
        root.right=mirrorTree(temp);

        return root;
    }
```
---
### 对称的二叉树
Q:请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

A：采用深度优先遍历。
```java
  public boolean isSymmetric(TreeNode root) {
       if(root==null) return true;
        return recur(root.left,root.right);
    }
    public boolean recur(TreeNode L,TreeNode R){
        if(L==null&&R==null) return true;
        //对称条件的判定(只有左右节点的值相等 )
        if(L==null||R==null||L.val!=R.val) return false;
        //左子树的右节点与右子树的左节点
        return recur(L.right,R.left)&&recur(L.left,R.right);
    }    
```
---
### 矩阵中的路径
请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用加粗标出）。
[["a","b","c","e"],
["s","f","c","s"],
["a","d","e","e"]]
但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。

A:使用深度遍历和剪枝算法：
  先找递归条件(索引和字符索引)，终止条件（if(数组越界和不匹配)）return 错误，if(字符长度==现在索引)，return正确。
  递归条件：（1）标记一个元素，递归元素上下左右，（2）字符匹配之后使用‘/’代替，防止重复访问。
  回溯:返回res，代表是否搜索到目标字符串。
  ```java
   char[] words=word.toCharArray();
        for(int i=0;i<board.length;i++){
            for (int j=0;j<board[0].length;j++){
                if(dfs(board,words,i,j,0)) return true;
            }
        }
        return false;
    }
    //递归函数
        boolean dfs(char[][]board,char[] word,int i,int j,int k){
        //越界或者是不相等
            if(i >= board.length || i < 0 || j >= board[0].length || j < 0 || board[i][j] != word[k]) return false;
        //返回结果
        if(k==word.length-1) return true;
        char temp=board[i][j];
        board[i][j]='/';
        boolean res=dfs(board,word,i+1,j,k+1)||dfs(board, word, i-1, j, k+1)||
                dfs(board, word, i, j-1, k+1) ||dfs(board, word, i, j+1, k+1);
        board[i][j]=temp;
        return res;

  ```
  
### 机器人的运动范围

题目链接:https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/

解决方案:方案一 采用深度优先遍历(递归)

  (1)递归参数:i, j, si, sj:横坐标,纵坐标,横坐标的和,纵坐标的和
  
  (2)终止条件:索引越界,坐标和大于k,该值已经遍历过
  
  (3)递归工作:标记当前单元格,推进下一个单元格
  
  (4)回溯参数:1 + 向下递归寻找到的解决方法个数 + 向右递归寻找到的解决方法个数
  
  ```java
  class Solution {
    boolean[][] visit;
    int rowLength;
    int colLength;
    int k;
    public int movingCount(int m, int n, int k) {
        rowLength = m;
        colLength = n;
        this.k = k;
        visit = new boolean[m][n];
        return dfs(0,0,0,0);
    }
    public int dfs(int i, int j, int si, int sj){
        //终止条件
        if(i >= rowLength || j >= colLength || si + sj > k || visit[i][j]){
            return 0;
        }
        visit[i][j] = true;
        //递推条件
        return 1 + dfs(i + 1, j, sum(i + 1), sum(j)) + dfs(i, j + 1, sum(i), sum(j + 1));  
    }
    public int sum(int i){
        int sumOfNum = 0;
        while(i > 0){
        sumOfNum += i % 10;
        i = i / 10;
        }
        
        return sumOfNum;
    }
}
  ```
  
解决方案:方案二:采用广度优先遍历(迭代)

  迭代流程
  
    (1)将元素从队列中取出
    
    (2)判断是否越界,横纵坐标和超过k,continue;
    
    (3)标记当前单元格,符合条件res++;
    
    (4)入队

```java
class Solution{
    public int movingCount(int m, int n, int k){
        boolean[][] visit;
        int res = 0;
        Queue<int[]> queue = new LinkedList<>();
        queue.add(new int[]{0,0,0,0});
        while(!queue.isEmpty()){
            //取出数组
            int[] x = queue.poll();
            int i = x[0], j = x[1], si = x[2], sj = x[3];
            if(i >= m || j >=  n || si + sj > k) continue;
            visit[i][j] = true;
            res++;
            queue.add(new int[]{i + 1, j, sum(i + 1), sum(j)});
            queue.add(new int[]{i, j + 1, sum(i), sum(j + 1)}); 
        }
    }
    public int sum(int i){
        int sumOfNum = 0;
        while(i > 0){
            sumOfNum += i % 10;
            i /= 10;
        }
        return sumOfNum;
    }    
}
```
    


  









Q:题目链接：https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/
  
A: 1.根据题意，需要知道二叉树前后顺序，所以考虑中序遍历。
    
   2.中序遍历之后，需要改变两个节点的连接顺序
    
 ```java
 class Solution{
    Node pre,cur head;
    public Node treeToDoublyList(Node root){
        if(toot==null) return null;
        dfs(root);
        //首尾相连
        head.left=pre;
        pre.right=head;
        return head
    }
    void dfs(Node cur){
        if(cur==null) return ;

        //中序遍历
        dfs(cur.left);
        if(pre!=null) pre.right=cur;
        else head=cur;
        //单向连接
        cur.left=pre;
        //保存当前节点
        pre=cur;
        dfs(cur.right);    
    }
}
 ```
问题描述: 输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。
 
例如，给出

前序遍历 preorder = [3,9,20,15,7]

中序遍历 inorder = [9,3,15,20,7]

返回如下的二叉树：

        3
       / \
      9  20
        /  \
       15   7
 
题目链接： https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/

解决思路：

    1.根据前序遍历确定根节点的root值
    2.根据root值将中序遍历数组分为两部分(左|根|右)
    3.然后在根据中序遍历数组去前序遍历再次划分
    递归参数：前序遍历节点值的索引：pre_root,左边数组的索引：in_left,右边数组的索引：in_right
    
```java
class Solution{
    int[] po;
    HashMap<Integer,Integer> dic = new HashMap<>();
    public TreeNode buildTree(int[] preorder, int[] inorder){
        int length = preorder.length;
        po = preorder;
        for(int i = 0; i < length; i++){
            dic.put(preorder[i], i);
        }
        return recur(0, 0, length - 1);
    }
    //递归遍历的参数
    public TreeNode recur(int pre_root, int in_left, int in_right){
        //终止条件
        if(in_left > in_right){
            return null;
        }
        //递推工作
            //1.建立根节点root(从索引中取出)
            TreeNode root = new TreeNode(po[pre_root]);
            //将中序遍历数组划分两部分(获取当前节点的索引)
            int i = dic.get(po[pre_root]);
            //左边部分(当前节点值+1, 左边界in_left, 有边界i - 1)
            root.left = recur(pre_root + 1, in_left, i - 1);
            //右边部分(当前节点值+1+左边界长度, 左边界i, 有边界in_right)
            root.right = recur(pre_root + 1 + i - in_left, i + 1, in_right);
            return root;
    }   
}
```
