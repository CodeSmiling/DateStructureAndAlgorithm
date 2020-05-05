# BFS

## 目录

* [二叉树的镜像](#二叉树的镜像)


---

 ### 二叉树的镜像
 Q:请完成一个函数，输入一个二叉树，该函数输出它的镜像。
 
 A:采用广度优先遍历的策略，使用辅助栈或者是队列来实现
 
 ```java
 public TreeNode mirrorTree(TreeNode root){
        if(root==null) return null;
        Stack<TreeNode> stack=new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()){
            //取出当前的节点
            TreeNode temp=stack.pop();
            if(temp.left!=null) stack.push(temp.left);
            if(temp.right!=null) stack.push(temp.right);
            //进行交换操作
            TreeNode node=temp.left;
            temp.left=temp.right;
            temp.right=node;
        }
        return root;
    }
 ```
 
