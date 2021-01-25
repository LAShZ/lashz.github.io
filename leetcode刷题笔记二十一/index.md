# LeetCode 剑指Offer 24


<!--more-->

## 题目：反转链表

[题目链接](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)

定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。

> 示例1

**输入**：

1->2->3->4->5->NULL

**输出**：

5->4->3->2->1->NULL

## 题解

保存当前节点，上一节点和下一节点，直接将当前节点的Next指针指向上一节点，然后将上一节点移动到当前节点，再将当前节点移动到下一节点即可。

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func reverseList(head *ListNode) *ListNode {
    var prev *ListNode
    curr := head
    for curr != nil {
        next := curr.Next	// 注意之后会改变Next指针，所以这里需要保存备份，不然curr无法移动
        curr.Next = prev
        prev = curr
        curr = next
    }
    return prev
}
```

> 执行用时: 0 ms
>
> 内存消耗: 2.5 MB
