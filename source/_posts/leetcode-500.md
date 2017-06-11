---
title: leetcode 500. Keyboard Row
date: 2017-06-11 23:18:39
tags: leetcode
---

#### Question

Given a List of words, return the words that can be typed using letters of alphabet on only one row's of American keyboard like the image below.
(就不放图了 这里假装有一张键盘的图片orz)

#### Example

```
Input: ["Hello", "Alaska", "Dad", "Peace"]
Output: ["Alaska", "Dad"]
```

#### Tip

用了一种罪常规的解法a的，遍历字符串

#### Code

```C++
class Solution {
public:
   vector<string> findWords(vector<string>& words) {
       string s1 = "qwertyuiopQWERTYUIOP";
       string s2 = "asdfghjklASDFGHJKL";
       string s3 = "zxcvbnmZXCVBNM";

       vector<string> result;
       for(int i = 0; i < words.size(); i++) {
           if(isSubstr(s1, words[i]) || isSubstr(s2, words[i]) || isSubstr(s3, words[i])) {
               result.push_back(words[i]);
           }
       }

       return result;
   }

   bool isSubstr(string totalStr, string target) {
       bool result = true;
       for(int i = 0; i < target.length(); i++) {
           result = result && isInStr(totalStr, target[i]);
       }
       return result;
   }

   bool isInStr(string totalStr, char target) {
       bool result = false;
       for(int i = 0; i < totalStr.length(); i++) {
           if(target == totalStr[i]) {
               result = true;
           }
       }
       return result;
   }
};

```

还有另一种操作，感觉实现的不好，但是就当练习hashtable了

```C++
class Solution {
public:
    vector<string> findWords(vector<string>& words) {
        string s1 = "qwertyuiopQWERTYUIOP";
        string s2 = "asdfghjklASDFGHJKL";
        string s3 = "zxcvbnmZXCVBNM";
        bool hashTable[3][256] = {false};
        for(int i = 0; i < s1.length(); i++) {
            hashTable[0][s1[i]] = true;
        }
        for(int i = 0; i < s2.length(); i++) {
            hashTable[1][s2[i]] = true;
        }
        for(int i = 0; i < s3.length(); i++) {
            hashTable[2][s3[i]] = true;
        }
        vector<string> result;
        for(int i = 0; i < words.size(); i++) {
            string curruntStr = words[i];
            int result0 = true, result1 = true, result2 = true;
            for(int j = 0; j < curruntStr.length(); j++) {
                result0 = result0 && hashTable[0][curruntStr[j]];
            }
            for(int j = 0; j < curruntStr.length(); j++) {
                result1 = result1 && hashTable[1][curruntStr[j]];
            }
            for(int j = 0; j < curruntStr.length(); j++) {
                result2 = result2 && hashTable[2][curruntStr[j]];
            }

            if(result0 || result1 || result2) {
                result.push_back(curruntStr);
            }

        }

        return result;
    }
};
```
