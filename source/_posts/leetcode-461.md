---
title: leetcode 461. Hamming Distance
date: 2017-06-20 19:05:14
tags: leetcode
---

#### Question

The Hamming distance between two integers is the number of positions at which the corresponding bits are different.

Given two integers `x` and `y`, calculate the Hamming distance.

#### Example

```
Input: x = 1, y = 4

Output: 2

Explanation:
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑

The above arrows point to positions where the corresponding bits are different.
```

#### Tip

位运算问题，对输入的两个数字做按位异或`^`操作，然后再利用位运算符`>>`计算结果中有多少个数字1出现

#### Code

```C++
class Solution {
public:
    int hammingDistance(int x, int y) {
        int z = x ^ y;
        int result = 0;
        for(int i = 0; i < 32; i++) {
            result += 1 & (z >> i);
        }
        return result;
    }
};
```
