---
title: Codeforces-题解：CF1491F Magnets
date: 2025-07-11
tags:
  - 题解
  - Codeforces
  - 二分
  - OI
licenseName: CC-BY-NC-SA 4.0
---
## Sol

如果我们能找到一个有磁极的磁铁，那么问题将非常简单。  
然而要想找出一个有磁极的磁铁，必须借助另外至少一个有磁极的磁铁。  
而现在，我们考虑一种极端情况：如果只有两块磁铁有磁极该如何是好？只能硬着头皮打暴力一个个找。  
我们每次查找 $S=\{i\},T=\{1,\cdots,i-1\}$。当遇到第二块有磁极的磁铁之前，答案一定为 $0$。当 $i$ 是第二块有磁极的磁铁时，答案变为 $\pm1$，以此我们就能够找到一块有磁极的磁铁了。  
利用这块磁铁找出 $>i$ 位置的无磁性的磁铁，这样总查询次数为 $n-1$。  
但是第一块有磁性的磁铁我们还没有找到，如果找到了剩下的都是无磁性的磁铁。这时没有一次一次找的必要，易得：二分找出来。于是继续构造 $S=\{i\},T=\{1,\cdots,j\}$ 来判断前缀 $j$ 的磁铁中有没有有磁性的磁铁，然后二分，这部分查询次数为 $\lceil\log_2n\rceil$。  
综上，总次 $=n-1\lceil\log_2n\rceil\leq n+\lfloor\log_2n\rfloor$。  
时间复杂度为 $\mathcal{O}(Tn^2)$。

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 2005;
int T, n, F;
int main() {
	scanf("%d", &T);
	while (T--) {
		scanf("%d", &n);
		for (int i = 2; i <= n; i++) {
			printf("? 1 %d\n%d\n", i - 1, i);
			for (int j = 1; j < i; j++) printf("%d ", j);
			puts(""); fflush(stdout);
			scanf("%d", &F);
			if (F) {
				vector<int> ans;
				for (int j = i + 1; j <= n; j++) {
					printf("? 1 1\n%d\n%d\n", i, j);
					fflush(stdout);
					scanf("%d", &F);
					if (!F) ans.push_back(j);
				}
				int l = 1, r = i - 1;
				while (l <= r) {
					int mid = l + r >> 1;
					printf("? 1 %d\n%d\n", mid, i);
					for (int j = 1; j <= mid; j++) printf("%d ", j);
					puts(""); fflush(stdout);
					scanf("%d", &F);
					if (F) r = mid - 1; else l = mid + 1;
				}
				for (int j = 1; j < i; j++)
					if (j != r + 1) ans.push_back(j);
				printf("! %d ", ans.size());
				for (int j = 0; j < ans.size(); j++) printf("%d ", ans[j]);
				puts(""); fflush(stdout); break;
			}
		}
	}
	return 0;
}
```