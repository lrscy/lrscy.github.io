---
title: LeetCode 982 Triples with Bitwise AND Equal To Zero
date: 2019-01-27 14:19:55
tags: [LeetCode, Solution]
categories: [LeetCode]

---

# Code

The solution of [problem 982](https://leetcode.com/problems/triples-with-bitwise-and-equal-to-zero/) is showed below. You can also find it [here](https://github.com/lrscy/LeetCode/blob/master/Algorithm/982-Triples%20with%20Bitwise%20AND%20Equal%20To%20Zero.cpp).

```
class Solution {
public:
    int countTriplets(vector<int>& A) {
        int n = A.size();
        int inv[1 << 16] = { 0 };
        for( auto a : A ) {
            for( int i = 0; i < ( 1 << 16 ); ++i )
                if( ( a & i ) == 0 )
                    ++inv[i];
        }
        int ret = 0;
        for( int i = 0; i < n; ++i ) {
            for( int j = 0; j < n; ++j ) {
                ret += inv[A[i] & A[j]];
            }
        }
        return ret;
    }
};
```

# Solution

The core method to solve it is to treat one number previously and traverse all combination of two numbers $a \& b$ in the vector. In this way, we can reduce the time complexity from $O(N^3)$ to $O(N^2)$. The treatment is to pre-calculate each number $a$ in the vector to find with which number $b$ $a \& b$ would get $0$.

# Valuable Method

The pre-treating method is really important when solving problems. If we enumerate all three numbers combination, for example $a \& b \& c$, we would repeatly calculate & operator with $c$ for many times. By remembering it, we could use more space to reduce run time. Moreoever, we could also store the result of $a \& b$ and check the result by $c$.
