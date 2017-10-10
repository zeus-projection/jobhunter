# 约瑟夫环

n个人 围成一圈，然后依次报数，第m个人自杀，然后从下个人开始继续报数，第m个人自杀，最后谁会剩下？

简单解析：n个人死掉一个 变成 n-1个人，并且编号往后移动了m的游戏重新开始，因此可以得到

f(n) = (f(n - 1) + m) mod n

```python
def josephus(n, m):
    s = 0;
    for i in range(2, n + 1):
        s = (s + m) % i
    print s
```