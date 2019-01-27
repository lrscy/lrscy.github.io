---
title: LeetCode 200 Number of Islands
date: 2019-01-27 13:24:09
tags: [LeetCode, Solution]
categories: [LeetCode]

---

# Code

The solution of [problem 200](https://leetcode.com/problems/number-of-islands/) is showed below. You can also find it [here](https://github.com/lrscy/LeetCode/blob/master/Algorithm/200-Number%20of%20Islands.cpp).

```
auto desyncio = []()
{
    std::ios::sync_with_stdio(false);
    cin.tie(nullptr);
    return nullptr;
}();

class Solution {
public:
    int dx[4] = { 1, 0, -1, 0 }, dy[4] = { 0, 1, 0, -1 };
    int nx, ny;
    
    void bfs( vector<vector<char>>& grid, int x, int y ) {
        queue<pair<int, int>> q;
        q.push( make_pair( x, y ) );
        grid[x][y] = '0';
        while( !q.empty() ) {
            auto tmp = q.front(); q.pop();
            for( int i = 0; i < 4; ++i ) {
                int tx = tmp.first + dx[i], ty = tmp.second + dy[i];
                if( tx < 0 || tx >= nx || ty < 0 || ty >= ny ) continue;
                if( grid[tx][ty] == '1' ) {
                    grid[tx][ty] = '0';
                    q.push( make_pair( tx, ty ) );
                }
            }
        }
    }
    
    int numIslands(vector<vector<char>>& grid) {
        int ret = 0;
        nx = grid.size();
        if( nx ) ny = grid[0].size();
        for( int i = 0; i < nx; ++i ) {
            for( int j = 0; j < ny; ++j ) {
                if( grid[i][j] == '1' ) {
                    ++ret;
                    bfs( grid, i, j );
                }
            }
        }
        return ret;
    }
};
```

# Solution

The solution of the problem is simply **BFS/DFS**. Check on each position and go BFS/DFS search when the position is "1".

# Valueable Method

I choose the BFS and here is a trick. If you choose to change status after retreat from queue, you will push a huge amount of duplicate position into queue which will cause **"Memory Limit Exceed"** and slow down your program. So If treating each action before pushing into queue, it would save huge amount of memory and run faster.
