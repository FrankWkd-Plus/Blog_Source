---
title: (倍增)LCA学习笔记
date: 2025-03-09
tags:
  - LCA
  - ST表
  - 倍增
  - 学习笔记
  - 做题记录
  - 算法
  - OI
licenseName: CC-BY-NC-SA 4.0
---

LCA指最长公共子序列,可以使用倍增的方法求解(复杂度较优)
## 步骤
- (1) 预处理
  - a. 求深度: 对于每个结点 $dfs$ 预处理出结点深度;
  - b. 求倍增祖先: 计算出每个结点向父元素跳 $2^0,2^1,2^2,...,2^k$ 步所到达的点( $2^k$ 指大于整棵树的最大深度)
    - (备注)a. $f[x][k]: x$ 上跳 $2$ 步能到达的祖先,如对应结点不存在 $f[x][k]=0$ .
    - (备注)b. 将路径长度 $2^k$ 分为两半, $x$ 跳 $2^k$ 次到的祖先 $=x$ 跳 $2^{k-2}$ 次到的祖先再次跳 $2^{k-2}$ 次.因此可得:  $f[x][k]=f[f[x][k-1]][k-1]$.
  - c. 预处理时间复杂度为 $O(nlog_2n)$
- (2)查询
  - 对于多次查询结点 $×$ 和 $y4 的公共祖先．
  a.  如果 $x$ 的深度 $<y$ 的深度，则交换 $xy$ ，使得 $x$ 更深；
  b．采用二进制拆分的思想，根据两个结点的深度差，让 $x$ 快速跳转到和 $y$ 一样深；
  c.  如果此时 $x==y$，可得 $LCA$
  d.  如果不相等，采用二进制拆分的思想，让 $xy$ 倍增上跳，直到 $x$ 的父和 $y$ 的父相遇
(因为跳出根以上，$f[x][k]=0$，因此如果让 $x$ 和 $y$ 相遇，可能结果为 $0$ ) ，可得 $LCA$ ;
  备注：单次查询的时间复杂度为 $O(log_2N)$。
- (2) 查询步骤:
  - a. x y两个结点中较深的点,倍增向上跳到两个结点深度相同;
  - b. 若相遇,得 $LCA$ ,否则 $x,y$ 同时向上倍增上跳直到 $xy$ 的父相遇.


## A. 树的公共祖先(LCA)(3)

### 题目描述

给定一棵 $n$ 个结点的树（结点标号 $1 \dots n$ ）以及树中结点的边，结点 $s$ 为树的根。

有 $m$ 次询问，请求出每次询问的两个结点 $x$ 和 $y$ 的最近的公共祖先结点。

### 输入

第 $1$ 行输入 $3$ 个整数 $n$、$m$、$s$（$n≤500000$，$m≤500000$，$1≤s≤n$）；

接下来 $n-1$ 行，每行两个整数 $a$ 和 $b$ ，结点 $a$ 和 $b$ 是父子关系，但**不保证 $a$ 是$b$ 的父**，数据保证一定能构成树；

接下来 $m$ 行，每行两个整数 $x$，$y$，表示要求出 $x$ 和 $y$ 结点的公共祖先。

### 输出

输出 $m$ 行，每行一个整数，表示 $m$ 次询问求出的结果。

### 样例

输入
```
5 5 4
3 1
2 4
5 1
1 4
2 4
3 2
3 5
1 2
4 5
```
输出
```
4
4
1
4
4
```

### 解法
- LCA板子题,直接套模版,注意 $i,j$不要写错啦
```cpp
#pragma GCC optimize(3)
#include <bits/stdc++.h>
using namespace std;
const int N = 5e5+10,L = 20;
int pre[N],fa[N][L],dep[N],lg[N];

struct node {
	int to,nxt;
} a[N*2]; //!!!

int n,m,s,x,y,k;

void add(int u,int v) {
	a[++k] = {v,pre[u]};
	pre[u] = k;
}

void dfs(int x,int fath) {
	dep[x] = dep[fath]+1;
	fa[x][0] = fath;
	for(int i = pre[x]; i; i = a[i].nxt) {
	    int to = a[i].to;
		if(to != fath) {
			dfs(to,x);
		}
	}
}

int lca(int u,int v){
    if(dep[u] < dep[v]) swap(u,v);//同一口径
    while(dep[u] != dep[v]){
        int k = lg[dep[u]-dep[v]];
        u = fa[u][k];//更新u使得uv更加的近
    }
    if(u == v) return u;
    for(int i = L-1;i >= 0;i--){
        if(fa[u][i] != fa[v][i]){
            u = fa[u][i];
            v = fa[v][i];
        }
    }
    return fa[u][0];
}

int main() {
    scanf("%d%d%d",&n,&m,&s);
	for(int i = 1; i <= n-1; i++) {//n个点n-1条边
		scanf("%d%d",&x,&y);
		add(x,y);
		add(y,x);
	}
	dfs(s,0);
	for(int i = 2;i <= n;i++) lg[i] = lg[i>>1]+1;
	for(int i = 1;i < L;i++){
	    for(int j = 1;j <= n;j++){
	        fa[j][i] = fa[fa[j][i-1]][i-1];//Important!!
	    }
	}
	while(m--){
	    scanf("%d%d",&x,&y);
	    printf("%d\n",lca(x,y));
	}
}
```
## B.【一本通提高倍增LCA】点的距离
#### 题目描述

给定一棵树，树上有 $n$ 个结点，结点编号为 $1 \sim n$ 。

再给定 $q$ 次询问，每次询问给定 $2$ 个结点编号，请编程计算出这两个结点在树上的距离。

#### 输入

第一行一个正整数 $n$，表示结点数量；

接下来 $n−1$ 行，每行两个整数 $x,y$ 表示 $x,y$ 之间有一条连边；

然后一个整数 $q$，表示有 $q$ 个询问；

接下来 $q$ 行每行两个整数 $x,y$ 表示询问 $x$ 到 $y$ 的距离。

#### 数据范围：

$1≤n,q≤10$<sup><span><span><span id="8afd34fe-310b-45bb-8e11-2355b8b2289d"><math xmlns="http://www.w3.org/1998/Math/MathML"><semantics><mrow><mn>5</mn></mrow><annotation encoding="application/x-tex">5</annotation></semantics></math></span></span></span></sup>，$1≤x,y≤n$；

#### 输出

输出 $q$ 行，每行一个整数，表示每次询问的答案。

#### 样例

##### 输入
```
6
1 2
1 3
2 4
2 5
3 6
2
2 6
5 6
```
##### 输出
```
3
4
```
### 解法
树上任意两个点 $u$、$v$的距离 $=dep[u]+dep[v]-2*dep[lca(u,v)]$
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1e5+20, L = 20;
struct node {
	int to, nxt;
} a[N * 2];
int n, m, fa[N][L], pre[N], k, dep[N], lg[N];
void add(int u, int v) {
	a[++k] = {v, pre[u]};
	pre[u] = k;
}
void dfs(int x, int fath) {
	dep[x] = dep[fath] + 1;
	fa[x][0] = fath;
	for (int i = pre[x]; i; i = a[i].nxt) {
		int to = a[i].to;
		if (to != fath) {
			dfs(to, x);
		}
	}
}
int LCA(int u, int v) {
	if (dep[u] < dep[v]) swap(u, v); //u永远>=v
	while (dep[u] > dep[v]) {
		u = fa[u][lg[dep[u] - dep[v]]];
		// 宗旨：要让大的序号变成小的序号 
	}
	if (u == v) return u;
	for (int i = L - 1; i >= 0; i--) {
		if (fa[u][i] != fa[v][i]) {
			u = fa[u][i];
			v = fa[v][i];
		}
	}
	return fa[u][0];//返回父节点
}
int main() {
	scanf("%d", &n);
	for (int i = 1; i < n; i++) {
		int x, y;
		scanf("%d%d", &x, &y);
		add(x, y);
		add(y, x);
	}
	dfs(1, 0);
	for (int i = 2; i <= n; i++) lg[i] = lg[i >> 1] + 1;
	for (int i = 1; i < L; i++) {
		for (int j = 1; j <= n; j++) {
			fa[j][i] = fa[fa[j][i - 1]][i - 1];
		}
	}
	scanf("%d", &m);
	while (m--) {
		int x, y;
		scanf("%d%d", &x, &y);
		int lc = LCA(x, y);
//		cout << "LCA: " << lc << endl;
		printf("%d\n",dep[x] - dep[lc] + dep[y] - dep[lc]);
	}
}
```
## C. P5836 [USACO19DEC] Milk Visits S

### 题目描述

Farmer John 计划建造 $N$ 个农场，用 $N-1$ 条道路连接，构成一棵树（也就是说，所有农场之间都互相可以到达，并且没有环）。每个农场有一头奶牛，品种为更赛牛或荷斯坦牛之一。

Farmer John 的 $M$ 个朋友经常前来拜访他。在朋友 $i$ 拜访之时，Farmer John 会与他的朋友沿着从农场 $A_i$ 到农场 $B_i$ 之间的唯一路径行走（可能有 $A_i = B_i$）。除此之外，他们还可以品尝他们经过的路径上任意一头奶牛的牛奶。由于 Farmer John 的朋友们大多数也是农场主，他们对牛奶有着极强的偏好。他的有些朋友只喝更赛牛的牛奶，其余的只喝荷斯坦牛的牛奶。任何 Farmer John 的朋友只有在他们访问时能喝到他们偏好的牛奶才会高兴。

请求出每个朋友在拜访过后是否会高兴。

### 输入格式

输入的第一行包含两个整数 $N$ 和 $M$。

第二行包含一个长为 $N$ 的字符串。如果第 $i$ 个农场中的奶牛是更赛牛，则字符串中第 $i$ 个字符为 `G`，如果第 $i$ 个农场中的奶牛是荷斯坦牛则为 `H`。

以下 $N-1$ 行，每行包含两个不同的整数 $X$ 和 $Y$（$1 \leq X, Y \leq N$），表示农场 $X$ 与 $Y$ 之间有一条道路。

以下 $M$ 行，每行包含整数 $A_i$，$B_i$，以及一个字符 $C_i$。$A_i$ 和 $B_i$ 表示朋友 $i$ 拜访时行走的路径的端点，$C_i$ 是 `G` 或 `H` 之一，表示第 $i$ 个朋友喜欢更赛牛的牛奶或是荷斯坦牛的牛奶。

### 输出格式

输出一个长为 $M$ 的二进制字符串。如果第 $i$ 个朋友会感到高兴，则字符串的第 $i$ 个字符为 `1`，否则为 `0`。

### 输入输出样例 #1

#### 输入 #1

```
5 5
HHGHG
1 2
2 3
2 4
1 5
1 4 H
1 4 G
1 3 G
1 3 H
5 5 H
```

#### 输出 #1

```
10110
```

### 说明/提示

在这里，从农场 1 到农场 4 的路径包括农场 1、2 和 4。所有这些农场里都是荷斯坦牛，所以第一个朋友会感到满意，而第二个朋友不会。

关于部分分：

测试点 $1$ 样例。

测试点 $2\sim 5$ 满足 $N\le 10^3$，$M\le 2\cdot 10^3$。

对于 $100\%$ 的数据，$1 \leq N \leq 10^5$，$1 \leq M \leq 10^5$。

供题：Spencer Compton
### 解法
我们在 $dfs$ 时记录一下当前结点到根结点所经过的路线的GH数量,然后根据前缀和公式直接求解.
$xy$ 之间的 $H$ 的数量 $=$ $x$ 到根结点的 $H$ 的数量(算上起点和终点) $+$ $y$ 到根结点的 $H$ 的数量(算上起点和终点) $-$ $2\times $ 他们的公共祖先到根结点的 $H$ 的数量(算上起点和终点)+公共祖先是否为 $H$ (是$=1$,否$=0$).
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 5e5+10,L = 20;
struct node {
	int to,nxt;
} a[N*2];
int lg[N],pre[N],fa[N][L],dep[N],h[N],g[N];
char s[N];
int n,m,k;
void add(int u,int v) {
	a[++k] = {v,pre[u]};
	pre[u] = k;
}
void dfs(int x,int fath) {
	fa[x][0] = fath;//注意这里是fath! 2^0=1!!
	dep[x] = dep[fath]+1;
	g[x] = g[fath] + (s[x] == 'G');
	h[x] = h[fath] + (s[x] == 'H');
	for(int i = pre[x]; i; i = a[i].nxt) {
		int to = a[i].to;
		if(to != fath) {
			dfs(to,x);
		}
	}
}
int lca(int u,int v) {
	if(dep[u] < dep[v]) swap(u,v);
	while(dep[u] > dep[v]) {
		u = fa[u][lg[dep[u]-dep[v]]];//注意是u-v还是v-u!(注意大小关系)
	}
	if(u == v) return u;
	for(int i = L-1; i >= 0; i--) {
		if(fa[u][i] != fa[v][i]) {
			u = fa[u][i];
			v = fa[v][i];
		}
	}
	return fa[u][0];
}
int main() {
    int x,y;
	scanf("%d%d",&n,&m);
	scanf("%s",s+1);
	for(int i = 1; i <= n-1; i++) {
		scanf("%d%d",&x,&y);
		add(y,x);
		add(x,y);
	}
	dfs(1,0);
	for(int i = 2; i <= n; i++) lg[i] = lg[i>>1]+1;
	for(int i = 1; i < L; i++) {//注意从1开始!0已经在dfs中被赋值过了! 与RMQ正好相反QwQ
		for(int j = 1; j <= n; j++) {
			fa[j][i] = fa[fa[j][i-1]][i-1];
		}
	}
	int c1,c2,r;
	char q[2];
	for(int i = 1; i <= m; i++) {
		scanf("%d%d%s",&x,&y,&q);
		r = lca(x,y);
		c1 = h[x]+h[y]-2*h[r]+(s[r] == 'H');
		c2 = g[x]+g[y]-2*g[r]+(s[r] == 'G');
		if(q[0] == 'H' and c1 > 0){
		    printf("1");
		    goto endd;
		}
		if(q[0] == 'G' and c2 > 0){
		    printf("1");
		    goto endd;
		}
		printf("0");
		endd:;
	}
}
```

## D.distance树上距离
[题面Link](https://loj.ac/p/10134)
#### 解法:
需要在板子的基础上加一个前缀和数组记录从根节点到当前结点的距离.然后就变成B题了
```cpp
#pragma GCC optimize(2)
#pragma GCC optimize(3)
#include <bits/stdc++.h>
using namespace std;

const int N = 1e5+10, L = 20;
int fa[N][L], n, m, k, pre[N], lg[N], s, dep[N], val[N];
struct node {
	int to, next, value;
} a[N * 2];
int x, y, z;

void add(int u, int v, int w) {
	a[++k] = {v, pre[u], w};
	pre[u] = k;
}
void dfs(int x, int fath) {
	dep[x] = dep[fath] + 1;
	fa[x][0] = fath;
	for (int i = pre[x]; i; i = a[i].next) {
		int to = a[i].to;
		if (to != fath) {
			val[to] = val[x] + a[i].value;
			dfs(to, x);
		}
	}
}
int lca(int u, int v) {
	if (dep[u] < dep[v]) swap(u, v);
	while (dep[u] > dep[v]) {
		u = fa[u][lg[dep[u] - dep[v]]];
	}
	if (u == v) return u;
	for (int i = L - 1; i >= 0; i--) {
		if (fa[u][i] != fa[v][i]) {
		    // 他们要移动到的结点的父不相同
			u = fa[u][i];
			v = fa[v][i];
		}
	}
	return fa[u][0];
}
int main() {
	ios::sync_with_stdio(false);
	cin.tie(0);
	cout.tie(0);
	cin >> n >> m;
	for (int i = 1; i < n; i++) {
		cin >> x >> y >> z;
		add(x, y, z);
		add(y, x, z);
	}
	dfs(1, 0);
	for (int i = 2; i <= n; i++) lg[i] = lg[i >> 1] + 1;
	for (int i = 1; i < L; i++) {
		for (int j = 1; j <= n; j++) {
			fa[j][i] = fa[fa[j][i - 1]][i - 1];
		}
	}
	while (m--) {
		cin >> x >> y;
		cout << val[x] + val[y] - 2 * val[lca(x, y)] << "\n";
	}
}
```
## E. 大量的工作沟通

### 题目描述

某公司有 $N$ 名员工，编号从 $0$ 至 $N-1$。其中，除了 $0$ 号员工是老板，其余每名员工都有一个直接领导。我们假设 编号为 $i$ 的员工的直接领导是 $f_i$。

该公司有严格的管理制度，每位员工只能受到本人或直接领导或间接领导的管理。具体来说，规定员工 $x$ 可以管理员工 $y$，当且仅当 $x=y$，或 $x=f_y$，或 $x$ 可以管理 $f_y$。特别地，$0$ 号员工老板只能自我管理，无法由其他任何员工管理。

现在，有一些同事要开展合作，他们希望找到一位同事来主持这场合作，这位同事必须能够管理参与合作的所有同事。如果有多名满足这一条件的员工，他们希望找到编号最大的员工。你能帮帮他们吗？

### 输入

第一行一个整数 $N$，表示员工的数量。

第二行 $N-1$ 个用空格隔开的正整数，依次为 $f_1,f_2,...,f _ {N-1}$。

第三行一个整数 $Q$，表示共有 $Q$ 场合作需要安排。

接下来 $Q$ 行，每行描述一场合作：开头是一个整数 $m$（ $2 \leq m \leq N$ ），表示参与本次合作的员工数量；接着是 $m$个整数，依次表示参与本次合作的员工编号（保证编号合法且不重复）。

保证公司结构合法，即不存在任意一名员工，其本人是自己的直接或间接领导。

### 输出

输出 $Q$ 行，每行一个整数，依次为每场合作的主持人选。
### 解法
我们直接从大到小求所有数的 $LCA$ ,然后记住要向上爬树直到根节点,求他们中的最大值然后输出!
```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1e6 + 20, L = 18;
int lg[N], pre[N], fa[N][L], dep[N], b[N];
int n, m, x, y, z, k = 0;

struct node {
	int to, nxt, fr;
} a[N * 2];

void add(int u, int v) {
	a[++k] = {v, pre[u], u};
	pre[u] = k;
}

void dfs(int x, int fath) {
	dep[x] = dep[fath] + 1;
	fa[x][0] = fath;
	for (int i = pre[x]; i; i = a[i].nxt) {
		int to = a[i].to;
		if (to != fath) {
			dfs(to, x);
		}
	}
}

bool cmp(int i, int j) {
	return i > j;
}

int lca(int u, int v) {
	if (dep[u] < dep[v])
		swap(u, v);
	while (dep[u] != dep[v]) {
		u = fa[u][lg[dep[u] - dep[v]]];
	}
	if (u == v)
		return u;
	for (int i = L - 1; i >= 0; i--) {
		if (fa[u][i] != fa[v][i]) {
			u = fa[u][i];
			v = fa[v][i];
		}
	}
	return fa[u][0];
}

int main() {
	scanf("%d", &n);
	for (int i = 1; i < n; i++) {
		scanf("%d", &x);
		add(i, x);
		add(x, i);
	}
	dfs(0, 0);
	for (int i = 2; i <= n; i++)
		lg[i] = lg[i >> 1] + 1;
	for (int i = 1; i < L; i++) {
		for (int j = 1; j <= n; j++) {
			fa[j][i] = fa[fa[j][i - 1]][i - 1];
		}
	}
	scanf("%d", &m);
	while (m--) {
		scanf("%d", &x);
		for (int i = 1; i <= x; i++)
			scanf("%d", &b[i]);
		sort(b + 1, b + 1 + x, cmp);
		int l = b[1];
		for (int i = 2; i <= x; i++) {
			l = lca(l, b[i]);
		}
		int ma = l;
		while (l != 0) {
			for (int i = pre[l]; i; i = a[i].nxt) {
				if (dep[a[i].to] < dep[l]) {
					ma = max(a[i].to, ma);
					l = a[i].to;
					goto endd;
				}
			}
            endd:;
		}
		printf("%d\n", ma);

	}
}
```
