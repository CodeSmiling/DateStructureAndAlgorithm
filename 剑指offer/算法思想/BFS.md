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
### 从上到下打印二叉树
Q:从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。
例如:

给定二叉树: [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
A:采用广度遍历的策略
```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> resList=new ArrayList<>();
        //采用广度遍历策略
        if(root==null) return resList;
        Queue<TreeNode> queue=new LinkedList<>();
        queue.add(root);
        while(!queue.isEmpty()){
            List<Integer> tempList=new ArrayList<>();
            //此时队列是不断变化的
            for(int i=queue.size();i>0;i--){
                TreeNode node=queue.poll();
                tempList.add(node.val);
                if(node.left!=null) queue.add(node.left);
                if(node.right!=null) queue.add(node.right);
            }
            resList.add(tempList);
        }
        return resList;
    }
}
```
