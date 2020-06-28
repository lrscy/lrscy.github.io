---
title: LeetCode 380 Insert Delete GetRandom O(1)
date: 2020-06-28 19:24:30
tags: [LeetCode, Solution]
categories: [LeetCode]

---

# Code

Solution of [problem 380](https://leetcode.com/problems/insert-delete-getrandom-o1/) is showed below. You can also find it [here](https://github.com/lrscy/LeetCode/blob/master/Algorithm/380-Insert%20Delete%20GetRandom%20o1.cpp).

```
auto ii = [](){
  ios_base::sync_with_stdio(false);
  cin.tie(NULL);
  cout.tie(NULL);
  return 0;
} ();

class RandomizedSet {
public:
    /** Initialize your data structure here. */
    RandomizedSet() {

    }

    /** Inserts a value to the set. Returns true if the set did not already contain the specified element. */
    bool insert(int val) {
        if(mp.count(val)) return false;
        v.push_back(val);
        mp[val] = v.size() - 1;
        return true;
    }

    /** Removes a value from the set. Returns true if the set contained the specified element. */
    bool remove(int val) {
        if(!mp.count(val)) return false;
        mp[v.back()] = mp[val];
        v[mp[val]] = v.back();
        v.pop_back();
        mp.erase(val);
        return true;
    }

    /** Get a random element from the set. */
    int getRandom() {
        return v[rand() % v.size()];
    }
private:
    unordered_map<int, int> mp;
    vector<int> v;
};

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet* obj = new RandomizedSet();
 * bool param_1 = obj->insert(val);
 * bool param_2 = obj->remove(val);
 * int param_3 = obj->getRandom();
 */
```

# Solution

The hard point of the problem is the **random** operation. The `insert` and `remove` operator can be easily done by **dictionary** structure, like `std:map` or `std:unordered_map`. To implement the **random** operator, **array-like** stucture is necessary. But it is hard to add or delete in `O(1)` time. To combine them, we can save the data in a array-like structure and save their position in a dictionary structure. To remove a element, we can find the position from the dictionary structure and swap it with the last one so that we can remove it in both structure in `O(1)` time.

# VAluable Method

To recall the problem, it is not that hard. But it took me long time on the random operator. It's weird. When inserting or deleting elements in a heap, you add an element to the last position or swap the top element with the last element, and then follow the heap rules to adjust it. Here, it uses the same method.
