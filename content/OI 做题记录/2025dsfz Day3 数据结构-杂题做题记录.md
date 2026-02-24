---
title: 2025dsfz Day3 数据结构-杂题做题记录
date: 2025-07-21
tags:
  - dsfz
  - 做题记录
  - 数据结构
  - OI
licenseName: CC-BY-NC-SA 4.0
---
# RMQ  
- 之前博客有过  
## 【一本通提高篇RMQ】数列区间最大值 & 【省选基础数据结构 RMQ】[Usaco2007 Jan]Balanced Lineup排队  
- 两个题都是RMQ板子，直接求就行，第二题需要求两遍。  
#### Code【一本通提高篇RMQ】数列区间最大值  
```cpp  
#pragma GCC optimize(3)  
#include <bits/stdc++.h>  
using namespace std;  
const int N = 1e6+10, L = 20;  
int f[N][L];  
int a[N], n, k, lg[N];  
int l, r;  
  
int main() {  
	ios::sync_with_stdio(0);  
	cin.tie(0);  
	cout.tie(0);  
	cin >> n >> k;  
	for (int i = 1; i <= n; i++) {  
		cin >> a[i];  
		f[i][0] = a[i];  
	}  
	for (int i = 2/*!!!Danger!!!*/; i <= n; i++) lg[i] = lg[i >> 1] + 1;  
	for (int i = 1; i < L; i++) {  
		for (int j = 1; j + (1 << i) -1/*!!Warning!!: 这里要提前考虑位移后的下标是否越界*/ <= n; j++) {  
			f[j][i] = max(f[j][i - 1], f[j + (1 << i - 1)][i - 1]);  
		}  
	}  
	for (int i = 1; i <= k; i++) {  
		cin >> l >> r;  
		cout << max(f[l][lg[r - l + 1]], f[r - (1 << lg[r - l + 1]) + 1][lg[r - l + 1]]) << '\n';  
	}  
}  
```  
#### Code【省选基础数据结构 RMQ】[Usaco2007 Jan]Balanced Lineup排队  
```cpp  
#pragma GCC optimize(3)  
#include <bits/stdc++.h>  
using namespace std;  
const int N = 5e4+20, L = 20;  
int f[N][L], n, a[N], fi[N][L], lg[N], q;  
  
int main() {  
	ios::sync_with_stdio(0);cin.tie(0);cout.tie(0);  
	cin >> n >> q;  
	for (int i = 2; i <= n; i++) {  
		lg[i] = lg[i >> 1] + 1;  
	}  
	for (int i = 1; i <= n; i++) {  
		cin >> a[i];  
	}  
	for (int i = 0; i < L; i++) {  
		for (int j = 1; j + (1 << i) - 1/*!!!!Exsiting Danger!!!!: 注意j的范围！考虑到下面状态转换的公式j+(1<<i-1),但是在这个下标的基础上又右移了2^(i-1)位！*/ <= n; j++) {  
			if (i == 0) {  
				f[j][0] = a[j];  
				fi[j][0] = a[j];  
			} else {  
				f[j][i] = max(f[j][i - 1], f[j + (1 << i - 1)][i - 1]);  
				fi[j][i] = min(fi[j][i - 1], fi[j + (1 << i - 1)][i - 1]);  
			}  
		}  
	}  
	for (int i = 1; i <= q; i++) {  
		int l, r;  
		cin >> l >> r;  
		int kkk = lg[r - l + 1];  
		cout << max(f[l][kkk], f[r - (1 << kkk) +1][kkk]) - min(fi[l][kkk], fi[r - (1 << kkk) +1][kkk]) << " ";  
	}  
}  
```  
# LCA  
- 之前博客有过  
## LCA 最近公共祖先  
- 纯模版  
```cpp  
#pragma GCC optimize(3)  
#include <bits/stdc++.h>  
using namespace std;  
const int N = 3e5 + 10, L = 20;  
int f[N][L], pre[N], k, n, m;  
int dep[N];  
int lg[N];  
struct node {  
    int to, nxt;  
} a[2 * N];  
  
void add(int u, int v) {  
    a[++k] = {v, pre[u]};  
    pre[u] = k;  
}  
void dfs(int x, int fa) {  
    dep[x] = dep[fa] + 1;  
    f[x][0] = fa;  
    for (int i = pre[x]; i; i = a[i].nxt) {  
        int to = a[i].to;  
        if (to != fa) {  
            dfs(to, x);  
        }  
    }  
}  
  
int lca(int u, int v) {  
    if (dep[u] < dep[v]) {  
        swap(u, v);  
    }  
    while (dep[u] > dep[v]) {  
        u = f[u][lg[dep[u] - dep[v]]];//!!!!Existing Danger!!!!: dep向上跳的时候是从大到小！！！  
    }  
    if (u == v)  
        return u;  
    for (int i = L - 1; i >= 0; i--) {  
        if (f[u][i] != f[v][i]) {  
            u = f[u][i];  
            v = f[v][i];  
        }  
    }  
    return f[u][0];  
}  
  
int main() {  
    scanf("%d",&n);  
	int root;  
    for (int i = 1; i <= n; i++) {  
        int x, y, z;  
        scanf("%d",&x);  
		if(x == 0) root = i;  
        add(x, i);  
        add(i, x);  
    }  
    dfs(root, 0);  
    for (int i = 2; i <= n; i++) {  
        lg[i] = lg[i >> 1] + 1;  
    }  
    for (int i = 1; i < L; i++) {  
        for (int j = 1; j <= n; j++) {  
            f[j][i] = f[f[j][i - 1]][i - 1];  
        }  
    }  
	scanf("%d",&m);  
    for (int i = 1; i <= m; i++) {  
        int x, y;  
        scanf("%d%d",&x,&y);  
	    printf("%d\n" /*Fuck acc*/ ,lca(x,y));  
    }  
}  
```  
# 分块入门  
- 反正是没太听懂  
- 可以见这篇题解，蛮不错的 [https://www.cnblogs.com/Foggy-Forest/p/13267943.html](https://www.cnblogs.com/Foggy-Forest/p/13267943.html)  
## Loj #6277. 数列分块入门 1  
```cpp  
/*  
 * > CPP Code Snippet <  
 * > Powered by Microsoft Visual Studio Code <  
 *  
 * @Author    FrankWKD (wangkedi01)  
 * @Date      2025-07-22  
 * @Network   "https://loj.ac/p/6277"  
 * @License   GNU General Public License 2.0  
 * @Platform  Win7 on dsfz  
 * @FileName  untitled1.cpp  
 * @FilePath  C:\Users\Administrator\Documents\untitled1.cpp  
 * @Solution  https://www.cnblogs.com/Foggy-Forest/p/13267943.html  
 */  
  
#pragma GCC optimize(3)  
#include <bits/stdc++.h>  
using namespace std;  
const int N = 5e4 + 5;  
int a[N], blk[N], sizB, tagA[N];  
void add(int l, int r, int val) {  
    if (blk[r] - blk[l] <= l) {  
        for (int i = l; i <= r; i++) a[i] += val;  
        return;  
    }  
    for (int i = l; i <= blk[l] * sizB; i++) a[i] += val;  
    for (int i = (blk[r] - 1) * sizB + 1; i <= r; i++) a[i] += val;  
    for (int i = blk[l] + 1; i <= blk[r] - 1; i++) {  
        tagA[i] += val;  
    }  
}  
int main() {  
    // freopen("sample.in","r",stdin);  
    // freopen("sample.out","w",stdout);  
    ios::sync_with_stdio(false);  
    cin.tie(0);  
    cout.tie(0);  
    int n, opt, l, r, c;  
    cin >> n;  
    sizB = sqrt(n);  
    for (int i = 1; i <= n; i++) {  
        cin >> a[i];  
        blk[i] = (i - 1) / sizB + 1;  
    }  
    for (int i = 1; i <= n; i++) {  
        cin >> opt >> l >> r >> c;  
        if (opt) {  
            cout << tagA[blk[r]] + a[r] << "\n";  
        } else  
            add(l, r, c);  
    }  
    // fclose(stdin);  
    // fclose(stdout);  
    return 0;  
}  
```  
## Loj #6278. 数列分块入门 2  
```cpp  
/*  
 * > CPP Code Snippet <  
 * > Powered by Microsoft Visual Studio Code <  
 *  
 * @Author    FrankWKD (wangkedi01)  
 * @Date      2025-07-22  
 * @Network   "https://loj.ac/p/6278"  
 * @License   GNU General Public License 2.0  
 * @Platform  [Frank]iMac Ubuntu Pro 24.04 LTS  
 * @FileName  T2.cpp  
 * @FilePath  C:\Users\Administrator\Documents\T2.cpp  
 * @Solution  https://www.cnblogs.com/Foggy-Forest/p/13267943.html  
 */  
  
// #pragma GCC optimize(3)  
#include <bits/stdc++.h>  
using namespace std;  
const int N = 5e4 + 10;  
int a[N], blk[N], tagA[N], sizB, n;  
vector<int> v[250];  
int query(int l, int r, int val) {  
    int ans = 0;  
  
    for (int i = l; i <= min(blk[l] * sizB, r); i++) {  
        if (a[i] + tagA[blk[i]] < val)  
            ans++;  
    }  
  
    if (blk[l] != blk[r]) {  
        for (int i = (blk[r] - 1) * sizB + 1; i <= r; i++) {  
            if (a[i] + tagA[blk[i]] < val) {  
                ans++;  
            }  
        }  
    }  
  
    for (int i = blk[l] + 1; i <= blk[r] - 1; i++) {  
        int tmp = val - tagA[i];  
        ans += lower_bound(v[i].begin(), v[i].end(), tmp) - v[i].begin();  
    }  
  
    return ans;  
}  
void reset(int x) {  
    v[x].clear();  
  
    for (int i = (x - 1) * sizB + 1; i <= min(x * sizB, n); i++) {  
        v[x].push_back(a[i]);  
    }  
  
    sort(v[x].begin(), v[x].end());  
}  
void add(int l, int r, int val) {  
    if (blk[l] == blk[r]) {  
        for (int i = l; i <= r; i++) {  
            a[i] += val;  
        }  
  
        reset(blk[l]);  
        return;  
    }  
  
    for (int i = l; i <= blk[l] * sizB; i++) {  
        a[i] += val;  
    }  
  
    for (int i = (blk[r] - 1) * sizB + 1; i <= r; i++) {  
        a[i] += val;  
    }  
  
    for (int i = blk[l] + 1; i <= blk[r] - 1; i++) {  
        tagA[i] += val;  
    }  
  
    reset(blk[l]);  
    reset(blk[r]);  
}  
int main() {  
    // freopen("sample.in","r",stdin);  
    // freopen("sample.out","w",stdout);  
    // ios::sync_with_stdio(false);  
    // cin.tie(0); cout.tie(0);  
    int opt, l, r, c;  
    cin >> n;  
    sizB = sqrt(n);  
  
    for (int i = 1; i <= n; i++) {  
        cin >> a[i];  
        blk[i] = (i - 1) / sizB + 1;  
        v[blk[i]].push_back(a[i]);  
    }  
  
    for (int i = 1; i <= blk[n]; i++)  
        sort(v[i].begin(), v[i].end());  
  
    for (int i = 1; i <= n; i++) {  
        cin >> opt >> l >> r >> c;  
  
        if (opt) {  
            cout << query(l, r, pow(c, 2)) << "\n";  
        } else {  
            add(l, r, c);  
        }  
    }  
  
    // fclose(stdin);  
    // fclose(stdout);  
    return 0;  
}  
```  
# 树状数组  
- 之前博客讲过  
## 	【算法进阶 数据结构进阶树状数组】楼兰图腾  
- 没啥难度，但是码量有点大，从前到后求最大，最小的树状数组，开始需要跑4遍。  
```cpp  
#include <iostream>  
#include <cstring>  
#include <algorithm>  
  
using namespace std;  
  
#define int long long  
  
const int N = 200010;  
  
int n;  
int a[N], low[N], high[N], tr[N];  
  
int lowbit(int x) {  
    return x & -x;  
}  
  
void update(int x, int c) { // 位置x加c  
    for (int i = x; i <= n; i += lowbit(i)) tr[i] += c;  
}  
  
int query(int x) { // 返回前x个数的和  
    int res = 0;  
    for (int i = x; i; i -= lowbit(i)) res += tr[i];  
    return res;  
}  
  
signed main() {  
    cin >> n;  
    for (int i = 1; i <= n; i ++ ) cin >> a[i];  
  
    for (int i = 1; i <= n; i ++ ) {  
        int y = a[i];  
        high[i] = query(n) - query(y);  
        low[i] = query(y - 1);  
        update(y, 1);  
    }  
  
    memset(tr, 0, sizeof tr);  
    int res1 = 0, res2 = 0;  
    for (int i = n; i; i -- ) {  
        int y = a[i];  
        res1 += high[i] * (query(n) - query(y));  
        res2 += low[i] * query(y - 1);  
        update(y, 1);  
    }  
  
    cout << res1 << " " << res2 << endl;  
  
    return 0;  
}  
```  
## 	树状数组 2 ：区间修改，单点查询  
- 差分解决。之前博客讲过  
```cpp  
#pragma GCC optimize(2)  
#include <bits/stdc++.h>  
using namespace std;  
#define int long long  
int a[1000010], tr[1001000], n;  
inline int lowbit(int x) {//卡常！  
	return x & -x;  
}  
inline void update(int v, int p) {  
	for (int i = p; i <= n; i += lowbit(i)) {  
		tr[i] += v;  
	}  
}  
inline int query(int x) {  
	int res = 0;  
	for (int i = x; i > 0; i -= lowbit(i)) res += tr[i];  
	return res;  
}  
signed main() {  
	ios::sync_with_stdio(false);  
	cin.tie(0);  
	cout.tie(0);  
	int q;  
	cin >> n >> q;  
	for (int i = 1; i <= n; i++) {  
		cin >> a[i];  
		update(a[i] - a[i - 1], i);  
	}  
	for (int i = 1; i <= q; i++) {  
		char op;  
		int l, r, x;  
		cin >> op;  
		if (op == '1') {  
			cin >> l >> r >> x;  
			update(x, l);  
			update(-x, r + 1);  
		} else {  
			cin >> x;  
			cout << query(x) << '\n';  
		}  
	}  
}  
```  
# 堆  
## 	【算法进阶 基本数据结构 二叉堆】合并果子  
- 其实是贪心，易得每次合并最小的两堆石子花费最小，我们把所有数整到小根堆里然后每次取堆顶 $2$ 个元素然后合并后再把合并后的石子数放进去，直到堆的大小为 $1$ 为止。  
```cpp  
#include <bits/stdc++.h>  
using namespace std;  
int a[100100], n;  
priority_queue<int, vector<int>, greater<int>> q;  
  
int main() {  
	cin >> n;  
	for (int i = 1; i <= n; i++) {  
		int x;  
		cin >> x;  
		q.push(x);  
	}  
	int ans = 0;  
	while (!q.empty()) {  
		int x = q.top();  
		q.pop();  
		if (q.empty()) {  
			cout << ans;  
			return 0;  
		}  
		int y = q.top();  
		q.pop();  
		ans += x + y;  
		q.push(x + y);  
	}  
}  
```  
# 栈  
## 	【单调栈】830. 单调栈  
- 板子。当你犹豫不决是单调递增还是单调递减时请把他们带入样例模拟一遍~  
```cpp  
#include<bits/stdc++.h>  
using namespace std;  
stack<int> st;  
int n,x;  
int main(){  
	ios::sync_with_stdio(false);  
	cin.tie(0);cout.tie(0);  
	cin>>n;  
	for(int i = 1;i<= n;i++){  
		cin>>x;  
		while(!st.empty() and st.top() >= x) st.pop();  
		if(st.empty()) cout<<-1<<" ";  
		else cout<<st.top()<<" ";  
		st.push(x);  
	}  
}  
```  
