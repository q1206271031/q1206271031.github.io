### 二叉树操作

> 编一个程序，读入用户输入的一串先序遍历字符串，根据此字符串建立一个二叉树（以指针方式存储）。 
  例如如下的先序遍历字符串： ABC##DE#G##F### 其中“#”表示的是空格，空格字符代表空树。建立起此二叉树以后，再对二叉树进行中序遍历，输出遍历结果。

[链接](https://www.nowcoder.com/practice/4b91205483694f449f94c179883c1fef?tpId=60&&tqId=29483&rp=1&ru=/activity/oj&qru=/ta/tsing-kaoyan/question-ranking)

```java
import java.util.Scanner;
class TreeNode{
    public char val;
    public TreeNode left;
    public TreeNode right;

    public TreeNode(char val) {
        this.val = val;
    }
}
//二叉树的构建及遍历（含有'#'(空的)）
public class TreeBuildDemo {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        //判断有没有下一个字符
        while(scanner.hasNext()){
            String line = scanner.next();
            //line就是一组先序遍历的结果（带#）
            TreeNode root = buildTree(line);
            inOrder(root);
            System.out.println();
        }
    }

    static int index = 0;
    public static TreeNode buildTree(String line){
        //记录当前递归过程中遍历到第几个字符
        index = 0;
        return createTreePreOrder(line);
    }
    //这个辅助函数完成递归
    public static TreeNode createTreePreOrder(String line){
        char c = line.charAt(index);//根节点
        if(c == '#'){
            return null;
        }
        TreeNode root = new TreeNode(c);
        index++;
        root.left = createTreePreOrder(line);
        index++;
        root.right = createTreePreOrder(line);
        return root;
    }

    public static void inOrder(TreeNode root){
        if (root == null) {
            return;
        }
        inOrder(root.left);
        System.out.print(root.val + " ");
        inOrder(root.right);
    }
}
```

***注意：若TreeNode类中val为int类型，则所得结果会是131，因为char会ASCII转化为数字(c + " "),即99+32=131***

> 给定一个二叉树，返回其按层次遍历的节点值。 （即逐层地，从左到右访问所有节点）

[链接](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)
  
