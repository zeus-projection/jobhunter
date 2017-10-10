# 3Sum

[3Sum](https://leetcode.com/problems/3sum/description/)

>Given an array *S* of *n* integers, are there elements *a*, *b*, *c* in *S* such that *a* + *b* + *c* = 0? Find all unique triplets in the array which gives the sum of zero.
>
>给定的一个有n个整数的数据S，判断S中是否包含这样的元素 a,b,c a + b + c = 0，找到所有这样的唯一的组合。
>
>**注意**：结果集中不包含重复的。



>```
>For example, given array S = [-1, 0, 1, 2, -1, -4],
>A solution set is:
>[
>  [-1, 0, 1],
>  [-1, -1, 2]
>]
>```
>

```java

public List<List<Integer>> threeSum(int[] nums) {
     Arrays.sort(nums);
        List<List<Integer>> res = new LinkedList<>();
        for (int i = 0; i < nums.length -2; i++) {
            if(i == 0 || (i > 0 && nums[i] != nums[i - 1]) ) {
                int lo = i + 1, hi = nums.length - 1, sum = 0 - nums[i];
                while(lo < hi) {
                    if(nums[lo] + nums[hi] == sum) {
                        res.add(Arrays.asList(nums[i], nums[lo], nums[hi]));
                        while((lo < hi) && nums[lo] == nums[lo + 1]) lo++;
                        while((lo < hi) && nums[hi] == nums[hi - 1]) hi--;
                        lo++; hi--;
                    } else if (nums[lo] + nums[hi] < sum){
                        lo++;
                    } else hi--;

                }
            }

        }
        return res; 
}
```