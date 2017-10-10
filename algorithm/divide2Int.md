# Divide Two Integers

[Divide Two Integers](https://leetcode.com/problems/divide-two-integers/#/description)

> 不用传统的乘法，除法，取余操作，进行两个数的除法，如果溢出返回 MAX_INT

除法的本质是减法。

比如15 和 3，首先 15 - 3 = 12 这时候3 左移一位 变成6 15 -6 = 9， 然后 6 再移位 12 15 - 12 = 3，这时候 12再移位，就变成了24，这样子就比15大了，所以，15 -12 然后，移位2次，得出结果4. 然后剩余的3，再通过减法，把剩余的位不上，得到结果5，直接上代码比较好理解，一定要关注正负数问题。

```java
public int divide(int dividend, int divisor) {
  if(divisor == 0 || (dividend == Integer.MIN_VALUE && divisor == -1)) return Integer.MAX_VALUE;
  if((dividend == Integer.MIN_VALUE && divisor == 1)) return Integer.MIN_VALUE;
  
  if(dividend == 0) return 0;
  
  int sign = ((dividend < 0) ^ (divisor < 0)) ? -1 : 1;
  long dividendl = Math.abs((long)dividend);
  long divisorl = Math.abs((long)divisor);
  
  int res = 0;
  while(dividendl >= divisorl) {
    long temp = divisorl, multiple = 1;
    while(dividendl >= (temp << 1)) {
      temp <<=1;
      multiple <<=1;
    }
    dividendl -= temp;
    res += multiple;
  }
  
  return sign == 1 ? res : -res;
}
```