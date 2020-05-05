# DFS

## 目录

* [二叉树的镜像](#二叉树的镜像)
* [对称的二叉树](#对称的二叉树)

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

