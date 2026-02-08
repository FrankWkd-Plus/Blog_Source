---
title: Luogu-题解：P13091 [FJCPC 2025] 中位数
date: 2025-07-25
tags:
  - Luogu
  - 二分
  - 题解
  - 贪心
  - 栈
  - OI
licenseName: CC-BY-NC-SA 4.0
---
# [传送门](https://www.luogu.com.cn/problem/P13091)
## Sol
我们使用二分答案二分结果的值 $x$。

- 对于检查当前的 $mid$ 是否合法，我们可以贪心将 $\ge mid$ 的数变为 $1$，否则变为 $0$。这样我们就可以快速得知相对大小。

考虑如何确保最优消去 $0$，保留 $1$。

- 对于原有的连续的 $0$，我们可以将其贪心区间操作，使其最终长度为 $1$ 或 $2$。
- 对于原有的连续的 $1$，我们肯定选择最后再消除。
 - 对于原有连续 $2$ 个 $0$ 以及 $1$ 个 $1$ 的情况，无论怎么消除，我们都只能变成 $0$，不过此时可以与后续的 $0$ 继续拼接。
    对于原有连续 $2$ 个 $1$ 以及 $1$ 个 $0$ 的情况，我们可以留到最后再消除。

处理完整的一个 $01$ 序列后，最后剩余 $1$ 的数量比 $0$ 多则存在某种解，否则就无解。

我们用栈模拟，直接记录连续个数，只要能确保最大化消掉 $0$ 即可。
```cpp
#include<bits/stdc++.h>
#define int long long
using namespace std;
int x[100010],y[100010],Q[100010],tail,n;
bool check(int u){
	tail=0;
	for(int i=1;i<=n;i++){
		if(x[i]>=u)y[i]=1;
		else y[i]=0;
	}
	for(int i=1;i<=n;i++){
		if(tail>1&&Q[tail]==0&&Q[tail-1]==0)tail--;
		else Q[++tail]=y[i];
	}
	int summ=0;
	for(int i=1;i<=tail;i++){
		if(Q[i]==1)summ++;
		else summ--;
	}
	return summ>0;
}
signed main(){
	int T;
	scanf("%lld",&T);
	while(T--){
		scanf("%lld",&n);
		for(int i=1;i<=n;i++)scanf("%lld",&x[i]);
		int l=1,r=1e9;
		while(l<r){
			int mid=(l+r+1)/2;
			if(check(mid))l=mid;
			else r=mid-1;
		}
		printf("%lld\n",l);
	}
	return 0;
}
```