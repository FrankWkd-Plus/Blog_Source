---
title: CSP-S 02 集训做题记录
date: 2025-07-12
tags:
  - CSP
  - 做题记录
  - OI
---
> Upd: 2025.07.1
> Upd: 2025.08.05 补充 T4
> Upd: 2025.08.06 补充 T5，更新 T6

# T1 扩容（Expend）

### 题目描述
小 $A$ 有一个云系统，初始容量为 $S$ 个单位。随着芯片技术的飞速发展，云系统会不定期的为小 $A$ 免费扩容。

具体的扩容方法为：枚举当前容量 $S$ 中每一位的数值。
- 如果当前数值为 $1$，扩容后当前这一位的值依然是 $1$。
- 如果当前数值为 $2$，扩容后当前这一位的值会扩展为 $22$。
- 如果当前数值为 $3$，扩容后当前这一位的值会扩展为 $333$。
- $\dots$
- 如果当前数值为 $i$，扩容后当前这一位的值会扩展为 $i$ 个 $i$。

例如：假设云系统的初始容量为 $S = 123$，那么接下来 $3$ 次扩容，系统容量的变化分别为。
- 第 $1$ 次扩容后，容量变为 $122333$。
- 第 $2$ 次扩容后，容量变为 $1222233333333$。
- 第 $3$ 次扩容后，容量变为 $122222222333333333333333333333333333$。

经过多年的扩容，云系统的容量将变得非常大。请你编程计算出，在经过 $10^{15}$ 次扩容之后，云系统容量从左向右数第 $C$ 位的数值是多少？

### 输入
第一行输入一个数字串 $S$，由数字 $1$ 到 $9$ 组成，表示初始容量。  
第 $2$ 行输入整数 $C$，表示答案要求的最终容量从左向右数的位数。  

### 输出
输出一个整数，表示经过 $10^{15}$ 次扩容之后，云系统容量从左向右数第 $C$ 位的数值。  

### 样例
#### 输入
```
123
5
```
#### 输出
```
2
```

#### 输入
```
5
180
```
#### 输出
```
5
```

#### 输入
```
179692458
9460730472580800
```
#### 输出
```
7
```

### 说明
#### 样例解释
##### 样例 1 解释  
请参考题目描述中的解释。  

##### 样例 2 解释  
初始容量为 $5$，无论怎样扩容，容量数值的每一位都将是 $5$。  

### 数据范围
对于所有的测试数据，保证：数字串 $S$ 的长度在 $[1, 100]$ 范围内，且仅由数字 $1$ 到 $9$ 组成。$1 \leq C \leq 10^{18}$。经过 $10^{15}$ 次扩容后，数字串的长度至少为 $C$。  

### 测试点
| 测试点      | 特殊性质     |
| ----------- | ------------ |
| $1 \sim 4$  | 特殊性质 $A$ |
| $6 \sim 8$  | 特殊性质 $B$ |
| $9 \sim 20$ | 无           |

#### 特殊性质 $A$：满足 $C = 1$。  
#### 特殊性质 $B$：满足 $S[0] \neq 1$。  

## T1 Sol
考虑到扩容次数非常巨大，即使是 $2$ 扩容 $10^{15}$ 之后也有 $2^{10^{15}}$ 位。这远远超出了可存储范围。

我们从前往后遍历。遇到 $1$ 就认为它扩容后不会变，遇到其他的就直接输出。如果直到第 $c$ 位还是 $1$ ，就直接输出 $1$。

### warning 警告！

请开 $long\ long$.  
$string$ 类型的字符数组要注意 $c > string.size()$ 时的越界问题。  
还要注意 $string$ 从 $0$ 开始计数。

## T1 Code **[40pts]**
> 根据特性 `A` & `B` 骗分

```cpp
/*
 * > CPP Code Snippet <
 * > Powered by Microsoft Visual Studio Code <
 *
 * @Author    FrankWKD (wangkedi01)
 * @Date      2025-07-14
 * @Network   "https://oj.czos.cn/contest/problem?id=25573&pid=0"
 * @License   GNU General Public License 2.0
 * @Platform  [Frank]iMac Ubuntu Pro 24.04 LTS
 * @FileName  T1[40pts].cpp
 * @FilePath  /media/frank/FrankW/_default/_Mine!/Working/code-spaces/czos/CSP-S_Practise/Lesson2/T1[40pts].cpp
 * @Solution  针对特殊性质拿分，40pts 极简代码。实际拿分 65pts。
 */

// #pragma GCC optimize(3)
#include <bits/stdc++.h>
using namespace std;

int main() {
    // freopen("sample.in","r",stdin);
    // freopen("sample.out","w",stdout);
    // ios::sync_with_stdio(false);
    // cin.tie(0); cout.tie(0);
    string s;
    int c;
    cin >> s >> c;
    cout << s[0] << endl;
    // fclose(stdin);
    // fclose(stdout);
    return 0;
}
```

## T1 Code **[100pts]**
```cpp
/*
 * > CPP Code Snippet <
 * > Powered by Microsoft Visual Studio Code <
 *
 * @Author    FrankWKD (wangkedi01)
 * @Date      2025-07-14
 * @Network   "https://oj.czos.cn/contest/problem?id=25573&pid=0&_pjax=%23p0"
 * @License   GNU General Public License 2.0
 * @Platform  [Frank]iMac Ubuntu Pro 24.04 LTS
 * @FileName  T1[100pts].cpp
 * @FilePath  /media/frank/FrankW/_default/_Mine!/Working/code-spaces/czos/CSP-S_Practise/Lesson2/T1[100pts].cpp
 * @Solution  --
 */

// #pragma GCC optimize(3)
#include <bits/stdc++.h>
using namespace std;

int main() {
    // freopen("sample.in","r",stdin);
    // freopen("sample.out","w",stdout);
    // ios::sync_with_stdio(false);
    // cin.tie(0); cout.tie(0);
    string s;
    long long c;
    cin >> s >> c;
    if (c == 1)
        cout << s[0];
    else {
        if (s[0] == '1') {
            long long i = 0;
            while (i < min(c - 1, (long long)(s.size() - 1)) and s[i] == '1') i++;
            cout << s[i];
        } else
            cout << s[0];
    }
    // fclose(stdin);
    // fclose(stdout);
    return 0;
}
```
# T2 解密游戏
### 题目描述
在一堂趣味盎然的数学课上，老师设计了一个名为 “解密纸条” 的游戏。班上有 $N$ 个同学，每位同学在一张纸条上秘密写下一个数字，要么是 $1$，要么是 $2$。

设第 $i$ 位同学纸条上写的数字为 $P_i$（$P_i$ 的值为 $1$ 或 $2$）。

你的任务是破解所有同学的纸条内容，即确定 $P_1, P_2, \ldots, P_N$ 的值。

老师提供了 $M$ 条解密线索，每条线索描述为：第 $X_i$ 位同学和第 $Y_i$ 位同学纸条上的数字之和加上一个值 $Z_i$ 总和为偶数（即 $P_{X_i} + P_{Y_i} + Z_i$ 是偶数）。

作为游戏的破解者，你可以执行以下操作任意次：选择一位同学，查看他/她纸条上的数字，每次查看需要消耗 $1$ 个单位的时间。

请计算确定所有同学纸条内容的最少总时间。

### 输入
第一行包含两个整数 $N$ 和 $M$，分别表示同学人数和线索数量。

接下来 $M$ 行，每行包含三个整数 $X_i, Y_i, Z_i$，表示一条线索：第 $X_i$ 位同学和第 $Y_i$ 位同学纸条上的数字之和加上 $Z_i$ 为偶数。

### 输出
输出一个整数，表示确定所有同学纸条内容的最少总时间。

### 样例
#### 样例输入 1
```
2 1
1 2 1
```
#### 样例输出 1
```
1
```

#### 样例输入 2
```
6 3
1 2 1
2 3 2
4 5 4
```
#### 样例输出 2
```
3
```

#### 样例输入 3
```
8 6
1 2 3
2 3 5
2 4 6
5 7 8
6 8 2
5 8 3
```
#### 样例输出 3
```
2
```

### 说明
#### 样例 1 说明
有 $2$ 个人，$1$ 个线索，线索指示 $P_1 + P_2 + 1$ 为偶数，因此只需要查看第 $1$ 位同学的纸条或者第 $2$ 位同学的纸条，就可以确定两位同学纸条上的数字了。

### 数据范围
对于 $100\%$ 的数据，满足 $2 \leq N \leq 10^5$，$1 \leq M \leq 10^5$，$1 \leq X_i < Y_i \leq N$，$1 \leq Z_i \leq 100$。

且满足：所有 $(X_i, Y_i)$ 互不相同，输入线索无矛盾，即一定可以解出一组 $P_1, P_2, \ldots, P_N$ 满足所有线索条件。

#### 测试点 特殊性质
| 测试点      | 特殊性质 |
| ----------- | -------- |
| $1 \sim 4$  | $M = 1$  |
| $5 \sim 20$ | 无       |

## T2 Sol
1．对于 $3$ 个数：$P_1 + P_2 + Z_i$，如果他们的和是偶数，且知道 $Z_i$ 的奇偶性，说明就能知道 $P_1 + P_2$ 的奇偶性，此时 $P_1$ 和 $P_2$ 只要知道 $1$ 个数，就能知道另一个数。  

2．考虑更复杂的情况：  
$P_1 + P_2 + Z_1$  
$P_2 + P_3 + Z_2$  
$P_3 + P_4 + Z_3$  

三组数中，看上去每组只要知道一个数就能知道另一个数。  

但其实第 $1$ 组数，如果知道了 $P_1$，$P_2$ 就知道了，这样第 $2$ 组的 $P_3$ 也能推导出来，第 $3$ 组的 $P_4$ 也能推导。  

因此：对于每一组中的 $P_i$ $P_j$，我们可以认为其是一个集合元素，对于一个集合中的元素只要知道一个就能推导出其余数，因此问题转换为求集合的数量。  

## T2 Code
```cpp
/*
 * > CPP Code Snippet <
 * > Powered by Microsoft Visual Studio Code <
 *
 * @Author    FrankWKD (wangkedi01)
 * @Date      2025-07-15
 * @Network   "https://oj.czos.cn/contest/problem?id=25573&pid=1"
 * @License   GNU General Public License 2.0
 * @Platform  [Frank]iMac Ubuntu Pro 24.04 LTS
 * @FileName  T2.cpp
 * @FilePath  /media/frank/FrankW/_default/_Mine!/Working/code-spaces/czos/CSP-S_Practise/Lesson2/T2.cpp
 * @Solution  --
 */

// #pragma GCC optimize(3)
#include <bits/stdc++.h>
using namespace std;
int xi, yi, ans, n, m;
int f[1010100];
int find(int x) {
    return f[x] == x ? x : f[x] = find(f[x]);
}
void merge(int x, int y) {
    f[find(x)] = f[find(y)];
}
int main() {
    // freopen("sample.in","r",stdin);
    // freopen("sample.out","w",stdout);
    // ios::sync_with_stdio(false);
    // cin.tie(0); cout.tie(0);

    cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        f[i] = i;
    }
    for (int i = 1; i <= m; i++) {
        int mi;
        cin >> xi >> yi >> mi;
        merge(xi, yi);
    }
    for (int i = 1; i <= n; i++) {
        if (find(f[i]) == i)
            ans++;
    }
    cout << ans << endl;

    // fclose(stdin);
    // fclose(stdout);
    return 0;
}
```

# T3 [CSP-S 2024] 染色

### 题目描述

给定一个长度为 $n$ 的正整数数组 $A$，其中所有数从左至右排成一排。

你需要将 $A$ 中的每个数染成红色或蓝色之一，然后按如下方式计算最终得分：

设 $C$ 为长度为 $n$ 的整数数组，对于 $A$ 中的每个数 $A_i$（$1 \leq i \leq n$）：

- 如果 $A_i$ 左侧没有与其同色的数，则令 $C_i = 0$。
- 否则，记其左侧**与其最靠近的同色数**为 $A_j$，若 $A_i = A_j$，则令 $C_i = A_i$，否则令 $C_i = 0$。

你的最终得分为 $C$ 中所有整数的和，即 $\sum \limits_{i=1}^n C_i$。你需要最大化最终得分，请求出最终得分的最大值。

### 输入格式

**本题有多组测试数据。**

输入的第一行包含一个正整数 $T$，表示数据组数。

接下来包含 $T$ 组数据，每组数据的格式如下：

第一行包含一个正整数 $n$，表示数组长度。

第二行包含 $n$ 个正整数 $A_1, A_2, \dots, A_n$，表示数组 $A$ 中的元素。

### 输出格式

对于每组数据：输出一行包含一个非负整数，表示最终得分的最大可能值。

### 输入输出样例 #1

### 输入 #1

```
3
1 2 1
4
1 2 3 4
8
3 5 2 5 1 2 1 4
```

### 输出 #1

```
1
0
8
```

### 说明/提示

**【样例 1 解释】**

对于第一组数据，以下为三种可能的染色方案：

1. 将 $A_1, A_2$ 染成红色，将 $A_3$ 染成蓝色（$\red{1}\red{2}\blue{1}$），其得分计算方式如下：
- 对于 $A_1$，由于其左侧没有红色的数，所以 $C_1 = 0$。
- 对于 $A_2$，其左侧与其最靠近的红色数为 $A_1$。由于 $A_1 \neq A_2$，所以 $C_2 = 0$。
- 对于 $A_3$，由于其左侧没有蓝色的数，所以 $C_3 = 0$。    
该方案最终得分为 $C_1 + C_2 + C_3 = 0$。
2. 将 $A_1, A_2, A_3$ 全部染成红色（$\red{121}$），其得分计算方式如下：
- 对于 $A_1$，由于其左侧没有红色的数，所以 $C_1 = 0$。
- 对于 $A_2$，其左侧与其最靠近的红色数为 $A_1$。由于 $A_1 \neq A_2$，所以 $C_2 = 0$。
- 对于 $A_3$，其左侧与其最靠近的红色数为 $A_2$。由于 $A_2 \neq A_3$，所以 $C_3 = 0$。    
该方案最终得分为 $C_1 + C_2 + C_3 = 0$。
3. 将 $A_1, A_3$ 染成红色，将 $A_2$ 染成蓝色（$\red{1}\blue{2}\red{1}$），其得分计算方式如下：
- 对于 $A_1$，由于其左侧没有红色的数，所以 $C_1 = 0$。
- 对于 $A_2$，由于其左侧没有蓝色的数，所以 $C_2 = 0$。
- 对于 $A_3$，其左侧与其最靠近的红色数为 $A_1$。由于 $A_1 = A_3$，所以 $C_3 = A_3 = 1$。    
该方案最终得分为 $C_1 + C_2 + C_3 = 1$。

可以证明，没有染色方案使得最终得分大于 $1$。

对于第二组数据，可以证明，任何染色方案的最终得分都是 $0$。

对于第三组数据，一种最优的染色方案为将 $A_1, A_2, A_4, A_5, A_7$ 染为红色，将 $A_3, A_6, A_8$ 染为蓝色（$\red{35}\blue{2}\red{51}\blue{2}\red{1}\blue{4}$），其对应 $C = [0, 0, 0, 5, 0, 1, 2, 0]$，最终得分为 $8$。

**【样例 2】**

见选手目录下的 color/color2.in 与 color/color2.ans。

**【数据范围】**

对于所有测试数据，保证：$1\leq T\leq 10$，$2\leq n\leq 2\times 10^5$，$1\leq A_i\leq 10^6$。

|   测试点    |         $n$         |    $A_i$    |
| :---------: | :-----------------: | :---------: |
|  $1\sim 4$  |      $\leq 15$      |  $\leq 15$  |
|  $5\sim 7$  |     $\leq 10^2$     | $\leq 10^2$ |
| $8\sim 10$  |     $\leq 2000$     | $\leq 2000$ |
|   $11,12$   | $\leq 2\times 10^4$ | $\leq 10^6$ |
| $13\sim 15$ | $\leq 2\times 10^5$ |  $\leq 10$  |
| $16\sim 20$ | $\leq 2\times 10^5$ | $\leq 10^6$ |

## Sol

题意：有 $N$ 个数，每个数设置为红或者蓝。  

1．如果 $A_i$ 左侧没有同色的数，$C_i=0$。  
2．如果有，找最近的同色的数 $A_j$，如：$A_i=A_j$，$C_i=A_i$，否则 $C_i=0$。  

最终得分 $= C_i$ 的和，求最大得分。  

**20pts：** 枚举每个数颜色取 $1$，和取 $2$ 这两种情况的所有的组合，求每种组合下，分数的最大值。  

### 1．状态定义  

$f[i][0]$：前 $i$ 个数，如果第 $i$ 个数对于答案没有贡献，最大得分。  
$f[i][1]$：前 $i$ 个数，如果第 $i$ 个数对于答案有贡献，最大得分。  

要注意：并不是第 $i$ 个数可以对答案有贡献时（即：左侧有等值的数），产生贡献一定最优。  

比如：$1\ 2\ 3\ 3\ 3\ 2\ 1$，对于第 $6$ 个数，如果要对答案有贡献，那么 $2\ 3\ 3\ 3\ 2$ 必须同色，此时得分 $= 0 + 0 + 0 + 3 + 0 + 1 = 4$ 分。  

而让第 $6$ 个数不产生贡献，答案反而更大，即：染色方案为 $1\ 2\ 3\ 3\ 3\ 2\ 1$，此时得分 $= 0 + 0 + 0 + 3 + 2 + 0 = 5$ 分。  

### 2．状态转移  

(1) 如果第 $i$ 个数对于得分无贡献。  

- 等价于第 $i$ 个数不存在：$f[i][0] = \max(f[i - 1][0], f[i - 1][1]);$  

(2) 如果第 $i$ 个数对于得分有贡献。  

- 此时需满足 $a[i]$ 左侧有和其值相等的数，假设 $a[i]$ 左侧和其值相等的最近的数的位置是 $p$。  

- ![img](https://img2024.cnblogs.com/blog/3594125/202507/3594125-20250716173300658-1875576247.png)  
- 则有：$p$ 和 $i$ 必然同色，且 $p+1 \sim i-1$，必然和 $p/i$ 异色。  

- 则到 $i$ 为止的最大得分可以分为 3 部分讨论。  

  - 第一部分：第 $i$ 个数的贡献，就是 $a[i]$。  

  - 第二部分：$p+2 \sim i-1$ 的贡献，这部分只能是每个数 $==$ 其左侧的数，才有贡献，可以通过前缀和求区间和维护出来。  

  - 第三部分：$1 \sim p+1$ 的贡献 $= \max(f[p + 1][0], f[p + 1][1]);$  

### 为什么有贡献时，一定是从最近的等值的数转移过来最优？  
![img](https://img2024.cnblogs.com/blog/3594125/202507/3594125-20250716173355764-1011999790.png)

设 $i$ 的左侧有多个和其值相等的点，比如有 $p$ 和 $p'$，如果从 $p'$ 转移过来，导致从 $p'+1 \sim p+1$ 这段必须只能是同色，而同色只是所有颜色排列的情况之一，讨论了更少的情况不可能产生更优解。

:::danger 

- 多测问题：  
  - 1．每组数据计算前：将需要清空的变量清空。$☆☆☆☆☆$  
  - 2．输出格式一定不能错！!! 每个输出都要检查！!!  
  - 使用桶排序等需要使用 $a_i$ 的取值范围！例如 `T3 Code` 里面的 `last` 数组。必须考虑到 $a_i$ 的取值范围，否则会爆下标 RE.

### tip

在 `windows` 下和 `linux` 下比较两个文件的不同：  

`Windows`:  
```
fc 1.out t1.out  
fc 1.out t1.out > result.txt  
fc /N file1.txt file2.txt > differences.txt  显示行号  
```

`Linux`: `diff` 指令  

### warning

特别注意：`fc` 指令比较时，要求严格一致，如果我们输出的内容每一行结尾多了空格，也认为是不同的。另外，如果题目给的测评数据 `ans` 文件是 `linux` 下创造的，但我们是在 `windows` 下生成的 `.out`，换行符是不一样，`fc` 也会比较出来！!!

## T3 Code
```cpp
/*
 * > CPP Code Snippet <
 * > Powered by Microsoft Visual Studio Code <
 *
 * @Author    FrankWKD (wangkedi01)
 * @Date      2025-07-16
 * @Network   "https://oj.czos.cn/contest/problem?id=25573&pid=2"
 * @License   GNU General Public License 2.0
 * @Platform  [Frank]iMac Ubuntu Pro 24.04 LTS
 * @FileName  T3.cpp
 * @FilePath  /media/frank/FrankW/_default/_Mine!/Working/code-spaces/czos/CSP-S_Practise/Lesson2/T3.cpp
 * @Solution  --
 */

// #pragma GCC optimize(3)
#include <bits/stdc++.h>
using namespace std;
#define int long long
const int N = 2e5 + 10;
int a[N], b[N], f[N][2], n, t, last[1000010];  // warning: last
signed main() {
    // freopen("sample.in","r",stdin);
    // freopen("sample.out","w",stdout);
    // ios::sync_with_stdio(false);
    // cin.tie(0); cout.tie(0);
    cin >> t;
    while (t--) {
        memset(last, 0, sizeof last);
        memset(b, 0, sizeof b);
        memset(f, 0, sizeof f);
        cin >> n;
        for (int i = 1; i <= n; i++) {
            cin >> a[i];
        }
        for (int i = 1; i <= n; i++) {
            if (a[i] == a[i - 1])
                b[i] = a[i] + b[i - 1];
            else
                b[i] = b[i - 1];
        }
        for (int i = 1; i <= n; i++) {
            f[i][0] = max(f[i - 1][1], f[i - 1][0]);
            if (last[a[i]] > 0) {
                f[i][1] = a[i] + max(f[last[a[i]] + 1][0], f[last[a[i]] + 1][1]) + ((last[a[i]] + 1 < i - 1) ? (b[i - 1] - b[last[a[i]] + 1]) : 0);
            }
            last[a[i]] = i;
        }
        cout << max(f[n][0], f[n][1]) << endl;
    }
    // fclose(stdin);
    // fclose(stdout);
    return 0;
}
```
# T4 P8819 [CSP-S 2022] 星战

## 题目描述

在这一轮的星际战争中，我方在宇宙中建立了 $n$ 个据点，以 $m$ 个单向虫洞连接。我们把终点为据点 $u$ 的所有虫洞归为据点 $u$ 的虫洞。

战火纷飞之中这些虫洞很难长久存在，敌人的打击随时可能到来。这些打击中的有效打击可以分为两类：

1. 敌人会摧毁某个虫洞，这会使它连接的两个据点无法再通过这个虫洞直接到达，但这样的打击无法摧毁它连接的两个据点。
2. 敌人会摧毁某个据点，由于虫洞的主要技术集中在出口处，这会导致该据点的所有还未被摧毁的虫洞被一同摧毁。而从这个据点出发的虫洞则**不会摧毁**。

注意：摧毁只会导致虫洞不可用，而不会消除它的存在。

为了抗击敌人并维护各部队和各据点之间的联系，我方发展出了两种特种部队负责修复虫洞：

- A 型特种部队则可以将某个特定的虫洞修复。
- B 型特种部队可以将某据点的所有损坏的虫洞修复。

考虑到敌人打击的特点，我方并未在据点上储备过多的战略物资。因此只要这个据点的某一条虫洞被修复，处于可用状态，那么这个据点也是可用的。

我方掌握了一种苛刻的空间特性，利用这一特性我方战舰可以沿着虫洞瞬移到敌方阵营，实现精确打击。

为了把握发动反攻的最佳时机，指挥部必须关注战场上的所有变化，为了寻找一个能够进行反攻的时刻。总指挥认为：

- 如果从我方的任何据点出发，在选择了合适的路线的前提下，可以进行无限次的虫洞穿梭（可以多次经过同一据点或同一虫洞），那么这个据点就可以**实现反击**。
- 为了使虫洞穿梭的过程连续，尽量减少战舰在据点切换虫洞时的质能损耗，当且仅当**只有一个从该据点出发的虫洞可用**时，这个据点可以**实现连续穿梭**。
- 如果我方所有据点都可以**实现反击**，也都可以**实现连续穿梭**，那么这个时刻就是一个绝佳的**反攻**时刻。

总司令为你下达命令，要求你根据战场上实时反馈的信息，迅速告诉他当前的时刻是否能够进行一次**反攻**。

#### 输入格式

输入的第一行包含两个正整数 $n,m$。

接下来 $m$ 行每行两个数 $u,v$，表示一个从据点 $u$ 出发到据点 $v$ 的虫洞。保证 $u \ne v$，保证不会有两条相同的虫洞。初始时所有的虫洞和据点都是完好的。

接下来一行一个正整数 $q$ 表示询问个数。

接下来 $q$ 行每行表示一次询问或操作。首先读入一个正整数 $t$ 表示指令类型：

- 若 $t = 1$，接下来两个整数 $u, v$ 表示敌人摧毁了从据点 $u$ 出发到据点 $v$ 的虫洞。保证该虫洞存在且未被摧毁。
- 若 $t = 2$，接下来一个整数 $u$ 表示敌人摧毁了据点 $u$。如果该据点的虫洞已全部被摧毁，那么这次袭击不会有任何效果。
- 若 $t = 3$，接下来两个整数 $u, v$ 表示我方修复了从据点 $u$ 出发到据点 $v$ 的虫洞。保证该虫洞存在且被摧毁。
- 若 $t = 4$，接下来一个整数 $u$ 表示我方修复了据点 $u$。如果该据点没有被摧毁的虫洞，那么这次修复不会有任何效果。

在每次指令执行之后，你需要判断能否进行一次反攻。如果能则输出 `YES` 否则输出 `NO`。

#### 输出格式

输出一共 $q$ 行。对于每个指令，输出这个指令执行后能否进行反攻。

#### 输入输出样例 #1

##### 输入 #1

```
3 6
2 3
2 1
1 2
1 3
3 1
3 2
11
1 3 2
1 2 3
1 1 3
1 1 2
3 1 3
3 3 2
2 3
1 3 1
3 1 3
4 2
1 3 2
```

##### 输出 #1

```
NO
YES
NO
YES
NO
YES
NO
```

#### 说明/提示

**【样例解释 \#1】**

虫洞状态可以参考下面的图片，图中的边表示存在且未被摧毁的虫洞：

![](https://cdn.luogu.com.cn/upload/image_hosting/giqzyc7r.png)

**【样例 \#2】**

见附件中的 `galaxy/galaxy2.in` 与 `galaxy/galaxy2.ans`。

**【样例 \#3】**

见附件中的 `galaxy/galaxy3.in` 与 `galaxy/galaxy3.ans`。

**【样例 \#4】**

见附件中的 `galaxy/galaxy4.in` 与 `galaxy/galaxy4.ans`。

**【数据范围】**

对于所有数据保证：$1 \le n \le 5 \times {10}^5$，$1 \le m \le 5 \times {10}^5$，$1 \le q \le 5 \times {10}^5$。

|    测试点    |      $n \le$      |      $m \le$      |      $q \le$      |              特殊限制              |
| :----------: | :---------------: | :---------------: | :---------------: | :--------------------------------: |
|  $1 \sim 3$  |       $10$        |       $20$        |       $50$        |                 无                 |
|  $4 \sim 8$  |     ${10}^3$      |     ${10}^4$      |     ${10}^3$      |                 无                 |
| $9 \sim 10$  | $5 \times {10}^5$ | $5 \times {10}^5$ | $5 \times {10}^5$ | 保证没有 $t = 2$ 和 $t = 4$ 的情况 |
| $11 \sim 12$ | $5 \times {10}^5$ | $5 \times {10}^5$ | $5 \times {10}^5$ |      保证没有 $t = 4$ 的情况       |
| $13 \sim 16$ |     ${10}^5$      | $5 \times {10}^5$ | $5 \times {10}^5$ |                 无                 |
| $17 \sim 20$ | $5 \times {10}^5$ |  $5\times 10^5$   | $5 \times {10}^5$ |                 无                 |
## T4 Sol
### D - 星战 [CSP-S 2022](图论，哈希)
**摧毁：**
1. 摧毁虫洞（有向边）。
2. 摧毁据点（摧毁点的入边，出边不影响）。

**修复：**
1. A 型：修复特定的虫洞（有向边）。
2. B 型：将某个据点所有损坏的虫洞修复（所有有向边，入边）。

**反攻：**
1. 从任何点出发，选择合适的路线，一定能走到环上。也就是能走到环上的点，是反击点。
2. 连续穿梭：某个点的出度为 $1$。
3. 反攻：所有点都能实现反击，也能实现连续穿梭。即：所有点出度都是 $1$，且能走到环上。

由于所有点出度为 $1$，必然有环，因此不用判环，只需要关注出度为 $1$ 的条件。

**操作：**
$t=1$，摧毁 $uv$ 之间的边
$t=2$，摧毁 $u$ 点（摧毁入边，出边不影响）
$t=3$，修复 $uv$ 之间的边
$t=4$，修复了 $u$（入边）

**解法一：** $40$ 分的做法：按题意模拟。
时间复杂度：$\mathcal{O}(q(n + m))$。
实际编程时，可以将所有的边反过来，将出度统计变成入度统计。
因为要找到哪些边指向当前点 $u$，是不方便的，但是要查看 $u$ 点的所有出边可以枚举邻接表。
### 100pts 做法
维护点 $1\sim n$ 出边集合的哈希值即可。
## T4 Code

```cpp
/*
 * > CPP Code Snippet <
 * > Powered by Microsoft Visual Studio Code <
 *
 * @Author    FrankWKD (wangkedi01)
 * @Date      2025-07-18
 * @Network   "https://oj.czos.cn/contest/problem?id=25573&pid=3&_pjax=%23p0"
 * @License   GNU General Public License 2.0
 * @Platform  [Frank]iMac Ubuntu Pro 24.04 LTS
 * @FileName  T4[100pts].cpp
 * @FilePath  /media/frank/FrankW/_default/_Mine!/Working/code-spaces/czos/CSP-S_Practise/Lesson2/T4[100pts].cpp
 * @Solution  --
 */

// #pragma GCC optimize(3)
#include <bits/stdc++.h>
using namespace std;
const int N = 5e5 + 10;
int n, m, q, w[N], x, y, t;
long long din[N], t_din[N], tot, ans;
mt19937 rnd(time(0));
int main() {
    cin >> n >> m;
    for (int i = 1; i <= n; i++) ans += (w[i] = rnd());
    for (int i = 1; i <= m; i++) {
        cin >> x >> y;
        din[y] += w[x],t_din[y] = din[y],tot += w[x];
    }
    cin >> q;
    for (int i = 1; i <= q; i++) {
        cin >> t >> x;
        if (t == 1) {
            cin >> y;
            din[y] -= w[x],tot -= w[x];
        } else if (t == 2) 
            tot -= din[x],din[x] = 0;
        else if (t == 3) {
            cin >> y;
            din[y] += w[x],tot += w[x];
        } else if (t == 4)
            tot += t_din[x] - din[x],din[x] = t_din[x];
        if (tot == ans) cout << "YES\n";
        else cout << "NO\n";
    }
    return 0;
}
```
# T5 P2668 [NOIP 2015 提高组] 斗地主

#### 题目背景

NOIP2015 Day1T3

## 题目描述

牛牛最近迷上了一种叫斗地主的扑克游戏。斗地主是一种使用黑桃、红心、梅花、方片的 $A$ 到 $K$ 加上大小王的共 $54$ 张牌来进行的扑克牌游戏。在斗地主中，牌的大小关系根据牌的数码表示如下：$3<4<5<6<7<8<9<10<J<Q<K<A<2<\text{小王}<\text{大王}$，而花色并不对牌的大小产生影响。每一局游戏中，一副手牌由 $n$ 张牌组成。游戏者每次可以根据规定的牌型进行出牌，首先打光自己的手牌一方取得游戏的胜利。

现在，牛牛只想知道，对于自己的若干组手牌，分别最少需要多少次出牌可以将它们打光。请你帮他解决这个问题。

需要注意的是，本题中游戏者每次可以出手的牌型与一般的斗地主相似而略有不同。具体规则如下：

 ![](https://cdn.luogu.com.cn/upload/pic/1827.png) 

**本题数据随机，不支持 hack，要 hack 或强力数据请点击 [这里](https://www.luogu.org/problem/P2540)**。

#### 输入格式

第一行包含用空格隔开的 $2$ 个正整数 $T,n$，表示手牌的组数以及每组手牌的张数。

接下来 $T$ 组数据，每组数据 $n$ 行，每行一个非负整数对 $a_i,b_i$，表示一张牌，其中 $a_i$ 表示牌的数码，$b_i$ 表示牌的花色，中间用空格隔开。特别的，我们用 $1$ 来表示数码 $A$，$11$ 表示数码 $J$，$12$ 表示数码 $Q$，$13$ 表示数码 $K$；黑桃、红心、梅花、方片分别用 $1-4$ 来表示；小王的表示方法为 `0 1`，大王的表示方法为 `0 2`。

#### 输出格式

共 $T$ 行，每行一个整数，表示打光第 $i$ 组手牌的最少次数。

#### 输入输出样例 #1

##### 输入 #1

```
1 8
7 4
8 4
9 1
10 4
11 1
5 1
1 4
1 1
```

##### 输出 #1

```
3
```

#### 输入输出样例 #2

##### 输入 #2

```
1 17
12 3
4 3
2 3
5 4
10 2
3 3
12 2
0 1
1 3
10 1
6 2
12 1
11 3
5 2
12 4
2 2
7 2
```

##### 输出 #2

```
6
```

#### 说明/提示

**样例 1 说明**

共有 $1$ 组手牌，包含 $8$ 张牌：方片 $7$，方片 $8$，黑桃 $9$，方片 $10$，黑桃 $J$，黑桃 $5$，方片 $A$ 以及黑桃 $A$。可以通过打单顺子（方片 $7$，方片 $8$，黑桃 $9$，方片 $10$，黑桃 $J$），单张牌（黑桃 $5$）以及对子牌（黑桃 $A$以及方片 $A$）在 $3$ 次内打光。

对于不同的测试点， 我们约定手牌组数 $T$ 与张数 $n$ 的规模如下：

| 测试点编号 | $T=$  | $n=$  |
| :--------: | :---: | :---: |
|     1      | $100$ |  $2$  |
|     2      | $100$ |  $2$  |
|     3      | $100$ |  $3$  |
|     4      | $100$ |  $3$  |
|     5      | $100$ |  $4$  |
|     6      | $100$ |  $4$  |
|     7      | $100$ | $10$  |
|     8      | $100$ | $11$  |
|     9      | $100$ | $12$  |
|     10     | $100$ | $13$  |
|     11     | $100$ | $14$  |
|     12     | $100$ | $15$  |
|     13     | $10$  | $16$  |
|     14     | $10$  | $17$  |
|     15     | $10$  | $18$  |
|     16     | $10$  | $19$  |
|     17     | $10$  | $20$  |
|     18     | $10$  | $21$  |
|     19     | $10$  | $22$  |
|     20     | $10$  | $23$  |

数据保证：所有的手牌都是随机生成的。

## T5 Sol

- 这道题的解法核心就在于暴力地去搜顺子，每搜完一层以后就贪心地出散牌。
- 无论怎样来说，搜顺子有利于我们快速而轻松地打出 $5$ 张甚至以上的牌，其余的东西，就留到最后慢慢打出就好了。
- NOIP 的搜索题以搜索为辅，核心在于模拟（几乎全是模拟）。所以说：细节很重要！！！
- 最好把每一种打牌的方式分拆开来，按照贪心的顺序自上而下排列，搜索时只搜三种顺子，万万不可将打散牌也放进搜索里去（就是这样我连样例二都出不来。）。
- 我的做法是每搜新的一层时，统计牌张数的数目，这样有利于减少枚举量（细节见 dfs 主程序以及打散牌的函数）。
- 在枚举顺子时，一定不可把`2`、`小王`、`大王`统计进去，但是在打散牌时，`小王`、`大王`当做一对来出，当做单牌或者与 $3$ 张 $4$ 张一起打出（虽说斗地主里王往往都在最后出，但你在这里是没有对手的）。
- 四带二不仅可以带两张单牌，带一对牌或带两对牌都是可以的！
## T5 Sol
```cpp
/*
 * > CPP Code Snippet <
 * > Powered by Microsoft Visual Studio Code <
 *
 * @Author    FrankWKD (wangkedi01)
 * @Date      2025-07-18
 * @Network   "https://oj.czos.cn/contest/problem?id=25573&pid=4"
 * @License   GNU General Public License 2.0
 * @Platform  [Frank]iMac Ubuntu Pro 24.04 LTS
 * @FileName  T5.cpp
 * @FilePath  /media/frank/FrankW/_default/_Mine!/Working/code-spaces/czos/CSP-S_Practise/Lesson2/T5.cpp
 * @Solution  --
 */

// #pragma GCC optimize(3)
#include <bits/stdc++.h>
using namespace std;
int T, n;
int ans, s[22], c[5];
int num() {
    int tot = 0;
    memset(c, 0, sizeof c);
    int t = (s[0] == 2);  // 判断是否有双王
    for (int i = 0; i <= 14; i++) {
        c[s[i]]++;  // 有 4 张牌的种类数，有 1 张牌的种类数，有 2 张牌的种类数。..
    }
    while (c[4] > 0 and c[2] - t >= 2) {
        c[4]--;
        c[2] -= 2;
        tot++;
    }
    while (c[4] > 0 and c[1] >= 2) {
        c[4]--;
        c[1] -= 2;
        tot++;
    }
    while (c[4] > 0 and c[2] > 0) {
        c[4]--;
        c[2] -= 1;
        tot++;
    }
    while (c[3] > 0 and c[2] - t > 0) {
        c[3]--;
        c[2] -= 1;
        tot++;
    }
    while (c[3] > 0 and c[1] > 0) {
        c[3]--;
        c[1] -= 1;
        tot++;
    }
    return tot + c[1] + c[2] + c[3] + c[4];
}
void dfs(int x) {
    // x: 出顺子的数量
    if (x >= ans)
        return;                 // 最优性剪枝
    ans = min(ans, x + num());  // 更新答案
    for (int i = 3; i > 0; i--) {
        for (int j = 3; j <= 14; j++) {
            int p = j;
            while (s[p] >= i and p <= 14) {
                if ((i == 3 and p - j + 1 >= 2) or (i == 2 and p - j + 1 >= 3) or (i == 1 and p - j + 1 >= 5)) {
                    for (int k = j; k <= p; k++) s[k] -= i;
                    dfs(x + 1);  // 搜索～
                    for (int k = j; k <= p; k++) s[k] += i;
                }
                p++;  // warning!
            }
        }
    }
}

int main() {
    // freopen("sample.in","r",stdin);
    // freopen("sample.out","w",stdout);
    // ios::sync_with_stdio(false);
    // cin.tie(0); cout.tie(0);
    cin >> T >> n;
    while (T--) {
        ans = n;
        memset(s, 0, sizeof s);
        for (int i = 1; i <= n; i++) {
            int a, b;
            cin >> a >> b;
            if (a == 1)
                a = 14;
            s[a]++;
        }
        dfs(0);
        cout << ans << endl;
    }
    // fclose(stdin);
    // fclose(stdout);
    return 0;
}
```
# T6 P5665 [CSP-S2019] 划分

## 题目描述

2048 年，第三十届 CSP 认证的考场上，作为选手的小明打开了第一题。这个题的样例有 $n$ 组数据，数据从 $1 \sim n$ 编号，$i$ 号数据的规模为 $a_i$。

小明对该题设计出了一个暴力程序，对于一组规模为 $u$ 的数据，该程序的**运行时间**为 $u^2$。然而这个程序运行完一组规模为 $u$ 的数据之后，它将在任何一组规模**小于** $u$ 的数据上运行错误。样例中的 $a_i$ 不一定递增，但小明又想在不修改程序的情况下正确运行样例，于是小明决定使用一种非常原始的解决方案：将所有数据划分成若干个数据段，段内数据编号**连续**，接着将同一段内的数据合并成新数据，其规模等于段内原数据的**规模之和**，小明将让新数据的规模能够递增。

也就是说，小明需要找到一些分界点 $1 \leq k_1 \lt k_2 \lt \cdots \lt k_p \lt n$，使得

$$ \sum_{i=1}^{k_1} a_i \leq \sum_{i=k_1+1}^{k_2} a_i \leq \cdots \leq \sum_{i=k_p+1}^{n} a_i $$

注意 $p$ 可以为 $0$ 且此时 $k_0 = 0$，也就是小明可以将所有数据合并在一起运行。

小明希望他的程序在正确运行样例情况下，运行时间也能尽量小，也就是**最小化**

$$ (\sum_{i=1}^{k_1} a_i)^2 + (\sum_{i=k_1+1}^{k_2} a_i)^2 + \cdots + (\sum_{i=k_p+1}^{n} a_i)^2 $$

小明觉得这个问题非常有趣，并向你请教：给定 $n$ 和 $a_i$，请你求出最优划分方案下，小明的程序的最小运行时间。

#### 输入格式

**由于本题的数据范围较大，部分测试点的 $a_i$ 将在程序内生成。**

第一行两个整数 $n, type$。$n$ 的意义见题目描述，$type$ 表示输入方式。
1. 若 $type = 0$，则该测试点的 $a_i$ **直接给出**。输入文件接下来：第二行 $n$ 个以空格分隔的整数 $a_i$，表示每组数据的规模。
2. 若 $type = 1$，则该测试点的 $a_i$ 将**特殊生成**，生成方式见后文。输入文件接下来：第二行六个以空格分隔的整数 $x, y, z, b_1, b_2, m$。接下来 $m$ 行中，第 $i (1 \leq i \leq m)$ 行包含三个以空格分隔的正整数 $p_i, l_i, r_i$。

对于 $type = 1$ 的 $23 \sim 25$ 号测试点，$a_i$ 的生成方式如下：

给定整数 $x, y, z, b_1, b_2, m$，以及 $m$ 个三元组 $(p_i, l_i, r_i)$。

保证 $n \geq 2$。若 $n \gt 2$，则 $\forall 3 \leq i \leq n, b_i = (x \times b_{i−1} + y \times b_{i−2} + z) \mod 2^{30}$。

保证 $1 \leq p_i \leq n, p_m = n$。令 $p_0 = 0$，则 $p_i$ 还满足 $\forall 0 \leq i \lt m$ 有 $p_i \lt p_{i+1}$。

对于所有 $1 \leq j \leq m$，若下标值 $i (1 \leq i \leq n)$满足 $p_{j−1} \lt i \leq p_j$，则有

$$a_i = \left(b_i \mod \left( r_j − l_j + 1 \right) \right) + l_j$$

**上述数据生成方式仅是为了减少输入量大小，标准算法不依赖于该生成方式。**

#### 输出格式

输出一行一个整数，表示答案。

#### 输入输出样例 #1

##### 输入 #1

```
5 0
5 1 7 9 9
```

##### 输出 #1

```
247
```

#### 输入输出样例 #2

##### 输入 #2

```
10 0
5 6 7 7 4 6 2 13 19 9
```

##### 输出 #2

```
1256
```

#### 输入输出样例 #3

##### 输入 #3

```
10000000 1
123 456 789 12345 6789 3
2000000 123456789 987654321
7000000 234567891 876543219
10000000 456789123 567891234
```

##### 输出 #3

```
4972194419293431240859891640
```

#### 说明/提示

【样例 1 解释】

最优的划分方案为 $\{5,1\}, \{7\}, \{9\}, \{9\}$。由 $5 + 1 \leq 7 \leq 9 \leq 9$ 知该方案合法。

答案为 $(5 + 1)^2 + 7^2 + 9^2 + 9^2 = 247$。

虽然划分方案 $\{5\}, \{1\}, \{7\}, \{9\}, \{9\}$ 对应的运行时间比 $247$ 小，但它不是一组合法方案，因为 $5 \gt 1$。

虽然划分方案 $\{5\}, \{1,7\}, \{9\}, \{9\}$ 合法，但该方案对应的运行时间为 $251$，比 $247$ 大。

【样例 2 解释】

最优的划分方案为 $\{5\}, \{6\}, \{7\}, \{7\}, \{4,6,2\}, \{13\}, \{19,9\}$。

【数据范围】

| 测试点编号   | $n \leq$        | $a_i \leq$ | $type =$ |
| :----------- | :-------------- | :--------- | :------- |
| $1 \sim 3$   | $10$            | $10$       | $0$      |
| $4 \sim 6$   | $50$            | $10^3$     | $0$      |
| $7 \sim 9$   | $400$           | $10^4$     | $0$      |
| $10 \sim 16$ | $5000$          | $10^5$     | $0$      |
| $17 \sim 22$ | $5 \times 10^5$ | $10^6$     | $0$      |
| $23 \sim 25$ | $4 \times 10^7$ | $10^9$     | $1$      |

对于 $type=0$ 的所有测试点，保证最后输出的答案 $\leq 4 \times 10^{18}$。

所有测试点满足：$type \in \{0,1\}$，$2 \leq n \leq 4 \times 10^7$，$1 \leq a_i \leq 10^9$，$1 \leq m \leq 10^5$，$1 \leq l_i \leq r_i \leq 10^9$，$0 \leq x,y,z,b_1,b_2 \lt 2^{30}$。
## T6 Sol
### 解法一：$12pts$
对于前 $n\le 10$，$dfs$ 暴力枚举所有分割的情况。
### 解法二：$36pts$
对于 $n\le 400$。
- 1．状态定义$f[j][i]$：前 $i$ 个数，将 $[j,i]$ 划到最后一组的最优解。
- 2．状态转移$f[j][i] = \min(f[j][i], f[k][j-1] + \text{sum}(j,i) \times \text{sum}(j,i))$；
    - 满足：$\text{sum}(j,i) > \text{sum}(k,j-1)$，$j\in [i,2]$，$k\in [j-1,1]$。
- 3．边界条件 $f=0$，$f[i]=\text{sum}(1,i)\times\text{sum}(1,i)$。
- 4．答案 $\min(f[j][n])$。
### 解法三：$64pts$
每个点的最佳决策点实际上是最靠后的决策点，因此只要记录每个点的最佳决策点，可以将时间复杂度降到 $O(n²)$。

### 解法四：$100pts$（单调队列优化）

1. 为方便解题，将 $p[i]$ 重新定义为：以第 $i$ 个点结尾的一段的开始位置 $-1$ 的位置，
    - 即：上一段的末尾，也就是 $[p[i]+1,i]$ 为最后一段的范围。
2. 则如果 $j$ 要满足是 $i$ 的转移点，则需有：$s[i]-s[j]\ge s[j]-s[p[j]]\to s[i] \ge 2 \times s[j] - s[p[j]]$。即：最后一个满足式子的 $j$ 即为 $i$ 的转移点。
3. 设：$g[j]=2\times s[j]-s[p[j]]$。则需找到满足：$s[i]\ge g[j]$的最后侧的 $j$。设有：$k<j,g[k]\ge g[j],s[i]\ge g[k]$。分析可知：$k$ 不会成为 $s[i]$ 的转移点（$j$ 更靠右），也不会成为 $s[i+1]$ 及后续 $s[j]$ 的转移点（$s[j]$ 不递减）。
因此点 $k$ 没有存在的意义？可以删除。因此可以通过单调递增队列，维护 $g[j]$。
#### 3．如何找到最佳转移点？
由于 $g[j]$ 通过单调队列维护，因此可以找到 $g[q[h]]\le s[i]$ 的元素出队，直到遇到 $g[q[h]]>s[i]$，则前一个 $q[h]$ 就是最佳转移点。
## T6 Code
```cpp
/*
 * > CPP Code Snippet <
 * > Powered by Microsoft Visual Studio Code <
 *
 * @Author     FrankWKD
 * @Date       2025-08-06
 * @Network    "https://oj.czos.cn/contest/problem?id=25573&pid=5"
 * @License    GNU General Public License 2.0
 * @Platform   [Frank]iMac Ubuntu Pro 24.04 LTS
 * @Name       T6.cpp
 * @Path       /media/frank/FrankW1/_default/Mine/Working/code-spaces/OI/OJ/czos/CSP-S_Practise/Lesson2/T6.cpp
 * @Sol        --
 */

// #pragma GCC optimize(3)
#define _for(cc, ba, ab) for (register int cc = (ba); cc <= (ab); cc++)
#define for_(cc, ba, ab) for (register int cc = (ba); cc >= (ab); cc--)
#include <bits/stdc++.h>
using namespace std;
const int N = 4e7 + 10;
const int M = 1e5 + 10;
const int mod = (1 << 30);
#define i2 __int128
// MLE Warning
int n, type, x, y, z, m, a[N], b[N], p[M], l[M], r[M];
int pre[N];

long long sum[N];

int h, t, q[N];
long long get(long long x) {
    return 2 * sum[x] - sum[pre[x]];
}
void prlonglong(i2 ans) {
    string r = "";
    while (ans) {
        r = to_string((long long)(ans % 10)) + r;
        ans /= 10;
    }
    cout << r;
}
signed main() {
    // freopen("sample.in","r",stdin);
    // freopen("sample.out","w",stdout);
    // ios::sync_with_stdio(false);
    // cin.tie(0); cout.tie(0);
    cin >> n >> type;
    if (type == 1) {
        cin >> x >> y >> z >> b[1] >> b[2] >> m;
        _for(i, 1, m) cin >> p[i] >> l[i] >> r[i];
        _for(i, 3, n) b[i] = ((long long)b[i - 1] * x + (long long)b[i - 2] * y + z) % mod;
        _for(i, 1, m) _for(j, p[i - 1] + 1, p[i]) a[j] = ((long long)b[j] % (r[i] - l[i] + 1)) + l[i], sum[j] = sum[j - 1] + a[j];
    } else
        _for(i, 1, n) cin >> a[i], sum[i] = sum[i - 1] + a[i];
    h = 0, t = 0;
    _for(i, 1, n) {
        while (h <= t and sum[i] >= get(q[h])) h++;
        h--;
        pre[i] = q[h];
        while (h <= t and get(i) <= get(q[t])) t--;
        q[++t] = i;
    }
    i2 ans = 0;
    for (long long i = n; i; i = pre[i]) {
        i2 s = sum[i] - sum[pre[i]];
        ans += s * s;
    }
    prlonglong(ans);
    return 0;
}
```