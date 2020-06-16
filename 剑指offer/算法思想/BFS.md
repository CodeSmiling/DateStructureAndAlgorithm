# BFS

## 目录

* [二叉树的镜像](#二叉树的镜像)
* [从上到下打印二叉树](#从上到下打印二叉树)
* [从上到下打印二叉树II](#从上到下打印二叉树II)
* [从上到下打印二叉树III](#从上到下打印二叉树III)

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
### 从上到下打印二叉树II

问题描述:从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。
例如:

给定二叉树: [3,9,20,null,null,15,7],

           3
          / \
         9  20
           /  \
          15   7
返回其层次遍历结果：

[
  [3],
  [9,20],
  [15,7]
]       

题目链接：https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/

```java
class Solution{
    public List<List<Integer>> levelOrder(TreeNode root){
        List<List<Integer>> resList = new ArrayList<>();
        if(root == null){
            return resList;
        }
        Deque<TreeNode> deque = new LinkedList<>();
        deque.add(root);
        while(deque.size() != 0){
            int size = deque.size();
            //建立一个队列存储返回结果
            List<Integer> tempList = new ArrayList<>();
            //遍历每一层的结果
            for(int i = 0; i < size; i++){
             TreeNode node = deque.poll();
                tempList.add(node.val);
                if(node.left != null){
                    deque.push(node.left);
                }
                if(node.right != null){
                    deque.push(node.right);
                }
            }           
            resList.add(tempList);
        }
        return resList;
    }
}
```
### 从上到下打印二叉树III

问题描述:从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。
例如:

给定二叉树: [3,9,20,null,null,15,7],

           3
          / \
         9  20
           /  \
          15   7
返回其层次遍历结果：

[
  [3],
  [20,9],
  [15,7]
]

题目链接：https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/

```java
class Solution{
    public List<List<Integer>> levelOrder(TreeNode root){
        List<List<Integer>> resList = new ArrayList<>();
        if(root == null){
            return resList;
        }
        Deque<TreeNode> deque = new LinkedList<>();
        deque.add(root);
        while(deque.size() != 0){
            //表名此时为奇数行
            boolean flag = false;
            List<Integer> tList = new ArrayList<>();
            //如果此时是奇数行
            if(!flag){
                for(int i = deque.size(); i > 0; i--){
                    TreeNode node = deque.removeFirst();
                    tList.add(node.val);
                    //从右到左加入双端队列(左右节点加入双端队列末尾)
                    if(node.left != null){
                        queue.addLast(node.left);
                    }
                    if(node.right != null){
                        queue.addLast(node.right);
                    }               
                }
                flag = true;
                resList.add(tList);
            }
            else{
                tList = new ArrayList<>();
                for(int i = deque.size(); i > 0; i--){
                    //从右到左取出双端队列
                    TreeNode node = deque.removeLast();
                    tList.add(node.val);
                    //从左到右加入双端队列(此时需要先将右边节点加入，再将左边节点加入双端队列前端)
                    if(node.right != null){
                        queue.addFirst(node.right);
                    }   
                    if(node.left != null){
                        queue.addFirst(node.left);
                    }             
                }
                flag = false;
                resList.add(tList);
            }
        }
        return resList;
    }
}
```
