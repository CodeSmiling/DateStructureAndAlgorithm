# DFS

## 目录

* [二叉树的镜像](#二叉树的镜像)
* [对称的二叉树](#对称的二叉树)
* [矩阵中的路径](#矩阵中的路径)
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
