---
layout:     post
title:      二叉树leetcode
subtitle:   二叉树
date:       2019-11-08
author:     BY
header-img: img/post-bg-cook.jpg
catalog: true
tags:
    - java
---

### leetcode二叉树操作

_***平衡二叉树：左右子树深度之差 <= 1***_

***如果是孩子兄弟表示法，那么层序遍历很简单
层序遍历核心思路：
需要一层一层访问.需要借助一个队列（非递归）***

> 用严谨的语言把完全二叉树规则表述清楚：

	  1.对二叉树进行层序遍历
	  2.遍历过程中，状态分为两个阶段这样的树
		  a)第一阶段，每个节点一定都有两个子树
		  b)第一阶段和第二阶段的过渡，遇到一个特殊的节点，这个节点可能有子树，也可能没有子树，就开启第二阶段
		    （这个临界点如果只是有右子树，没有左子树，那么说明一定不是完全二叉树）
		  c)在第二阶段，接下来的每个节点必须都没有子树，如果有子树，说明不是完全二叉树

> 先构建一颗这样的树：
![](https://github.com/q1206271031/photo/raw/master/%E4%BA%8C%E5%8F%89%E6%A0%91/%E6%9E%84%E5%BB%BA%E6%A0%91.png)

```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;

//shift + F6快速更名
class TreeNode{
    public int val;
    public TreeNode left;
    public TreeNode right;

    public TreeNode(int val) {
        this.val = val;
    }

    @Override
    public String toString() {
        return "Node{" +
                "val=" + val +
                '}';
    }
}

public class Solution {
    //当root为null时候，就是一个空树
    public static TreeNode root = null;
    //构建一棵树
    public static TreeNode build(){
        TreeNode A = new TreeNode(1);
        TreeNode B = new TreeNode(2);
        TreeNode C = new TreeNode(3);
        TreeNode D = new TreeNode(4);
        TreeNode E = new TreeNode(5);
        TreeNode F = new TreeNode(6);
        TreeNode G = new TreeNode(7);
        A.left = B;
        A.right = C;
        B.left = D;
        B.right = E;
        E.left = G;
        C.right = F;
        return A;
    }
```

***add与addAll***

	List.add() 的含义就是：你往这个List 中添加对象，它就把自己当初一个对象，你往这个List中添加容器，它就把自己当成一个容器。
	List.addAll()规定了，自己的这个List 就是容器，往里面增加的List 实例，增加到里面后，都会被看成对象。 
	因此，当需要把多个List 实例放到一起的时候，必须使用List.addAll()方法。


```
    //前序遍历
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        //访问根节点.此处访问指的是把当前节点的值插入到result中
        result.add(root.val);
        //递归遍历左子树
        result.addAll(preorderTraversal(root.left));
        //递归遍历右子树
        result.addAll(preorderTraversal(root.right));
        return result;
    }
    //若不使用addAll方法，只能利用返回值Node left = preorder(root.left); result.add(left);

    //中序遍历
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        //递归访问左子树
        result.addAll(inorderTraversal(root.left));
        //访问当前节点
        result.add(root.val);
        //递归访问右子树
        result.addAll(inorderTraversal(root.right));
        return result;
    }

    //后序遍历
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        //递归访问左子树
        result.addAll(postorderTraversal(root.left));
        //递归访问右子树
        result.addAll(postorderTraversal(root.right));
        //访问当前节点
        result.add(root.val);
        return result;
    }

    //判断二叉树是否相同（比较值，不是比较身份（对象））
    public boolean isSameTree(TreeNode p, TreeNode q) {
        //本质还是借助递归的拆分思想
        //判定两棵树是否相同 =》 比较根节点 + 比较左子树 + 比较右子树
        //1.如果两棵树都是空树 返回true
        if(p == null && q == null){
            return true;
        }
        //2.如果两棵树一个为空，一个不为空 返回false
        if(p == null || q == null){
            return false;
        }
        //3.都不为空
        // a)比较一下根节点的值是否相同，如果不相同  返回false
        if(p.val != q.val){
            return false;
        }
        // b_递归比较左子树和右子树看是不是也相同
        return isSameTree(p.left,q.left) && isSameTree(p.right,q.right);
    }

    //判断s是不是t的子树
    public boolean isSubtree(TreeNode s, TreeNode t) {
        ////本质还是借助递归的拆分思想
        //比较s是否包含t => s和t是不是相等 + s.left是不是包含t + s.right是不是包含t
        //1.如果两棵树都是空树  返回true
        if (s == null && t == null) {
            return true;
        }
        //2.如果两棵树一课是空，一颗不是空  返回false
        if(s == null || t == null){
            return true;
        }
        //3.如果两棵树都非空
        // a)比较根节点的值是不是相等，如果相等，比较一下s和t是不是相同的树
        boolean ret = false;
        if(s.val == t.val){
            ret = isSameTree(s,t);
        }
        // b)递归判定一下t是否被s的左子树包含
        if(!ret){
            ret = isSubtree(s.left,t);
        }
        // c)递归判定t是否被s的右子树包含
        if (!ret) {
            ret = isSubtree(s.right,t);
        }
        return ret;
    }

    //返回二叉树最大深度
    public int maxDepth(TreeNode root) {
        //借助递归把问题进行拆分
        //root的深度 =》 1 + 左子树最大深度 + 右子树最大深度
        //1.如果是空树 深度为0
        if (root == null) {
            return 0;
        }
        //2.如果只有根节点，没有左右子树 深度为1
        if(root.left == null && root.right == null){
            return 1;
        }
        //3.1 + max(左子树深度,右子树深度）
        int leftDepth = maxDepth(root.left);
        int rightDepth = maxDepth(root.right);
        return 1 + (Math.max(leftDepth, rightDepth));
    }

    //平衡二叉树
    public boolean isBalanced(TreeNode root) {
        //1.如果是空树 返回true
        if (root == null) {
            return true;
        }
        //2.如果没有子树 返回true
        if (root.left == null && root.right == null) {
            return true;
        }
        //3.求一下左右子树的高度，判断一下差值是否 <= 1
        int leftDepth = maxDepth(root.left);
        int rightDepth = maxDepth(root.right);
        if(leftDepth - rightDepth > 1 || rightDepth - leftDepth > 1){
            return false;
        }
        //4.递归判断左子树和右子树是否平衡
        return isBalanced(root.left) && isBalanced(root.right);
    }

    //对称二叉树
    //判断一棵树是不是对称的，可以看左右子树是不是镜像关系
    public boolean isSymmetric(TreeNode root) {
        //把判断对称转换成判定 左右子树是否是镜像关系
        //1.root是空树
        if (root == null) {
            return true;
        }
        return isMirror(root.left,root.right);
    }

    public boolean isMirror(TreeNode t1,TreeNode t2){
        //1.若两棵树都是空树 true
        if (t1 == null && t2 == null) {
            return true;
        }
        //2.若两棵树一个为空，一个不是空 false
        if (t1 == null || t2 == null) {
            return false;
        }
        //3.若两棵树都不为空
        // a)比较根节点是不是想相同，不行同就false
        if(t1.val != t2.val){
            return false;
        }
        // b)递归比较子树，t1.left 和 t2.right
        //    t1.right 和 t2.left是不是镜像
        return isMirror(t1.left,t2.right) && isMirror(t1.right,t2.left);
    }

    //求二叉树的镜像
    public TreeNode MakeMirror(TreeNode root){
        //遍历 + 交换左右子树
        if (root == null) {
            return null;
        }
        //交换就是此处的访问
        TreeNode tmp = root.left;
        root.left = root.right;
        root.right = tmp;
        MakeMirror(root.left);
        MakeMirror(root.right);
        return root;
    }



    //层序遍历
    public void levelOrder(TreeNode root){
        if(root == null){
            return;
        }
        //创建一个队列，辅助进行遍历(Stack是类，Queue是接口)
        Queue<TreeNode> queue = new LinkedList<>();
        //1.先把root插入队列
        queue.offer(root);//失败返回null
        //queue.add(root);//失败抛出异常
        while(!queue.isEmpty()){
            //2.循环取队首元素.访问这个元素
            TreeNode cur = queue.poll();
            System.out.println(cur.val + " ");
            //3.把当前这个队首元素左子树和右子树插入到队列之中
            //4.重复执行2
            if(cur.left != null){
                queue.offer(cur.left);
            }
            if (cur.right != null) {
                queue.offer(cur.right);
            }
        }
    }
    
    //层序遍历第二个版本
    public List<List<Integer>> levelOrder2(TreeNode root) {
        List<List<Integer>> ret = new ArrayList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        if(root != null) {
            queue.offer(root);
        }
        //队列的元素 就是一层数据
        while (!queue.isEmpty()) {
            int count = queue.size();//1  2
            //存放的是一层数据
            List<Integer> list = new ArrayList<>();
            while(count > 0) {
                TreeNode cur = queue.poll();
                System.out.print(cur.val+" ");
                list.add(cur.val);
                if(cur.left != null) {
                    queue.offer(cur.left);
                }
                if(cur.right != null) {
                    queue.offer(cur.right);
                }
                count--;//1 0
            }
            ret.add(list);
        }
        return ret;
    }

    //完全二叉树
    public boolean isComplete(TreeNode root){
        if (root == null) {
            return true;
        }
        //1.先对树进行层序遍历
        //如果这个标记为true,说明还是处于第一阶段
        //如果这个标记为true,接下来的节点就不能有子树
        boolean needNoChild = false;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        while (!queue.isEmpty()) {
            TreeNode cur = queue.poll();
            //对这个元素的情况进行判定.
            //访问是一组比较复杂的判断
            if(!needNoChild){
                //第一阶段的情况
                if (cur.left != null && cur.right != null) {
                    //合格状态，继续往下遍历
                    //就把左右子树加入队列
                    queue.offer(cur.left);
                    queue.offer(cur.right);
                }else if(cur.left == null && cur.right != null){
                    //这种情况不是完全二叉树
                    return false;
                } else if (cur.left != null && cur.right == null) {
                    //这是临界状态，开启第二阶段
                    queue.offer(cur.left);
                    needNoChild = true;
                }else{
                    //左右子树都为空，开启第二阶段
                    needNoChild = true;
                }
            }else{
                //第二阶段的情况
                //第二阶段要求节点必须没有子树.只要子树存在，就不是完全二叉树
                if(cur.left != null || cur.right != null){
                    return false;
                }
            }   //end if
        }   //end while
        return true;
    }
    
    
    //完全二叉树最终版
    public boolean isCompleteTree(TreeNode node){
        Queue<TreeNode> queue = new LinkedList<>();
        if (root != null) {
            queue.offer(root);
        }
        while (!queue.isEmpty()) {
            TreeNode cur = queue.poll();
            if (cur != null) {
                queue.offer(cur.left);
                queue.offer(cur.right);
            }else{
                //最后一个节点
                break;
            }
        }
        //遍历队列元素，判断
        while(!queue.isEmpty()){
            TreeNode cur = queue.poll();
            if (cur != null) {
                return false;
            }
        }
        return true;
    }

    public static void main(String[] args) {
        TreeNode root = build();
        Solution solution = new Solution();
//        boolean ret = solution.isBalanced(root);
//        System.out.println(ret);
        solution.levelOrder(root);
    }
}

```
