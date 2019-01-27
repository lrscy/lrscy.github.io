---
title: LeetCode 159 Longest Substring with At Most Two Distinct Characters
date: 2019-01-27 11:47:14
tags: [LeetCode, Solution]
categories: [LeetCode]

---

# Code

The solution of [problem 159](https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters/) is showed below. You can also find it [here](https://github.com/lrscy/LeetCode/blob/master/Algorithm/159-Longest%20Substring%20with%20At%20Most%20Two%20Distinct%20Characters.cpp).

```
class Solution {
public:
    int lengthOfLongestSubstringTwoDistinct(string s) {
        int ret = 0, len = s.length();
        int i = 0, j = 0;
        unordered_map<int, int> mp;
        if( s.size() <= 2 ) return s.size();
        while( j < len ) {
            mp[s[j]] = j;
            if( mp.size() > 2 ) {
                ret = max( ret, j - i );
                while( mp.size() > 2 ) {
                    int key, n = INT_MAX;
                    for( auto it = mp.begin(); it != mp.end(); ++it ) {
                        if( it->second < n ) {
                            n = it->second;
                            key = it->first;
                        }
                    }
                    i = n + 1;
                    mp.erase( key );
                }
            }
            ++j;
        }
        ret = max( ret, j - i );
        return ret;
    }
};
```

# Solution

To solve the problem, you need to build a **map** to store **the last position** of each unique element and use **sliding window** to get the final result.

The sliding window means that expanding the right side of the window first until exceeding the limit, three unique elements, then querying each element in map and kicking the most left element out. In this way, the window starts sliding until the end of the string.

# Valuable Method

At the first time, I use map to store number of elements in a sliding window rather than their position. It's OK but stores an useless information--number. Also, my first try will scan the whole string twice which slows down the whole program. However, if we just store the position of each element, we just need to scan the whole string once and also get the right result. It's also a kind of optimization on constant, which reduces the time complexity from O(2N) to O(N).
