# K个列表融合

K个有序列表融合成一个有序列表

```java
public class ListNode {
  int val;
  ListNode next;
  ListNode(int x) {val = x;}
}
```

merage sort

```java
ListNode dummy = new ListNode(0);
public ListNode mergeKList(ListNode[] lists) {
  if(lists.length == 0) return null;
  int i = 0;
  int j = lists.length -1;
  while(j != 0) {
    while(i < j){
      lsit[i] = mergeTwo(lists[i++], lists[j--]);
    }
    i = 0;
  }
  return lists[0];
}

public ListNode merageTwo(ListNode node1, ListNode node2) {
  ListNode head = dummy;
  while (node1 != null && node2 != null) {
    if(node1.val < node2.val) {
      head.next = node1;
      node1 = node1.next
    } else {
      head.next = node2;
      node2 = node2.next;
    }
    head = head.next;
  }
  if(node1 != null) {
    head.next = node1;
  }
  if(node2 != null) {
    head.next = node2;
  }
  return dummy.next;
}
```

堆排序（PriorityQueue）

```java
public ListNode mergeKLists(ListNode[] lists) {
  PriorityQueue<ListNode> heap = new PriorityQueue<>(new Comparator<ListNode>() {
    @Override
    public int compare(ListNode o1, ListNode o2) {
      return o1.val - o2.val;
    }
  });
  for(ListNode n : lsits) {
    if(n != null) {
      heap.add(n);
    }
  }
  
  ListNode head = heap.peek();
  
  while(!heap.isEmpty()) {
    ListNode min = heap.poll();
    if (min.next != null) {
      heap.add(min.next);
    }
    min.next = heap.peek();
  }
  return head;
}
```