---
title: LeetCode 146 LRU Cache
date: 2019-01-27 10:52:23
tags: [LeetCode, Solution]
categories: [LeetCode]

---

# Code

Solution of [problem 309](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/) is showed below. You can also find it [here](https://github.com/lrscy/LeetCode/blob/master/Algorithm/309-Best%20Time%20to%20Buy%20and%20Sell%20Stock%20with%20Cooldown.cpp).

```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size() < 2) return 0;
        //s0 denote maxprofit at i-th day without stock
        //s1 denote maxprofit at i-th day with stock
        //s2 denote maxprofit at i-th day when cooling down.
        //s0 could be transited from s0 at i-1th day (do nothing), or from s1 at i-1th day (sell)
        //s1 could be transited from s1 at i-1th day (do nothing), or from s2 at i-1th day (cool to buy)
        //s2 could be transited from s2 at i-1th day (do nothing), or from s0 at i-1th day (do nothing)
        int s0 = 0, s1 = -1e9, s2 = 0;
        for(const auto &p: prices) {
            int tmp = s0;
            s0 = max(s0, s1 + p);
            s1 = max(s1, s2 - p);
            s2 = max(s2, tmp);
        }
        return max(s0, s2);
    }
};
```

# Solution

The method of the solution is from [Discuss](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/discuss/375948/C%2B%2B-Simplified-DP-Solution-with-State-Machine).

There is another [method](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/discuss/385204/0ms-C%2B%2B-can-extend-to-Any-cooldown-days-easy-to-understand) which use operations rather than states to record status and do calculation.

# Valueable Function

The interest part of the first solution is that it uses state machine and minimizes the memory usage by just storing constant level variables. However, it can only be applied in the situation. If the problem said you can only buy stock two days later after selling it, the solution cannot hold it. So the second solution is versatility.
