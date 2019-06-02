https://leetcode.com/problems/merge-k-sorted-lists/description/
---

Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

Example:

Input:
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6

---

其实就是归并排序，有两种思路可以实现：递归的二分归并、利用 PriorityQueue 实现

在使用递归二分归并时，归并两个有序链表可以使用递归实现，刚好是递归的思想。

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */


// 使用递归的二分归并
class Solution {
  public ListNode mergeKLists(ListNode[] lists) {
    if (lists == null || lists.length == 0) {
      return null;
    }

    return mergeKHelper(lists, 0, lists.length - 1);
  }

  public ListNode mergeKHelper(ListNode[] lists, int left, int right) {
    if (right == left) {
      return lists[left];
    }
    ListNode listNode1 = mergeKHelper(lists, left, (left + right) / 2);
    ListNode listNode2 = mergeKHelper(lists, (left + right) / 2 + 1, right);
    return mergeTwoList(listNode1, listNode2);
  }

  public ListNode mergeTwoList(ListNode l1, ListNode l2) {
    if (l1 == null) {
      return l2;
    } else if (l2 == null) {
      return l1;
    }
    if (l1.val < l2.val) {
      l1.next = mergeTwoList(l1.next, l2);
      return l1;
    } else {
      l2.next = mergeTwoList(l1, l2.next);
      return l2;
    }
  }
}




// 使用 PriorityQueue
class Solution {
  public ListNode mergeKLists(ListNode[] lists) {
    ListNode res = new ListNode(0);
    ListNode next = res;

    PriorityQueue<ListNode> priorityQueue = new PriorityQueue<>(
            Comparator.comparing(x -> x.val)
    );
    
    for (ListNode list : lists) {
      if (list != null) {
        priorityQueue.add(list);
      }
    }
    
    while (!priorityQueue.isEmpty()) {
      ListNode listNode = priorityQueue.poll();
      next.next = listNode;
      next = next.next;
      listNode = listNode.next;
      if (listNode != null) {
        priorityQueue.add(listNode);
      }
    }
    
    return res.next;
  }
}
```

