# LeetCode 剑指Offer 06

<!--more-->
# LeetCode刷题笔记--剑指Offer 06

## 题目：从尾到头打印链表

[题目链接](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)。

输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

> 示例


**输入**：

head = [1, 3, 2]

**输出**：

[2, 3, 1]

**限制**:

0 <= 链表长度 <= 10000

## 题解

本题最容易想到的就是使用一个**栈**，利用栈先进后出的特性将链表内的值都放入，然后再从栈中取出放入数组内，即可实现倒序输出。不过由于Go语言并无现成的栈的数据结构，且要自己实现显得整到题目的代码较为臃肿，故可以使用别的方法。下面给出我的两种思路（其实归根结底还是用的栈）

### 利用defer和匿名函数

虽然Go语言中并无现成的栈的数据结构，但是Go语言的`defer`的执行顺序为先进后出（关于defer的介绍可以参考博主的[这篇博客](../go语言学习之旅二)，关于匿名函数和闭包的介绍可以参考博主的[这篇博客](../go语言闭包研究)），因此我们可以利用这个特性，循环到链表的每一个节点时，都通过defer调用一个匿名函数，匿名函数的功能为将该节点的值加到数组尾部。当函数执行到return语句时，就按照倒序将链表的值放入了数组中。需要注意的一点是，使用这种方式时，函数的**返回值必须命名**，否则执行完return语句后函数将返回一个空值，而不会把所有defer执行完成后的数组返回。下面给出代码：

```go

/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reversePrint(head *ListNode) (list []int) {
    for ; head != nil; head = head.Next {
    temp := head.Val
    defer func() {
        list = append(list, temp)
    }()
    }
    return
}
```

执行用时: 4 ms

内存消耗: 4.1 MB

可以看到这种方法虽然简便，且利用了Go的特性，但是用时和内存消耗都较大，比较不理想。下面给出第二种更好的方法

### 两次遍历

先通过一次遍历整个链表，得到整个链表的长度，然后使用make初始化一个与链表长度相等的数组，再通过第二次遍历整个链表，将链表中的元素按从数组尾部到头部的顺序填充入数组内。下面给出代码：

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reversePrint(head *ListNode) []int {
    var length int
    temp := head
    for ;temp != nil; length++ {
        temp = temp.Next
    }
    list := make([]int, length)
    for ;head != nil; length-- {
        list[length - 1] = head.Val
        head = head.Next
    }
    return list
}
```

执行用时: 0 ms

内存消耗: 2.8 MB

这种方法的执行用时和内存消耗均击败了100%的用户。
