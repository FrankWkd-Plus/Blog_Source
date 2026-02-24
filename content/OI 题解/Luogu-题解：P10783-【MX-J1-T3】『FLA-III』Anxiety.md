---
title: Luogu-题解：P10783 【MX-J1-T3】『FLA - III』Anxiety
date: 2025-07-11
tags:
  - 树
  - OI
  - Luogu
  - 题解
licenseName: CC-BY-NC-SA 4.0
---
# [传送门](https://www.luogu.con.cn/problen/P10783?contestId=181138)  
## 思路  
我们先找 $ x,y $ 在树上路径的中点，如果路径上点数为奇数，那么就仅存在一个中点 $ n $；否则就会存在两个中点 $ n_1,n_2 $。  
  
**第一种情况**：易得：到 $ x,y $ 距离均不超过 $ k $，相当于到 $ n $ 距离不超过 $ k - \frac{dis - 1}{2} $（其中 $ dis $ 为路径长度）我们可以先 DFS 预处理某个点 $ i $ 子树内到它距离不超过 $ j $ 的点的权值和 $ v_{i,j} $。先将答案加上 $ v_{n,k - \frac{dis - 1}{2}} $，然后向上跳，计算子树外的权值就可以啦。  
  
**第二种情况**：若 $ n_1 $ 是 $ n_2 $ 的儿子，则到 $ x,y $ 距离均不超过 $ k $，等价于 $ n_1 $ 子树内到 $ n_1 $ 距离不超过 $ k - \frac{dis - 2}{2} $，$ n_1 $ 子树外到 $ n_2 $ 距离不超过 $ k - \frac{dis - 2}{2} $。同样统计就可以啦～  
```cpp  
#include <bits/stdc++.h>  
  
using namespace std;  
  
const int maxn = (1 << 20) + 5;  
  
int n, q;  
long long w[maxn], val[maxn][55];  
  
void dfs(int x) {  
    if (x >= (1 << n))  
        return;  
    val[x][0] = w[x];  
    dfs(x << 1), dfs(x << 1 | 1);  
    for (int i = 1; i <= 50; i++) val[x][i] = val[x << 1][i - 1] + val[x << 1 | 1][i - 1];  
}  
  
int main() {  
    scanf("%d %d", &n, &q);  
    for (int i = 1; i <= (1 << n) - 1; i++) scanf("%lld", &w[i]);  
    dfs(1);  
    for (int i = 1; i <= (1 << n) - 1; i++) for (int j = 1; j <= 50; j++) val[i][j] += val[i][j - 1];  
    while (q--) {  
        int x, y, k;  
        scanf("%d%d%d", &x, &y, &k);  
        if (k > 50) k = 50;  
        int x1 = x, y1 = y, dx = 0, dy = 0;  
        while (x1) dx++, x1 /= 2;  
        while (y1) dy++, y1 /= 2;  
        x1 = x, y1 = y;  
        if (dx > dy)for (int i = 1; i <= dx - dy; i++) x1 /= 2;  
        else for (int i = 1; i <= dy - dx; i++) y1 /= 2;  
        int dl = min(dx, dy);  
        while (x1 != y1) x1 /= 2, y1 /= 2, dl--;  
        int lca = x1, dis = dx + dy - 2 * dl + 1,mid;  
        if (dx > dy) {  
            mid = x;  
            for (int i = 1; i <= (dis - 1) / 2; i++) mid /= 2;  
        } else {  
            mid = y;  
            for (int i = 1; i <= (dis - 1) / 2; i++) mid /= 2;  
        }  
        if (dis & 1) {  
            if (dis / 2 > k) {  
                printf("0\n");  
                continue;  
            }  
            k -= dis / 2;  
            long long ans = val[mid][k];  
            int pre = mid;  
            mid /= 2;  
            while (k && mid) {  
                k--;  
                ans += w[mid];  
                if (k)  
                    if (pre == mid * 2) ans += val[mid << 1 | 1][k - 1];  
                    else ans += val[mid << 1][k - 1];  
                pre = mid, mid /= 2;  
            }  
            printf("%lld\n", ans);  
        } else {  
            if (dis / 2 > k) {  
                printf("0\n");  
                continue;  
            }  
            k -= dis / 2;  
            long long ans = val[mid][k];  
            int pre = mid;  
            mid /= 2;  
            while (k + 1 && mid) {  
                ans += w[mid];  
                if (pre == mid * 2) ans += val[mid << 1 | 1][k - 1];  
                else ans += val[mid << 1][k - 1];  
                k--,pre = mid, mid /= 2;  
            }  
            printf("%lld\n", ans);  
        }  
    }  
}  
```  
