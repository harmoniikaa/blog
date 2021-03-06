---
title: 清北集训Day6 数论
date: 2017-05-03 09:26:29
tags: [清北集训,数论]
---

# 清北集训Day6 数论

<!--more-->

## 常用公式
$$x^n \equiv x^{min(n,n\bmod\phi(m)+\phi(m))}$$
$$x \equiv z (\bmod p) \Leftrightarrow ax \equiv az (\bmod ap)$$

## 素性判定
随机整数a,根据费马小定理判断是否$a^{n-1}=1$,这样正确率比较低.

- 二次探测定理:
若p是素数,x是一个整数,且$x^2\bmod p \equiv 1$,那么$x=\pm 1 (\bmod p)$,即模一个质数下,1不存在"非平凡平方根".

- miller-rabbin
分解$n-1=2^rd$.
先随机生成一个a,判断数列:

$$a^d,a^{2d},a^{4d},a^{8d}...a^{2^r*d}$$

在模n意义下是否是先是一堆奇奇怪怪的数,然后是n-1,然后是一堆1.
如果符合上述,则n可能是一个素数.
首先n如果是素数,数列的最后一项一定是1.如果中间没有n-1而直接从某个数跳到了1,那么这个数根据二次探测定理,一定不是素数,如果最后一项不是1,那么它一定不是素数.

## 拓展BSGS
每次从底数中提一个数出来,利用:
$$x \equiv z (\bmod p) \Leftrightarrow ax \equiv az (\bmod ap)$$
提出一个数来,直到底数与模数互质,如果直到最后都不能互质,那么绝对无解.

## 拓展欧几里德
保证|y|<|a|,|x|<|b|,|x|+|y|最小.

