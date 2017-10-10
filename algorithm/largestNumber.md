# Largest number

[Largest number](https://leetcode.com/problems/largest-number/description/)

> 一个不包含在负数的数组，找到他们排列组合出来的最大的数字。
>
> 比如  `[3, 30, 34, 5, 9]`, 最大的数字组合是 `9534330`.

**排序**

```java
public String largestNumber(int[] nums) {
  if(nums == null || nums.length == 0) {
    return "";
  }
  String sNums = new String[nums.length];
  for(int i = 0; i < nums.length; i++) {
    sNums[i] = String.valueOf(nums[i]);
  }
  
  Comparator<String> comp = new Comparator<String>() {
    @Override
    public int compare(String s, String t1) {
      String s1 = s + t1;
      String s2 = t1 + s;
      return s2.compareTo(s1);
    }
  };
  
  Arrays.sort(sNums, comp);
  
  if(sNums[0].charAt(0) == '0') return "0";
  
  StringBuilder result = new StringBuilder)0;
  for(String s : sNums) {
    sb. append(s);
  }
  return sb.toString();
  
}
```



**自定义排序**

```java
public String largestNumber(int[] nums) {
  int n;
  for(n = 0; n < nums.length; n++) {
    if(nums[n] != 0)
      break;
  }
  if( n == nums.length) return new String("0");
  
  mergesort(nums);
  StringBuilder reuslt = new StringBuilder();
  for(int i = 0; i < nums.length; i++) {
    result.append(nums[i]);
  }
  return result.toString();
}

private void mergesort(int[] A) {
  int n = A.length;
  if(n < 2) return;
  int mid = n / 2, i;
  int[] l = new int[mid], r = new int[n - mid];
  for(i = 0; i < mid; i++) {
    l[i] = A[i];
  }
  for(int j = 0; j<r.length;j++) {
    r[j] = A[i++];
  }
  mergesort(l);
  mergesort(r);
  merge(l, r, A);
}

private void merge(int[] l, int[] r, int[] A) {
  int x = l.length, y = r.length, i = 0, j = 0, k = 0;
  while(i < x && j < y) {
    if(compare(l[i], r[j])) 
      A[k++] = r[j++];
    else 
      A[k++] = l[i++];
  }
  while(j < y)
    A[k++] = r[j++];
  while(i < x) 
    A[k++] = l[i++];
}

public boolean compare(int a, int b) {
  if(a == 0 && b != 0)
    return true;
  else if(a != 0 && b == 0)
    return false;
  int i = (int) Math.log10(a) + 1, j = (int)Math.log10(b) + 1;
  if(i == j && a < b) return true;
  else if (i == j && a > b) return false;
  else 
    return (a*Math.pow(10, j) + b) < (b * Math.pow(10, i) + a);
}
```