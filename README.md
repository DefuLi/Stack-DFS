# Stack-DFS
本文对使用栈结构实现深度优先搜索类型的编程题进行分析和记录，所有的题来自于LeetCode。栈的特性是先入后出，后入先出，所以用在深度优先搜索时有着特殊的结构优势。更加形象的使用栈进行深度搜索的过程，建议观看[LeetCode上动画](https://leetcode-cn.com/explore/learn/card/queue-stack/219/stack-and-dfs/881/)。<br>
在实现深度搜索时，可以使用编程语言提供的递归隐式栈，也可以手动创建显式栈，如Java的Stack类。

## 目标和
给定一个非负整数数组，a1, a2, ..., an, 和一个目标数，S。现在你有两个符号 + 和 -。对于数组中的任意一个整数，你都可以从 + 或 -中选择一个符号添加在前面。

返回可以使最终数组和为目标数 S 的所有添加符号的方法数。
```java
输入: nums: [1, 1, 1, 1, 1], S: 3
输出: 5
解释: 

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

一共有5种方法让最终目标和为3。
```
在下面代码中提供了两种方法，分别是递归和显示栈。每次用当前值减去或加上数组中的1，减去和加上分别代表两种状态。在使用的时候，一定要注意停止的边界条件。
```java
package practice;

import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

// 目标和
public class FindTargetSumWays {
    public static void main(String[] args) {
        int[] nums = {1, 1, 1, 1, 1};
        int S = 3;
        FindTargetSumWays find = new FindTargetSumWays();
        System.out.println(find.findTargetSumWays(nums, S));
    }

    public int findTargetSumWays(int[] nums, int S) {
        Stack<kvClass> stack = new Stack<>();
        stack.push(new kvClass(S, 0));
        int res = 0;
        while (!stack.isEmpty()) {
            kvClass head = stack.pop();
            if (head.k == 0 && head.v == nums.length) {
                res++;
            }
            if (head.v < nums.length) {
                stack.push(new kvClass(head.k + nums[head.v], head.v + 1));
                stack.push(new kvClass(head.k - nums[head.v], head.v + 1));
            }

        }
        return res;
    }

    /**
     int res = 0;
     public int findTargetSumWays(int[] nums, int S) {
     //        int res = 0;
     int start = 0;
     return helper(nums, S, start, res);
     }

     public int helper(int[] nums, int S, int start, int res) {
     if (start >= nums.length) {
     if (S == 0) res = res + 1;
     return res;
     }
     res = helper(nums, S + nums[start], start + 1, res);
     res = helper(nums, S - nums[start], start + 1, res);
     return res;
     }
     **/
}

class kvClass {
    public int k;
    public int v;

    public kvClass(int k, int v) {
        this.k = k;
        this.v = v;
    }
}
```
## 二叉树的中序遍历
给定一个二叉树，返回它的中序遍历。
```java
输入: [1,null,2,3]
   1
    \
     2
    /
   3

输出: [1,3,2]
```
代码中还是提供了两种方法去解决该问题，分别是递归和显示栈。在使用显示栈的时候，首先判断当前节点左子节点是否为空，如果为空，则需要将当前节点存入List中，再判断右子节点是否为空，进行操作；如果当前节点的左子节点不为空，则需要将左子节点存入栈中。<br>
在二叉树的遍历中，中序遍历、前序遍历、后续遍历在使用显示栈的时候，规则相差较大。所以建议使用递归的方法进行遍历，规则比较相近，代码写起来会比较轻松。<br>
```java
package practice;

import java.util.*;

// 二叉树的中序遍历
public class InOrderTraversal {
    public static void main(String[] args) {
        TreeNode root = new TreeNode(1);
        TreeNode node1 = new TreeNode(2);
        TreeNode node2 = new TreeNode(3);
        TreeNode node3 = new TreeNode(4);
        TreeNode node4 = new TreeNode(5);
        TreeNode node5 = new TreeNode(6);
        TreeNode node6 = new TreeNode(7);

        root.left = node1;
        root.right = node2;
        node1.left = node3;
        node1.right = node4;
        node2.left = node5;
        node2.right = node6;
        node3.left = null;
        node3.right = null;
        node4.left = null;
        node4.right = null;
        node5.left = null;
        node5.right = null;
        node6.left = null;
        node6.right = null;

        InOrderTraversal in = new InOrderTraversal();
        List<Integer> list = in.inorderTraversal(root);
        for (Integer integer : list) {
            System.out.println(integer);
        }
    }

    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()){
            if(stack.peek() == null) return list;
            if(stack.peek().left != null){
                stack.push(stack.peek().left);
            }else {
                list.add(stack.peek().val);
                TreeNode temp = stack.pop();
                if(!stack.isEmpty()) stack.peek().left = null;
                if(temp.right != null) stack.push(temp.right);
            }
        }
        return list;
    }

    /** 递归法
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        if (root == null) {
            return list;
        }
        tra(root, list);
        return list;
    }

    public void tra(TreeNode root, List<Integer> list) {
        if (root == null) {
            return;
        } else {
            tra(root.left, list);
            list.add(root.val);
            System.out.println(root.val);
            tra(root.right, list);
        }
    }
     **/
}
```
## 字符串解码
1) 给定一个经过编码的字符串，返回它解码后的字符串。<br>
2) 编码规则为: k\[encoded_string]，表示其中方括号内部的 encoded_string 正好重复 k 次。注意 k 保证为正整数。<br>
3) 你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。<br>
4) 此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 k ，例如不会出现像 3a 或 2\[4] 的输入。<br>
```java
s = "3[a]2[bc]", 返回 "aaabcbc".
s = "3[a2[c]]", 返回 "accaccacc".
s = "2[abc]3[cd]ef", 返回 "abcabccdcdcdef".
```
对于该题，在循环迭代中要首先判断当前字符是否为“]”，如果是的话，代表遇到了第一个闭括号，此时需要往前找到“\[”，并拼接字符成encoded_string，进而找到0-9的数字进行迭代生成k次；如果当前字符不是“]”，则把该字符添加到栈中。
```java
package practice;

import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

// 字符串解码
public class DecodeString {
    public static void main(String[] args) {
        DecodeString obj = new DecodeString();
//        String s = "3[a2[c]]";
//        String s = "3[a]2[bc]";
        String s = "100[leetcode]";
        System.out.println(obj.decodeString(s));  // aaabcbc
    }

    public String decodeString(String s){
        char[] charArr = s.toCharArray();
        String[] sArr = new String[charArr.length];
        for (int i = 0; i < charArr.length; i++) {
            sArr[i] = String.valueOf(charArr[i]);
        }
        Stack<String> stack = new Stack<>();
        String encodedString = "";
        String tempStr = "";
        for (int i = 0; i < charArr.length; i++) {
            if(charArr[i] == ']'){
                while (!stack.peek().equals("[")){
                    encodedString = stack.pop() + encodedString;
                }
                stack.pop();
                String tempNums = "";
                while (!stack.isEmpty() && Character.isDigit(stack.peek().charAt(0))){
                    tempNums = stack.pop() + tempNums;
                }
                int nums = Integer.parseInt(tempNums);
//                int nums = Integer.parseInt(stack.pop());
                for (int j = 0; j < nums; j++) {
                    tempStr += encodedString;
                }
                stack.push(tempStr);
                encodedString = "";
                tempStr = "";
            }else {
                stack.push(sArr[i]);
            }
        }
        String res = "";
        while (!stack.isEmpty()){
            res = stack.pop() + res;
        }
        return res;
    }
}

```
