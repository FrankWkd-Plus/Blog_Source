---
title: RMQ学习笔记
date: 2025-03-03
tags:
  - RMQ
  - 算法
  - ST表
  - 倍增
  - 学习笔记
  - OI
licenseName: CC-BY-NC-SA 4.0
---
  
# RMQ学习笔记  
前言:这个算法无论是从适配性还是长度来说都很有实力...💦  
  
## 关于 RMQ  
RMQ 是英文 Range Maximum/Minimum Query 的缩写，表示区间最大（最小）值。  
  
## 详细信息  
  
## 求 $l-r$ 区间内的最大/最小数.  
### 区间构造  
 - 本质是DP.设 $f[i][j]$ 为 $i\sim i+2^{j-1}$ 的区间最大值.特别地,$f[i][0]=a[i]$.(一个数的最大值是它本身).  
 - 状态转移方程: $f[i][j]=max(f[i][j-1],f[i+2^{j-1}][j-1])$.  
### 区间查询  
- 设要查询的区间为 $[L,R]$ (包括LR)  
- $$k = log2(R-L+1)$$  
- $$res[L][R] = max(f[L][K],f[R-2^k+1][k])$$  
- 注意:为保证精度,请自己推导 $2^k$ ,而不是直接使用 $log$ 函数.  
### 时间复杂度: $O(nlog_2n)$  
  
### A. 超级记忆力  
  
#### 题目描述  
  
小A同学拥有无与伦比的超级记忆力，他可以一次性记住很多数字。  
  
为了考验一下小A同学的记忆力，王老师一次性给小A展示了 $N$ 个整数。然后问了他 $M$ 个问题，每个问题给定一个区间，要求小A同学说出这个区间中的最大数是多少？  
  
为方便老师检验小A同学的答案是否正确，请你先编程求出正确的答案。  
  
#### 输入  
  
第一行两个整数 $N,M$ 表示数字的个数和要询问的次数；  
  
接下来一行为 $N$ 个数；  
  
接下来 $M$ 行，每行都有两个整数 $X,Y$ 表示询问的区间。  
  
#### 数据范围：  
  
$1≤N≤10^5,1≤M≤10^6,1≤X≤Y≤N$。数字不超过 C/C++ 的 int 范围。  
#### 解法:  
- 板子,没什么可说的.注意:要开scanf!  
```cpp  
#include <bits/stdc++.h>  
using namespace std;  
  
const int N = 1e5 + 10, L = 20;  
int a[N], f[N][L];  
int lg[N];  
int n, m, x, y, k;  
int main() {  
    scanf("%d%d", &n, &m);  
    for (int i = 1; i <= n; i++) {  
        scanf("%d", &a[i]);  
    }  
    lg[1] = 0;  
    for (int i = 2; i <= n; i++) lg[i] = lg[i / 2] + 1;  
    for (int j = 0; j < L; j++) {  
        for (int i = 1; i + (1 << j) - 1 <= n; i++) {  
            if (j == 0)  
                f[i][j] = a[i];  
            else  
                f[i][j] = max(f[i][j - 1], f[i + (1 << j - 1)][j - 1]);  
        }  
    }  
    while (m--) {  
        scanf("%d%d", &x, &y);  
        k = lg[y - x + 1];  
        printf("%d\n", max(f[x][k], f[y - (1 << k) + 1][k]));  
    }  
}  
```  
  
### B. 荣耀之战  
  
#### 题目描述  
  
小 $A$ 是一名游戏玩家，他正在玩一款叫做“荣耀之战”的游戏。在这个游戏中，他需要通过完成任务来提升自己的等级。  
  
游戏地图上有 $N$ 个排成一排的装备，每个装备都标注好了经验值和危险值，第 $i$ 个装备的经验值为 $V_i$ 危险值为 $D_i$。  
  
小 $A$ 接到了一个任务，他需要在这 $N$ 个装备中，选取**连续的**若干个装备，并使得这些装备的经验值总和不小于 $K$（$\ge K$）。同时要使得这些装备的最大的危险值尽可能的小。  
  
请编程计算出，满足题意的方案中，最大的危险值最小是多少？  
  
输入  
  
第 $1$ 行读入 $2$ 个整数 $N,K$。  
  
接下来 $N$ 行，每行读入 $2$ 个整数 $V_i,D_i$ ，分别表示每个装备的经验值和危险值。  
  
  
#### 数据范围  
  
对于 $100\%$ 的数据，满足 $1 \le N \le 10^5$，$1 \le V_i,D_i \le 10^9$，$1 \le K \le 10^{18}$。  
  
- 要选出一个区间,使得他们在 $V_i\ge K$ 的情况下 $D_i$ 的总和最小.能够转化为: 选择 $V_l+V_{i+1}+...+V_r \ge K$ 且  $D_l+D_{i+1}+...+D_r $ 最小.很容易想到用双指针来解决.注意:一定要让已经过去的数出队!!!  
```cpp  
#include <bits/stdc++.h>  
using namespace std;  
const int N = 1e5 + 10;  
const int L = 20;  
long long a[N];  
long long lg[N], f[N][L];  
long long n, m, v[N], w[N];  
#define value long long  
value max(value a, value b) {  
    return a > b ? a : b;  
}  
value min(value a, value b) {  
    return a < b ? a : b;  
}  
int main() {  
    cin >> n >> m;  
    for (int i = 1; i <= n; i++) {  
        scanf("%lld%lld", &v[i], &w[i]);  
    }  
    lg[1] = 0;  
    for (int i = 2; i <= n; i++) {  
        lg[i] = lg[i / 2] + 1;  
    }  
    for (int i = 0; i < L; i++) {  
        for (int j = 1; j + (1 << i) - 1 <= n; j++) {  
            if (i == 0)  
                f[j][i] = w[j];  
            else  
                f[j][i] = max(f[j][i - 1], f[j + (1 << (i - 1))][i - 1]);  
        }  
    }  
    long long r = 0, sum = 0;  
    long long ans = INT_MAX;  
    for (int l = 1; l <= n; l++) {  
        while (r + 1 <= n and sum < m) {  
            sum += v[++r];  
        }  
        if (sum >= m) {  
            long long  
                len = lg[r - l + 1];  
            ans = min(ans, max(f[l][len], f[r - (1 << len) + 1][len]));  
        }  
        sum -= v[l];  // 一定要注意让已经过去的数出队!!!!  
    }  
  
    printf("%lld\n", ans);  
}  
```  
  
### C.异或  
#### 题目内容  
有一个长度为 $N$ 的数列 $A_1,A_2,\dots,A_n$。  
  
请问该数列中任意取一个区间 $[L,R]$ 中，是否存在 $2$ 个数，使得这两个数异或的结果为 $T$。  
  
请注意，本题会发起 $M$ 次询问，对于每次询问的区间，如果能找到符合题意的数对，请输出 `yes`，否则请输出 `no`。  
  
#### 输入  
第一行包含三个整数 $n, m, T$ 。  
  
第二行包含 $n$ 个整数，数字之间用空格隔开。  
  
接下来的 $M$ 行，每行有一个询问区间，每个询问区间包含 $2$ 个整数 $L,R$。  
  
#### 数据规模  
  
对于 $20 \%$ 数据, $1 \leq n, m \leq 100$;  
  
对于另外 $40 \%$ 数据, $1 \leq n, m \leq 1000$;  
  
对于 $100 %$ 的数据, $1 \leq n, m \leq 10^5,0 \leq T < 2^{20},1\le L \le R \le n,0 \le a_i \le 2^{20}$  
  
#### 思路  
- 首先,我们暴力肯定过不了.  
- 题目要求给定区间内是否有一对数.$a,b$ 满足 $a^b=T$  
- 简单推到可得: $a^T=b$ 是由上面式子转化而来的.  
- 我们随便列一组数据,并存储到 $a$ 数组里面: $a[]=\{2,1,3,4,2,3,2,3\}$  
- 然后没个数异或 $T$ 得: $a_2[]=\{3,0,2,5,3,2,3,2\}$  
- 然后在每个数的前面查找异或 $T$ 后的数,如果找不到标记为 $0$ :$a_3[]=\{0,0,1,0,3,5,6,7\}$  
- 如果要找的区间内对应的所有 $a_3[i]$ 中有大于 $L$ 而且小于 $R$ 的数,那就成功了.输出 $yes.$ 否则就是 $no$ .  
- 这个找符合规定的数的过程可以使用RMQ.记录每个区间内的最小数然后方便之后查找,并且具有最优性.(不信可以试试不同的数据)  
  
2856-异或  
1.如果 $x⊕y=T$，那么一定满足 $x⊕T=y$；  
2.对于每个数 $a_i$，如果能够存储在 $a_i$ 的左侧，最后一个 $x⊕T$ 的位置 $p$ ，那么 $[L，R]$ 中是否存在数对异或后为 $T$ 的条件为：如果 $[L，R]$ 中存在的一个 $p$ ，满足 $p\ge L$ 。  
3.因此我们只要求出 $[L，R]$ 中最大的 $p$ ，如果  $p\ge L$，即可满足条件，因此用RMQ，维护 $[L，R]$ 区间 $p$ 的最大值。  
举例：  
$$a[]=2,1,3,4,2,3,2,3$$  
$$p[]=0,0,1,0,3,5,6,7$$  
$last_i$ ：记录任何一个数的位置  
  - 如何求 $p_i$ ?  
1.用 $t$ 数组来存储每个数的位置，每遇到一个数 $a_i$，其位置 $last_{a_i}=i$ ，显然如果有相同的 $a_i$，$last_{a_i}$ 将会记录最后一个 $a_i$ 的位置；  
2.因此查 $a_i⊕T$ 的最后一个位置，直接取  $last_{a_{i⊕T}}$，如果该位置不存在，则输出 $no$。注意：由于 $a_i⊕T$ 的最大值可能是 $2^{20}-1$，因此要注意 $last$ 数组要开大一些。  
![](https://img2024.cnblogs.com/blog/3594125/202503/3594125-20250305224622182-1689290442.jpg)  
```cpp  
#include <bits/stdc++.h>  
using namespace std;  
const int N = 1e5 + 20, L = 20;  
int f[N][L], p[N], ls[(1 << 20) + 10], lg[N];  
int n, m, l, r, t;  
  
int main() {  
    scanf("%d%d%d", &n, &m, &t);  
    for (int i = 1; i <= n; i++) {  
        int x;  
        scanf("%d", &x);  
        p[i] = ls[x ^ t];  
        ls[x] = i;  
    }  
    lg[1] = 0;  
    for (int i = 2; i <= n; i++) lg[i] = lg[i >> 1] + 1;  
    for (int i = 0; i < L; i++) {  
        for (int j = 1; j + (1 << i) - 1 <= n; j++) {  
            if (i == 0)  // 一定要记得i初始值为0啊啊啊啊啊啊忘了好多遍啦  
                f[j][i] = p[j];  
            else  
                f[j][i] = max(f[j][i - 1], f[j + (1 << i - 1)][i - 1]);  
        }  
    }  
    while (m--) {  
        scanf("%d%d", &l, &r);  
        int len = lg[r - l + 1];  
        int maxx = max(f[l][len], f[r - (1 << len) + 1][len]);  
        if (maxx >= l)  
            puts("yes");  
        else  
            puts("no");  
    }  
}  
```  
### D.连续k个数的最值  
#### 题目描述  
给定 $n$ 个整数，求从第 $1$ 个数到第 $n-k+1$ 个数为起点的每个数开始，连续 $k$ 个数的最大数和最小数。  
$0\le k \le n \le 10^5$  
#### 输入  
```  
5 3  
1 2 3 4 5  
```  
#### 输出  
```  
3 1  
4 2  
5 3  
```  
#### 解法  
很好的板子题.跑两遍RMQ,分别记录最大最小值即可.  
```cpp  
#include <bits/stdc++.h>  
using namespace std;  
  
const int N = 1e5 + 10, L = 20;  
int f[N][L], a[N], lg[N];  // lg数组的本质就是求出两个下标之间的差值然后log2一下,所以直接开N就行  
int fmi[N][L];  
int n, q;  
  
int main() {  
    cin >> n >> q;  
    for (int i = 1; i <= n; i++) {  
        cin >> a[i];  
    }  
    // 初始化lg数组  
    lg[1] = 0;  
    for (int i = 2; i <= n; i++) {  
        lg[i] = lg[i / 2] + 1;  
    }  
    // 初始化最大值ST表  
    for (int i = 0; i < L; i++) {  // i从0开始!!!!2^0也是正整数!!!!  
        for (int j = 1; j + (1 << i) - 1 <= n; j++) {  
            if (i == 0)  // 同上个注释  
                f[j][i] = a[j];  
            else  
                f[j][i] = max(f[j][i - 1], f[j + (1 << (i - 1))][i - 1]);  
        }  
    }  
    // 初始化最小值ST表  
    for (int i = 0; i < L; i++) {  
        for (int j = 1; j + (1 << i) - 1 <= n; j++) {  
            if (i == 0)  
                fmi[j][i] = a[j];  
            else  
                fmi[j][i] = min(fmi[j][i - 1], fmi[j + (1 << (i - 1))][i - 1]);  
        }  
    }  
    // 构造完毕.  
    for (int i = 1; i + q - 1 <= n; i++) {  
        int len = lg[q];  
        int minn = min(fmi[i][len], fmi[i + q - 1 - (1 << len) + 1][len]);  
        int maxx = max(f[i][len], f[i + q - 1 - (1 << len) + 1][len]);  
        cout << maxx << " " << minn << "\n";  
    }  
    return 0;  
}  
```  
#### E. 体育课  
  
题目描述  
  
体育课上，$N$ 名同学排成了一排，他们的编号为 $1 \sim N$。  
  
体育老师安排大家玩 $M$ 轮游戏，每轮游戏会从邀请编号在 $[L,R]$ 之间的的同学参加。  
  
这个游戏主要考验同学们的团队协作能力，不过，如果被邀请的同学身高差距太大，会很难完成游戏。  
  
为了让每次邀请的同学都能顺利完成游戏，体育老师要求每次选取出编号在 $[L,R]$ 之间的同学之后，请该组的同学告诉老师，这组同学最高身高和最低身高的差值是多少。老师将根据这个身高的差值，来设置游戏的难度。  
  
输入  
  
第 $1$ 行读入 $2$ 个整数 $N,M$。  
  
接下来 $N$ 行每行读入一个整数，第 $i$ 个整数 $A_i$ 代表编号为 $i$ 同学的身高。  
  
接下来读入 $M$ 行，每行读入 $2$ 个整数 $L,R$，表示被邀请参加游戏同学的编号范围。  
  
#### 输入  
```  
6 3  
1  
7  
3  
4  
2  
5  
1 5  
4 6  
2 2  
```  
#### 输出  
```  
6  
3  
0  
```  
#### 数据范围  
  
$1 \le N \le 5\times10^4$，$1 \le M \le 2 \times 10^5$，$1 \le A_i \le 10^6$，$1 \le L \le R \le N$。  
#### 解法  
跟Extra T1 一摸一样.数据范围反倒小啦  
```cpp  
#include <bits/stdc++.h>  
using namespace std;  
  
const int N = 1e4 * 5 + 10, L = 20;  
int f[N][L], a[N], lg[N];  // lg数组的本质就是求出两个下标之间的差值然后log2一下,所以直接开N就行  
int fmi[N][L];  
int n, q, l, r;  
  
int main() {  
    scanf("%d%d", &n, &q);  
    for (int i = 1; i <= n; i++) {  
        scanf("%d", &a[i]);  
    }  
    // 初始化lg数组  
    lg[1] = 0;  
    for (int i = 2; i <= n; i++) {  
        lg[i] = lg[i / 2] + 1;  
    }  
    // 初始化最大值ST表  
    for (int i = 0; i < L; i++) {  // i从0开始!!!!2^0也是正整数!!!!  
        for (int j = 1; j + (1 << i) - 1 <= n; j++) {  
            if (i == 0)  // 同上个注释  
                f[j][i] = a[j];  
            else  
                f[j][i] = max(f[j][i - 1], f[j + (1 << (i - 1))][i - 1]);  
        }  
    }  
    // 初始化最小值ST表  
    for (int i = 0; i < L; i++) {  
        for (int j = 1; j + (1 << i) - 1 <= n; j++) {  
            if (i == 0)  
                fmi[j][i] = a[j];  
            else  
                fmi[j][i] = min(fmi[j][i - 1], fmi[j + (1 << (i - 1))][i - 1]);  
        }  
    }  
    // 构造完毕.  
    while (q--) {  
        scanf("%d%d", &l, &r);  
        int len = lg[r - l + 1];  
        int maxx = max(f[l][len], f[r - (1 << len) + 1][len]);  
        int minn = min(fmi[l][len], fmi[r - (1 << len) + 1][len]);  
        printf("%d\n", maxx - minn);  
    }  
    return 0;  
}  
```  
### Extra T1 [P2880 [USACO07JAN] Balanced Lineup G](https://www.luogu.com.cn/problem/P2880)  
没什么可说的,两次RMQ板子分别最大最小,还需要多打打板子题啊...好多小细节需要注意,但是大体上没什么难度.  
  
```cpp  
#include <bits/stdc++.h>  
using namespace std;  
  
const int N = 1e5 * 5 + 10, L = 20;  
int f[N][L], a[N], lg[N];  // lg数组的本质就是求出两个下标之间的差值然后log2一下,所以直接开N就行  
int fmi[N][L];  
int n, q, l, r;  
  
int main() {  
    cin >> n >> q;  
    for (int i = 1; i <= n; i++) {  
        cin >> a[i];  
    }  
    // 初始化lg数组  
    lg[1] = 0;  
    for (int i = 2; i <= n; i++) {  
        lg[i] = lg[i / 2] + 1;  
    }  
    // 初始化最大值ST表  
    for (int i = 0; i < L; i++) {  // i从0开始!!!!2^0也是正整数!!!!  
        for (int j = 1; j + (1 << i) - 1 <= n; j++) {  
            if (i == 0)  // 同上个注释  
                f[j][i] = a[j];  
            else  
                f[j][i] = max(f[j][i - 1], f[j + (1 << (i - 1))][i - 1]);  
        }  
    }  
    // 初始化最小值ST表  
    for (int i = 0; i < L; i++) {  
        for (int j = 1; j + (1 << i) - 1 <= n; j++) {  
            if (i == 0)  
                fmi[j][i] = a[j];  
            else  
                fmi[j][i] = min(fmi[j][i - 1], fmi[j + (1 << (i - 1))][i - 1]);  
        }  
    }  
    // 构造完毕.  
    while (q--) {  
        cin >> l >> r;  
        int len = lg[r - l + 1];  
        int maxx = max(f[l][len], f[r - (1 << len) + 1][len]);  
        int minn = min(fmi[l][len], fmi[r - (1 << len) + 1][len]);  
        cout << maxx - minn << "\n";  
    }  
    return 0;  
}  
```  
### ExtraT2 P3865 【模板】ST 表 && RMQ 问题  
#### 解法  
就是模版好吧,不会的请看A题,注意:时间限制0.8s  
```cpp  
#include <bits/stdc++.h>  
using namespace std;  
  
const int N = 1e5 + 10, L = 20;  
int a[N], f[N][L];  
int lg[N];  
int n, m, x, y, k;  
int main() {  
    scanf("%d%d", &n, &m);  
    for (int i = 1; i <= n; i++) {  
        scanf("%d", &a[i]);  
    }  
    lg[1] = 0;  
    for (int i = 2; i <= n; i++) lg[i] = lg[i / 2] + 1;  
    for (int j = 0; j < L; j++) {  
        for (int i = 1; i + (1 << j) - 1 <= n; i++) {  
            if (j == 0)  
                f[i][j] = a[i];  
            else  
                f[i][j] = max(f[i][j - 1], f[i + (1 << j - 1)][j - 1]);  
        }  
    }  
    while (m--) {  
        scanf("%d%d", &x, &y);  
        k = lg[y - x + 1];  
        printf("%d\n", max(f[x][k], f[y - (1 << k) + 1][k]));  
    }  
}  
```  
  
![](https://img2024.cnblogs.com/blog/3594125/202503/3594125-20250303202219501-1304077497.png)  
