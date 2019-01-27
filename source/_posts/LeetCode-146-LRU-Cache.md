---
title: LeetCode 146 LRU Cache
date: 2019-01-27 10:52:23
tags: [LeetCode, Solution]
categories: [LeetCode]

---

# Code

Solution of [problem 146](https://leetcode.com/problems/lru-cache/) is showed below. You can also find it [here](https://github.com/lrscy/LeetCode/blob/master/Algorithm/146-LRU%20Cache.cpp).

```
static const int _ = []() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    return 0;
}();

class LRUCache {
public:
    LRUCache(int capacity) {
        cap = capacity;
    }
    
    int get(int key) {
        if( !mp.count( key ) ) return -1;
        lst.splice( lst.begin(), lst, mp[key] );
        return mp[key]->second;
    }
    
    void put(int key, int value) {
        if( mp.count( key ) ) {
            mp[key]->second = value;
            lst.splice( lst.begin(), lst, mp[key] );
        } else {
            lst.push_front( make_pair( key, value ) );
            mp[key] = lst.begin();
            if( lst.size() > cap ) {
                mp.erase( lst.rbegin()->first );
                lst.pop_back();
            }
        }
    }
    
private:
    int cap;
    unordered_map<int, list<pair<int, int>>::iterator> mp;
    list<pair<int, int>> lst;
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```

# Solution

To solve the problem, you need to build a **list** to store the order of "key"-"value" pair and to build a **map** to store "key"-"position in list" pair. Thus, when putting a "key"-"value" pair, you will easily know its position and update the order.

# Valueable Function

The interesting part of this code is the function **"splice"** of list. It can be found on [c++ reference](http://www.cplusplus.com/reference/list/list/splice/).
```
void splice (iterator position, list& x, iterator i);
```
The function will transfer element "i" in list "x" to specific "position". 

Before using this funcition, I wrote a segment of code to implement the function by myself but it runs slower than the function. In this case, by using this function, I transfer the "mp[key]" in list "lst" to the front of the list "lst".
