---
title: Codeforces 800C
date: 2017-04-17 10:55:55
tags: [数论,拓展欧几里得]
---
先来考虑在什么情况下存在一个x使得$ax \equiv b (\bmod m)$.

首先,如果a与m互质,那么一定存在一个x满足条件无疑.

如果a与m不互质,因为当$k|m$,$k|a$且$k|b$有$\frac{a}{k}x \equiv \frac{b}{k} (\bmod \frac{m}{k})$成立.而且只有当$k=gcd(a,m)$时,存在逆元.因此如果$gcd(a,m)|gcd(b,m)$,则存在一个解.

因此DP处理出应该选哪些k作为gcd,然后用exgcd来进行转移即可.

<!--more-->

```cpp
#include<cstdio>
#define N 200010
#define ll long long
using namespace std;

int gcd(int a,int b){
  return b==0?a:gcd(b,a%b);
}

void exgcd(ll a,ll b,ll &x,ll &y){
  if(b==0){
    x=1;y=0;
    return;
  }
  exgcd(b,a%b,x,y);
  ll t=x;x=y;y=t-a/b*y;
}

int n,m;
int vis[N],g[N],c[N],p[N];

int main(){
  scanf("%d%d",&n,&m);
  for(int i=1;i<=n;i++){
    int x;
    scanf("%d",&x);
    vis[x]=1;
  }
  for(int i=1;i<m;i++){
    g[i]=gcd(i,m);
    if(vis[i]==0) c[g[i]]++;
  }
  for(int i=m-1;i>=1;i--){
    for(int j=i+i;j<m;j+=i){
      if(c[j]>c[p[i]]) p[i]=j;
    }
    c[i]+=c[p[i]];
  }
  if(vis[0]==0) c[1]++;
  printf("%d\n",c[1]);
  ll a=1,pre=1;;
  for(int k=1;k;k=p[k]){
    for(int i=1;i<m;i++){
      if(g[i]==k&&vis[i]==0){
	ll x,y;
	exgcd(a,m,x,y);
	x=(x%m+m)%m;
	x=(x*i/pre)%m;
	pre=k;
	a=a*x%m;
	printf("%lld ",x);
      }
    }
  }
  if(vis[0]==0) printf("0\n");
  return 0;
}
```