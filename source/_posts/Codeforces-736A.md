---
title: Codeforces 736A
date: 2017-04-16 10:13:56
tags: 递推
---
考虑如果要造出一个打过i场的胜利者,需要先造出一个打过i-1场的和一个打过i-2场的.假设要造出一个打过i场的胜利者最少需要$f_i$个人参加比赛,则有: $f_i = f_{i-1} + f_{i-2}$.初始时$f_0 = 1, f_1 = 2$,当$f_i$第一次大于n时,i-1即是答案.

<!--more-->

```cpp
#include<cstdio>
#define ll long long
using namespace std;

ll f[100];
ll n;

int main(){
  scanf("%lld",&n);
  f[0]=1;f[1]=2;
  for(int i=2;;i++){
    f[i]=f[i-1]+f[i-2];
    if(f[i]>n){
      printf("%d\n",i-1);
      return 0;
    }
  }
  return 0;
}
```