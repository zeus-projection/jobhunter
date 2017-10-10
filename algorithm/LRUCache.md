# LRUCache

最近最久未使用，**如果一个数据在最近一段时间没有被访问，那么在将来它被访问的可能性很小。**

比较好的解决方案：**双链表 + hashtable**

当一个位置被命中，通过调整链表指向，将该位置调整到链表头位置，新加入的直接放到链表头位置，这样，最近命中的，向链表头移动，没有命中的向链表后移动。

```java
public class LRUCache {
  private int cacheSize;
  private Hashtable<Object, Entry> nodes;
  private int currentSize;
  private Entry first;
  private Entry last;
  
  public LRUCache(int i) {
    currentSize = 0;
    cacheSize = i;
    nodes = new Hashtable<Object, Entry>(i);
  }
  
  public Entry get(Object key) {
    Entry node = nodes.get(key);
    if(node != null) {
      moveToHead(node);
      return node;
    } else {
      return null;
    }
  }
  
  public void put(Object key, Object value) {
    Entry node = nodes.get(key);
    
    if(node == null) {
      if(currentSize >= cacheSize) {
        node.remove(last.key);
        removeLast();
      } else {
        currentSize++;
      }
      node = new Entry();
    }
    
    node.value = value;
    moveToHead(node);
    nodes.put(key, node);
  }
  
  public void remove(Object key) {
    Entry node = nodes.get(key);
    
    if(node != null) {
      if(node.prev != null) {
        node.prev.next = node.next;
      } 
      if (node.next != null) {
        node.next.prev = node.prev;
      }
      if(last == node) {
        last == node.prev;
      }
      if(first == node) {
        first = node.next;
      }
    }
    nodes.remove(key);
  }
  
  private void removeLast() {
    if(last != null) {
      if(last.prev != null)
        last.prev.next = null;
      else 
        first = null;
      last = last.prev;
    }
  }
  
  private void moveToHead(Entry node) {
    if(node == first)
      return;
    if(node.prev != null) 
      node.prev.next = node.next;
    if(node.next != null) 
      node.next.prev = node.prev;
    if(last == node) 
      last = node.prev;
    
    if(first != null) {
      node.next = first;
      first.prev = node;
    }
    first = node;
    node.prev = null;
    if(last == null) 
      last = first;
  }
  
  
  class Entry {
    Entry prev;
    Entry next;
    Object value;
    Object key;
  }
}
```