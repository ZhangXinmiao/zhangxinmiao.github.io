---
title: leetcode 575. Distribute Candies
date: 2017-06-16 09:32:20
tags: leetcode
---

#### Question

Given an integer array with even length, where different numbers in this array represent different kinds of candies. Each number means one candy of the corresponding kind. You need to distribute these candies equally in number to brother and sister. Return the maximum number of kinds of candies the sister could gain.

#### Example

Input: candies = [1,1,2,2,3,3]
Output: 3
Explanation:
There are three different kinds of candies (1, 2 and 3), and two candies for each kind.
Optimal distribution: The sister has candies [1,2,3] and the brother has candies [1,2,3], too.
The sister has three different kinds of candies.

#### Tip

一个数组分成两份，某份数字种类最多。

理想情况下分出的某一部分每个数字都不重复。

这样问题就转化成找数组中不重复的个数，若这个数字大于数组长度 / 2,那么最后所求结果应该是数组长度 / 2.

#### Code

```C++
class Solution {
public:
    int distributeCandies(vector<int>& candies) {
        int result = 1;
        sort(candies.begin(), candies.end());
        for(int i = 0; i <candies.size() - 1; i++) {
            if(candies[i] != candies[i + 1]) {
                result ++;
            }
        }
        result = result > candies.size() / 2 ? candies.size() / 2 : result;
        return result;
    }
};
```
