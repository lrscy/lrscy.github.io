---
title: LeetCode Solution Summary
date: 2019-01-27 10:05:53
tags: [LeetCode, Summary]
categories: [LeetCode]

---

This blog is a summary of my solution of LeetCode problems. I will share several interesting problems in this blog and category. Most of my solutions are on my [Github](https://github.com/lrscy/LeetCode).

Most of my algorithm problem solutions are written in C++ and some of them are written in Python3. Most of my databse problem solutions are written in MySQL.

# Basic Acceleration

Here is a way to reduce runtime when judging online with C++. Here is the code:

``` c++
static int desyncio = []() {
    std::ios::sync_with_stdio( false );
    cin .tie( nullptr );
    cout.tie( nullptr );
    return 0;
}();
```

This code will close the synchronize of two kinds of input and output in C format and C++ format. In C, mostly, we use "scanf", "printf", and other similar functions to control the input and output of our program, while in C++, we use "cin", "cout", and other funcitons respectively. Since in some case, we would use both of them in our program and they won't work as what we want without being controlled, C++ use synchronize to manage them. However, this rule will significantly slow down our program, especially when inputing and outputing large scale of data only in C or C++ format. Thus, we can close the synchronize by using codes upward.

# Algorithms

When solving problems on LeetCode, I found some problems are really interesting, some solutions are so amazing, and some methods are valuable to learn and remember. I will post individual blogs for them and here is the index of them.

- [146 LRU Cache](/2019/01/27/LeetCode-146-LRU-Cache)
- [159 Longest Substring with At Most Two Distinct Characters](/2019/01/27/LeetCode-159-Longest-Substring-with-At-Most-Two-Distinct-Characters)
- [200 Number of Islands](/2019/01/27/LeetCode-200-Number-of-Islands)
- [843 Guess the word](/2019/01/27/LeetCode-843-Guess-the-word)
- [982 Triples with Bitwise AND Equal To Zero](/2019/01/27/LeetCode-982-Triples-with-Bitwise-AND-Equal-To-Zero)
- [984 String Without AAA or BBB](/2019/01/27/LeetCode-984-String-Without-AAA-or-BBB)

To be uploaded:
