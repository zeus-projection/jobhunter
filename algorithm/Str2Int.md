# String to Intger

[string to integer](https://leetcode.com/problems/string-to-integer-atoi/#/description)

> atoi函数，把一个字符串转成整数。

关键点：处理空格，确定正数还是负数，整型的最大和最小值 INT_MAX(2147483647) INT_MIN(-2147483648)

```java
public int myAtoi(String str) {
  int index = 0, sign = 1, total = 0;
  
  if(str.length() == 0) return 0;
  //处理掉空格
  while(str.charAt(index) == ' ' && index < str.length()) {
    index++;
  }
  
  //处理正负值
  if(str.charAt(index) == '+' || str.charAt(index) == '-') {
    sign = str.charAt(index) == '+' ? 1 : -1;
    index++;
  }
  
  while(index < str.length()) {
    int digit = str.charAt(index) - '0';
    if(digit < 0 || digit > 9) break;
    //处理溢出
    if(Integer.MAX_VALUE /10 <  total || Integer.MAX_VALUE/10 == total && Integer.MAX_VALUE % 10 < digit) {
     return sign == 1 ? Integer.MAX_VALUE : Integer.MIN_VALUE;
     
    }
    total = 10 * total + digit;
    index++;
  }
  return total * sign;
}
```