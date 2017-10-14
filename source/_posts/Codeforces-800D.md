---
title: Codeforces 800D
date: 2017-04-17 14:06:56
tags: [容斥原理,递推]
---
发现$G(x)=x*...$,然后再将所有$G(x)$异或到一起,那么感觉上必须先把所有$G(x)$求出来再继续进行.所以$G(x)$前面乘上的那个x可以先忽略不计.

先以样例为例,考虑$G(x)$统计时与哪些集合有关:将此三个数分别设为a,b,c,则有:

|G(x)      |S           |
|---------|---------|
|123       |a,ac     |
|321       |b,bc     |
|121       |ab,abc|
|555       |c            |

显然这些数并不好统计进$G(x)$,因此不妨设$g(x)=\sum_{x' \leq x}{G(x')}$,这里的大于等于号表示$x'$的每一位均大于等于$x$的每一位.则有新的$g(x)$:

|g(x)                              |S                                     |
|-----------------------|-----------------------|
|123                               |a,c,ac                           |
|321                               |b,c,bc                           |
|121                               |a,b,c,ab,bc,ac,abc|
|555                               |c                                    |

如此看起来就要好统计地多了,$g(x)$将由所有大于等于x的$g(x')$转移而来.假设大于等于x的所有数有$c(x)$个,和为$s(x)$,平方的和为$s2(x)$,则有$g(x)=2^{c(x)-2}(s2(x)+s(x)^2)$,简单容斥一下就可以推的出来.

由上可以求出$g(x)$在0..999999上的值,思考$g(x)$的定义,可以枚举子集,利用容斥原理得到$G(x)$的值等于有6个数大于等于x的-有5个数+有4个数-....则本题得解.

细节比较多,需要认真考虑.

<!--more-->

```cpp
#include<cstdio>
#define ll long long
#define mod 1000000007ll
#define N 1000010
using namespace std;

int n;
ll c[N],s[N],s2[N],bin[N],g[N],G[N];

int main(){
  scanf("%d",&n);
  for(int i=1;i<=n;i++){
    ll x;
    scanf("%lld",&x);
    c[x]++;s[x]+=x;s2[x]+=x*x%mod;
  }
  bin[0]=1;
  for(int i=1;i<=n;i++) bin[i]=bin[i-1]*2%mod;
  for(int i=1;i<1e6;i*=10){
    for(int j=1e6-1;j>=0;j--){
      if((j/i)%10!=9){
	c[j]+=c[j+i];
	c[j]%=mod;
	s[j]+=s[j+i];
	s[j]%=mod;
	s2[j]+=s2[j+i];
	s2[j]%=mod;
      }
    }
  }
  for(int j=0;j<1e6;j++){
    if(c[j]==0) continue;
    if(c[j]==1) g[j]=s2[j];
    else g[j]=bin[c[j]-2]*(s2[j]+s[j]*s[j]%mod)%mod;
  }
  for(int i=0;i<1e6;i++){
    for(int S=0;S<64;S++){
      bool b=1;
      for(int k=0,p=1;k<6;k++,p*=10){
	if((S&(1<<k))&&((i/p)%10==9)){
	  b=0;
	  break;
	}
      }
      if(!b) continue;
      ll d=1,r=i;
      for(int k=0,p=1;k<6;k++,p*=10){
	if((S&(1<<k))&&((i/p)%10!=9)){
	  d=d*-1;
	  r+=p;
	}
      }
      G[i]+=d*g[r];
    }
    G[i]%=mod;
    G[i]=(G[i]+mod)%mod;
  }
  ll ret=0;
  for(int i=0;i<1e6;i++){
    ret^=(1ll*i*G[i]);
  }
  printf("%lld\n",ret);
  return 0;
}
```