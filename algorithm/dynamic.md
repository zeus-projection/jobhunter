# 动态规划

动态规划问题的3个性质

- 最优化原理：如果问题的最优解包含的子问题的解也是最优的，那这个问题具有最优子结构，即满足最优化原理；
- 无后效性：某阶段的状态一旦确定，就不受这个状态以后的决策的影响；
- 有重叠子问题：即子问题之间不独立，一个子问题在下一阶段决策中可能被多次使用到。



动态规划的过程：

- 划分阶段：按照问题的时间或空间特征，把问题分为若干阶段。在划分时 **划分后的阶段一定要是有序的或者是可排序的**，否则问题就无法求解。
- 确定状态和状态变量：将问题发展到各个阶段时所处于的各种客观情况用不同的状态表示出来。
- 确定决策并写出状态转移方程：因为决策和状态转移有天然的联系，**状态转移就是根据上一阶段的状态和决策来导出本阶段的状态**。  **根据相邻两个阶段的状态之间的关系来确定决策方法和状态转移方程**。
- 寻找边界条件：给出的状态转移方程是一个递推式，需要一个递推的终止条件或边界条件。

相关的算法的基本框架

例子:

斐波那契数列

```javascript
function fib(n)
	if n = 0 or n = 1
    	return n
     return fib(n - 1) + fib(n - 2)
```

优化

```javascript
array map [0 .. n] = { 0-> 0. 1 -> 1}
fib(n)
	if(map m does not contain key n)
      m[n] = fib(n - 1) + fib(n - 2)
    return m[n]

```

**背包问题**

整数背包问题：设有n件物品，每件价值记为Pi，每件体积记为Vi，用一个最大容积为Vmax的背包，求装入物品的最大价值。用一个数组f[i，j]表示取i件物品填充一个容积为j的背包的最大价值，那么这个问题的解就是f[n，Vmax]

```c++
for(int i = 1; i <= n; i++)
  for(j = totv; j>=v[i]; j--)
    f[j] = max(f[j], f[j - v[i]] + pi);
printf("%d", f[totv]);
```

如果限定每种物品只能选择0个或1个，则称为**0-1背包问题**；

如果限定物品j最多只能选择bj个，则问题称为**有界背包问题**；

如果不限定每种物品的数量，则问题称为**无界背包问题**；

**贪心法**

贪心法一般不能得到我们所要求的答案，一旦一个问题能通过贪心法来解决，那么贪心法一般是解决这个问题的最好办法。

- 建立数学模型来描述问题；
- 把求解的问题分成若干子问题；
- 对每一个子问题求解，得到子问题的局部最优解；
- 把子问题的解局部最优解合成原来解问题的一个解；

**最长公共子串和最长公共子序列**

最长公子串—要求子串一定连续

```java
public class LongestCommonSubString {
  public static int compute(char[] str1, char[] str2) {
    int size1 = str1.length;
    int size2 = str2.length;
    
    if(size1 == 0 || size2 == 0) return 0;
    
    int start1 = -1;
    int start2 = -1;
    
    int longest = 0;
    
    for (int i = 0; i < size1; ++i) {
      int m = i;
      int n = 0;
      int length = 0;
      while(m < size1 && n < size2) {
        if(srt1[m] != str2[n]) {
          length = 0;
        } else {
          ++length;
          if(longest < length) {
            longest = length;
            start1 = m - longest + 1;
            start2 = n - longest + 1;
          }
        }
        ++m;
        ++n;
        
      }
    }
    
    for (int j = 1; j < size2; j++) {
      int m = 0;
      int n = j;
      int length = 0;
      
      while(m < size1 && n <size2) {
        if (str1[m] != str2[n]) {
          length = 0;
        } else {
          ++length;
          if(longest < length) {
            longest = length;
            start1 = m - longest + 1;
            start2 = n - longest + 1;
          }
        }
        ++m;
        ++n;
      }
    }
    return longest;
  }
}
```

最长公共子序列问题

```java
//定义d[i][j] = A[0..i] B[0..j]的LCS，那么
0       (i == 0 并且 j==0);
dp[i][j] = dp[i-1][j-1] + 1    (i >0 && j > 0, && A[i] = B[j]);
		= max(dp[i][j-1], dp[i-1][j]) (i >0 && j>0 && A[i] != B[j]);

public class LongestCommonSubsequence {
  public static int compute(char[] str1, char[] str2) {
    int subStringLength1 = str1.length;
    int subStringLength1 = str2.length;
    
    int[][] opt = new int[subStringLength1 + 1][subStringLength2 + 1];
    
    for(int i = subStringLength1 -1; i >= 0; i--) {
      for (int j = subStringLength2 -1; j >= 0; j--) {
        if(str1[i] == str2[j]) {
          opt[i][j] = opt[i+1][j+1] +1;
        } else {
          opt[i][j] = Math.max(opt[i+1][j], opt[i][j+1]);
        }
      }
    }
    
    int i = 0, j = 0;
    
    while(i < subStringLength1 && j < subStringLength2) {
      if(str1[i] == str2[j]) {
        i++;
        j++
      }
      else if(opt[i + 1][j] >= opt[i][j + 1])
        i++;
      else 
        j++;
        
    }
    return opt[0][0];
    
  }
}
```