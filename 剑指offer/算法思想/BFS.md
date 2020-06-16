# BFS

## 目录

* [二叉树的镜像](#二叉树的镜像)
* [从上到下打印二叉树](#从上到下打印二叉树)

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
---
### 从上到下打印二叉树 II
问题描述:从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。
例如:

给定二叉树: [3,9,20,null,null,15,7],

           3
          / \
         9  20
           /  \
          15   7
返回：

[3,9,20,15,7] 

题目链接：https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/

解决方案:采用广度遍历的策略
```java
class Solution{
    public int[] levelOrder(TreeNode root){
        if(root == null){
            return new int[0];
        }
        //使用辅助栈
        Deque<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        List<Integer> resList = new ArrayList<>();
        //使用辅助栈或者是队列的时候需要判断是否为空
        while(queue.size() != 0){
            TreeNode node = queue.poll();
            resList.add(node.val);
            if(node.left != null){
                queue.addLast(node.left);
            }
            if(node.right != null){
                queue.addLast(node.right);
            }
        }
        return resList.toArray(new int[resList.size()]);
    }
}
```
