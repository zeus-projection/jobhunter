# Fraction to Recurring Decimal

[Fraction to Recurring Decimal](https://leetcode.com/problems/fraction-to-recurring-decimal/#/description)

> 分数转小数（字符串形式），输入 分子 分母，输出 小数字符串

> Given numerator = 1, denominator = 2, return "0.5";
>
> Given numerator = 2, denominator = 1, return "2";
>
> Given numerator = 2, denominator = 3, return "0.(6)"

分步骤处理，先处理整数部分，再处理小数部分，通过map来记录循环。

```java
public String fractionToDecimal(int numerator, int denominator) {
  if(numerator == 0) return "0";
  StringBuilder result = new StringBuilder();
  
  res.append(((numerator >0) ^ (denominator > 0)) ? "-" : "");
  long num = Math.abs((long)numerator);
  long den = Math.abs((long)denominator);
 
  //处理整数部分
  res.append(num / den);
  num %= den;
 
  if(num == 0) {
    return res.toString();
  }
  
  res.append(".");
  
  //处理小数部分
  HashMap<Long, Integer> map = new HashMap<>();
  map.put(num, res.length());
  while(num != 0) {
    num *= 10;
    res.append(num / den);
    num %= den;
    if(map.containsKey(num)) {
      int index = map.get(num);
      res.insert(index, "(");
      res.append(")");
      break;
    } else {
      map.put(num, res.length);
    }
  }
  return res.toString();
}
```