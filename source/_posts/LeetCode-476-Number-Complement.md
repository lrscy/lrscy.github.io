---
title: LeetCode 476 Number Complement
date: 2020-05-04 11:39:08
tags: [LeetCode, Solution]
categories: [Leetcode]

---

# Code

Solution of [problem 476](https://leetcode.com/problems/number-complement/) is showed below. You can also find it [here](https://github.com/lrscy/LeetCode/blob/master/Algorithm/476-Number%20Complement.cpp).

My solution
```cpp
class Solution {
public:
    int findComplement(int num) {
        int ret = 0, cnt = 0;
        while(num) {
            ret |= ((num & 1) ^ 1) << cnt;
            num >>= 1;
            ++cnt;
        }
        return ret;
    }
};
```

A solution on Leetcode:
```cpp
class Solution {
public:
    int findComplement(int num) {
        int mask = 1;
        while(mask < num)
            mask = (mask << 1) + 1;
        return num ^ mask;        
    }
};
```

# Solution

This is a simple and easy problem. Just flip each bit and it's all solved. You can iterate each bit or handle them all together.

# Valueable Method

When seeing this kind of problem, I used to handle each bit and assemble them together. However, vectorize them (create a number and XOR with the original number) is a better way. Also, I ignore the feature that the all 1 bits sequence is the largest number at the sequence length.

Think more and code less all the time.
