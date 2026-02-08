---
title: KMP学习笔记
date: 2025-03-11
tags:
  - 学习笔记
  - 算法
  - OI
licenseName: CC-BY-NC-SA 4.0
---
> Upd 2025-07-30: 补充注释和文字内容

前言：这把高端局

## 关于KMP

- 时间复杂度为 $O(n+m)$ 的优秀字符串查找算法。
- 适用于在句子/文章中查找一段文字（词语）。

## KMP实现

### 关于共同前后缀数组（PMT）

- 说人话就是 $next$ 数组。
- 是什么？

  - $next_i$ 表示下标从 $1$ 到 $i$ 的子串（后文中叫做 $Orgin$ 串）中既是 $Orgin$ 的前缀串又是 $Orgin$ 的后缀串的字符串的最长长度。
  - 举例说明：开始时候的串为：`abcab`
    - 那 $next_5=2$ ，因为存在串 $ab$ 既是 $Orgin$ 的前缀又是 $Orgin$ 的后缀，而且在所有满足条件的串中，它是最长的。所以把 $next_5$ 设为它的长度—— $2$.
- 构建 $PMT$ 的详细步骤

  - 初始化：设模式串为 $P$，长度为 $m$，创建一个长度为 $m$ 的数组 $pmt$ 来存储部分匹配表(相当于 $next$ 数组)的值。初始化 $pmt[0] = 0$，因为模式串的第一个字符的前缀子串只有一个字符，不存在相等的前后缀。
  - 遍历模式串：从模式串的第二个字符开始遍历，即 $i = 1$ 到 $i = m - 1$。对于每个位置 $i$，设两个指针，一个指针 j 指向当前前缀子串的最长相等前后缀的下一个位置，初始时 $j = pmt[i - 1]$。
    -比较字符：比较 $P[i]$ 和 $P[j]$。如果 $P[i]$ 等于 $P[j]$，说明找到了更长的相等前后缀，此时 $pmt[i] = j + 1$，然后将 $j$ 后移一位，即 $j = j + 1$，继续下一个位置的比较。
  - 不相等情况：如果 $P[i]$ 不等于 $P[j]$，且 $j > 0$，则将 $j$ 更新为 $pmt[j - 1]$，继续比较 $P[i]$ 和 $P[j]$。这是因为当 $P[i]$ 和 $P[j]$ 不相等时，需要回溯到 $j$ 位置之前的最长相等前后缀的下一个位置，继续进行比较。
  - 最终结果：当遍历完整个模式串后，$pmt(next)$ 数组中存储的就是模式串的部分匹配表。
- 示例

  - 以模式串 `“ababaca”` 为例：
  - 首先初始化 $pmt[0] = 0$。
    - 对于 $i = 1$，即字符 `“b”`，其前缀子串 `“a”` 不存在相等前后缀，所以 $pmt[1] = 0$。
    - 对于 $i = 2$，字符 `“a”`，前缀子串 `“ab”` 也不存在相等前后缀，$pmt[2] = 0$。
    - 对于 $i = 3$，字符 `“b”`，此时前缀子串 `“aba”`，发现前缀 `“a”` 和后缀 `“a”` 相等，所以 $pmt[3] = 1$。
    - 对于 $i = 4$，字符 `“a”`，前缀子串 `“abab”`，最长相等前后缀为 `“ab”`，所以 $pmt[4] = 2$。
    - 对于 $i = 5$，字符 `“c”`，先看 $j = pmt[4] = 2$，比较 `“c”` 和 `“b”` 不相等，再将 $j$ 更新为 $pmt[2 - 1] = 0$，此时 `“c”` 和 `“a”` 也不相等，所以 $pmt[5] = 0$。
    - 对于 $i = 6$，字符 `“a”`，先看 $j = pmt[5] = 0$，比较 `“a”` 和 `“a”` 相等，所以 $pmt[6] = 1$。
    - 最终得到的 PMT 数组为 `[0, 0, 0, 1, 2, 0, 1]`。														
- $next$ 数组生成代码
- 如果没明白随便用这组数据模拟一遍 $next$ 数组的生成过程：`ABACABAB			`

```cpp
	#include<bits/stdc++.h>
using namespace std;
const int N = 1010, MOD = 1e9+7;
const int P = 131;
int nxt[N];
int n,m,j = 0,i = 2;//Notice: i从2开始长度为0和1的串共同前后缀为零
char s[N];
int main() {
	cin>>n>>s+1;
	while(i <= n){
		while(s[j+1] != s[i] and j != 0) j = nxt[j];//如果不匹配就根据next移动
        //如果匹配就j++
		if(s[i] == s[j+1]) j++;
		nxt[i] = j; //因为数组从1开始计算，所以j的大小就是公共前后缀的长度
		i++;//注意这里i++
	}
	for(int i = 1;i <= strlen(s+1);i++){
		cout<<nxt[i]<<endl;
	}
	return 0;
}
```

## 查找

- 如果当前字符匹配，那就继续进行，否则就把首个模式串的位置设置为当前位置的 $next$ 数组对应的值的下标。(咕咕咕)

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 1010, MOD = 1e9+7;
const int P = 131;

char p[N], s[N]; //p=Pattern
int nxt[N];
int n, m;
/*
abacdaba
00100123


*/
int main() {
	scanf("%d%s%d%s", &n, p + 1, &m, s + 1);
	for (int i = 2, j = 0; i <= n; i++) {
		while (j and p[i] != p[j + 1]) j = nxt[j];
		if (p[i] == p[j + 1]) j++;
		nxt[i] = j;
	}
	for (int i = 1, j = 0; i <= m; i++) {
		while (j != 0 and s[i] != p[j + 1]) {//不匹配就根据next移动
			j = nxt[j];
		}
		if (s[i] == p[j + 1]) j++;
		if (j == n) {
			printf("%d ", i - n + 1);
			j = nxt[j];
		}
	}
	return 0;
}
```

### 问题 A: 【一本通提高篇KMP】剪花布条 P3375 【模板】KMP

#### 解法

- 就是板子

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 1010, MOD = 1e9+7;
const int P = 131;

char p[N], s[N]; //p=Pattern
int nxt[N];
int n, m;
/*
abacdaba
00100123


*/
int main() {
	while (1) {
		scanf("%s",  s + 1);
		if (s[1] == '#') return 0;
		scanf("%s",p+1);
		int ans = 0;
		n = strlen(p + 1);
		m = strlen(s + 1);
		for (int i = 2, j = 0; i <= n; i++) {
			while (j and p[i] != p[j + 1]) j = nxt[j];
			if (p[i] == p[j + 1]) j++;
			nxt[i] = j;
		}
		for (int i = 1, j = 0; i <= m; i++) {
			while (j != 0 and s[i] != p[j + 1]) {//不匹配就根据next移动
				j = nxt[j];
			}
			if (s[i] == p[j + 1]) j++;
			if (j == n) {
				ans++;
				j = 0;//注意这里j为０，而KMP算法中这里是j=F[j-1]+1，因为一块花纹不能重复出现在多条小饰条上
			}
		}
		printf("%d\n",ans);
	}
	return 0;
}
```

### P4391 [BalticOI 2009]【一本通提高篇KMP】Radio Transmission(BZOJ1355)

#### 解法

就是利用next数组的离奇关系来解决.

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 1010000, MOD = 1e9+7;
const int P = 131;

char p[N], s[N]; //p=Pattern
int nxt[N];
int n, m;
/*
abacdaba
00100123


*/
int main() {

	scanf("%d%s", &n, p + 1);
	for (int i = 2, j = 0; i <= n; i++) {
		while (j and p[i] != p[j + 1]) j = nxt[j];
		if (p[i] == p[j + 1]) j++;
		nxt[i] = j;
	}
	printf("%d\n", n-nxt[n]);

	return 0;
}
```
