# LeetCode 剑指Offer 22


<!--more-->

## 题目：链表中倒数第k个节点

[题目链接](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。例如，一个链表有6个节点，从头节点开始，它们的值依次是1、2、3、4、5、6。这个链表的倒数第3个节点是值为4的节点。

> 示例1

**输入**：

给定一个链表: 1->2->3->4->5, 和 k = 2.

**输出**：

返回链表 4->5.

## 题解

直接先遍历得到链表长度再遍历即可。

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func getKthFromEnd(head *ListNode, k int) *ListNode {
    l := 0
    bak := head
    for bak != nil {
		l++
		bak = bak.Next
    }
    for i:=0;i<l-k;i++ {
        head = head.Next
    }
    return head
}
```

> 执行用时: 0 ms
>
> 内存消耗: 2.2 MB

给出一种更巧妙的解法

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func getKthFromEnd(head *ListNode, k int) *ListNode {
	start := head
	end := head
	for i := 0; i < k; i++ {
		if start == nil {
			return nil
		}
		start = start.Next
	}
	for start != nil {
		start = start.Next
		end = end.Next
	}
	return end
}
```

> 执行用时: 0 ms
>
> 内存消耗: 2.2 MB
