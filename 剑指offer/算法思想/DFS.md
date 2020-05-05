# DFS

## 目录

* [二叉树的镜像](#二叉树的镜像)


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
 
