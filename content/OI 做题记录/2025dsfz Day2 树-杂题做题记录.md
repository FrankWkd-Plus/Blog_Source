---
title: 2025dsfz Day2 树-杂题做题记录
date: 2025-07-21
tags:
  - dsfz
  - 做题记录
  - 数据结构
  - OI
licenseName: CC-BY-NC-SA 4.0
---
# 树的DFS遍历  
- 先用链式前向星存，然后DFS遍历一遍。  
板子如下：  
```cpp  
#include <bits/stdc++.h>  
using namespace std;  
int n,x,y,z,pre[110],k;  
struct node{  
	int to,len,nxt;  
}a[220];  
void add(int u,int v,int l){  
	a[++k] = {v,l,pre[u]};  
	pre[u] = k;  
}  
void dfs(int x,int fa){  
	for(int i = pre[x];i;i = a[i].nxt){  
		if(a[i].to != fa) dfs(a[i].to,x);  
	}  
}  
int main(){  
	cin>>n;  
	for(int i = 1;i < n;i++){  
		cin>>x>>y>>z;  
		add(x,y,z);  
		add(y,x,z);  
	}  
	dfs(1,0/*！Tips!*/);  
}  
```  
## P1364 医院设置  
- 我们定义 `int dfs(int x,int fa,int dis)` 计算路径和。$dis$ 代表从起点到当前节点的边权和。  
```cpp  
#include <bits/stdc++.h>  
using namespace std;  
int n, x, y, z, pre[110], val[110], k;  
struct node {  
	int to, len, nxt;  
} a[220];  
void add(int u, int v, int l) {  
	a[++k] = {v, l, pre[u]};  
	pre[u] = k;  
}  
int dfs(int x, int fa, int dis) {  
	int res = dis * val[x];  
	for (int i = pre[x]; i; i = a[i].nxt) {  
		if (a[i].to != fa) {  
			res += dfs(a[i].to, x, dis + a[i].len);  
		}  
	}  
	return res;  
}  
int main() {  
	cin >> n;  
	for (int i = 1; i <= n; i++) {  
		int lc, rc; //left connect,right connect  
		cin >> val[i] >> lc >> rc;//猎奇输入  
		if (lc) {  
			add(i, lc, 1);  
			add(lc, i, 1);  
		}  
		if (rc) {  
			add(i, rc, 1);  
			add(rc, i, 1);  
		}  
	}  
	int ans = INT_MAX;  
	for (int i = 1; i <= n; i++) {  
		ans = min(ans, dfs(i, 0, 0));  
	}  
	cout<<ans<<endl;  
}  
```  
## 树的直径  
## 做法 1. 两次 DFS  
  
### 过程  
  
首先从任意节点 $y$ 开始进行第一次 DFS，到达距离其最远的节点，记为 $z$，然后再从 $z$ 开始做第二次 DFS，到达距离 $z$ 最远的节点，记为 $z'$，则 $\delta(z,z')$ 即为树的直径。  
  
显然，如果第一次 DFS 到达的节点 $z$ 是直径的一端，那么第二次 DFS 到达的节点 $z'$ 一定是直径的一端。我们只需证明在任意情况下，$z$ 必为直径的一端。  
  
定理：在一棵树上，从任意节点 $y$ 开始进行一次 DFS，到达的距离其最远的节点 $z$ 必为直径的一端。  
  
### 证明  
> OI Wiki  
  
![image](https://img2024.cnblogs.com/blog/3594125/202507/3594125-20250721090744188-430348748.png)  
  
  
  
### Warning 负权边  
上述证明过程建立在所有路径均不为负的前提下。如果树上存在负权边，则上述证明不成立。故若存在负权边，则无法使用两次 DFS 的方式求解直径。  
```cpp  
#include <bits/stdc++.h>  
using namespace std;  
  
struct node {  
	int to, nxt;  
} a[4001010];  
  
int dep[2000010];  
int pre[2000010], k, n;  
int x, y;  
void add(int u, int v) {  
	a[++k] = {v, pre[u]};  
	pre[u] = k;  
}  
void dfs(int x, int fa) {  
	  
	for (int i = pre[x]; i; i = a[i].nxt) {  
		int to = a[i].to;  
		if (to != fa) {  
			dep[to] = dep[x] + 1;  
			dfs(to, x);  
		}  
	}  
}  
  
int main() {  
	cin >> n;  
	for (int i = 1; i < n; i++) {  
		cin >> x >> y;  
		add(x, y);  
		add(y, x);  
	}  
	dep[1] = 0;  
	dfs(1, 0);  
	int ma = 0, mai;  
	for (int i = 1; i <= n; i++) {  
		if (dep[i] > ma) {  
			ma = dep[i];  
			mai = i;  
		}  
	}  
	dep[mai] = 0;  
	dfs(mai, 0);  
	int maxx = 0;  
	for (int i = 1; i <= n; i++) {  
		maxx = max(maxx, dep[i]);  
	}  
	cout << maxx;  
}  
```  
## 求先序序列  
- 递归水过，后序的最后一个字母就是当前的根节点，然后在中序中以根节点为中心分成两个子树再进行递归即可。  
```cpp  
#include <bits/stdc++.h>  
using namespace std;  
  
struct node {  
	int to, nxt;  
} a[4001010];  
  
int dep[2000010];  
int pre[2000010], k, n;  
int x, y;  
void add(int u, int v) {  
	a[++k] = {v, pre[u]};  
	pre[u] = k;  
}  
void dfs(int x, int fa) {  
	  
	for (int i = pre[x]; i; i = a[i].nxt) {  
		int to = a[i].to;  
		if (to != fa) {  
			dep[to] = dep[x] + 1;  
			dfs(to, x);  
		}  
	}  
}  
  
int main() {  
	cin >> n;  
	for (int i = 1; i < n; i++) {  
		cin >> x >> y;  
		add(x, y);  
		add(y, x);  
	}  
	dep[1] = 0;  
	dfs(1, 0);  
	int ma = 0, mai;  
	for (int i = 1; i <= n; i++) {  
		if (dep[i] > ma) {  
			ma = dep[i];  
			mai = i;  
		}  
	}  
	dep[mai] = 0;  
	dfs(mai, 0);  
	int maxx = 0;  
	for (int i = 1; i <= n; i++) {  
		maxx = max(maxx, dep[i]);  
	}  
	cout << maxx;  
}  
```  
## P1827 [USACO3.4] 美国血统 American Heritage  
- 跟上一题同理。  
```cpp  
#include <bits/stdc++.h>  
using namespace std;  
void dfs(string s1, string s2) {  
	if (s1.size() <= 0) return ;  
	char mid = s2[0];//root	  
	  
	int div = s1.find(mid);  
	dfs(s1.substr(0, div), s2.substr(1, div));  
	dfs(s1.substr(div+1), s2.substr(div+1));  
	cout << mid;  
}  
int main() {  
	string s1, s2;  
	cin >> s1 >> s2;  
	dfs(s1, s2);  
}//  
```  
## [!Important] P2680 [NOIP 2015 提高组] 运输计划  
- 如果你没有思路建议看 [这个题解](https://www.luogu.com.cn/article/wmsdoy0p) ，这个写的很有实力。  
### 关于题解的梳理和总结： By DS  
#### **问题描述**  
给定一棵 $n$ 个节点的树和 $m$ 条运输路径，要求选择一条边将其权值置零，使得所有路径的最大耗时最小化。  
  
#### **算法思路**  
1. **二分答案框架**  
- 设当前检查的答案为 $T$，判断是否存在一条边，置零后所有路径长度 $\leq T$。  
- **关键性质**：所有长度 $> T$ 的路径必须**共用**被置零的边。  
  
2. **可行性检查（`check(T)`）**  
- **步骤1**：统计超限路径（长度 $> T$）的数量 $k$。  
- **步骤2**：用树上差分标记这些路径覆盖的边。  
- **步骤3**：检查是否存在边满足：  
- 被所有超限路径覆盖（覆盖次数 $= k$）。  
- 边权 $w \geq \max(\text{路径长度}) - T$。  
  
3. **技术实现**  
- **LCA计算**：倍增法预处理，单次查询 $O(\log n)$。  
- **路径长度公式**：  
$$  
\text{dis}(u, v) = \text{dist}[u] + \text{dist}[v] - 2 \times \text{dist}[\text{LCA}(u, v)]  
$$  
- **树上差分**：  
- 对路径 $(u, v)$：`cnt[u]++`, `cnt[v]++`, `cnt[LCA(u,v)] -= 2`。  
- DFS汇总后，`cnt[x]` 表示边 $(\text{父节点}, x)$ 被覆盖的次数。  
  
#### **复杂度分析**  
- **预处理**：  
- LCA倍增表：$O(n \log n)$ 时间，$O(n \log n)$ 空间。  
- 路径长度计算：$O(m \log n)$。  
- **单次 `check`**：$O(n + m)$。  
- **总复杂度**：$O((n + m) \log T)$，其中 $T$ 为最大路径长度。  
  
#### **关键优化**  
1. **二分边界**：  
- 下界 $L = 0$，上界 $R = \max(\text{路径长度})$。  
- 每次调整：  
$$  
\begin{cases}  
R = \text{mid} - 1 & \text{if } \text{check}(\text{mid}) = \text{true}, \\  
L = \text{mid} + 1 & \text{otherwise}.  
\end{cases}  
$$  
2. **差分数组重置**：  
- 每次 `check` 前清零，避免全局数组反复初始化。  
  
#### **正确性证明**  
- **充分性**：若存在满足条件的边，置零后所有路径长度 $\leq T$。  
- **必要性**：若 $T$ 可行，则必须存在这样的边。  
  
#### **注意事项**  
1. **根节点选择**：任意选根（如节点1），不影响LCA和路径计算。  
2. **边权处理**：通过子节点存储父边的权值（`dist[v] - dist[fa[v]]`）。  
3. **数据范围**：  
- $n, m \leq 3 \times 10^5$，需使用高效输入输出（快读快写）。  
  
---
## 卡常方法  
- ![](https://images.cnblogs.com/cnblogs_com/blogs/838391/galleries/2441761/o_250722010513_1.png)  
```cpp  
/*  
程序执行流程概述（核心逻辑：二分答案 + 树上差分 + LCA）：  
1. 输入处理：读取树的节点数、路径数，构建邻接表。  
2. 预处理阶段：  
a. 第一次DFS（dfs）：计算节点深度、父节点（倍增基础），并将边权"下沉"到子节点（用子节点唯一标识边）。  
b. 构建倍增表（f数组）：预处理每个节点的2^j级祖先，用于快速查询LCA。  
c. 第二次DFS（dfs_dist）：计算根节点到所有节点的距离（distRoot数组），用于快速计算路径长度。  
3. 预计算路径信息：读取每条路径的起点/终点，计算其LCA和原始长度（pathLen数组），记录最大路径长度（用于二分边界）。  
4. 二分答案：通过二分法寻找**最小的最大路径长度T**，使得存在一条边置零后，所有路径长度≤T。  
a. 可行性检查（check函数）：  
i. 统计超限路径（长度> T的路径）。  
ii. 用树上差分标记这些路径覆盖的边。  
iii. 汇总差分数组，得到每条边的覆盖次数。  
iv. 检查是否存在边被所有超限路径覆盖，且边权足够大（置零后可使最大超限路径≤T）。  
5. 输出结果：输出最小的T。  
*/  
/*  
关于卡常：  
#1 头文件  
#2 for int -> for register int  
#3 void -> inline void  
#4 [!Important] 改二分逻辑（左边界）。见 https://images.cnblogs.com/cnblogs_com/blogs/838391/galleries/2441761/o_250722010513_1.png  
*/  
#include "bits/stdc++.h" //卡常#1  
using namespace std;  
const int N = 3e5 + 10; // 最大节点数（适配题目3e5的数据范围）  
const int L = 20;       // 倍增深度（2^20=1e6，足够覆盖3e5节点的深度）  
  
// 全局变量定义（按功能分类）  
// 1. LCA与树结构  
int f[N][L];         // 倍增数组：f[i][j]表示i的2^j级祖先  
int pre[N];          // 邻接表头指针：pre[i]指向i的第一条边  
int lg[N];           // 预处理log2值：lg[i] = floor(log2(i))，用于快速计算倍增层级  
int n, k, q;         // n：节点数；k：边计数；q：路径数  
int dep[N];          // 节点深度：dep[i]表示i的深度（根节点1的深度为1）  
  
// 2. 路径与差分  
int st[N], ed[N];    // 路径的起点/终点：st[i]是第i条路径的起点，ed[i]是终点  
long long pathLen[N];// 路径原始长度：pathLen[i]是第i条路径的长度（未置零任何边）  
int pathLCA[N];      // 路径的LCA：pathLCA[i]是第i条路径的起点与终点的LCA  
int mark[N];         // 差分数组：用于标记路径覆盖的边，汇总后表示边的覆盖次数  
  
// 3. 边权与距离  
int base[N];         // 边权下沉数组：base[i]表示i到父节点的边的权值（根节点1无父边，base[1]无意义）  
long long distRoot[N];// 根到节点的距离：distRoot[i]是根节点1到i的距离  
  
// 快读快写模板  
namespace FastIO {  
	char buf[1 << 20], *p1 = buf, *p2 = buf;  
	inline char gc() {  
		if (p1 == p2) p2 = (p1 = buf) + fread(buf, 1, 1 << 20, stdin);  
		return p1 == p2 ? EOF : *p1++;  
	}  
	template<typename T>  
	inline void read(T &x) {  
		x = 0;  
		bool neg = 0;  
		char ch = gc();  
		while (!isdigit(ch)) neg |= (ch == '-'), ch = gc();  
		while (isdigit(ch)) x = (x << 3) + (x << 1) + (ch ^ 48), ch = gc();  
		if (neg) x = -x;  
	}  
	inline void read(char *s) {  
		char ch = gc();  
		while (isspace(ch)) ch = gc();  
		while (!isspace(ch)) *s++ = ch, ch = gc();  
		*s = '\0';  
	}  
	char pbuf[1 << 20], *pp = pbuf;  
	inline void push(const char &c) {  
		if (pp - pbuf == (1 << 20)) fwrite(pbuf, 1, 1 << 20, stdout), pp = pbuf;  
		*pp++ = c;  
	}  
	template<typename T>  
	inline void write(T x) {  
		static int sta[40], top = 0;  
		if (x < 0) push('-'), x = -x;  
		do {  
			sta[top++] = x % 10;  
		} while (x /= 10);  
		while (top) push(sta[--top] + '0');  
	}  
	inline void flush() {  
		fwrite(pbuf, 1, pp - pbuf, stdout);  
		pp = pbuf;  
	}  
} using namespace FastIO;  
  
// 邻接表节点结构体（存储边信息）  
struct Node {  
	int to;   // 边的目标节点  
	int nxt;  // 下一条边的索引（链式存储）  
	int cost; // 边的权值  
} a[N * 2]; // 邻接表数组（无向树，每条边存两次，大小为2*N）  
  
// 添加边到邻接表（无向边，所以添加两次）  
// 参数：u（起点）、v（终点）、t（权值）  
inline void add(int u, int v, int t) {  
	a[++k] = {v, pre[u], t}; // 创建新边：目标v，下一条边是pre[u]，权值t  
	pre[u] = k;               // 更新u的邻接表头指针为当前边的索引  
}  
  
// 第一次DFS（建树）：计算深度、父节点（f[i][0]）、边权下沉（base数组）  
// 参数：x（当前节点）、fa（当前节点的父节点）  
inline void dfs(int x, int fa) {  
	dep[x] = dep[fa] + 1; // 当前节点深度=父节点深度+1（根节点1的父节点是0，dep[0]=0，所以dep[1]=1）  
	f[x][0] = fa;         // 当前节点的直接父节点（2^0级祖先）是fa  
	// 遍历x的所有邻接边（邻接表链式存储）  
	for (register int i = pre[x]; i; i = a[i].nxt) {  
		int to = a[i].to; // 边的目标节点  
		if (to == fa) continue; // 跳过父节点（避免循环）  
		base[to] = a[i].cost;   // 边权下沉：将边(x→to)的权值存到子节点to中（用to唯一标识这条边）  
		dfs(to, x);             // 递归处理子节点to，父节点是x  
	}  
}  
  
// 第二次DFS（计算距离）：计算根节点1到所有节点的距离（distRoot数组）  
// 参数：x（当前节点）、fa（当前节点的父节点）  
inline void dfs_dist(int x, int fa) {  
	// 遍历x的所有邻接边  
	for (register int i = pre[x]; i; i = a[i].nxt) {  
		int to = a[i].to; // 边的目标节点  
		if (to == fa) continue; // 跳过父节点（避免循环）  
		// 子节点to的距离=父节点x的距离 + 边(x→to)的权值（base[to]）  
		distRoot[to] = distRoot[x] + base[to];  
		dfs_dist(to, x); // 递归处理子节点to，父节点是x  
	}  
}  
  
// 倍增法查询LCA（最近公共祖先）：快速找到u和v的最近公共祖先  
// 参数：u、v（要查询的两个节点）  
// 返回值：u和v的LCA  
int lca(int u, int v) {  
	// 步骤1：调整u和v的深度，使u的深度≥v的深度（如果u比v浅，交换两者）  
	if (dep[u] < dep[v]) swap(u, v);  
	// 步骤2：将u提升到与v同一深度（用倍增法快速提升）  
	int depth_diff = dep[u] - dep[v]; // u和v的深度差  
	for (register int i = lg[depth_diff]; i >= 0; i--) { // 从最大的2^i开始尝试  
		if (dep[f[u][i]] >= dep[v]) { // 如果u的2^i级祖先的深度≥v的深度，就提升u  
			u = f[u][i];  
		}  
	}  
	// 步骤3：如果提升后的u等于v，说明v是u的祖先，直接返回v  
	if (u == v) return u;  
	// 步骤4：同步提升u和v，直到它们的祖先相同（此时祖先就是LCA）  
	for (register int i = L - 1; i >= 0; i--) { // 从最大的2^i开始尝试  
		if (f[u][i] != f[v][i]) { // 如果u和v的2^i级祖先不同，就提升它们  
			u = f[u][i];  
			v = f[v][i];  
		}  
	}  
	// 步骤5：u和v的直接父节点（2^0级祖先）就是它们的LCA  
	return f[u][0];  
}  
  
// 差分汇总DFS：自底向上累加差分数组mark，得到每条边的覆盖次数  
// 参数：x（当前节点）、fa（当前节点的父节点）  
// 说明：mark[x]表示边(fa[x]→x)被覆盖的次数（因为边权下沉到x）  
inline void dfs_sum(int x, int fa) {  
	// 遍历x的所有邻接边  
	for (register int i = pre[x]; i; i = a[i].nxt) {  
		int to = a[i].to; // 边的目标节点  
		if (to == fa) continue; // 跳过父节点（避免循环）  
		dfs_sum(to, x);     // 先递归处理子节点to（自底向上）  
		mark[x] += mark[to];// 将子节点to的mark值累加到x的mark值（边(to的父节点→to)的覆盖次数是mark[to]，而to的父节点是x）  
	}  
}  
  
// 可行性检查函数：判断是否存在一条边，置零后所有路径长度≤mid  
// 参数：mid（当前要检查的最大路径长度上限）  
// 返回值：true（存在这样的边）/ false（不存在）  
bool check(long long mid) {  
	memset(mark, 0, sizeof(mark)); // 重置差分数组（每次检查都要重新标记）  
	int cnt_over = 0;               // 超限路径计数：长度>mid的路径数量  
	long long max_l = 0;            // 最大超限路径长度：用于计算需要减少的长度  
  
	// 步骤1：标记所有超限路径（长度>mid的路径）  
	for (register int i = 1; i <= q; i++) {  
		if (pathLen[i] > mid) { // 如果第i条路径的长度超过mid  
			cnt_over++;         // 超限路径数量+1  
			max_l = max(max_l, pathLen[i]); // 更新最大超限路径长度  
			// 树上差分标记：路径(st[i]→ed[i])的起点、终点+1，LCA-2  
			// 原理：路径u-v的覆盖边是u到LCA和v到LCA的边（除了LCA本身），通过差分标记，汇总后mark[x]表示边(fa[x]→x)的覆盖次数  
			mark[st[i]]++;     // 起点st[i]的mark值+1  
			mark[ed[i]]++;     // 终点ed[i]的mark值+1  
			mark[pathLCA[i]] -= 2; // LCA的mark值-2（修正重复计数）  
		}  
	}  
  
	// 步骤2：判断是否有超限路径（没有的话，mid可行）  
	if (cnt_over == 0) {  
		return true;  
	}  
  
	// 步骤3：汇总差分数组，得到每条边的覆盖次数  
	dfs_sum(1, 0); // 从根节点1开始，自底向上汇总mark数组  
  
	// 步骤4：检查是否存在符合条件的边  
	// 遍历所有节点（从2开始，因为根节点1没有父边）  
	for (register int i = 2; i <= n; i++) {  
		// 条件1：边(fa[i]→i)被所有超限路径覆盖（mark[i] == cnt_over）  
		// 条件2：边的权值足够大（base[i] >= max_l - mid）  
		// 解释：max_l是最大超限路径长度，需要将其减少到<=mid，所以需要减少的长度是max_l - mid。如果边的权值≥这个值，置零该边后，max_l会减少base[i]，从而≤mid。  
		if (mark[i] == cnt_over && base[i] >= max_l - mid) {  
			return true; // 存在这样的边，mid可行  
		}  
	}  
  
	// 没有找到符合条件的边，mid不可行  
	return false;  
}  
  
// 主函数（程序入口）  
int main() {  
	long long max_edge = 0; // 最长边  
	// 步骤1：输入处理  
	read(n); // 读取节点数n  
	// 预处理lg数组（log2值）：lg[i] = floor(log2(i))  
	// 例如：lg[1] = 0，lg[2] = 1，lg[3] = 1，lg[4] = 2，依此类推  
	for (register int i = 2; i <= n; i++) {  
		lg[i] = lg[i >> 1] + 1; // i >> 1等价于i/2（整数除法）  
	}  
	read(q); // 读取路径数q  
	// 读取树的边信息，构建邻接表  
	for (register int i = 1; i < n; i++) { // 树有n-1条边  
		int x, y, t;  
		read(x); // 读取边的节点x  
		read(y); // 读取边的节点y  
		read(t); // 读取边的权值t  
		max_edge = max(max_edge, 1ll * t); // 更新最长边  
		add(x, y, t); // 添加边x→y（权值t）  
		add(y, x, t); // 添加边y→x（权值t，无向树）  
	}  
  
	// 步骤2：预处理阶段  
	dep[0] = 0; // 虚拟根节点0的深度为0（根节点1的父节点是0）  
	dfs(1, 0); // 第一次DFS：建树，处理深度、父节点、边权下沉  
	// 构建倍增表f[i][j]：f[i][j] = f[f[i][j-1]][j-1]（i的2^j级祖先等于i的2^(j-1)级祖先的2^(j-1)级祖先）  
	for (register int i = 1; i < L; i++) { // 遍历倍增的层级（从1到L-1）  
		for (register int j = 1; j <= n; j++) { // 遍历所有节点  
			f[j][i] = f[f[j][i - 1]][i - 1]; // 计算j的2^i级祖先  
		}  
	}  
	distRoot[1] = 0; // 根节点1到自身的距离为0  
	dfs_dist(1, 0); // 第二次DFS：计算根到每个节点的距离  
  
	// 步骤3：预计算路径信息  
	long long max_len = 0; // 所有路径中的最大长度（用于设置二分的右边界）  
	for (register int i = 1; i <= q; i++) {  
		read(st[i]); // 读取第i条路径的起点st[i]  
		read(ed[i]); // 读取第i条路径的终点ed[i]  
		pathLCA[i] = lca(st[i], ed[i]); // 计算第i条路径的LCA  
		// 计算第i条路径的长度：distRoot[st[i]] + distRoot[ed[i]] - 2 * distRoot[pathLCA[i]]  
		// 原理：路径st[i]-ed[i]的长度等于st[i]到根的距离加上ed[i]到根的距离，减去两倍的LCA到根的距离（LCA到根的距离被计算了两次）  
		pathLen[i] = distRoot[st[i]] + distRoot[ed[i]] - 2 * distRoot[pathLCA[i]];  
		max_len = max(max_len, pathLen[i]); // 更新最大路径长度  
	}  
  
	// 步骤4：二分答案（寻找最小的T）  
	long long l = max_len - max_edge; // 二分左边界（最小可能的T），详见 https://images.cnblogs.com/cnblogs_com/blogs/838391/galleries/2441761/o_250722010513_1.png  
	long long r = max_len; // 二分右边界（最大可能的T，即未置零任何边时的最大路径长度）  
	while (l <= r) { // 当左边界小于右边界时继续循环  
		long long mid = (l + r) >> 1; // 取中点（等价于(l + r)/2，位运算更快）  
		if (check(mid)) { // 如果mid可行（存在边置零后所有路径≤mid）  
			r = mid - 1; // 缩小右边界，尝试寻找更小的T  
		} else { // 如果mid不可行（不存在这样的边）  
			l = mid + 1; // 扩大左边界，寻找更大的T  
		}  
	}  
  
	// 步骤5：输出结果（此时l == r，是最小的T）  
	write(l); // 输出最小的最大路径长度  
	flush();  
	return 0; // 程序结束  
}  
```  
