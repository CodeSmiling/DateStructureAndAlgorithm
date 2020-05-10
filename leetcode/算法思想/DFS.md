# DFS

## 目录

* [验证二叉搜索树](#验证二叉搜索树)
* [二叉树最近的公共祖先](#二叉树最近的公共祖先)


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

### 二叉树最近的公共祖先
Q:给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为:"对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。"

https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/

A:采用深度优先遍历的策略，fx表示x节点左子树或者是右子树是否包含p，q节点。

```java
public class Solution {
    private TreeNode ans=null;
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        this.dfs(root, p, q);
        return this.ans;
    }

    /**
     * 该函数用于判断该节点及其子树是否包含了q或者p的值
     */
    public boolean dfs(TreeNode root,TreeNode p,TreeNode q){
        if(root==null) return false;
        boolean lson=dfs(root.left,p,q);
        boolean rson=dfs(root.right, p, q);
        /**1.如果root的左子树包含并且右子树也包含
         * 2.节点的值等于p的值或者是q的值并且root的左子树或者是右子树也包含p，q
         * 此时节点值等于公共祖先
         */
        
        if((lson&&rson)||((root.val==p.val||root.val==q.val)&&(lson||rson))){
            ans=root;
        }
        /**
         * 左子树或者是右子树或者是该节点是否包含了p或者是q的值
         */
        return lson||rson||(root.val==p.val||root.val==q.val);
    }
}
```
