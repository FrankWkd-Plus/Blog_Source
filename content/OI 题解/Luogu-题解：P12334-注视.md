---
title: Luogu-题解：P12334 注视
date: 2025-07-11
tags:
  - 题解
  - Luogu
  - 数论
  - 枚举
  - OI
licenseName: CC-BY-NC-SA 4.0
---
# [传送门](https://www.luogu.com.cn/problem/P12334)  
#### 核心性质  
对于任意正整数 $x$，其数位和 $f(x)$ 满足以下性质：  
1.$f(x) \equiv x \pmod{9}$  
   - 证明：设 $ x = a_n10^n + a_{n-1}10^{n-1} + \cdots + a_110 + a_0 $ 由于 $ 10^k \equiv 1 \pmod{9} $，故 $ x \equiv a_n + a_{n-1} + \cdots + a_1 + a_0 \equiv f(x) \pmod{9} $。  
  
2. $f(x^2) \equiv x^2 \equiv (f(x))^2 \pmod{9}$  
   同时有 $ f(x^2) \leq (f(x))^2 $。  
  
#### 思路  
  
当给定 $y = f(x^2)$ 时，我们可以从 $ \lceil \sqrt{y} \rceil$ 开始枚举可能的 $ f(x) $，因为 $ f(x^2) \leq (f(x))^2 \Rightarrow f(x) \geq \lceil \sqrt{y} \rceil $。枚举到 $ \lceil \sqrt{y} \rceil + 9 $，数学依据：数位和 $ f(x) $ 的模 $9$ 周期性；检查 $ i^2 \equiv y \pmod{9} $，符合条件的最小 $ i $ 即为答案。  
  
#### 代码  
  
```cpp  
#include <iostream>  
#include <cmath>  
using namespace std;  
  
int main() {  
    int y;  
    cin >> y;  
    int st = ceil(sqrt(y));  
    for (int i = st; i <= st + 9; i++) {  
        if (i * i % 9 == y % 9) {  
            cout << i;  
            return 0;  
        }  
    }  
    cout << -1;  
    return 0;  
}  
```  
