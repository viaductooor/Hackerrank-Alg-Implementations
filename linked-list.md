# Find Merge Point of Two Lists

<https://www.hackerrank.com/challenges/find-the-merge-point-of-two-joined-linked-lists/problem>

两个单链表（无环）在某一个节点开始汇合，求这个汇合点。

**思路**

先求两个单链表的长度，然后他们的差k。创建两个工作指针指向两个链表的头节点，让长的链表的指针先向前移动k，然后两个指针同时向前移动，直到他们两个指针相同。

```python
def findMergeNode(head1, head2):
    len1,len2=0,0
    # Get their respective lengths
    p1,p2 = head1,head2
    while p1!=None:
        len1 += 1
        p1 = p1.next
    while p2!=None:
        len2 += 1
        p2 = p2.next

    # Let the longer list to forward abs(len1-len2)
    p1,p2 = head1,head2
    if len1>len2:
        for _ in range(len1-len2):
            p1 = p1.next
    else:
        for _ in range(len2-len1):
            p2 = p2.next

    # Tow lists forward simultaneously until they point to the same node
    while p1!=None and p2!=None:
        if p1 == p2:
            return p2.data
        else:
            p1 = p1.next
            p2 = p2.next
    return None

```

**扩展**

如果链表中有环的话因该怎么处理呢？

# Inserting a Node Into a Sorted Doubly Linked List

<https://www.hackerrank.com/challenges/insert-a-node-into-a-sorted-doubly-linked-list/problem>

给定一个有序的双向链表，将一个节点插入到这个链表中并保持有序，返回新的链表。

**思路**

重点是要讨论三种情况：

- 新节点是否应该插入到头端
- 新节点是否应该插入到尾端
- 新节点是否应该插入到链表中间

```python
def sortedInsert(head, data):
    newNode = DoublyLinkedListNode(data)
    bigger = head
    while bigger.next!=None:
        if bigger.data>data:
            break
        bigger = bigger.next
    if bigger.next == None and bigger.data < data:
        # data is the biggest, add new node to tail
        bigger.next = newNode
        newNode.prev = bigger
    elif bigger == head:
        # all the nodes of the list are bigger than data, add new node to head
        newNode.next = head
        head.prev = newNode
        head = newNode
    else:
        # bigger is the first node that is bigger than data, add new node as its previous node
        previous = bigger.prev
        previous.next = newNode
        newNode.prev = previous
        newNode.next = bigger
        bigger.prev = newNode
    return head
```

# Reverse a doubly linked list

<https://www.hackerrank.com/challenges/reverse-a-doubly-linked-list/problem>

逆转一个双向链表，仅通过改变链表中各节点的next指针和prev指针。

**思路**

跟单链表类似，使用头插法或尾插法实现。不同的地方在于同时要考虑next和prev。

使用头插法取第一个节点时，注意将其next和prev设置为空，否则有可能出现环。

```python
def reverse(head):
    newHead = head
    head = head.next
    newHead.next = None
    while head!=None:
        p = head.next
        newHead.prev = head
        head.next = newHead
        head.prev = None
        newHead = head
        head = p
    return newHead
```