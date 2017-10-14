---
title: Hackerrank GCD Product
date: 2017-06-13 11:45:12
tags: [莫比乌斯反演]
---

题意:求:

$$\prod_{i=1}^{n}{\prod_{j=1}^{m}{gcd(i,j)}}$$

不妨先假设$n<m$
首先枚举所有可能的$gcd$,记为$d$:

$$\prod_{d=1}^{n}{d^{f(d)}}$$

其中,$f(d)$表示$d$这个$gcd$出现了多少次:

$$f(d)=\sum_{i=1}^{n}{\sum_{j=1}^{m}{[gcd(i,j)=d]}} \\
=\sum_{k=1}^{\lfloor \frac{n}{d} \rfloor}{\mu (k)\lfloor \frac{n}{kd} \rfloor\lfloor \frac{m}{kd} \rfloor}$$

带入原式,可得:

$$\prod_{d=1}^{n}{d^{\sum_{k=1}^{\lfloor \frac{n}{d} \rfloor}{\mu (k)\lfloor \frac{n}{kd} \rfloor\lfloor \frac{m}{kd} \rfloor}}}$$

设$T=kd$,枚举$T$,则有:

$$\prod_{T=1}^{n}{(\prod_{d|T}{d^{\mu(\frac{T}{d})}})^{\lfloor \frac{n}{T} \rfloor\lfloor \frac{m}{T} \rfloor}}$$

令:

$$g(T)=\prod_{d|T}{d^{\mu(\frac{T}{d})}}$$

因为莫比乌斯函数可以线性求出,若$g$也可以线性求出那么这道题目就可以在$O(n+\sqrt{n}\log n)$的复杂度下求出.

观察函数的性质,发现$g$并不满足积性.再次观察发现:

- $g(1)=1$.
- 如果$T$是某个质数的正整数次幂,设$T=p^q$,那么$g(T)=p$.
- 其余情况均满足$g(T)=1$.

所以可以枚举所有质数,并枚举它们的所有正整数次幂.因为质数的个数$\pi(n) \sim \frac{n}{\ln n}$,每个质数最多枚举$O(\log n)$个幂,所以求出所有$g$的复杂度就是$O(\frac{n}{\ln n} \times \log n)=O(n)$,总时间复杂度为$O(n+\sqrt{n}\log n)$.

<!--more-->

```cpp
#include<cstdio>
#include<algorithm>
#define ll long long
#define N 15000010
#define mod 1000000007
using namespace std;

bool notPrime[N];
int mu[N];
int inv[N];
int prime[N],tot;
int g[N];
int n,m;

void linearShaker(){
  mu[1]=1;
  for(int i=2;i<=n;i++){
    if(!notPrime[i]){
      prime[++tot]=i;
      mu[i]=-1;
    }
    for(int j=1;(j<=tot)&&(prime[j]*i<=n);j++){
      notPrime[i*prime[j]]=1;
      if(i%prime[j]==0){
        mu[i*prime[j]]=0;
        break;
      }
      mu[i*prime[j]]=-mu[i];
    }
  }
  inv[1]=1;
  for(int i=2;i<=n;i++) inv[i]=(-((ll)mod/i*inv[mod%i]%mod)+mod)%mod;
}

ll ksm(ll a,ll b){
  ll ret=1;
  while(b){
    if(b&1) ret=ret*a%mod;
    a=a*a%mod;
    b>>=1;
  }
  return ret;
}

int main(){
  scanf("%d%d",&n,&m);
  ll ans=1;
  if(n>m) swap(n,m);
  linearShaker();
  for(int i=1;i<=n;i++) g[i]=1;
  for(int i=1;i<=tot;i++){
    ll now=prime[i];
    for(ll j=now;j<=n;j=j*now){
      g[j]=now;
    }
  }
  g[0]=1;
  for(int i=1;i<=n;i++) g[i]=(ll)g[i]*g[i-1]%mod;
  int next;
  for(int i=1;i<=n;i=next+1){
    next=min(n/(n/i),m/(m/i));
    ans=ans*ksm((ll)g[next]*ksm(g[i-1],mod-2)%mod,(ll)((ll)n/i)*((ll)m/i))%mod;
  }
  printf("%lld\n",ans);
  return 0;
}
```
