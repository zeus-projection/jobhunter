# 质数相关

- 判断一个数是否是质数；
- 各一个自然数N，打印出所有小于N的质数；
- 给一个自然数N，打印出前N个质数；

质数的几个特点：除了2之外的质数一定是奇数，因为因数都是成对出现的 比如 

100 可以拆解为 1 * 100， 2 * 50，4 * 25， 5 * 20， 10 * 10；

```java
boolean isPrime(int n) {
  if(n < 2) return false;
  if(n == 2) return true;
  for (int i = 3; i * i < n; i += 2) {
    if (n % i == 0) return false;  
  }
  return true;
}
```

筛除法:

因为2是质数，因此可以把2的倍数全部去掉；

因为3是质数，因此可以把3的倍数全部去掉；

继续最小的数是5，把5的倍数全部去掉，因此类推再把7的倍数全部去掉，这样不断筛除，最后剩下的全是素数；

```python
def eliminate(N):
    flag = []
    result = []
    for i in range(0, N+1):
        flag.append(True)
    exceed = int(math.sqrt(N))
    for i in range(2, exceed):
        if(flag[i] == True):
            for j in range(i+i, N+1, i):
                flag[j] = False
    
    for i in range(2, N+1):
        if(flag[i] == True):
            result.append(i)
    print result
```