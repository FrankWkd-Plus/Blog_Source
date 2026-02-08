---
title: Luogu-题解：AT_colopl2018_final_b 異世界数式
date: 2025-07-25
tags:
  - 题解
  - Luogu
  - 模拟
  - 栈
  - OI
licenseName: CC-BY-NC-SA 4.0
---
# [传送门](https://www.luogu.com.cn/problem/AT_colopl2018_final_b)
# Sol
可以说是一个模拟。

对于这一题，我们使用栈（系统栈和手写栈均可）。  
运用栈存储运算符号：
- 当遇到运算符时，将其入栈；
- 当遇到逗号时，输出栈顶的符号；
- 当遇到右括号时，输出右括号并弹出栈顶的运算符。

根据字符类型进行相应处理后直接输出转换后的中缀表达式即可。
# Code
```cpp
#include<bits/stdc++.h>
using namespace std;
string s; 
stack<char> a;
int main(){
	cin>>s;
	for(char c:s){
		if(c=='+'||c=='-'||c=='*'||c=='/') a.push(c);
		else if(c==')'){
			cout<<c;
			a.pop();
		}
		else if(c==',') cout<<a.top();
		else cout<<c;
	}
	cout<<endl;
}
```