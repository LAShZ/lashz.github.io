# LeetCode 剑指Offer 07


# LeetCode刷题笔记--剑指Offer 07

## 题目：重建二叉树

[题目链接](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)。

输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

> 示例

**输入**：

前序遍历 preorder = [3,9,20,15,7]

中序遍历 inorder = [9,3,15,20,7]

**输出**：
```
    3
   / \
  9  20
    /  \
   15   7
```
**限制**:

0 <= 节点个数 <= 5000

## 题解

本题难度不高，主要是对于二叉树的遍历方式的掌握。

**前序遍历**是先遍历`根`节点，然后遍历`左`子树，最后遍历`右`子树。

**中序遍历**是先遍历`左`子树，然后遍历`根`节点，最后遍历`右`子树。

**后续遍历**是先遍历`左`子树，然后遍历`右`子树，最后遍历`根`节点。

所以，可以通过前序遍历，找到二叉树的根节点，再通过找到的根节点，在中序遍历序列中，将树在根节点的前后划分为左子树和右子树，最后对子树进行递归或是迭代，直到将所有节点都插入树内。

以给出的示例为例，首先通过前序遍历序列的第一个值preorder[0] = 3知道根节点为3，然后在中序遍历序列中找到值为3的节点inorder[1]，这样就知道了左子树的节点为[9]，右子树的节点为[15,20,7]。因为preorder[0]为根节点，且由中序遍历可知，左子树加上根节点的节点数序号为index = 1（从0开始计数），所以`左`子树的节点为`preorder[1: index+1]`或是`inorder[:index]`，`右`子树的节点为`preorder[index+1: ]`或是`inorder[index+1:]`。

下面给出递归和迭代两种方法的代码。

### 递归法

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func buildTree(preorder []int, inorder []int) *TreeNode {
    if len(preorder)==0{
        return nil
    }
    root := &TreeNode{Val:preorder[0]}	// 建树
    
    var index int
    for i := range inorder{
        if inorder[i]==preorder[0]{	// 找到根节点
            index = i
            break
        }
    }
    root.Left = buildTree(preorder[1:index+1],inorder[:index])	// 左子树递归
    root.Right = buildTree(preorder[index+1:],inorder[index+1:])	// 右子树递归
    return root
}
```
递归法使用的是DFS，用示例的输入演示整个递归的过程如下图所示，数字代表插入子树的顺序：

![image](https://cdn.jsdelivr.net/gh/LAShZ/blog-pic-repo@main//img/%E5%89%91%E6%8C%87Offer07-1.png)

### 迭代法

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func buildTree(preorder []int, inorder []int) *TreeNode {
    if len(preorder) == 0 {
		return nil
	}
	var stack []*TreeNode	//储存节点的栈
	root := &TreeNode{Val: preorder[0]}
	cur := root
	for pre, in := 1, 0; pre < len(preorder); pre++ {
		if cur.Val != inorder[in] {	// 当前节点不是叶子节点
			stack = append(stack, cur)	// 节点入栈
			cur.Left = &TreeNode{Val: preorder[pre]}	// 插入左子树
			cur = cur.Left	// 向下层移动
		} else {	// 已经碰到了叶子节点
			in++	// 将该叶子节点从给出的序列中删除
			for len(stack) > 0 && stack[len(stack)-1].Val == inorder[in] {	// 退回到上一个有叶子节点的根节点
				in++
				cur = stack[len(stack)-1]
				stack = stack[:len(stack)-1]
			}
			cur.Right = &TreeNode{Val: preorder[pre]}	// 插入右子树
			cur = cur.Right
		}
	}
	return root
}
```

迭代法出现在字节跳动面试题中。使用的方法是DFS，一路向下将节点插入树内，直到找到叶子节点，然后退回至有右叶子的第一个根节点，再对右子树DFS。用示例的输入演示整个迭代的过程如下图所示，数字代表插入节点的顺序：

![iamge](https://cdn.jsdelivr.net/gh/LAShZ/blog-pic-repo@main/img/%E5%89%91%E6%8C%87Offer07-2.png)
