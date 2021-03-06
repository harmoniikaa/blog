---
title: 清北集训Day4 数据结构
date: 2017-05-01 08:02:51
tags: [清北集训,线段树,平衡树,点分治,动态点分治,树链剖分]
---

# 清北集训Day4 数据结构

<!--more-->

# 线段树

## Sakuya
对每个区间保存从左向右/从右向左,0/1进来之后出去是什么结果。

## 算术天才⑨与等差数列 (bzoj 4373)
满足组成等差数列的两个要求:
- max(l,r)-min(l,r)=(r-l)*k
- 区间[l,r]差分后的gcd为k.
以上两项都可以用线段树维护.

## 魔法少女LJJ (bzoj 4399)
平衡树启发式合并可以做.
可以用并查集套动态开点线段树来做.
然后并查集合并时线段树合并.

## The Street (CodeChef MARCH14)
超哥线段树.

## [Coci2015]Norma (bzoj 3745)
右端点往右移,然后变成经典套路题.用单调队列维护一下.

## [清华集训2014]奇数国 (uoj 38)
用一个线段树维护每段区间的积,再用一颗二进制线段树维护每个区间每个数是否出现过,维护时将两个long long或起来就可以了.
这样我们只要求出区间的积,再将这段区间中出现过的每个数利用逆元修改答案即可.

## [清华集训2015]V (uoj 164)
经过若干次操作后,每个数会变成max(A+x,B).
维护这样的当前标记然后合并:max(x+a+A,max(b+A,B)).
对于某个节点,维护这个节点积存的最大的max(AA+x,BB).
若进行了一次max(x+A,B),则标记变为:max(x+max(A,AA),max(B,BB)).
在下传标记时,将子节点的当前标记接在父节点的最大标记之前.

# 平衡树

## Dynamic Trees and Queries Solved (CodeChef)
平衡树维护括号序列.

## wangxz与OJ (bzoj 3678)
平衡树拆点.

## Simple Queries
令A表示3次和,B表示2次和,C表示和.
$$ans=\frac{C^3-3BC+2A}{6}$$

# 可持久化数据结构

## 最大异或和 (bzoj 3261)
用b表示前缀异或和,找的是$max(b_n \oplus b_{p-1} \oplus x)$
用可持久化Trie树维护.

## Queries with Points
可持久化平衡树维护扫描线.

# 点分治

## 采药人的路径 (bzoj 3697)
在点分dfs时,令$f_{i,j}$表示i为路径权值和,j=0/1表示是否有休息站.
统计$\sum{f_{i,j}f{-i,k}}$其中j+k>0.注意特判重心是否为休息站.

## 小奇的树 (bzoj 3784)
二分答案,然后就好做了.

## Union on Tree (CodeChef)
动态点分治+虚树.

## [Zjoi2015]幻想乡战略游戏 (bzoj 3924)
坑.

## [ZJOI2007]Hide 捉迷藏 (bzoj 1095)
坑.

# 树链剖分

## QTREE 6 (CodeChef)
先预处理出每个点的答案,记录如果这个点为白色/黑色,子树中的最大同色联通块为多少.
每次修改一个点时,它到它同颜色的连续的最浅的祖先的路径上的每一个点都要减去一个值,它也要减去一个值.
然后黑色白色一样一个树链剖分.

## Fibonacci Numbers on Tree (CodeChef)
区间加等比数列,然后强行放在树上.