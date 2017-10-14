---
title: Codeforces 797E
date: 2017-04-16 01:08:34
tags: 分块
---
考虑$n,q \leq1e5$,可以考虑使用根号算法在$O(\sqrt n)$的时间内完成单次询问.

所以将询问分类讨论,当$k \leq \sqrt n$时,对于每一个k暴力DP求解.当$k \geq \sqrt n$时,直接暴力求解即可.

<!--more-->

```cpp
#include<cstdio>
#include<algorithm>
#include<cstring>
#include<cmath>
#include<queue>
#include<stack>
#include<set>
#include<map>
#include<cstdlib>
#include<ctime>
#include<climits>
#define rep(i,a,b) for(int i=(a);i<=(b);i++)
#define per(i,a,b) for(int i=(a);i>=(b);i--)
#define inf 0x3f3f3f3f
#define N 100010
#define M 
using namespace std;

inline int read(){
    int x=0,f=1;char ch=getchar();
    while(ch<'0'||ch>'9'){if(ch=='-')f=-1;ch=getchar();}
    while(ch>='0'&&ch<='9'){x=x*10+ch-'0';ch=getchar();}
    return x*f;
}

struct Query{
  int q,k,ans,ord;
}q[N];

int n,m,a[N],f[N];

bool cmp(Query A,Query B){
  return A.k<B.k;
}

bool recmp(Query A,Query B){
  return A.ord<B.ord;
}

void dp(int k){
  per(i,n,1){
    if(i+a[i]+k<=n){
      f[i]=f[i+a[i]+k]+1;
    }else{
      f[i]=1;
    }
  }
}

int main(){
  n=read();
  rep(i,1,n) a[i]=read();
  m=read();
  rep(i,1,m) q[i].q=read(),q[i].k=read(),q[i].ord=i;
  sort(q+1,q+1+m,cmp);
  rep(i,1,m){
    if(q[i].k>300){
      int ans=0,x=q[i].q;
      while(x<=n){
	x=x+a[x]+q[i].k;
	ans++;
      }
      q[i].ans=ans;
    }else{
      if(q[i].k==q[i-1].k){
	q[i].ans=f[q[i].q];
      }else{
	dp(q[i].k);
	q[i].ans=f[q[i].q];
      }
    }
  }
  sort(q+1,q+1+m,recmp);
  rep(i,1,m) printf("%d\n",q[i].ans);
  return 0;
}
```