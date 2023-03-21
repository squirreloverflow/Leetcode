链表：
链表是一种线性数据结构，其中每个元素是一个独立的对象，包含两个部分：数据和对下一个元素的引用.
在C++中，链表可以使用结构体和指针实现：
```c++
struct ListNode {
    int val;
    ListNode* next;
    ListNode(int val, ListNode* next) : val(val), next(next) {}
};

ListNode* head = new ListNode();

```
在Python中，链表可以用类表示：
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

head = ListNode()
```

链表是LeetCode上常见的数据结构，以下是一些常见的链表题目：

1.反转链表（Reverse Linked List）：将一个单链表按照逆序重新排列。例如，给定一个链表1->2->3->4->5，将其翻转为5->4->3->2->1。

基本思路：
这个问题的基本思路是从头到尾遍历链表，并将每个节点的next指针指向其前一个节点。为了实现这个操作，我们需要使用三个指针：prev，curr和next。prev指向前一个节点，curr指向当前节点，next指向下一个节点。在每次迭代中，我们将curr的next指针指向prev，然后将prev、curr和next分别向后移动一个位置。最终，prev将指向反转链表的头节点。
c++:
```c++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* prev = nullptr;
        ListNode* curr = head;
        while (curr != nullptr) {
            ListNode* next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }
};
```
python:
```python
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        prev = None
        curr = head
        while curr:
            next_node = curr.next
            curr.next = prev
            prev = curr
            curr = next_node
        return prev
```

2.链表的中间结点（Middle of the Linked List）：给定一个单链表，返回其中间位置的结点。例如，给定链表1->2->3->4->5，返回结点3。

基本思路：这个问题的基本思路是使用两个指针slow和fast，其中slow指向链表的中间节点，fast指向链表的尾部节点。在每次迭代中，slow向前移动一个节点，fast向前移动两个节点，直到fast指向链表的尾部节点为止。当fast到达链表尾部时，slow将指向链表的中间节点。

c++:
```c++
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast != nullptr && fast->next != nullptr) {
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }
};

```
python:
```python
class Solution:
    def middleNode(self, head: ListNode) -> ListNode:
        slow = head
        fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        return slow
```

3.删除链表的倒数第N个结点（Remove Nth Node From End of List）：给定一个单链表和一个整数n，删除该链表中倒数第n个结点。例如，给定链表1->2->3->4->5和n=2，删除倒数第二个结点4，得到链表1->2->3->5。

基本思路：本题使用双指针，设两个指针 fast 和 slow。fast 指针先向前移动 n 步，然后 fast 和 slow 指针同时向前移动，直到 fast 指向链表末尾时停止。此时，slow 指针指向的就是倒数第 n 个节点的前一个节点，将其 next 指针指向下下个节点即可删除倒数第 n 个节点。

注意，在实现时需要创建一个虚拟头节点，以处理删除链表头部节点的情况。

```c++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode(0);
        dummy->next = head;
        ListNode* slow = dummy;
        ListNode* fast = dummy;
        for (int i = 0; i <= n; i++) {
            fast = fast->next;
        }
        while (fast != nullptr) {
            slow = slow->next;
            fast = fast->next;
        }
        slow->next = slow->next->next;
        return dummy->next;
    }
};
```

4.合并两个有序链表（Merge Two Sorted Lists）：将两个按照升序排列的单链表合并成一个按照升序排列的单链表。例如，给定链表1->3->5和链表2->4->6，合并后得到链表1->2->3->4->5->6。

5.删除链表中的重复元素（Remove Duplicates from Sorted List）：给定一个按照升序排列的单链表，删除其中所有重复的元素，只留下原链表中没有重复出现的元素。例如，给定链表1->1->2->3->3，删除重复元素后得到链表1->2->3。


