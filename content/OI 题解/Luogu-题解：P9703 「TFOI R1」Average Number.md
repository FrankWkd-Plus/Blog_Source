---
title: Luogu-题解：P9703 「TFOI R1」Average Number
date: 2025-07-11
tags:
  - Luogu
  - 题解
  - 二分
  - OI
licenseName: CC-BY-NC-SA 4.0
---
# [传送门](https://www.luogu.com.cn/problem/P9703)
## Sol
易得：$\frac{ac + b}{c} = \frac{\left(\sum_{i = 1}^{n} i\right) - m}{n - 1}$。

因为 $\left(\sum_{i = 1}^{n} i\right) - m \in \mathbb{Z} $，则 $n - 1 = s \times c$，$\left(\sum_{i = 1}^{n} i\right) - m = s \times (ac + b)$。

不难发现，$s$ 是我们需要找的一个整数值，当 $m \in [1, n]$ 时，此时有解且我们能够找到答案。

因此我们只需二分 $s$ 的值即可。

随着 $s$ 的增加，$\sum_{i = 1}^{n} i$ 比 $s \times (ac + b)$ 增长的快，$m$ 逐渐变大，因此，二分查找时算出的 $m$ 如果大于 $n$ 时，$s$ 过于大，进行缩小，如果小于 $1$，则扩大。

对于 $m \in [1, n]$ 的情况，应当缩小（或不变）。 
```cpp
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++) {
        __int128 a, b, c;
        long long a1, a2, a3;
        cin >> a1 >> a2 >> a3;
        a = a1, b = a2, c = a3;
        b += a * c;
        __int128 l, r;
        l = 1, r = 1e18, r /= c;
        while (l < r) {
            __int128 mi;
            mi = (l + r) / 2;
            __int128 x, y;
            x = mi * c, y = mi * b;
            __int128 vv;
            x++, vv = x, x = x * x + x, x /= 2, x -= y;
            if (x < 1)
                l = mi + 1;
            else
                r = mi;
        }
        __int128 x, y, z;
        x = r * c, y = r * b, x++, z = x, x = x + x * x, x /= 2, x -= y, a1 = z, a2 = x;
        cout << a1 << " " << a2 << '\n';
    }
}
```