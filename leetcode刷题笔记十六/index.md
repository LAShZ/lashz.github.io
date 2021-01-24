# LeetCode 剑指Offer 18

<!--more-->

## 题目：删除链表的节点

[题目链接](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)

给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。

返回删除后的链表的头节点。

> 示例1

**输入**：

head = [4,5,1,9], val = 5

**输出**：

[4,1,9]

> 示例2

**输入**：

head = [4,5,1,9], val = 1

**输出**：

[4,5,9]

**说明**:

题目保证链表中节点的值互不相同

## 题解

遍历找到节点和前驱节点，然后将前驱节点的Next指针指向找到的节点的下一个节点即可，无需考虑内存回收问题。

需要注意若为空链表或是找到的节点为头节点时的情况要单独考虑。

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func deleteNode(head *ListNode, val int) *ListNode {
    if head == nil {
        return nil
    }
    if head.Val == val {
        head = head.Next
        return head
    }
    prior := head
    next := prior.Next
    for next != nil {
        if next.Val == val {
            prior.Next = next.Next
            return head
        }
        prior = prior.Next
        next = next.Next
    }
    return head
}
```

> 执行用时: 0 ms
>
> 内存消耗: 2.8 MB

也可以采用递归的形式，每一次都只考虑头节点，然后将当前头节点的子链表递归，下面给出递归代码

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */

func deleteNode(head *ListNode, val int) *ListNode {
    if head == nil {
        return nil
    }

    if head.Val == val {
        return head.Next
    }

    head.Next = deleteNode(head.Next, val)
    return head
}
```

> 执行用时: 4 ms
>
> 内存消耗: 3 MB
