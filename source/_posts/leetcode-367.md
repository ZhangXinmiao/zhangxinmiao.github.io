---
title: leetcode 367. Valid Perfect Square
date: 2017-06-21 10:17:31
tags: leetcode
---

#### Question

Given a positive integer num, write a function which returns True if num is a perfect square else False.

**Note**: Do not use any built-in library function such as sqrt.

#### Example

Example 1:

```
Input: 16
Returns: True
Example 2:
```
```
Input: 14
Returns: False
```

#### Tip

判断输入数字是否是完全平方数

不停的对数字进行除以2的处理，判断结果的平方与输入数字的大小，如果大，继续除以2，否则，就在当前数和上次的数之间寻找是否有平方恰好等于输入数的。

#### Code

```C++
class Solution {
public:
    bool isPerfectSquare(int num) {
        if (num == 1) return true;
        long x = num / 2, t = x * x;
        while (t > num) {
            x /= 2;
            t = x * x;
        }
        for (int i = x; i <= 2 * x; ++i) {
            if (i * i == num) return true;
        }
        return false;
    }
}; 
```
