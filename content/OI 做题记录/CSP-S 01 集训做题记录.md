---
title: CSP-S 01 集训做题记录
date: 2025-07-12
tags:
  - CSP
  - 做题记录
  - OI
licenseName: CC-BY-NC-SA 4.0
---
> Upd: 2025.07.14  
> Upd: 2025-08-02 补充 T5。T4 待补充。  
> Upd: 2025-08-05 补充 T4。  
# T1 禁卫军  
  
#### 题目描述  
  
数字王国正在挑选最强壮，最独一无二的勇士作为国王的禁卫军。  
  
有 $n$ 个数字士兵参与了这次选拔，分别为 $w_1, w_2, \dots, w_n$。其中可能存在多个相同的数字士兵，选拔的条件是：如果这个数不能被剩下的 $n - 1$ 个数整除，就可以成为禁卫军。  
  
请问，其中会有多少个士兵能顺利加入禁卫军。  
  
#### 输入  
  
第一行读入一个整数 $n$。  
  
第二行读入 $n$ 个整数表示所有数字士兵 $w_i$，用空格隔开。  
  
#### 输出  
  
输出一个整数，表示有多少个士兵能顺利加入禁卫军。  
  
#### 样例  
  
**样例 1**  
  
- 输入：  
  
```  
5  
3 7 9 16 17  
```  
  
输出：  
  
```  
4  
```  
  
**样例 2**  
输入：  
  
```  
5  
1 2 3 4 5  
```  
  
输出：  
  
```  
1  
```  
  
**样例 3**  
输入：  
  
```  
5  
2 2 3 3 5  
```  
  
输出：  
  
```  
1  
```  
  
#### 说明  
  
- **样例 1 解释**：数列中 $3, 7, 16, 17$ 不能被数列中其它整数整除，$9$ 会被 $3$ 整除，所以有 $4$ 个。  
- **数据规模**：  
  
  - $1 \le w_i \le 10^6$。  
  - 对于 $50\%$ 的数据：$1 \le n \le 10000$。  
  - 对于 $100\%$ 的数据：$1 \le n \le 100000$。  
  
## T1 Sol  
  
**问题描述**：  
求有多少个数，不能被自己以外的其他数字整除。  
  
#### 部分分思路  
  
若暴力枚举，需遍历每个数与其他数的整除关系，时间复杂度为 $\mathcal{O}(n^2)$  。  
  
#### 满分思路  
  
样例隐含规律：若存在 `1`，则其他数会被 `1`整除；但多个 `1`间可互相整除。  
  
可借鉴**埃筛**思想，标记不符合要求的数：  
  
1. 先将数排序，再从小到大遍历。对每个未被标记的数，标记其所有倍数为「不符合要求」（因这些倍数能被当前数整除）。  
2. 枚举到某数时，若未被标记为「不符合要求」，则该数可能符合条件。  
3. 若存在相等的数，第 $1$ 个虽可能未被标记，但相等数间可互相整除，**一定不符合要求** 。  
  
### warning 注意！  
  
对于相同的数，他们可以互相牵制，都不算做合法答案。  
  
## T1 Code  
  
```cpp  
/*  
 * > CPP Code Snippet <  
 * > Powered by Microsoft Visual Studio Code <  
 *  
 * @Author    FrankWKD (wangkedi01)  
 * @Date      2025-07-09  
 * @Network   "https://oj.czos.cn/contest/problem?id=25410&pid=0"  
 * @License   GNU General Public License 2.0  
 * @Platform  [Frank]iMac Ubuntu Pro 24.04 LTS  
 * @FileName  T1.cpp  
 * @FilePath  /media/frank/FrankW/_default/_Mine!/Working/code-spaces/czos/CSP-S_Practise/Lesson1/T1.cpp  
 * @Solution  --  
 */  
  
// #pragma GCC optimize(3)  
#include <bits/stdc++.h>  
using namespace std;  
int a[1010100];  
int n;  
bool f[1010100];  
int ans;  
int main() {  
    // freopen("sample.in","r",stdin);  
    // freopen("sample.out","w",stdout);  
    // ios::sync_with_stdio(false);  
    // cin.tie(0); cout.tie(0);  
    cin >> n;  
    for (int i = 1; i <= n; i++) {  
        cin >> a[i];  
    }  
    sort(a + 1, a + 1 + n);  
    if (a[1] == 1) {  
        if (a[2] != 1)  
            cout << 1;  
        else  
            cout << 0;  
        return 0;  
    }  
    for (int i = 1; i <= n; i++) {  
        if (!f[a[i]] and a[i] != a[i + 1])  // 如果相邻的相等还不行呢  
            ans++;  
        for (int j = a[i]; j <= a[n]; j += a[i]) {  
            f[j] = 1;  
        }  
    }  
    cout << ans << endl;  
    // fclose(stdin);  
    // fclose(stdout);  
    return 0;  
}  
```  
  
# T2 [CSP-S2024] 决斗  
  
## 题目描述  
  
今天是小 Q 的生日，他得到了 $n$ 张卡牌作为礼物。这些卡牌属于火爆的“决斗怪兽”，其中，第 $i$ 张卡代表一只攻击力为 $r_i$，防御力也为 $r_i$ 的怪兽。  
  
一场游戏分为若干回合。每回合，小 Q 会选择某只怪兽 $i$ 以及**另一只**怪兽 $j(i \neq j)$，并让怪兽 $i$ 向怪兽 $j$ 发起攻击。此时，若怪兽 $i$ 的攻击力小于等于怪兽 $j$ 的防御力，则无事发生；否则，怪兽 $j$ 的防御被打破，怪兽 $j$ 退出游戏不再参与到剩下的游戏中。一只怪兽在整场游戏中**至多**只能发起一次攻击。当未退出游戏的怪兽都已发起过攻击时，游戏结束。  
  
小 Q 希望决定一组攻击顺序，使得在游戏结束时，未退出游戏的怪兽数量尽可能少。  
  
## 输入格式  
  
输入的第一行包含一个正整数 $n$，表示卡牌的个数。  
  
输入的第二行包含 $n$ 个正整数，其中第 $i$ 个正整数表示第 $i$ 个怪兽的攻击力及防御力 $r_i$。  
  
## 输出格式  
  
输出一行包含一个整数表示游戏结束时未退出游戏的怪兽数量的最小值。  
  
## 输入输出样例 #1  
  
### 输入 #1  
  
```  
5  
1 2 3 1 2  
```  
  
### 输出 #1  
  
```  
2  
```  
  
## 输入输出样例 #2  
  
### 输入 #2  
  
```  
10  
136 136 136 2417 136 136 2417 136 136 136  
```  
  
### 输出 #2  
  
```  
8  
```  
  
## 说明/提示  
  
**【样例 1 解释】**  
  
其中一种最优方案为：第一回合让第 $2$ 只怪兽向第 $1$ 只怪兽发起攻击，第二回合让第 $5$ 只怪兽向第 $4$ 只怪兽发起攻击，第三回合让第 $3$ 只怪兽向第 $5$ 只怪兽发起攻击。此时没有退出游戏的怪兽都进行过攻击，游戏结束。可以证明没有更优的攻击顺序。  
  
**【样例 3】**  
  
见选手目录下的 duel/duel3.in 与 duel/duel3.ans。  
  
该样例满足 $\forall 1 \leq i \leq n, r_i \leq 2$。  
  
**【样例 4】**  
  
见选手目录下的 duel/duel4.in 与 duel/duel4.ans。  
  
**【数据范围】**  
  
对于所有测试数据，保证：$1 \leq n \leq 10^5$，$1 \leq r_i \leq 10^5$。  
  
|   测试点    |     $n$     |    $r_i$    |  特殊性质  |  
| :---------: | :---------: | :---------: | :--------: |  
|  $1\sim 4$  |  $\leq 10$  | $\leq 10^5$ | 无特殊性质 |  
| $5\sim 10$  | $\leq 10^5$ |  $\leq 2$   | 无特殊性质 |  
| $11\sim 15$ |  $\leq 30$  | $\leq 10^5$ | 特殊性质 A |  
| $16\sim 20$ | $\leq 10^5$ | $\leq 10^5$ | 无特殊性质 |  
  
特殊性质 A：保证每个 $r_i$ 在可能的值域中独立均匀随机生成。  
  
## T2 Sol  
  
采用**贪心策略**，结合双指针配对：  
假设存在三个数满足 $a < b < c$，若 $c$ 选择击败 $a$，则 $b$ 无可以击败的对象（因 $b$ 仅能击败比它小的数，而比 $b$ 小的 $a$ 已被 $c$ 击败 ）。  
基于此规律，可用双指针法对数组排序后进行配对，优化剩余数的计算逻辑，得到最少剩余数量。  
  
（注：完整实现需先对数组排序，再通过指针模拟攻击配对过程，此处为核心思路提炼 。）  
  
## T2 Code  
  
```cpp  
/*  
 * > CPP Code Snippet <  
 * > Powered by Microsoft Visual Studio Code <  
 *  
 * @Author    FrankWKD (wangkedi01)  
 * @Date      2025-07-09  
 * @Network   "https://oj.czos.cn/contest/problem?id=25410&pid=1"  
 * @License   GNU General Public License 2.0  
 * @Platform  [Frank]iMac Ubuntu Pro 24.04 LTS  
 * @FileName  T2.cpp  
 * @FilePath  /media/frank/FrankW/_default/_Mine!/Working/code-spaces/czos/CSP-S_Practise/Lesson1/T2.cpp  
 * @Solution  -  
 */  
  
// #pragma GCC optimize(3)  
#include <bits/stdc++.h>  
using namespace std;  
const int N = 100005;  
int a[N], n;  
bool cmp(int m, int b) {  
    return m > b;  
}  
int main() {  
    scanf("%d", &n);  
    for (int i = 1; i <= n; i++) {  
        scanf("%d", &a[i]);  
    }  
    sort(a + 1, a + n + 1, cmp);  
    int ans = n;  
    for (int i = n, j = n; i >= 1; i--) {  
        if (a[i] > a[j]) {  
            j--;  
            ans--;  
        }  
    }  
    printf("%d", ans);  
    return 0;  
}  
```  
  
# T3 [CSP-S 2024] 超速检测  
  
## 题目描述  
  
小 D 新入职了某国的交管部门，他的第一个任务是负责国家的一条长度为 $L$ 的南北主干道的车辆超速检测。为了考考小 D，上司首先需要他解决一个简化的场景。  
  
这个周末，主干道上预计出现 $n$ 辆车，其中第 $i$ 辆车从主干道上距离最南端 $d_i$ 的位置驶入，以 $v_i$ 的初速度和 $a_i$ 的加速度做匀加速运动向北行驶。我们只考虑从南向北的车辆，故 $v_i > 0$，但 $a_i$ 可正可负，也可以为零。当车辆行驶到主干道最北端（即距离最南端为 $L$ 的位置）或速度降为 $0$（这只可能在 $a_i < 0$ 时发生）时，我们认为该车驶离主干道。  
  
主干道上设置了 $m$ 个测速仪，其中第 $j$ 个测速仪位于主干道上距离最南端 $p_j$ 的位置，每个测速仪可以设置开启或关闭。当某辆车经过某个开启的测速仪时，若这辆车的瞬时速度**超过**了道路限速 $V$，那么这辆车就会被判定为超速。注意当车辆驶入与驶出主干道时，如果在对应位置有一个开启的测速仪，这个测速仪也会对这辆车进行测速。  
  
上司首先想知道，如果所有测速仪都是开启的，那么这 $n$ 辆车中会有多少辆车被判定为超速。  
  
其次，为了节能，部门想关闭一部分测速仪。然而，他们不希望漏掉超速的车，也就是说，当 $n$ 辆车里的某辆车在所有测速仪都开启时被判定为超速，他们希望在关闭一部分测速仪以后它依然被判定为超速。上司还想知道在这样的条件下最多可以关闭多少测速仪。  
  
由于 $n$ 很大，上司允许小 D 使用编程解决这两个问题，于是小 D 找到了你。  
  
如果你对于加速度并不熟悉，小 D 贴心地在本题的“提示”部分提供了有关加速度的公式。  
  
## 输入格式  
  
输入的第一行包含一个正整数 $T$，表示数据组数。  
  
接下来包含 $T$ 组数据，每组数据的格式如下：  
  
第一行包含四个整数 $n, m, L, V$，分别表示车辆数量、测速仪数量、主干道长度和道路限速。  
  
接下来 $n$ 行：  
  
第 $i$ 行包含三个整数 $d_i, v_i, a_i$ 描述一辆车。  
  
最后一行包含 $m$ 个整数 $p_1, p_2, \dots , p_m$ 描述道路上所有测速仪的位置。  
  
## 输出格式  
  
对于每组数据：输出一行包含两个整数，第一个整数为所有测速仪都开启时被判定为超速的车辆数量，第二个整数为在不漏掉超速车辆的前提下最多可以关闭的测速仪数量。  
  
## 输入输出样例 #1  
  
### 输入 #1  
  
```  
1  
5 5 15 3  
0 3 0  
12 4 0  
1 1 4  
5 5 -2  
6 4 -4  
2 5 8 9 15  
```  
  
### 输出 #1  
  
```  
3 3  
```  
  
## 说明/提示  
  
**【样例 1 解释】**  
  
在该组测试数据中，主干道长度为 $15$，限速为 $3$，在距离最南端 $2, 5, 8, 9, 15$ 的位置各设有一个测速仪。  
  
- 第一辆车在最南端驶入，以 $3$ 的速度匀速行驶。这辆车在整个路段上都没有超速。  
- 第二辆车在距离最南端 $12$ 的位置驶入，以 $4$ 的速度匀速行驶。在最北端驶离主干道时，它会被距离最南端 $15$ 的测速仪判定为超速。  
- 第三辆车在距离最南端 $1$ 的位置驶入，以 $1$ 的初速度、$4$ 的加速度行驶。其在行驶了 $\frac{3^2-1^2}{2\times 4}=1$ 的距离，即到达 $2$ 的位置时，速度变为 $3$，并在之后一直超速。因此这辆车会被除了距离最南端 $2$ 的测速仪以外的其他测速仪判定为超速。  
- 第四辆车在距离最南端 $5$ 的位置驶入，以 $5$ 的初速度、$-2$ 的加速度行驶。其在行驶了 $\frac{3^2-5^2}{2\times (-2)}$ 的距离，即到达 $9$ 的位置时，速度变为 $3$。因此这辆车在距离最南端 $[5, 9)$ 时超速，会被距离最南端 $5$ 和 $8$ 的两个测速仪判定为超速。  
- 第五辆车在距离最南端 $6$ 的位置驶入，以 $4$ 的初速度、$−4$ 的加速度行驶。在其行驶了 $\frac{3^2-4^2}{2\times (-4)}=\frac{7}{8}$ 的距离后，即这辆车到达 $6\frac{7}{8}$ 的位置时，其速度变为 $3$。因此这辆车在距离最南端 $[6,6\frac{7}{8})$ 时超速，但这段区间内没有测速仪，因此不会被判定为超速。  
  
因此第二、三、四辆车会被判定为超速，输出的第一个数为 $3$。  
  
我们可以关闭距离最南端 $2, 8, 9$ 的三个测速仪，保留 $5$ 和 $15$ 的两个测速仪，此时三辆之前被判定为超速的车依然被判定为超速。可以证明不存在更优方案，因此输出的第二个数为 $3$。  
  
**【样例 2】**  
  
见选手目录下的 detect/detect2.in 与 detect/detect2.ans。  
  
该组样例满足 $n, m \leq 10$。  
  
**【样例 3】**  
  
见选手目录下的 detect/detect3.in 与 detect/detect3.ans。  
  
该组样例满足特殊性质 A，其中前十组测试数据满足 $n, m \leq 3000$。  
  
**【样例 4】**  
  
见选手目录下的 detect/detect4.in 与 detect/detect4.ans。  
  
该组样例满足特殊性质 B，其中前十组测试数据满足 $n, m \leq 3000$。  
  
**【样例 5】**  
  
见选手目录下的 detect/detect5.in 与 detect/detect5.ans。  
  
该组样例满足特殊性质 C，其中前十组测试数据满足 $n, m \leq 3000$。  
  
**【数据范围】**  
  
对于所有测试数据，保证：  
  
- $1 \leq T \leq 20$；  
- $1 \leq n, m \leq 10^5$，$1 \leq L \leq 10^6$，$1 \leq V \leq 10^3$；  
- $0 \leq d_i < L$，$1 \leq v_i \leq 10^3$，$|a_i| \leq 10^3$；  
- $0 \leq p_1 < p_2 < \dots < p_m \leq L$。  
  
| 测试点 | $n,m\leq$ | 特殊性质 |  
| :----: | :-------: | :------: |  
|  $1$   |   $10$    |    无    |  
|  $2$   |   $20$    |    无    |  
|  $3$   |  $3000$   |    A     |  
|  $4$   |  $10^5$   |    A     |  
|  $5$   |  $3000$   |    B     |  
|  $6$   |  $10^5$   |    B     |  
|  $7$   |  $3000$   |    C     |  
|  $8$   |  $10^5$   |    C     |  
|  $9$   |  $3000$   |    无    |  
|  $10$  |  $10^5$   |    无    |  
  
特殊性质 A：保证 $a_i = 0$。  
  
特殊性质 B：保证 $a_i > 0$。  
  
特殊性质 C：保证 $a_i < 0$，且所有车都不在最北端驶离主干道。  
  
**【提示】**  
  
与加速度有关的定义和公式如下：  
  
- 匀加速运动是指物体在运动过程中，加速度保持不变的运动，即每单位时间内速度的变化量是恒定的。  
- 当一辆车的初速度为 $v_0$、加速度 $a\neq 0$，做匀加速运动，则 $t$ 时刻后它的速度 $v_1 = v_0 + a \times t$，它的位移（即行驶路程）$s=v_0\times t+0.5\times a\times t^2$。  
- 当一辆车的初速度为 $v_0$、加速度 $a \neq 0$，做匀加速运动，则当它的位移（即行驶路程）为 $s$ 时，这辆车的瞬时速度为 $\sqrt{v_0^2+2\times a\times s}$。  
- 当一辆车的初速度为 $v_0$、加速度 $a \neq 0$，在它的位移（即行驶路程）为 $\frac{v_1^2-v_0^2}{2a}$ 时，这辆车的瞬时速度为 $v_1$。  
  
如果你使用浮点数进行计算，需要注意潜在的精度问题。  
  
## T3 Sol  
  
#### 题意：  
  
1. 道路长度 $L$，有 $n$ 辆车，第 $i$ 辆车出现在 $d_i$ 点，以 $v_i$ 的初速度和 $a_i$ 的加速度运动。  
2. $a_i$ 可正负或 $0$，当车辆到达 $L$ 或者速度为 $0$（$a_i \lt 0$），表示离开道路。  
3. 有 $m$ 个测速仪，第 $j$ 个测速仪在 $p_j$ 位置，如果车到达 $p_j$ 位置车速 $\gt v$，则超速。  
  
#### 求：  
  
1. 如果测速仪都开启，有几辆被监控拍到超速。  
2. 可以最多关闭几个测速仪，保证还是能测到所有测速仪都打开时的超速车。  
  
以下是为内容中的字母、数字等加上 LaTeX 格式（主要是对变量、公式等进行数学环境标记 ）后的排版，内容本身未做修改：  
  
### 思路：  
  
#### 1. 特判特殊性质 $A$ 匀速行驶。  
  
对于匀速行驶的情况，只有一开始超速，才会被认定为超速。  
关闭几个测速仪：没有车被拍到超速，关 $m$ 个，有车被拍到超速，关 $m\text{-}1$ 个，留最后一个。  
  
#### 2. $100$ 分的讨论。  
  
- 1) 关于超速情况的讨论。  
  
  - 情况一：如果位置 $d_i > p[m]$，即$>$最后一个监控的位置，拍不到。  
  - 情况二：如果初始速度 $v_i \leq v$，且 $a_i \leq 0$，即一开始没有超速，且是匀速或减速，则一定不超速。  
  - 情况三：如果加速运动，且到最后一个监控的位置，速度还未达到 $v$，则不超速。  
    - 如何判断从起点 $d_i$ 到某个位置 $d_2$ 是否超速？用题目给的公式。  
    - 如果满足：$v_i^2 + 2 * a_i * (d_2 - d_i) > v^2$，则超速。  
  - 情况四：如果**减速运动**，且到 $d_i$ 后面的第 $1$ 个监控的位置，速度还未达到 $v$，则不超速。  
    - $d_i$ 后面的第 $1$ 个监控的位置，即：$p$ 数组中从 $d_i$ 开始找第 $1$ 个满足 $\geq d_i$ 的位置，显然二分。其余情况，必然超速。  
    - 对于**加速运动**：二分找到第 $1$ 个超速的监控位置 $L$，从 $L$ 到 $m$ 的所有监控都能拍到超速。  
      对于减速运动：二分找到最后一个超速的监控位置 $R$，从 $d_i$ 后面的第 $1$ 个监控到 $R$ 都能拍到超速。  
      将能拍到每辆车超速监控的起止位置存下来，转换为区间选点问题。  
- 2) 关于区间选点问题的讨论  
  
  - 按右端点升序，优先讨论可能被“包含”的区间，选点时，优先选择最右侧的点即可。  
  - 1. 匀速  
  
    - (1) 第 i 辆车是否超速：$vi > V$。  
    - (2) 最多可以关闭几个测速仪：如果没有车超速，关 $m$ 个，如果有车超速，打开最后一个，关 $m-1$ 个。  
  - 2. 加速  
  
    - (1) 第 i 辆车是否超速：求车到最后一个监控的位置是否超速。  
    - (2) 最多可以关闭几个测速仪：如果没有车超速，关 $m$ 个，如果有车超速，关 $m-1$ 个。  
  - 3. 减速  
  
    - (1) 第 i 辆车是否超速：求出 $\geq di$ 的第 $1$ 个监控的位置 $p$，求到 $p$ 为止是否超速。  
    - (2) 最多可以关闭几个测速仪：计算出能测到当前测速的第 $1$ 个和最后一个测速仪。将问题转换为区间选点问题。  
  
![img](https://img2024.cnblogs.com/blog/3594125/202507/3594125-20250714172354626-1372506292.png)  
  
> cnblogs@FrankWkd  
  
## T3 Code **[40pts WA]**  
  
> 根据特殊性质骗分。  
  
```cpp  
/*  
 * > CPP Code Snippet <  
 * > Powered by Microsoft Visual Studio Code <  
 *  
 * @Author    FrankWKD (wangkedi01)  
 * @Date      2025-07-14  
 * @Network   "https://oj.czos.cn/contest/problem?id=25410&pid=2"  
 * @License   GNU General Public License 2.0  
 * @Platform  [Frank]iMac Ubuntu Pro 24.04 LTS  
 * @FileName  T3[40pts].cpp  
 * @FilePath  /media/frank/FrankW/_default/_Mine!/Working/code-spaces/czos/CSP-S_Practise/Lesson1/T3[40pts].cpp  
 * @Solution  --  
 */  
  
// #pragma GCC optimize(3)  
#include <bits/stdc++.h>  
using namespace std;  
const int N = 1e6 + 10;  
struct node {  
    int d, v, a;  
} a[N];  
int p[N];  
int n, m, len, v;  
int cnt, res, T;  
int pf(int x) {  
    return x * x;  
}  
bool check(int di, int vi, int ai, int ed) {  
    return pf(vi) + 2 * ai * (ed - di) > pf(v);  
}  
int main() {  
    // freopen("sample.in","r",stdin);  
    // freopen("sample.out","w",stdout);  
    // ios::sync_with_stdio(false);  
    // cin.tie(0); cout.tie(0);  
    cin >> T;  
    while (T--) {  
        cin >> n >> m >> len >> v;  
        for (int i = 1; i <= n; i++) {  
            scanf("%d%d%d", &a[i].d, &a[i].v, &a[i].a);  
        }  
        for (int i = 1; i <= m; i++) {  
            cin >> p[i];  
        }  
        cnt = 0;  
        int c0 = 0, c1 = 0;  
        for (int i = 1; i <= n; i++) {  
            if (a[i].a == 0)  
                c0++;  
            else if (a[i].a > 0)  
                c1++;  
            if (a[i].d > p[m])  
                continue;  
            if (a[i].v > v and a[i].a == 0) {  
                cnt++;  
                continue;  
            }  
            if (a[i].a > 0 and check(a[i].d, a[i].v, a[i].a, p[m])) {  
                cnt++;  
                continue;  
            }  
        }  
        if (c0 == n or c1 == n) {  
            if (cnt == 0)  
                res = m;  
            else  
                res = m - 1;  
        }  
        cout << cnt << " " << res << endl;  
    }  
    // fclose(stdin);  
    // fclose(stdout);  
    return 0;  
}  
```  
  
## T3 Code **[100pts AC]**  
  
```cpp  
/*  
 * > CPP Code Snippet <  
 * > Powered by Microsoft Visual Studio Code <  
 *  
 * @Author    FrankWKD (wangkedi01)  
 * @Date      2025-07-14  
 * @Network   "https://oj.czos.cn/contest/problem?id=25410&pid=2&_pjax=%23p0"  
 * @License   GNU General Public License 2.0  
 * @Platform  [Frank]iMac Ubuntu Pro 24.04 LTS  
 * @FileName  T3[100pts].cpp  
 * @FilePath  /media/frank/FrankW/_default/_Mine!/Working/code-spaces/czos/CSP-S_Practise/Lesson1/T3[100pts].cpp  
 * @Solution  --  
 */  
  
// #pragma GCC optimize(3)  
#include <bits/stdc++.h>  
using namespace std;  
const int N = 1e6 + 10;  
struct node {  
    int d, v, a;  
} a[N];  
  
struct range {  
    int b, e;  
} b[N];  
int p[N];  
int n, m, len, v;  
int cnt, T, res;  
  
int pf(int x) {  
    return x * x;  
}  
bool check(int di, int vi, int ai, int ed) {  
    return pf(vi) + 2 * ai * (ed - di) > pf(v);  
}  
pair<int, int> find(int di, int vi, int ai) {  
    int t = lower_bound(p + 1, p + m + 1, di) - p;  
    if (ai > 0) {  
        int l = t;  
        int r = m;  
        while (l <= r) {  
            int mid = l + r >> 1;  
            if (check(di, vi, ai, p[mid]))  
                r = mid - 1;  
            else  
                l = mid + 1;  
        }  
        return {l, m};  
    }  
    if (ai < 0) {  
        int l = t, r = m, mid;  
        while (l <= r) {  
            mid = l + r >> 1;  
            if (check(di, vi, ai, p[mid]))  
                l = mid + 1;  
            else  
                r = mid - 1;  
        }  
        return {t, l - 1};  
    }  
}  
bool cmp(range r1, range r2) {  
    return r1.e < r2.e;  
}  
int solve() {  
    sort(b + 1, b + cnt + 1, cmp);  
    int res = 0, ls = -1;  
    for (int i = 1; i <= cnt; i++) {  
        if (b[i].b > ls) {  
            res++;  
            ls = b[i].e;  
        }  
    }  
    return m - res;  
}  
  
int main() {  
    // freopen("sample.in","r",stdin);  
    // freopen("sample.out","w",stdout);  
    // ios::sync_with_stdio(false);  
    // cin.tie(0); cout.tie(0);  
    cin >> T;  
    while (T--) {  
        cin >> n >> m >> len >> v;  
        for (int i = 1; i <= n; i++) {  
            cin >> a[i].d >> a[i].v >> a[i].a;  
        }  
        for (int i = 1; i <= m; i++) {  
            cin >> p[i];  
        }  
  
        cnt = 0;  
        int c0 = 0, c1 = 0;  
        for (int i = 1; i <= n; i++) {  
            if (a[i].a == 0)  
                c0++;  
            else if (a[i].a > 0)  
                c1++;  
            if (a[i].d > p[m])  
                continue;  
            if (a[i].v > v and a[i].a == 0) {  
                int st = lower_bound(p + 1, p + m + 1, a[i].d) - p;  
                b[++cnt] = {st, m};  
                continue;  
            }  
            if (a[i].v <= v and a[i].a <= 0)  
                continue;  
            if (a[i].a > 0 and !check(a[i].d, a[i].v, a[i].a, p[m]))  
                continue;  
            int t = lower_bound(p + 1, p + 1 + m, a[i].d) - p;  
            if (a[i].a < 0 and !check(a[i].d, a[i].v, a[i].a, p[t]))  
                continue;  
            pair<int, int> pos = find(a[i].d, a[i].v, a[i].a);  
            cnt++;  
            b[cnt] = {pos.first, pos.second};  
        }  
        if (c0 == n or c1 == n) {  
            if (cnt == 0)  
                res = m;  
            else  
                res = m - 1;  
        } else  
            res = solve();  
        cout << cnt << " " << res << endl;  
    }  
    // fclose(stdin);  
    // fclose(stdout);  
    return 0;  
}  
```  
# T4 P9755 [CSP-S 2023] 种树  
  
## 题目描述  
  
你是一个森林养护员，有一天，你接到了一个任务：在一片森林内的地块上种树，并养护至树木长到指定的高度。  
  
森林的地图有 $n$ 片地块，其中 $1$ 号地块连接森林的入口。共有 $n-1$ 条道路连接这些地块，使得每片地块都能通过道路互相到达。最开始，每片地块上都没有树木。  
  
你的目标是：在每片地块上均种植一棵树木，并使得 $i$ 号地块上的树的高度生长到不低于 $a_i$ 米。  
  
你每天可以选择一个未种树且**与某个已种树的地块直接邻接**（**即通过单条道路相连**）的地块，种一棵高度为 $0$ 米的树。如果所有地块均已种过树，则你当天不进行任何操作。特别地，第 $1$ 天你只能在 $1$ 号空地种树。  
  
对每个地块而言，从该地块被种下树的当天开始，该地块上的树每天都会生长一定的高度。由于气候和土壤条件不同，在第 $x$ 天，$i$ 号地块上的树会长高 $\max(b_i + x \times c_i, 1)$ 米。注意这里的 $x$ 是从整个任务的第一天，而非种下这棵树的第一天开始计算。  
  
你想知道：最少需要多少天能够完成你的任务？  
  
#### 输入格式  
  
输入的第一行包含一个正整数 $n$，表示森林的地块数量。  
  
接下来 $n$ 行：每行包含三个整数 $a_i, b_i, c_i$，分别描述一片地块，含义如题目描述中所述。  
  
接下来 $n-1$ 行：每行包含两个正整数 $u_i, v_i$，表示一条连接地块 $u_i$ 和 $v_i$ 的道路。  
  
#### 输出格式  
  
输出一行仅包含一个正整数，表示完成任务所需的最少天数。  
  
#### 输入输出样例 #1  
  
##### 输入 #1  
  
```  
4  
12 1 1  
2 4 -1  
10 3 0  
7 10 -2  
1 2  
1 3  
3 4  
```  
  
##### 输出 #1  
  
```  
5  
```  
  
#### 说明/提示  
  
**【样例 1 解释】**  
  
第 $1$ 天：在地块 $1$ 种树，地块 $1$ 的树木长高至 $2$ 米。  
  
第 $2$ 天：在地块 $3$ 种树，地块 $1, 3$ 的树木分别长高至 $5, 3$ 米。  
  
第 $3$ 天：在地块 $4$ 种树，地块 $1, 3, 4$ 的树木分别长高至 $9, 6, 4$ 米。  
  
第 $4$ 天：在地块 $2$ 种树，地块 $1, 2, 3, 4$ 的树木分别长高至 $14, 1, 9, 6$ 米。  
  
第 $5$ 天：地块 $1, 2, 3, 4$ 的树木分别长高至 $20, 2, 12, 7$ 米。  
  
**【样例 2】**  
  
见选手目录下的 `tree/tree2.in` 与 `tree/tree2.ans`。  
  
**【样例 3】**  
  
见选手目录下的 `tree/tree3.in` 与 `tree/tree3.ans`。  
  
**【样例 4】**  
  
见选手目录下的 `tree/tree4.in` 与 `tree/tree4.ans`。  
  
**【数据范围】**  
  
对于所有测试数据有：$1 \le n \le 10^5,1 \le a_i \le 10^{18}, 1 \le b_i \le 10^9,0 \le |c_i| \le 10^9, 1 \le u_i, v_i \le n$。保证存在方案能在 $10^9$ 天内完成任务。  
  
![T4](https://cdn.luogu.com.cn/upload/image_hosting/aqqd4l0y.png?x-oss-process=image/resize,m_lfit,h_400,w_300)  
  
特殊性质 A：对于所有 $1 ≤ i ≤ n$，均有 $c_i = 0$；  
  
特殊性质 B：对于所有 $1 ≤ i < n$，均有 $u_i = i$、$v_i = i + 1$；  
  
特殊性质 C：与任何地块直接相连的道路均不超过 $2$ 条；  
  
特殊性质 D：对于所有 $1 ≤ i < n$，均有 $u_i = 1$。  
  
## T4 Sol  
### 链  
我们首先来考虑链的部分分。  
  
不难发现，这部分的关键是实现一个形如 $\textbf{check}(x,l,r)$ 的函数，表示 $x$ 结点在 $[l,r]$ 这段时间的生长高度能否达成目标。同时，这也是正解的一个关键函数。  
  
不妨列出 $x$ 结点高度的表达式：  
$$  
\sum_{i=l}^r \max(1, b_x + i c_x)  
$$  
  
先考虑 $c_x \geq 0$ 的情况，那么上式可按如下化简：  
$$  
\begin{aligned}  
\sum_{i=l}^r (b_x + i c_x) &= \sum_{i=l}^r b_x + \sum_{i=l}^r i c_x \\  
&= b_x(r - l + 1) + c_x \cdot \frac{(l + r)(r - l + 1)}{2}  
\end{aligned}  
$$  
  
接下来考虑 $c_x < 0$，此时我们不妨找到使得 $b_x + i c_x \geq 1$ 的最大的 $i$，不难得出：  
$$  
i_{\text{max}} = \left\lfloor \frac{1 - b_x}{c_x} \right\rfloor  
$$  
接下来分情况讨论，可得：  
$$  
h = \begin{cases}  
r - l + 1 & i_{\text{max}} < l, \\  
b_x(r - l + 1) + c_x \cdot \frac{(l + r)(r - l + 1)}{2} & i_{\text{max}} > r, \\  
b_x(i_{\text{max}} - l + 1) + c_x \cdot \frac{(i_{\text{max}} + l)(i_{\text{max}} - l + 1)}{2} + r - i_{\text{max}} & i_{\text{max}} \in [l, r].  
\end{cases}  
$$  
  
至此这个函数实现完毕。而统计答案，你就对链上的第 $i$ 个点二分得出最小的 $r$ 满足 $\textbf{check}(i,i,r)$ 为真的 $r$，所有 $r$ 取个 $\max$ 即可。  
  
### 特殊性质 A  
接着我们考虑特殊性质 A。  
  
不难发现此时每个结点生长到目标高度所需时间与种下的时间无关，即有：  
$$  
t_x = \left\lceil \frac{a_x}{b_x} \right\rceil  
$$  
  
将结点按 $t_x$ 从大到小排序。考虑这样一个贪心，我们依次考虑所有结点，若该结点已被种树就跳过，否则将根到该结点这条链按顺序把未种树的结点种树。显然这样做会得到一个种树顺序的序列，显然由于 $t_x$ 更小的结点不是时间的瓶颈，我们这样做是最优的。  
  
实际实现过程中，我们标记一下每个结点是否种过树。当考虑当前结点 $x$ 时，暴力跳到最后一个未被标记的祖先结点，然后倒序再给每个结点赋上开始种树的时间 $s_x$。取 $s_x + t_x - 1$ 的最大值即可。  
  
### 正解  
想明白了前两个部分，正解就是容易的。  
  
考虑一般情况与特殊性质 A 的区别，不难发现我们无法得到上述的 $t_x$ 了，究其根本是因为树种下的时间会影响种树需要的时间。  
  
我们考虑二分答案，并修改 $t_x$ 的定义为使该结点合法的最晚种树时间。显然在该前提下，$t_x$ 可以用二分答案加上链部分所实现的函数求出。而这时我们按照 $t_x$ 从小到大考虑，不难发现问题就转化为了特殊性质 A 时的问题，套用以上做法即可解决。  
  
时间复杂度 $\mathcal{O}(n \log n \log v)$。用桶排并上二次函数相关知识应该能将 $\log n$ 去掉，不过没必要。  
  
## T4 Code  
```cpp  
/*  
 * > CPP Code Snippet <  
 * > Powered by Microsoft Visual Studio Code <  
 *  
 * @Author     FrankWKD  
 * @Date       2025-08-04  
 * @Network    "https://oj.czos.cn/contest/problem?id=25410&pid=3"  
 * @License    GNU General Public License 2.0  
 * @Platform   [Frank]iMac Ubuntu Pro 24.04 LTS  
 * @Name       T4.cpp  
 * @Path       /media/frank/FrankW/_default/Mine/Working/code-spaces/OI/OJ/czos/CSP-S_Practise/Lesson1/T4.cpp  
 * @Sol        --  
 */  
  
// #pragma GCC optimize(3)  
#define _for(cc, ba, ab) for (int cc = (ba); cc <= (ab); cc++)  
#define for_(cc, ba, ab) for (int cc = (ba); cc >= (ab); cc--)  
#include <bits/stdc++.h>  
using namespace std;  
#define int long long  
#define i2 __int128  
const int N = 1e5 + 10;  
struct tree {  
    int ai, bi, ci, ti, idx;  
} a[N], b[N];  
struct node {  
    int to, nxt;  
} edge[N * 2];  
int pre[N], k = 0;  
int fa[N];  
int n, x, y;  
bool f[N];  
int plant[N];  
int poi[N], len = 0;  
bool cmp(tree n1, tree n2) {  
    return n1.ti < n2.ti;  
}  
void add(int u, int v) {  
    edge[++k] = {v, pre[u]};  
    pre[u] = k;  
}  
void dfs(int x, int fath) {  
    fa[x] = fath;  
    for (int i = pre[x]; i; i = edge[i].nxt) {  
        int to = edge[i].to;  
        if (to != fath) {  
            dfs(to, x);  
        }  
    }  
}  
i2 height(int x, i2 s, i2 e) {  
    if (a[x].ci >= 0)  
        return a[x].bi * (e - s + 1) + a[x].ci * (s + e) * (e - s + 1) / 2;  
    i2 maxt = (1 - a[x].bi) / a[x].ci;  
    if (maxt < s)  
        return e - s + 1;  
    if (maxt > e)  
        return a[x].bi * (e - s + 1) + a[x].ci * (s + e) * (e - s + 1) / 2;  
    return a[x].bi * (maxt - s + 1) + a[x].ci * (s + maxt) * (maxt - s + 1) / 2 + e - maxt;  
}  
int find(int x, int end_time) {  
    int l = 1, r = n, mid;  
    while (l <= r) {  
        mid = l + r >> 1;  
        if (height(x, mid, end_time) >= a[x].ai)  
            l = mid + 1;  
        else  
            r = mid - 1;  
    }  
    return l - 1;  
}  
bool check(int mid) {  
    for (int i = 1; i <= n; i++) {  
        if (height(i, 1, mid) < a[i].ai)  
            return 0;  
        b[i] = a[i];  
        int ti = find(i, mid);  
        b[i].ti = ti;  
        plant[i] = ti;  
        f[i] = 0;  
    }  
    sort(b + 1, b + n + 1, cmp);  
    int day = 0, x, p;  
    for (int i = 1; i <= n; i++) {  
        if (f[b[i].idx] == 1)  
            continue;  
        len = 0;  
        x = b[i].idx;  
        poi[++len] = x;  
        while (fa[x] != 0 and f[fa[x]] == 0) {  
            x = fa[x];  
            poi[++len] = x;  
        }  
        for (int j = len; j >= 1; j--) {  
            p = poi[j];  
            day++;  
            f[p] = 1;  
            if (plant[p] < day)  
                return 0;  
        }  
    }  
    return 1;  
}  
  
signed main() {  
    // freopen("sample.in","r",stdin);  
    // freopen("sample.out","w",stdout);  
    ios::sync_with_stdio(false);  
    cin.tie(0);  
    cout.tie(0);  
    cin >> n;  
    for (int i = 1; i <= n; i++) {  
        cin >> a[i].ai >> a[i].bi >> a[i].ci;  
        a[i].idx = i;  
    }  
    for (int i = 1; i <= n - 1; i++) {  
        cin >> x >> y;  
        add(x, y);  
        add(y, x);  
    }  
    dfs(1, 0);  
    int l = n, r = 1e9, mid;  
    while (l <= r) {  
        int mid = l + r >> 1;  
        if (check(mid))  
            r = mid - 1;  
        else  
            l = mid + 1;  
    }  
    cout << l;  
    return 0;  
}  
```  
  
# T5 草地上的羊  
  
## 题目描述  
小 A 的牧场里有很多块草地，每块草地上有若干只羊。他的羊经常在草地之间移动。为了确保每块草地的羊都有足够的草，小 A 决定不仅要考虑每块草地本身的羊的数量，还要计算周围草地可能会来到该草地的羊的数量。  
牧场包含 $N$ 块草地，这些草地之间通过双向小路相连，总共有 $N-1$ 条小路，连接了所有的草地。  
第 $i$ 块草地上有 $A_i$ 只羊，小羊可能通过跨越最多 $C$ 条小路去其他草地。  
小 A 希望为每块草地计算出能到达该草地的最多的数量，但他的牧场实在太大了，他不得不求助于聪明的你。  
请你编程帮助小 A 计算出，对于每块草地，最多能到达该草地的羊的数量。  
### 输入格式  
第 1 行读入 2 个整数，$N$ 和 $C$。  
接下来$N-1$行，每行两个空格分隔的整数 $U_i, V_i$，表示编号为 $U_i$ 和编号为 $V_i$ 的草地之间有一条双向的小路。  
接下来$N$行，每行有一个整数 $A_i$，表示编号为 $i$ 的草地上的羊的数量。  
### 输出格式  
输出 $N$ 行，第 $i$ 行输出的是，对于编号为 $i$ 的草地，最多能到达该草地的羊的数量。  
### 样例输入输出  
  
**输入**  
```  
6 2  
1 2  
2 3  
3 4  
4 5  
5 6  
10  
20  
30  
40  
50  
60  
```  
**输出**  
```  
150  
60  
130  
140  
```  
**输入**  
```  
10 3  
1 2  
1 3  
2 4  
2 5  
3 6  
3 7  
4 8  
4 9  
5 10  
2  
3  
4  
5  
6  
7  
8  
9  
10  
1  
```  
**输出**  
```  
36  
28  
45  
15  
26  
44  
37  
34  
19  
25  
```  
**输入**  
```  
10 2  
1 2  
1 1  
1  
2  
3  
4  
5  
6  
7  
8  
9  
10  
```  
**输出**  
```  
46  
```  
### 说明  
**样例 1 说明**  
小 A 有 $6$ 块草地，每块草地上有 $10$、$20$、$30$、$40$、$50$、$60$ 只羊，小羊最多跨越 $C=2$ 条小路去其他草地。  
能到达编号为 $1$ 的草地，除了 $1$ 号草地本身的羊以外，还可能来自编号为 $2$、$3$、$4$、$5$ 的草地，一共有 $10+20+30+40+50=150$ 只。  
能到达编号为 $2$ 的草地，除了 $2$ 号草地本身的羊以外，还可能来自编号为 $1$、$3$ 的草地，一共有 $20+10+30=60$ 只。  
### 数据范围  
- 对 $10\%$ 的数据，满足所有输入的 $V_i$ 一定相等。  
- 对于另外 $10\%$ 的数据，满足 $C=2$。  
- 对于另外 $20\%$ 的数据，满足 $1 \leq N \leq 100$，$1 \leq C \leq 10$。  
- 对于 $100\%$ 的数据，满足 $1 \leq N \leq 10^4$，$1 \leq C \leq 20$，$1 \leq U_i, V_i \leq N$，$0 \leq A_i \leq 1000$。  
  
## T5 Sol  
题意：求树上从每个点出发，经过不超过 $C$ 条边，能走到的点的点权和。  
  
思路：树形 DP。  
1. 状态定义  
   $son[i][j]$：从点 $i$ 出发，经过不超过 $j$ 条边，能走到子孙结点的点权和。  
$f[i][j]$：从点 $i$ 出发，经过不超过 $j$ 条边，能走到所有结点的点权和。  
  
2. 状态转移  
        1) 对于 $dfs$ 中的点 $x$ 及点 $x$ 的子元素 $to$，有：  
        $$son[x][j] += son[to][j - 1]$$  
        2) 在求解出所有的 $son[i][j]$ 的基础上，考虑 $i$ 向上走不超过 $j$ 条边。  
   对于 $dfs$ 中的点 $x$ 及点 $x$ 的子元素 $to$，有：  
   $$f[to][j] += f[x][j - 1] - son[to][j - 2](减掉重复计算的部分)$$  
  
3. 边界条件  
$$son[i][j] = w[i] (j\in [0, c])$$  
$$f[i][j] = son[i][j]$$  
$$f[to][1] += son[x][0]$$  
  
4. 答案  
$$f[i][c] (i\in [1, n])$$  
## T5 Code  
```cpp  
/*  
 * > CPP Code Snippet <  
 * > Powered by Microsoft Visual Studio Code <  
 *  
 * @Author     FrankWKD  
 * @Date       2025-08-02  
 * @Network    "https://oj.czos.cn/contest/problem?id=25410&pid=4"  
 * @License    GNU General Public License 2.0  
 * @Platform   [Frank]iMac Ubuntu Pro 24.04 LTS  
 * @Name       T5.cpp  
 * @Path       /media/frank/FrankW/_default/Mine/Working/code-spaces/OI/OJ/czos/CSP-S_Practise/Lesson1/T5.cpp  
 * @Sol        --  
 */  
  
// #pragma GCC optimize(3)  
#define _for(cc, ba, ab) for (int cc = (ba); cc <= (ab); cc++)  
#define for_(cc, ba, ab) for (int cc = (ba); cc >= (ab); cc--)  
#include <bits/stdc++.h>  
using namespace std;  
// #define int long long  
const int N = 1e5 + 10;  
int pre[N], n, c, son[N][30], f[N][30], k, val[N];  
struct node {  
    int to, nxt;  
} a[N * 2];  
void add(int u, int v) {  
    a[++k] = {v, pre[u]};  
    pre[u] = k;  
}  
void dfs_son(int x, int fa) {  
    for (int i = pre[x]; i; i = a[i].nxt) {  
        if (a[i].to != fa) {  
            dfs_son(a[i].to, x);  
            _for(j, 1, c) {  
                son[x][j] += son[a[i].to][j - 1];  
            }  
        }  
    }  
}  
void dfs_f(int x, int fa) {  
    for (int i = pre[x]; i; i = a[i].nxt) {  
        if (a[i].to != fa) {  
            f[a[i].to][1] += son[x][0];  
            _for(j, 2, c) {  
                f[a[i].to][j] += f[x][j - 1] - son[a[i].to][j - 2];  
            }  
            dfs_f(a[i].to, x);  
        }  
    }  
}  
int main() {  
    // freopen("sample.in","r",stdin);  
    // freopen("sample.out","w",stdout);  
    // ios::sync_with_stdio(false);  
    // cin.tie(0); cout.tie(0);  
    cin >> n >> c;  
    _for(i, 1, n - 1) {  
        int x, y;  
        cin >> x >> y;  
        add(x, y);  
        add(y, x);  
    }  
    _for(i, 1, n) {  
        int x;  
        cin >> x;  
        _for(j, 0, c) son[i][j] = x;  
    }  
    dfs_son(1, 0);  
    memcpy(f, son, sizeof son);  
    dfs_f(1, 0);  
    _for(i, 1, n) cout << f[i][c] << "\n";  
    return 0;  
}  
```  
