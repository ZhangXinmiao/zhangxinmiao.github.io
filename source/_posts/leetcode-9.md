---
title: leetcode-9. Palindrome Number
date: 2017-06-15 19:16:46
tags: leetcode
---

#### Question

Determine whether an integer is a palindrome. Do this without extra space.

#### Example

NULL

#### Tip

输入一个数，判断其是否为回文数

将数字“反过来”与原值进行比较

考虑负数的情况，负数不算回文数

#### Code

```C++
class Solution {
public:
    bool isPalindrome(int x) {
        long long init = x;
        long long result = 0;
        while(x) {
            result = result * 10 + x % 10;
            x /= 10;
        }

        return init == result && init >= 0 ? true : false;
    }
};
```
