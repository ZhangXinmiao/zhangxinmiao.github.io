---
title: leetcode 118. Pascal's Triangle
date: 2017-06-20 20:48:09
tags: leetcode
---

#### Question

Given numRows, generate the first numRows of Pascal's triangle.

For example, given numRows = 5,

return

```
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]

```

#### Tip

array

#### Code

```C++
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        vector<vector<int>> result;
        vector<int> tmp;
        for(int i = 0; i < numRows; i++) {
            tmp.clear();
            if(i == 0) {
                tmp.push_back(1);
            }else {
                for(int j = 0; j < i + 1; j++) {
                    if(j == 0 || j == i) {
                        tmp.push_back(1);
                    } else {
                        tmp.push_back(result[i - 1][j - 1] + result[i - 1][j]);
                    }
                }
            }
            result.push_back(tmp);
        }
        return result;
    }
};
```
