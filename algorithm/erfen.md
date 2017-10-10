# 二分查找

**二分查找（binary search）** 在一种有序数组中查找某一特定元素的搜索算法。搜索过程从数组的中间元素开始，如果中间元素正好是要查找的元素，则搜索过程结束；如果该特定元素大于或小于中间元素，则在数组大于或小于中间元素的那一半中查找，而且跟开始一样，从中间元素开始比较，如果在某一步骤数组为空，则代表找不到，这种搜索算法每次比较都使搜索范围缩小一半。



#### 算法步骤

​	给予一个包含n个带值元素的数组A 使得 a1 <= a2 <= ……<= an, 以及目标值T

	1. 令L = 0， R=n-1。
	2. 如果L > R, 则搜索失败
	3. 令m(中间值元素)为 (L + R)／2
	4. 如果am<T，令L为m+1 并返回到步骤2
	5. 如果am>T，令R为m-1 并返回到步骤2
	6. 如果am=T，搜索结束。

时间复杂度 **O(logn)**

空间复杂度 **O(1)**

```java
//递归版本
int binary_search(const int arr[], int start, int end, int khey) {
  if(start > end)
    return -1;
  
  int mid = start + (end - start)/2; //直接平均可能会溢出，所以用这个算法
  if(arr[mid] > khey)
    return binary_search(arr, start, mid - 1, khey);
  if(arr[mid] < khey)
    return binary_search(arr, mid + 1, end, khey);
  return mid;
}
```



```java
//while 循环
int binary_search(const int arr[], int start, int end, int khey) {
  int mid;
  while(start <= end) {
    mid = start + (end - start) / 2;
    if(arr[mid] < khey) {
      start = mid + 1;
    } else if(arr[mid] > khey) {
      end = mid - 1;
    } else 
      return mid;
  }
  return -1;
}
```

说一个leetcode的问题吧：

在有序矩阵中寻找第K小的元素 （Kth Smallest Element in a sort Matrix）

```
一个n * n的矩阵，每一行每一列都是排好续的，从这个矩阵中找第K小的元素。
例如
matrix = [
[1 , 5 ,  9],
[10, 11, 13],
[12, 13, 15]
],
K = 8,
return 13.
```

这个矩阵有点像最小堆，那就先用堆的方法来解决一下，我们用优先级队列来构建堆。

```java
public class Solution {
  public int kthSmallest(int[][] matrix, int k) {
    int n = matrix.length;
    PriorityQueue<Tuple> pq = new PriorityQueue<Tuple>();
    for(int j = 0; j <= n-1; j++) 
      pq.offer(new Tuple(0, j, matrix[0][j])); 
    for(int i = 0; i < k-1; i++) {
      Tuple t = pq.poll();
      if(t.x == n-1) continue;
      pq.offer(new Tuple(t.x+1, t.y, matrix[t.x+1][t.y]));
    }
  }
}

class Tuple implements Comparable<Tuple> {
  int x, y, val;
  public Tuple (int x, int y, int val) {
    this.x = x;
    this.y = y;
    this.val = val;
  }
  
  @Override
  public int compareTo(Tuple that) {
    return this.val - that.val;
  }
}
```

解法2，二分查找，由于这个矩阵两端排序，因此最小的元素是左上角的元素，最大的元素是右下角的元素。

```java
public class Solution {
  public int kthSmallest(int[][] matrix, int k) {
    int lo = matrix[0][0], hi = matrix[matrix.length -1][matrix[0].length - 1] + 1;
    while(lo < hi) {
      int mid = lo + (hi - lo) / 2;
      int count = 0, j = matrix[0].length - 1;
      for(int i = 0; i < matrix.length; i++) {
        while(j >= 0 && matrix[i][j] > mid) j--;
        count += (j + 1);
      }
      if(count < k) lo = mid + 1
        else hi = mid
    }
    return lo;
  }
}
```