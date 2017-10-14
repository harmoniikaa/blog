---
title: Codeforces 797F
date: 2017-04-16 23:17:05
tags: 动态规划
---
首先一个显然的思路,是连边跑费用流.但是边数过大存不下所以肯定不行.

所以考虑DP:先将mice和hole升序排序,令$dp_{i,j}$表示前i只mice放进前j个hole的最小代价,并且最后一只mouse一定被放进了第j个hole.枚举最后有多少个mice被同时放进第j个hole进行转移,可以建出DP方程:

$$dp_{i,j}=min_{i-c_j \leq k \leq i-1,0 \leq l \leq j-1}{ dp_{j,l} + cost(k + 1, i, j) }$$

其中cost(l,r,j)表示将第l到第r只mice放进第j个洞所需的代价,可以在转移的同时求出.

这样的DP是$O(n^2m^2)$的,显然跑不过.

考虑另外一种状态表示形式:令$dp_i$表示只考虑前i只mice放进洞里所需的最小代价,然后顺序枚举洞,倒序枚举mouse来转移:

$$dp_i = min_{i-c_j \leq k \leq i-1}{dp_k + cost(k+1,i,j)}$$

这样DP复杂度为$O(n^2m)$,经验证可以跑过47个点.

事实上分析可知,在枚举k进行转移的时候,i与i+1的决策点是具有单调性的.然后可以直接利用决策单调性来优化DP,复杂度就可以达到$O(nm)$了.

<!--more-->

```cpp
#include<cstdio>
#include<algorithm>
#define Pair pair<int,int>
#define N 5010
#define ll long long
#define inf 0x3f3f3f3f3f3f3f3fll
using namespace std;

int n,m,sum;
int x[N];
Pair y[N];
ll dp[N];

int main(){
  scanf("%d%d",&n,&m);
  for(int i=1;i<=n;i++) scanf("%d",&x[i]);
  for(int i=1;i<=m;i++){
    scanf("%d%d",&y[i].first,&y[i].second);
    sum+=y[i].second;
  }
  if(sum<n){
    printf("-1");
    return 0;
  }
  sort(x+1,x+1+n);sort(y+1,y+1+m);
  dp[0]=0;
  for(int i=1;i<=n;i++) dp[i]=inf;
  for(int j=1;j<=m;j++){
    int c=y[j].second,pos=y[j].first;
    for(int i=n;i>=1;i--){
      ll cost=0;
      for(int k=i-1;k>=max(0,i-c);k--){
	cost+=abs(x[k+1]-pos);
	dp[i]=min(dp[i],dp[k]+cost);
      }
    }
  }
  printf("%lld\n",dp[n]);
  return 0;
}
```

```cpp
#include<cstdio>
#include<algorithm>
#define Pair pair<int,int>
#define N 5010
#define ll long long
#define inf 1e18
using namespace std;

int n,m,sum;
int x[N];
Pair y[N];
ll dp[N],f[N];

int main(){
  scanf("%d%d",&n,&m);
  for(int i=1;i<=n;i++) scanf("%d",&x[i]);
  for(int i=1;i<=m;i++){
    scanf("%d%d",&y[i].first,&y[i].second);
    sum+=y[i].second;
  }
  if(sum<n){
    printf("-1");
    return 0;
  }
  sort(x+1,x+1+n);sort(y+1,y+1+m);
  dp[0]=0;sum=0;
  for(int i=1;i<=n;i++) dp[i]=inf;
  for(int j=1;j<=m;j++){
    int c=y[j].second,pos=y[j].first,p=0;
    ll cost=0;sum+=c;
    for(int i=0;i<=n;i++) f[i]=dp[i];
    for(int i=1;i<=min(sum,n);i++){
      cost+=abs(x[i]-pos);
      while(((dp[p]+cost>dp[p+1]+cost-abs(x[p+1]-pos))&&(p+1<i))||(p<i-c)){
	p++;
	cost-=abs(x[p]-pos);
      }
      f[i]=dp[p]+cost;
    }
    for(int i=0;i<=n;i++) dp[i]=min(dp[i],f[i]);
  }
  printf("%lld\n",dp[n]);
  return 0;
}
```