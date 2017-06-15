---
title: leetcode-3. Longest Substring Without Repeating Characters
date: 2017-06-15 19:21:52
tags: leetcode
---


#### Question

Given a string, find the length of the longest substring without repeating characters.

#### Example

Given `abcabcbb`, the answer is `abc`, which the length is 3.

Given `bbbbb`, the answer is `b`, with the length of 1.

Given `pwwkew`, the answer is `wke`, with the length of 3. Note that the answer must be a substring, `pwke` is a subsequence and not a substring.

#### Tip

找到给定字符串的最长的无重复子字符串

令 i 和 j 都指向字符串第一位，理想的情况下 j 会一直向后移，这种情况下最长子串长度就是该字符串长度；每次记录下字符串长度，并且，当发现从某个字符开始重复的时候可以直接将 i 移动到这一位。

可利用哈希表来确定是否有重复的字符

#### Code

```C++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int n = s.length();
        int i = 0, j = 0;
        int maxLen = 0;
        bool exist[256] = { false };
        while (j < n) {
            if (exist[s[j]]) {
                maxLen = max(maxLen, j-i);
                while (s[i] != s[j]) {
                    exist[s[i]] = false;
                    i++;
                }
                i++;
                j++;
            } else {
                exist[s[j]] = true;  
                j++;  
            }  
        }  
        maxLen = max(maxLen, n-i);  
        return maxLen;  
    }
};
```
