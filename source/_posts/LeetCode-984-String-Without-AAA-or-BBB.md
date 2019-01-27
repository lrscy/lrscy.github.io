---
title: LeetCode 984 String Without AAA or BBB
date: 2019-01-27 15:18:19
tags: [LeetCode, Solution]
categories: [LeetCode]

---

# Code

The solution of [problem 984](https://leetcode.com/problems/string-without-aaa-or-bbb/) is showed below. You can also find it [here](https://github.com/lrscy/LeetCode/blob/master/Algorithm/984-String%20Without%20AAA%20or%20BBB.cpp).

```
class Solution {
public:
    string strWithout3a3b(int A, int B) {
        string s = "";
        int a = 0, b = 0;
        while( A || B ) {
            int ta = 0, tb = 0;
            while( ta < 2 && B <= 2 * A && A > 0 ) { s += "a"; ++ta; --A; }
            while( tb < 2 && A <= 2 * B && B > 0 ) { s += "b"; ++tb; --B; }
        }
        return s;
    }
};
```

# Solution

The core method of the problem is to make sure that the number of each character is no less than half of another character and no exceed two consecutively.

# Valuable Method

I've tried to find pattern at first time, such as "aab", "abb", and etc. Finally, I find that if we want to prevent those "aaa" and "bbb" appear, we just need to keep the ratio of number of two characters. It's easier to apply a simple rule rather than setting many patterns and it looks clean and beautiful.
