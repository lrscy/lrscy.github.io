---
title: LeetCode 146 LRU Cache
date: 2019-01-27 10:52:23
tags: [LeetCode, Solution]
categories: [LeetCode]

---

# Code

Solution of [problem 416](https://leetcode.com/problems/partition-equal-subset-sum/) is showed below. You can also find it [here](https://github.com/lrscy/LeetCode/blob/master/Algorithm/416-partition%20equal%20subset%20sum.cpp).

```
static int desyncio = []() {
    std::ios::sync_with_stdio( false );
    cin .tie( nullptr );
    cout.tie( nullptr );
    return 0;
}();

class Solution {
public:
    bool canPartition(vector<int>& nums) {
        bitset<20010> bs(1);
        int sum = 0;
        for(auto n: nums) sum += n;
        if(sum & 1) return false;
        for(auto n: nums) {
            bs |= (bs << n);
        }
        return bs[sum >>= 1];
    }
};
```

# Solution

There are two solutions. The core method of both solutions is dp, specifically 0-1 Knapsack problem. 

The first solution uses **"bitset"** to storage status.

The second solution, which doesn't show here since it's too simple and you need to implement it by yourself, uses **"bool array"** to storage all status.

The core operation (at line 16) of two solutions is different due to feature of the data structure they use. According to the result on LeetCode, the second solution is slower than the first one. In my opinion, bit operations is much faster than scanning through bool array.

# Valueable Function

It is a good problem to practice **"bitset"** data structure. It is made of bits and can store more than 64 bits, which is the length of **long long** or **unsigned long long**. It also implements almost all bit operations.
