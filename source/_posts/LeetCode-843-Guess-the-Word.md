---
title: LeetCode 843 Guess the Word
date: 2019-01-27 13:54:15
tags: [LeetCode, Solution]
categories: [LeetCode]

---

# Code

The solution of [problem 843](https://leetcode.com/problems/guess-the-word/) is showed below. You can also find it [here](https://github.com/lrscy/LeetCode/blob/master/Algorithm/843-Guess%20the%20Word.cpp).

```
/**
 * // This is the Master's API interface.
 * // You should not implement it, or speculate about its implementation
 * class Master {
 *   public:
 *     int guess(string word);
 * };
 */

auto desyncio = []()
{
    std::ios::sync_with_stdio(false);
    cin.tie(nullptr);
    return nullptr;
}();

class Solution {
public:
    void findSecretWord(vector<string>& wordlist, Master& master) {
        long long prob[10][30] = { 0 };
        for( auto &w : wordlist ) {
            for( int i = 0; i < 6; ++i ) prob[i][w[i] - 'a'] += 1;
        }
        while( true ) {
            string tmp = generate( prob, wordlist );
            int ret = master.guess( tmp );
            if( ret == tmp.length() ) break;
            for( auto it = wordlist.begin(); it != wordlist.end(); ) {
                if( match( *it, tmp ) != ret ) {
                    for( int i = 0; i < 6; ++i )
                        prob[i][( *it )[i] - 'a'] -= 1;
                    wordlist.erase( it );
                } else ++it;
            }
        }
        return ;
    }
    
    string generate( long long prob[][30], vector<string>& wordlist ) {
        string ret;
        long long best = 0;
        for( auto w : wordlist ) {
            long long tpscore = 1;
            for( int i = 0; i < 6; ++i ) tpscore *= prob[i][w[i] - 'a'];
            if( tpscore > best ) {
                best = tpscore;
                ret = w;
            }
        }
        return ret;
    }
    
    int match( string a, string b ) {
        int ret = 0;
        for( int i = 0; i < a.length(); ++i ) {
            if( a[i] == b[i] ) ++ret;
        }
        return ret;
    }
};
```

# Solution

The thought of the problem is from a [discussion](https://leetcode.com/problems/guess-the-word/discuss/134087/C%2B%2B-elimination-histogram-beats-Minimax) of the problem. Basically, we count all characters on each position from 1 to 6 as there probability. Then we generate a string with highest probability and guess it. Based on what return to us, we eliminate all other candidates which does not meet the requirement. Finally, we will guess the right word.

# Interesting Point

It's a whole new kind of problem. The method to solve it is now fixed. Statistic is a candidate method to this problem. This kind of problem is just so fun and interesting!
