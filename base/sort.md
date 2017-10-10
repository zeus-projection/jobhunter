# 排序的一些小问题

##### 快排和堆排的区别，为什么快排比堆排快


1. 堆排还是稳定，不会恶化；

2. 快排的比较次数少于堆排，差不多是堆排的一半；常数项太高。堆排每次调整的时候，都是从最下面那一个元素，这个元素肯定会下沉很深，上面会有很多无意义的比较，因为最后的元素，一定比上面的元素小。

3. 堆排cache不友好了，每次比较都是跳跃到2i元素，相邻元素没法缓存到cache里面。

   ​

##### 归并排序和快排的区别，应用场景

外部排序，主要是切换成子问题，处理内存放不下的问题。

##### 基数排序

效率很高  logb（N） * n   **O(k * n)**

```python
import math
def sort(a, radix = 10):
    K = int(math.ceil(math.log(max(a), radix)))
    bucket = [[] for i in range(radix)]
    
    for i in range(1, K + 1):
        for val in a:
            bucket[val%(radix ** i)/ (radix ** (i - 1))].append(val)
           del a[:]
        for each in bucket:
            a.extend(each)    
```



##### topn个数字

从大量数字有限内存中取出最小的前n个：

建立一个n个节点的大根堆，然后，剩下的元素跟根比较，如果元素比n小，那么就扔到根元素，把这个元素加进来，重新建堆。