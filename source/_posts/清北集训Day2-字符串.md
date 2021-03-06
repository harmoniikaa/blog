---
title: 清北集训Day2 字符串
date: 2017-04-29 08:02:16
tags: [清北集训,AC自动机,后缀数组,Z-BOX]
---
# 清北集训Day2 字符串

<!--more-->

## AC自动机基础
如果当前走到的是s2,想要找一个s1,满足s1是s2的一个后缀.
AC自动机fail指针的目的就是找一个比当前的节点更浅的节点,使得目标节点所代表的字符串是当前节点所代表的字符串的一个后缀.

## 后缀数组基础
构造复杂度:
$O(n^2logn)$:暴力排序
$O(nlog^2n)$:二分+HASH
$O(nlogn)$:倍增
$O(n)$:DC3,SAM
sa[i]:第i小的后缀
height[i]:sa[i]与sa[i-1]的最长公共前缀

## Z-BOX基础
求的是每个后缀与原串的最长公共前缀.
令$f_i$代表第i个后缀与原串的最长公共前缀长度.
$f_1=|S|$.
$f_2$暴力求.
有若干j..r[j]与原串的前缀匹配.
记录一个能和原串匹配且r[j]最靠右的某一个r[j].
所以对于一个新的i,因为j到r[j]和某个前缀一样,所以i一定有一个k(=i-j)可以与之对应使得$f_i\geq min(f_k,r[j]-i)$.和manacher算法类似.

## Problem 0.0:
- Question:
给一个长度为n的字符串,希望你能找到一个最短的串能够循环生成这个串.
- Input:
abcabcab
- Output:
abc
- Sol:
Z-BOX.找到第一个i+f[i]=n的后缀.

## Problem 0.1:
- Question:
给一个长度为n的字符串,希望知道删去哪一个字符后,使得能够循环生成这个串的串的长度最小(也可以不删).
- Input:
abcdabcab
- Output:
d
- Sol:
枚举一个长度,然后暴力去匹配就可以了.因为$\sum{\frac{1}{i}}=\ln n$.可能第一块不对,那么用第二块匹配就可以了.

## Problem 1 (bzoj 2746):
Fail树非常神奇的性质:往上走一定会走到它的一个后缀.求对应的Fail树上的LCA即可.

## Problem 2 (bzoj 2754):
使用AC自动机.换一种想法,我们对所有询问串建一个AC自动机.
然后对于每一个原串,在Fail树中询问.
每次走到一个点,首先询问从这个点到根的所有点的权值和,然后用一个区间修改把从点到根的路径全部赋值为0,用一个树链剖分去维护它.
这样是$O(log^2)$.
果然是用一个LCT去替代,就可以做到log了.

## Problem 4:
第一段后缀数组贪心.
后面两段倒序拷贝一份然后后缀数组找一段连续的最小的即可.

## Problem 5 (poj 3376):
考虑一个串aaaba,如果要找一个比它短的串放在它后面,那么长度一定为2或4,因为最后的若干位一定是自回文的("a","aba").
然后对于每一个长度建两个map,存顺序和逆序哈希就可以了.

## Problem 6 (bzoj 3473):
连在一起建后缀数组,用sliding-window来维护"至少k个字符串"这个量,然后对一个区间取min,用线段树维护.

## a
p进制展开然后数位DP.

## b
找循环节然后线段树.

## c
乱搞.