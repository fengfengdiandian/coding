https://leetcode.com/problems/longest-substring-without-repeating-characters/description/
---

Given a string, find the length of the longest substring without repeating characters.

Example 1:

Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3. 
Example 2:

Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
Example 3:

Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.

---

注意到是求子串，非子序列

似乎可以用动归实现。
dp 的定义为以该点为右界，最长不重复子串的长度。

当charAt(i)这个字符不重复时，dp[i]=dp[i-1] + 1，否则dp[i] = 1。这个用dp思想的理解就是，dp[i]可能的情况有两种。

那现在的问题就转变为如果判断字符是否重复了。因为是字符，所以我们可以用数组来判断是否出现过，同时要记录左界的值。

有一个小技巧，记录是否出现过的数组可以记录该字符出现的下标，而不是简单的1，通过判断值是否大于左界去判断是否重复。


```
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        
        int start = 0;
        int lenMax = 1;
        
        int[] exist = new int[130];
        Arrays.fill(exist, -1);
        
        exist[s.charAt(0)] = 0;
        
        for (int i = 1; i < s.length(); i++) {
            char c = s.charAt(i);
            if (exist[c] >= start) {
                lenMax = Math.max(lenMax, i - start);
                start = exist[c] + 1;
            }
            exist[c] = i;
        }
        
        lenMax = Math.max(lenMax, s.length() - start); 
        return lenMax;
    }
}
```
