---
title: Luogu-题解：P12677 Brooklyn Round 1 & NNOI Round 1 A - Flying Flower
date: 2025-07-11
tags:
  - Luogu
  - 题解
  - 数论
  - OI
---
#### 题目  
给定两个数组 $a$ 和 $b$，以及多个查询 $k$（默认 $k$ 为质数），判断在以下规则下，小 X 和小 Z 谁会获胜：  
1. 小 X 从 $a$ 中选一个能被 $k$ 整除的数。  
2. 小 Z 从 $b$ 中选一个能被 $k$ 整除的数。  
3. 比较两者选的数的大小，大的一方获胜。  
  
#### 思路  
- 使用线性筛预处理所有质数。  
- 对于每个 $a[i]$ 和 $b[i]$，分解其质因数，并记录每个质因数对应的最大数。  
   - 如果 $a$ 中没有数能被 $k$ 整除，小 Z 获胜。  
   - 如果 $a$ 中有数能被 $k$ 整除，且 $b$ 中没有，小 X 获胜。  
   - 否则，比较 $a$ 和 $b$ 中能被 $k$ 整除的最大数，大的一方获胜。  
  
```cpp  
#include<bits/stdc++.h>  
using namespace std;  
const int N=5e5+10, MAX=8e6+10;  
int n, m, q, a[N], b[N], max_a[MAX], max_b[MAX];  
bool che[MAX];  
vector<int> pri;  
  
// 线性筛预处理质数  
void init() {  
    for(int i=2; i<MAX; i++) {  
        if(!che[i]) pri.push_back(i);  
        for(int p : pri) {  
            if(i*p >= MAX) break;  
            che[i*p] = true;  
            if(i%p == 0) break;  
        }  
    }  
}  
  
int main() {  
    ios::sync_with_stdio(0); cin.tie(0);  
    init();  
    cin >> n >> m >> q;  
  
    // 处理数组a，记录每个质因数对应的最大值  
    for(int i=1; i<=n; i++) {  
        cin >> a[i];  
        int x = a[i];  
        for(int p : pri) {  
            if(p*p > x) break;  
            if(x%p == 0) {  
                max_a[p] = max(max_a[p], a[i]);  
                while(x%p == 0) x /= p;  
            }  
        }  
        if(x > 1) max_a[x] = max(max_a[x], a[i]);  
    }  
  
    // 处理数组b，同理  
    for(int i=1; i<=m; i++) {  
        cin >> b[i];  
        int x = b[i];  
        for(int p : pri) {  
            if(p*p > x) break;  
            if(x%p == 0) {  
                max_b[p] = max(max_b[p], b[i]);  
                while(x%p == 0) x /= p;  
            }  
        }  
        if(x > 1) max_b[x] = max(max_b[x], b[i]);  
    }  
  
    // 处理查询  
    while(q--) {  
        int k; cin >> k;  
        if(che[k]) cout << "Z\n";  // k为合数，直接小Z获胜  
        else {  
            if(max_a[k] == 0) cout << "Z\n";  // a中无数能被k整除  
            else if(max_b[k] == 0 || max_a[k] >= max_b[k]) cout << "X\n";  // a的最大值更大或b中无数  
            else cout << "Z\n";  // b的最大值更大  
        }  
    }  
    return 0;  
}  
```  
