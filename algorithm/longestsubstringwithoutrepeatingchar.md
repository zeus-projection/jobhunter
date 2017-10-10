# Longest Substring Without Repeating Characters

[Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/description/)

> 给定一个字符串，找到 **没有重复字符的最长子字符串的大小**。
>
> 比如：
>
> Given `"abcabcbb"`, the answer is `"abc"`, which the length is 3.
>
> Given `"bbbbb"`, the answer is `"b"`, with the length of 1.
>
> Given `"pwwkew"`, the answer is `"wke"`, with the length of 3.
>
> 



双指针配合set，快指针用来判断最长字符串的末尾，慢指针用来找到重复的字符串，并且丢掉重复字符串前面的所有字符。

```java
public int lengthOfLongestSubstring(String s) {
  int i = 0, j = 0, max = 0;
  Set<Character> set = new HashSet<>();
  while (j < s.length()) {
    if(!set.contains(s.charAt(j))) {
      set.add(s.charAt(j++));
      max = Math.max(max, set.size());
    } else {
      set.remove(s.charAt(i++));
    }
  }
  return max;
}
```

优化，通过一个map来记录慢指针最后出现的位置，这样子，就不用一个一个的淘汰了。直接把重复的字符前面的全部扔掉了，两套代码如下

```java
public int lengthOfLongestSubstring(String s) {
  int n = s.length(), ans = 0;
  Map<Charater, Integer> map = new HashMap<>();
  for(int j = 0, i = 0; j < n; j++) {
    if(map.containsKey(s.charAt(j))) {
      i = Math.max(map.get(s.charAt(j)), i);
    }
    ans = Math.max(ans. j - i + 1);
    map.put(s.charAt(j), j + 1); //更新位置;
  }
  return ans;
}
```

```java
public int lengthOfLongestSubstring(String s) {
  int n = s.length(), ans = 0;
  int[]  index = new int[256];
  for(int j = 0, i = 0; j < n; j++) {
    i = Math.max(index[s.charAt(j)], i);
    ans = Math.max(ans, j - i + 1);
    index[s.charAt(j)] = j + 1;
  }
  return ans;
}
```