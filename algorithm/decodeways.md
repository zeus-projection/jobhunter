# Decode ways

[Decode ways](https://leetcode.com/problems/decode-ways/description/)

>一条消息（message）按照下面的映射关系，采用数字的方式进行编码
>
>```
>'A' -> 1
>'B' -> 2
>...
>'Z' -> 26
>
>```
>
>给出一条用数字编码过的消息，判断有多少种方式对他进行解码。
>
>比如：
>
>
>`"12"`, 有两种解码方案   `"AB"` (1 2) or  `"L"` (12).
>
> `"12"` 的解码方案有`"2"` 种.



**DP的解决方案，从后往前**

```java
public int numDecodings(String s) {
  int n = s.length;
  if(n== 0) return 0;
  
  int memo = new int[n+1];
  memo[n] = 1;
  memo[n - 1] = s.charAt(n - 1) != '0' ? 1 : 0;
  
  for (int i = n -2; i >= 0; i--) {
    if(s.charAt(i) == '0') continue;
    else memo[i] =
      (Integer.parseInt(s.substring(i, i + 2)) <= 26) ? memo[i+1] + memo[i+2] : memo[i + 1];    
  }
  return memo[0];
}
```

**从前往后的DP**

```java
public int numDecodings(String s) {
  if(s == null || s.length() ==0) {
    return 0;
  }
  int n = s.length();
  int[] dp = new int[n+ 1];
  dp[0] = 1;
  dp[1] = s.charAt(0) != '0' ? 1 : 0;
  for (int i = 2; i <= n; i++) {
    int first = Integer.valueOf(s.substring(i-1, i));
    int second = Integer.valueOf(s.substring(i -2, i));
    if(first >= 1 && first <= 9) {
      dp[i] += dp[i-1];
    }
    if(second >= 10 && second <= 26) {
      dp[i] += dp[i-2];
    }
  }
  return dp[n];
}
```

**Fibonacci**

```java
public int numDecoding(String s) {
  if(s == null || s.length()<1 || s.charAt(0) == '0') {
    return 0;
  }
  int a, b, c;
  a = 1;
  b = 1;
  for (int i = 1; i< s.length(); i++) {
    if(s.charAt(i) == '0') {
      if(s.charAt(i-1) > '3') {
        return 0;
      }
      b = 0;
    }
    if(s.charAt(i -1) > '2' || (s.charAt(i-1) == '2' && s.charAt(i) > 6)
       || s.charAt(i - 1) == '0') {
         a = 0;
       }
    c = a + b;
    a = b;
    b = c;
  }
  return b;
}
```