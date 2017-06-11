---
title: leetcode 561. Array Partition I
date: 2017-06-11 23:09:02
tags: leetcode
---

#### Question

Given an array of 2n integers, your task is to group these integers into n pairs of integer, say (a1, b1), (a2, b2), ..., (an, bn) which makes sum of min(ai, bi) for all i from 1 to n as large as possible.

#### Example

```
Input: [1,4,3,2]

Output: 4
Explanation: n is 2, and the maximum sum of pairs is 4 = min(1, 2) + min(3, 4).
```

#### Tip

输入一个数组，对数组中元素进行两两分组，使得每一组最小值相加得到的结果尽可能大,最后给出最大值。

解题思路是现将数组进行排序，再进行两两组合的结果便是最优解

#### Code

```C++
class Solution {
public:
    int arrayPairSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int len = nums.size();
        int result = 0;
        for(int i = 0; i < len; i += 2) {
            result += nums[i];
        }

        return result;
    }
};
```
