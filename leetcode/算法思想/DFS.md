# DFS

## 目录

* [验证二叉搜索树](#验证二叉搜索树)
* [二叉树的镜像](#二叉树的镜像)

---

### 验证二叉搜索树
Q:给定一个二叉树，判断其是否是一个有效的二叉搜索树。

假设一个二叉搜索树具有如下特征：
节点的左子树只包含小于当前节点的数。
节点的右子树只包含大于当前节点的数。
所有左子树和右子树自身必须也是二叉搜索树

A:采用深度优先遍历，先验证当前的条件，之后在进行下一步遍历

```java
 //采用深度优先遍历策略(DFS)
    public boolean isValidBST(TreeNode root) {
        return helper(root,null,null);
    }
    //使用辅助函数(当前节点，最小整数，最大整数)
    public boolean helper(TreeNode root,Integer lower,Integer upper){
        if(root==null) return true;
        int val=root.val;
        //先比较当前节点
        if(lower!=null&&lower>=val) return false;
        if(upper!=null&&upper<=val) return false;

        //如果左节点返回错误
        if(!helper(root.left,lower,val)) return false;
        if(!helper(root.right,val,upper)) return false;
        return true;
    }
```

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
 
