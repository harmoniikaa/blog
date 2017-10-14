---
title: Codeforces 814E
date: 2017-06-11 19:34:41
tags: [动态规划]
---

由于题目中所给的限制,第$i$个节点到第1个节点的最短路一定只有一条,并且第$i$节点到第1个节点的最短路一定要大于第$i-1$个节点的.所以动态规划的阶段就应该是类似:
$$f[节点][当前长度的最短路]$$
的样子.

考虑一个图从点1开始的bfs树,会发现如果添加上一个点,要不与前一个点在同一层,要不比前一个深一层;并且新添加的节点一定与且只于上一层节点连一条边.所以可以记录$f_{i,p1,p2,c1,c2}$表示考虑到节点i,上一层有$p1$个节点还有1个度数,有$p2$个节点还有2个度数,当前层有$c1$个节点还有1个度数,有$c2$个节点还有2个度数,然后考虑新接入的节点与当前层的链接情况,就可以动态规划转移了.

时间复杂度:$O(n^5)$,空间可以滚动一维.

<!--more-->
```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<iostream>
#define ll long long
#define N 52
#define mod 1000000007ll
#define add(x,y) {x+=(y);if((x)>=mod) x-=mod;}
using namespace std;

ll f[2][N][N][N][N];
int n,d[N];

int main(){
  scanf("%d",&n);
  for(int i=1;i<=n;i++) scanf("%d",&d[i]);
  f[0][d[1]==2][d[1]==3][d[2]==2][d[2]==3]=1;
  for(int i=2;i<n;i++){
    int cur=i&1,nxt=cur^1;
    memset(f[nxt],0,sizeof(f[nxt]));
    for(int p1=0;p1<=n;p1++){
      for(int p2=0;p1+p2<=n;p2++){
        for(int c1=0;c1+p1+p2<=n;c1++){
          for(int c2=0;c1+c2+p1+p2<=n;c2++){
            if(f[cur][p1][p2][c1][c2]<=0) continue;
            ll val=f[cur][p1][p2][c1][c2];
            if(p1==0&&p2==0){
              add(f[cur][c1][c2][0][0],val);
              continue;
            }
            for(int t=0;t<=1;t++){
              ll w;
              if(t==0){
                w=p1;
                p1--;
                if(p1<0){
                  p1++;
                  continue;
                }
              }else{
                w=p2;
                p2--;
                if(p2<0){
                  p2++;
                  continue;
                }else{
                  p1++;
                }
              }
              if(d[i+1]==2){
                add(f[nxt][p1][p2][c1+1][c2],val*w%mod);
                if(c1) add(f[nxt][p1][p2][c1-1][c2],val*w*c1%mod);
                if(c2) add(f[nxt][p1][p2][c1+1][c2-1],val*w*c2%mod);
              }else{
                add(f[nxt][p1][p2][c1][c2+1],val*w%mod);
                if(c1) add(f[nxt][p1][p2][c1][c2],val*w*c1%mod);
                if(c2) add(f[nxt][p1][p2][c1+2][c2-1],val*w*c2%mod);
                if(c1&&c2) add(f[nxt][p1][p2][c1][c2-1],val*w*c1*c2%mod);
                if(c1>=2) add(f[nxt][p1][p2][c1-2][c2],val*w*(c1*(c1-1)/2)%mod);
                if(c2>=2) add(f[nxt][p1][p2][c1+2][c2-2],val*w*(c2*(c2-1)/2)%mod);
              }
              if(t==0){
                p1++;
              }else{
                p2++;
                p1--;
              }
            }
          }
        }
      }
    }
  }
  cout<<f[n&1][0][0][0][0]<<endl;
  return 0;
}
```
