https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/
---
Given an array of integers where 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once.

Find all the elements of [1, n] inclusive that do not appear in this array.

Could you do it without extra space and in O(n) runtime? You may assume the returned list does not count as extra space.

Example:

Input:
[4,3,2,7,8,2,3,1]

Output:
[5,6]
---
刚开始看的时候，差点掉坑里了，因为之前做过一个看起来类似的题目：有一个数只出现一次，剩余全部出现过了两次，求那个只出现了一次的数。

所以我一开始全部是往异或运算的思路思考。

再看一下题目，里面的值只能是 1 - n，这是个关键。所以可以用原来的数组去表示是否出现过。（这个题的关键就在于去标记出现过的数，剩下的就是没出现过的数）

现在的思路就是如何去标记出现过的数。

我想到的是，把 nums[i] 换到 nums[i] - 1 的位置上（因为数组下标从 0 开始），这样把每个数换好之后，再遍历一边去查看 nums[i] == i + 1，如果不等就是没出现过的。需要注意的点就是如果判断这个位置上的数已经换好了。
- 要么换了之后 num[i] == i + 1
- 要么换之前的数等于换之后的数

```
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        int lastNum = -1;
        for (int i = 0; i < nums.length; i++) {
            while (lastNum != nums[i] && nums[i] != i + 1) {
                lastNum = nums[i];
                nums[i] = nums[lastNum - 1];
                nums[lastNum - 1] = lastNum;
            }
        }
        
        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != i + 1) {
                list.add(i + 1);
            }
        }
        return list;
    }
}
```

还有一个思路是，注意到数组里的所有数都是正的，所以可以用负号去表示是否出现过，这个更直观，更简单。

