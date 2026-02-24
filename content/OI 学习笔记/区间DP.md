---
title: 区间DP
date: 2025-07-28
tags:
  - DP
  - 算法
  - 区间DP
  - 做题记录
  - 学习笔记
  - OI
licenseName: CC-BY-NC-SA 4.0
---
> Upd 2025-07-30 修复公式错误，修复文章内链接错误，补充题目和模板公式  
  
- 不是很难的算法，思路也不算难～  
  
## 定义  
  
区间类动态规划是线性动态规划的扩展，它在分阶段地划分问题时，与阶段中元素出现的顺序和由前一阶段的哪些元素合并而来有很大的关系。  
  
令状态 $f(i,j)$ 表示将下标位置 $i$ 到 $j$ 的所有元素合并能获得的价值的最大值，那么 $f(i,j)=\max\{f(i,k)+f(k+1,j)+cost\}$，$cost$ 为将这两组元素合并起来的价值。  
  
## 性质  
  
区间 DP 有以下特点：  
  
**合并**：即将两个或多个部分进行整合，当然也可以反过来；  
  
**特征**：能将问题分解为能两两合并的形式；  
  
**求解**：对整个问题设最优值，枚举合并点，将问题分解为左右两个部分，最后合并两个部分的最优值得到原问题的最优值。  
  
- 一般的区间 DP 题目的状态转移方程：  
  
  $$  
  dp_{i,j}=max\{dp_{i,k}+dp_{k+1,j}+<decision>\} (i\le k\le j)  
  $$  
  
> From: OI-Wiki  
  
## 题目  
  
### #1 [「NOI1995」石子合并](https://loj.ac/problem/10147)  
  
题目大意：在一个环上有 $n$ 个数 $a_1,a_2,\dots,a_n$，进行 $n-1$ 次合并操作，每次操作将相邻的两堆合并成一堆，能获得新的一堆中的石子数量的和的得分。你需要最大化你的得分。  
  
需要考虑不在环上，而在一条链上的情况。  
  
令 $f(i,j)$ 表示将区间 $[i,j]$ 内的所有石子合并到一起的最大得分。  
  
写出 **状态转移方程**：$f(i,j)=\max\{f(i,k)+f(k+1,j)+\sum_{t=i}^{j} a_t \}~(i\le k<j)$，其中 $\sum_{t=i}^{j} a_t$ 表示区间 $[i,j]$ 内所有石子的质量之和，也就是本次合并的收益。而 $f(i,k)+f(k+1,j)$ 是之前的收益之和。  
  
令 $sum_i$ 表示 $a$ 数组的前缀和，状态转移方程变形为 $f(i,j)=\max\{f(i,k)+f(k+1,j)+sum_j-sum_{i-1} \}$。  
  
#### 怎样进行状态转移  
  
由于计算 $f(i,j)$ 的值时需要知道所有 $f(i,k)$ 和 $f(k+1,j)$ 的值，而这两个中包含的元素的数量都小于 $f(i,j)$，所以我们以 $len=j-i+1$ 作为 DP 的阶段。首先从小到大枚举 $len$，然后枚举 $i$ 的值，根据 $len$ 和 $i$ 用公式计算出 $j$ 的值，然后枚举 $k$，时间复杂度为 $O(n^3)$  
  
#### 怎样处理环  
  
题目中石子围成一个环，而不是一条链，怎么办呢？  
  
**方法一**：由于石子围成一个环，我们可以枚举分开的位置，将这个环转化成一个链，由于要枚举 $n$ 次，最终的时间复杂度为 $O(n^4)$。  
  
**方法二**：我们将这条链延长两倍，变成 $2\times n$ 堆，其中第 $i$ 堆与第 $n+i$ 堆相同，用动态规划求解后，取 $f(1,n),f(2,n+1),\dots,f(n,2n-1)$ 中的最优值，即为最后的答案。时间复杂度 $O(n^3)$。  
  
> 个人建议：将最大值和最小值设为两个DP数组然后在同一个for循环中计算，否则码量超级大（尴尬  
  
#### 代码  
  
```cpp  
/*  
 * > CPP Code Snippet <  
 * > Powered by Microsoft Visual Studio Code <  
 *  
 * @Author    FrankWKD (wangkedi01)  
 * @Date      2025-07-28  
 * @Network   "https://www.luogu.com.cn/problem/P1880"  
 * @License   GNU General Public License 2.0  
 * @Platform  [Frank]iMac Ubuntu Pro 24.04 LTS  
 * @FileName  StoneMergeDP.cpp - P1880 [NOI1995] 石子合并  
 * @FilePath  /media/frank/FrankW/_default/Mine/Working/code-spaces/OI/OJ/Accoders/DP/StoneMergeDP.cpp  
 * @Solution  --  
 */  
  
// #pragma GCC optimize(3)  
#include <bits/stdc++.h>  
using namespace std;  
#define _for(cc, ba, ab) for (int cc = (ba); cc <= (ab); cc++)  
#define for_(cc, ba, ab) for (int cc = (ba); cc >= (ab); cc--)  
int dp[210][210], n, a[110], sum[210], dpm[210][210], ans1 = INT_MAX, ans2;  
int main() {  
    cin >> n;  
    _for(i, 1, n) cin >> a[i];  
    _for(i, 1, n) a[i + n] = a[i];  
    _for(i, 1, 2 * n) sum[i] = sum[i - 1] + a[i];  
    _for(i, 1, 2 * n) _for(j, 1, 2 * n) if (i != j) dp[i][j] = INT_MAX;  
    _for(len, 2, n) _for(i, 1, n * 2 - len) {  
        int j = i + len - 1;  
        _for(k, i, j - 1) dp[i][j] = min(dp[i][j], dp[i][k] + dp[k + 1][j] + sum[j] - sum[i - 1]), dpm[i][j] = max(dpm[i][j], dpm[i][k] + dpm[k + 1][j] + sum[j] - sum[i - 1]);  
    }  
    _for(i, 1, n) ans1 = min(dp[i][i + n - 1], ans1), ans2 = max(dpm[i][i + n - 1], ans2);  
    cout << ans1 << endl  
         << ans2;  
}  
```  
  
### #2 [「NOIP 2006 提高组」 能量项链](https://www.luogu.com.cn/problem/P1063)  
  
题目大意：有 $n$ 个能量项链，编号为 $1$ 到 $n$，每个能量项链都有一个能量值 $a_i$。你需要将这些能量项链合并成一条链，合并的方式是将相邻的两个能量项链合并成一个新的能量项链，新的能量项链的能量值为两个原有能量项链的能量值之和。你需要最大化最终得到的能量值。  
  
同样的，令 $f(i,j)$ 表示将区间 $[i,j]$ 内的所有能量项链合并到一起得到的最大能量值。  
  
如果你对 [#1](#1-「NOI1995」石子合并) 非常熟悉，那么不难推导出状态转移方程（特别注意不是 $f(k+1,j)$ 而是 $f(k,j)$，还要注意 $for$ 循环从 $i+1$ 而不是 $i$ 开始）:  
  
$$  
f(i,j) = \sum_{k=i+1}^{j-1}(f(i,k)+f(k,j)+a_k\times a_i\times a_j)  
$$  
  
因为本题而是一个环，所以采用和 [#1](#1-「NOI1995」石子合并) 相同的方式处理环即可。  
  
#### 代码  
  
**数组一定要开二倍！数组一定要开二倍！！数组一定要开二倍！！！  
  
```cpp  
#include <bits/stdc++.h>  
using namespace std;  
int dp[310][310], n, a[310];  
int main() {  
    memset(dp, 0, sizeof(dp));  
    scanf("%d", &n);  
    for (int i = 1; i <= n; i++) {  
        scanf("%d", &a[i]);  
        a[i + n] = a[i];  
    }  
    for (int len = 3; len <= n + 1; len++) {  
        for (int i = 1; i + len - 1 <= 2 * n; i++) {  
            int j = i + len - 1;  
            for (int k = i + 1; k < j; k++) {  
                dp[i][j] = max(dp[i][j], dp[i][k] + dp[k][j] + a[i] * a[j] * a[k]);  
            }  
        }  
    }  
    int ans = 0;  
    for (int i = 1; i <= n; i++) {  
        ans = max(ans, dp[i][i + n]);  
    }  
    printf("%d", ans);  
    return 0;  
}  
```  
  
### #3 [10149.「一本通 5.1 例 3」凸多边形的划分](https://loj.ac/p/10149)  
  
#### 题面  
  
给定一个具有 $N$ 个顶点的凸多边形，将顶点从 $1$ 至 $N$ 标号，每个顶点的权值都是一个正整数。将这个凸多边形划分成 $N-2$ 个互不相交的三角形，试求这些三角形顶点的权值乘积和至少为多少。  
  
##### 输入格式  
  
输入第一行为顶点数 $N$  
第二行依次为顶点 $1$ 至顶点 $N$ 的权值。  
  
##### 输出格式  
  
输出仅一行，为这些三角形顶点的权值乘积和的最小值。  
  
##### 样例  
  
###### 输入  
  
```  
5  
121 122 123 245 231  
```  
  
###### 输出  
  
```  
12214884  
```  
  
##### 数据范围与提示  
  
对于 $100\%$ 的数据，有 $N\le 50$，每个点权值小于 $10^9$。  
  
#### Sol		  
  
如果我们按顺时针将顶点编号，从顶点 $i$ 到顶点 $j$ 的凸多边形表示为如下图：  
![img](https://i.vgy.me/2CXfFn.png)  
设 $f[i][j]$ $(i<j)$ 表示从顶点 $i$ 到顶点 $j$ 的凸多边形三角剖分后所得到的最大乘积，当前我们可以枚举点 $k$，考虑凸多边形 $(i,j)$ 中剖出三角形 $(i,j,k)$，凸多边形 $(i,k)$，凸多边形 $(k,j)$ 的最大乘积和。我们可以得到动态转移方程：$(1<=i<k<j<=n)$  
  
$$  
dp_{i,j} = max\{dp_{i,k}+dp_{k,j}+a_i\times a_j\times a_k\}  
$$  
- 初始条件：$dp_{i,i+1} = 0$  
- 目标状态：$dp_{1,n}$  
