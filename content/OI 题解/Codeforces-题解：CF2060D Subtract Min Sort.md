---
title: Codeforces-题解：CF2060D Subtract Min Sort
date: 2025-07-25
tags:
  - 题解
  - Codeforces
  - 贪心
  - OI
licenseName: CC-BY-NC-SA 4.0
---
# [传送门](https://www.luogu.com.cn/problem/CF2060D)  
# Sol  
- 使用贪心算法。  
  
因为我们要使操作后原数组单调不降  
  
所以我们要让前面的数尽可能小：  
  
- 先遍历一遍数组，位置为 $i$ 时，如果 $a_i<a_{i−1}$，就不符合条件，输出 `NO`；  
- 接下来我们进行题目中的操作：选择 $i−1$，将 $a_i$ 和 $a_{i−1}$ 减去 $\min{a_i,a_{i−1}}$，由于 $a_i\le a_{i−1}$，所以操作相当于将 $a_i$ 减去 $a_{i−1}$，再将 $a_{i−1}$ 设为 $0$。若遍历完还没有输出 `NO`，就输出 `YES`。  
```cpp  
#include<bits/stdc++.h>  
using namespace std;  
const int N=2e5+5;  
int a[N];  
int main(){  
	int t;  
	scanf("%d",&t);  
	while(t--){  
		int n;  
		scanf("%d",&n);  
		for(int i=1;i<=n;i++){  
			scanf("%d",&a[i]);  
		}  
		for(int i=2;i<=n;i++){  
			if (a[i]<a[i-1]){  
				puts("NO");  
				goto endd;  
			}  
  
			a[i]-=a[i-1];  
			a[i-1]=0;  
		}  
		puts("YES");  
		endd:;  
	}  
	return 0;  
}  
```  
