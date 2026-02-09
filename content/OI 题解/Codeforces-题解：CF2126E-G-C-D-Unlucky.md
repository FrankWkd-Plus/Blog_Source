---
title: 题解：CF2126E G-C-D, Unlucky!
published: 2025-08-02
tags:
  - Codeforces
  - 题解
  - 数论
  - OI
licenseName: CC-BY-NC-SA 4.0
---
# [传送门](https://www.luogu.com.cn/problem/CF2126E)
# Sol
已经得知：$p_i = \gcd(a_1, a_2, \dots, a_i)$ 和 $s_i = \gcd(a_i, a_{i+1}, \dots, a_n)$。

不难发现：$p_i \mid a_i$ 与 $s_i \mid a_i$。

换句话说就是 $a_i$ 为 $p_i$ 和 $s_i$ 的倍数。

所以 $a_i$ 可以是 $lcm(p_i, s_i)$，这之后遍历看生成出来的是不是 $s$ 和 $p$ 就行了。

猜你想问：为什么要用 $lcm$ 不用他们的其他倍数呢？因为其他倍数都可以写为 $k \times lcm(p_i, s_i)$ 的形式。

但是如果下一个也写成 $k \times lcm(p_{i+1}, s_{i+1})$，他们的 $\gcd$ 就会多出一个系数 $k$。

```cpp
#include<bits/stdc++.h>
#define ll long long
#define PII pair<int,int>
using namespace std;
ll t=1,n,s[100010],p[100010],a[100010],x;
void solve(){
	cin>>n;
	for(int i=1;i<=n;i++)cin>>s[i];
	for(int i=1;i<=n;i++)cin>>p[i];
	for(int i=1;i<=n;i++)a[i]=lcm(s[i],p[i]);
	x=0;
	for(int i=1;i<=n;i++){
		x=gcd(x,a[i]);
		if(x!=s[i]){
			cout<<"NO\n";
			return;
		}
	}
	x=0;
	for(int i=n;i>=1;i--){
		x=gcd(x,a[i]);
		if(x!=p[i]){
			cout<<"NO\n";
			return;
		}
	}
	cout<<"YES\n";
}
int main(){
	cin>>t;
	while(t--){
		solve();
	}
	return 0;
}

```