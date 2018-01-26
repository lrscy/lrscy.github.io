---
layout: post
title: Markdown中MathJax的用法
date: 2018-01-26 09:25:33
tags: [Markdown, MathJax]
categories: [Usage]

---

# 前言

Markdown和MathJax在一些语法上有交集，在此记录下两者有冲突的地方，作为今后的提醒。

# 下划线

在Markdwon中，下划线代表斜体，例如：`_a_`的效果既是_a_。在MathJax中，下划线代表下标，例如：`$a\\_2$'的效果既是$a\_2$。

在Markdown解析过程中，可能会出现错误解析MathJax下划线的事情。因此在MathJax公式中要将`_`替换成`\_`，将下划线转义成真正的下划线符号。

# 多行公式

在MathJax中，`\\`代表换行。在Markdown中，`\\`代表将转义字符`\`转义成真正的`\`字符，因此写`\\`后被解析出来时只有一个`\`，因此无法达成换行效果。因此在写换行时连续输入三个`\`即`\\\`即可达成换行要求。例如：
```
\begin{align}
r\_t &= \sigma(W\_rx\_t + Urh\_{t-1} + b\_r) \\\
u\_t &= \sigma(W\_ux\_t + r\_t \odot (U\_uh\_{t-1}) + b\_u) \\\
h\_t &= u\_t \odot h\_{t-1} + (1-u\_t) \odot \tanh(Wx\_t + r\_t \odot (U\_uh\_{t-1}) + b)
\end{align}
```
执行结果如下：

$$
\begin{align}
r\_t &= \sigma(W\_rx\_t + Urh\_{t-1} + b\_r) \\\
u\_t &= \sigma(W\_ux\_t + r\_t \odot (U\_uh\_{t-1}) + b\_u) \\\
h\_t &= u\_t \odot h\_{t-1} + (1-u\_t) \odot \tanh(Wx\_t + r\_t \odot (U\_uh\_{t-1}) + b)
\end{align}
$$

目前只踩到了这些坑，今后再有新坑再往后填入。
