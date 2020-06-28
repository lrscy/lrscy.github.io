---
title: LeetCode 332 Reconstruct Itinerary
date: 2020-06-28 17:28:49
tags: [LeetCode, Solution]
categories: [LeetCode]

---

# Code

Solution of [problem 332](https://leetcode.com/problems/reconstruct-itinerary/) is showed below. You can also find it [here](https://github.com/lrscy/LeetCode/blob/master/Algorithm/332-Reconstruct%20Itinerary.cpp).

```
class Solution {
public:
    unordered_map<string, vector<pair<string, bool>>> ump;
    int tot = 0;
    
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        vector<string> ret;
        tot = tickets.size();
        if(!tot) return ret;
        ret.push_back("JFK");
        for(auto &t: tickets) {
            if(!ump.count(t[0])) ump[t[0]] = vector<pair<string, bool>>();
            ump[t[0]].push_back({t[1], false});
        }
        for(auto it = ump.begin(); it != ump.end(); ++it) {
            sort(it->second.begin(), it->second.end());
        }
        dfs(ret, 0);
        return ret;
    }
    
    bool dfs(vector<string> &v, int n) {
        if(n == tot) return true;
        string s = v.back();
        if(!ump.count(s)) return false;
        for(auto it = ump[s].begin(); it != ump[s].end(); ++it) {
            if(!it->second) {
                it->second = true;
                v.push_back(it->first);
                if(dfs(v, n + 1)) return true;
                v.pop_back();
                it->second = false;
            }
        }
        return false;
    }
};
```

Another one is from the web.

```
class Solution {
public:
    vector<string> findItinerary(vector<pair<string, string>> tickets) {
        vector<string> res;
        stack<string> st{{"JFK"}};
        unordered_map<string, multiset<string>> m;
        for (auto t : tickets) {
            m[t.first].insert(t.second);
        }
        while (!st.empty()) {
            string t = st.top();
            if (m[t].empty()) {
                res.insert(res.begin(), t);
                st.pop();
            } else {
                st.push(*m[t].begin());
                m[t].erase(m[t].begin());
            }
        }
        return res;
    }
};
```

# Solution

It is an example of [Eulerian path](https://en.wikipedia.org/wiki/Eulerian_path) finding. The first solution is just a regular DFS. It tries every possible way and save the current result. However, it obviously records more information it needs. The reason it needs to reset failed path is that it meets a **dead end** but not all edges are visited. But, there must exists an Eulerian path. Thus, if we meet a dead end, it tells us that we **just need to visit other points and there must be a way to go back to the start point of the dead end path**. As a result, we can record the dead end path and output them at last. It is the method of Hierholzer's algorithm. The second solution is the Hierholzer's algorithm.

# Valuable Method

It is not hard to write the first method and also not hard to think about the second method even we don't know Hierholzer's algorithm before. If you know the Hierholzer's algorithm, you can realize it immediately and solve the problem. But if not, just dig more from the Eulerian path rather than submitting a passable solution and you will learn more from the problem.
