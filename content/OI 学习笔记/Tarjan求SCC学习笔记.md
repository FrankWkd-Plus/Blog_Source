---
title: Tarjan求SCC学习笔记
date: 2025-06-21
tags:
  - Tarjan
  - SCC
  - 学习笔记
  - OI
licenseName: CC-BY-NC-SA 4.0
---
# Tarjan求SCC学习笔记
网上看了几篇博客，还有OI Wiki，觉得整合度不够，于是特意写了篇博客。

## 参考资料

[Cnblogs@miaopan](https://www.cnblogs.com/miaopan/p/14632462.html)
[全网最!详!细!Tarjan算法讲解。](https://blog.csdn.net/hurmishine/article/details/75248876)  
[强连通分量(SCC)与缩点](https://hawa130.wordpress.com/2018/08/09/scc%e4%b8%8e%e7%bc%a9%e7%82%b9/)  
[强连通分量 - OI Wiki](https://oi-wiki.org/graph/scc/)

## 正文

在学习强连通分量和缩点之前，请务必理解邻接表。

**以下有OI Wiki的内容，有大佬博客里的内容，也有我自己的内容。**

## 强连通分量

### 引入

**强连通**的定义是：**有向图** G 强连通是指，G 中任意两个结点连通。  
**强连通分量**（Strongly Connected Components，简称SCC）的定义是：极大的**强连通**子图。

举一个简单的例子。下面是一个有向图。

[![](https://img2020.cnblogs.com/blog/2074326/202104/2074326-20210407155502931-1113229678.png)](https://img2020.cnblogs.com/blog/2074326/202104/2074326-20210407155502931-1113229678.png)

在这个图中， 1和2互相有路径可以到达对方，所以这两个点**强连通**；  
1、2和3中任意两个点都连通，它们是这整个有向图的**强连通分量**。

### 求强连通分量。

下面我们介绍使用Tarjan算法求强连通分量。

我们举个例子说明这个算法是怎么运作的。图片取自《算法竞赛进阶指南》。

[![](https://img2020.cnblogs.com/blog/2074326/202104/2074326-20210407171359016-1783656070.png)](https://img2020.cnblogs.com/blog/2074326/202104/2074326-20210407171359016-1783656070.png)

从 $①$ 进入，`dfn[1] = low[1] = ++index == 1`  
入栈 $①$，此时的栈中元素为 $①$，`vis[1] = true`，即点 $①$ 在栈中。

往下找到 $②$，`dfn[2] = low[2] = ++index == 2`  
入栈 $②$，此时栈中元素为 $①②$，`vis[2] = true`

继续往下找到 $③$，`dfn[3] = low[3] = ++index == 3`  
入栈 $③$，此时栈中元素为 $①②③$，`vis[3] = true`

继续往下找到 $④$，`dfn[4] = low[4] = ++index == 4`  
入栈 $④$，此时栈中元素为 $①②③④$，`vis[4] = true`

继续往下找到 $⑤$，`dfn[5] = low[5] = ++index == 5`  
入栈 $5$，此时栈中元素为 $①②③④⑤$，`vis[5] = true`

继续往下找到 $②$ 他太爷爷，`dfn[2]`被访问过并且还在栈中，说明这个 $②$ 还在这个强连通分量中，值得发现。`low[5] = min(low[5], dfn[2])`  
确定关系：$⑤$ 节点比 $②$ 节点出现得晚，即`low[5] > dfn[2]`，所以 $⑤$ 是 $①$ 的子节点；`low[5] = 2`

[![](https://img2020.cnblogs.com/blog/2074326/202104/2074326-20210408120750087-1239579580.png)](https://img2020.cnblogs.com/blog/2074326/202104/2074326-20210408120750087-1239579580.png)

然后发现 $⑤$ 这个点没有出边了，返回到 $④$ 这个点。  
于是`low[4] = min(low[4], low[5]) = 2`

$④$ 这个点也没有出边了，返回到 $③$ 这个点。  
于是`low[3] = min(low[3], low[4]) = 2`

$③$ 这个点也没有出边了，返回到 $②$ 这个点。  
于是`low[2] = min(low[2], low[2]) = 2`，对没错，还是2

$②$ 也没有出边，但是这时我们会发现`low[2] == dfn[2] == 2`，这就是说，点 $②$ 是这个强连通分量的根，于是`sccnt++`（`sccnt`：强连通分量的个数），一直弹出栈顶元素，并把栈顶元素的颜色标记为`sccnt`（`color[2] = sccnt`），直到栈顶元素为2时最后弹出一次并标记颜色。此时栈中元素为 $①$，`vis[5] = vis[4] = vis[3] = vis[2] = false`。

返回到 $①$。因为 $①$ 比 $②$ 出现得早，即`low[1] < low[2]`，所以`low[1]`的值没变，还是1

___

点 $①$ 还有出边，继续往下找到 $⑤$。嗯？`dfn[5]`已经有一个值`5`了，再看`vis[5] == false`，说明 $⑤$ 这个点已经不在栈中了，所以`low[1] = min(low[1], dfn[5]) = 1`（因为`low[1] == 1`，`dfn[5] == 2`）

[![](https://img2020.cnblogs.com/blog/2074326/202104/2074326-20210408153303833-1606888411.png)](https://img2020.cnblogs.com/blog/2074326/202104/2074326-20210408153303833-1606888411.png)

___

~唉，现在同学都在准备省选，要不是我去年NOIP第一题用了邻接矩阵，数组开得太大了，我也去参加省选了（看出我有多蒻了吧）~

点 $①$ 还有出边，继续往下找找到 $⑥$，`dfn[6] = low[6] = ++index == 6`  
入栈 $⑥$，此时栈中元素为 $①⑥$ ，`vis[6] = true`

继续往下找到 $⑦$，`dfn[7] = low[7] = ++index = 7`  
入栈 $⑦$，此时栈中元素为 $①⑥⑦$，`vis[7] = true`

继续往下找到 $④$，发现`dfn[4] == 4`，说明 $④$ 已经被访问过了，又因为`vis[4] == false`，也就是 $④$ 不在栈中，就不用管它。

$⑦$ 没有别的出边了。判断此时`dfn[7] == low[7] == 7`，所以 $⑦$ 是一个强连通分量的根。于是：Ⅰ：`sccnt++`；Ⅱ：把栈顶元素（即 $⑦$ 的颜色标记为`sccnt`），将其弹出。此时栈中元素为 $①⑥$。

[![](https://img2020.cnblogs.com/blog/2074326/202104/2074326-20210408153336297-1727278672.png)](https://img2020.cnblogs.com/blog/2074326/202104/2074326-20210408153336297-1727278672.png)

返回到 $⑥$，找到点 $⑧$，`dfn[8] = low[8] = ++index = 8`  
入栈 $⑧$，此时栈中元素为 $①⑥⑧$，`vis[8] = true`

往下找到 $⑦$，发现`dfn[7] == 7`，说明 $⑦$ 已经被访问过了，又因为`vis[7] ==false`，$⑦$ 不在栈中，不用管。

[![](https://img2020.cnblogs.com/blog/2074326/202104/2074326-20210408134943028-1679655274.png)](https://img2020.cnblogs.com/blog/2074326/202104/2074326-20210408134943028-1679655274.png)

回到 $⑧$，它还有一条出边到 $⑨$，`dfn[9] = low[9] = ++index = 9`  
入栈 $⑨$，此时栈中元素为 $①⑥⑧⑨$，`vis[9] = true`

再往下找到 $⑥$，找到了他爷爷（他爸是 $⑧$），`dfn[6]`被访问过并且还在栈中，说明这个 $⑥$ 还在这个强连通分量中。`low[9] = min(low[9], dfn[6]) = 6`。

接着就是：  
`low[8] = min(low[8], low[9]) = 6`  
`low[6] = min(low[6], low[8]) = 6`

此时 $⑥$ 的出边已经找完了，又因为`low[6] == dfn[6] == 6`，所以 $⑥$ 是强连通分量的根，`sccnt++`，一直弹出栈顶元素，将栈顶元素颜色标记为`sccnt`，直到栈顶元素为6，最后弹出一次，标记颜色。此时栈中元素为 $①$，`vis[9] = vis[8] = vis[6] = false`。

[![](https://img2020.cnblogs.com/blog/2074326/202104/2074326-20210408135157404-1017651600.png)](https://img2020.cnblogs.com/blog/2074326/202104/2074326-20210408135157404-1017651600.png)

返回到 $①$，$①$的出边也已经找完了，因为`low[1] == dfn[1] == 1`，所以 $①$ 是强连通分量的根，`sccnt++`，弹出栈顶元素（1），将栈顶元素颜色标记为`sccnt`。此时栈为空，`vis[1] = false`。

[![](https://img2020.cnblogs.com/blog/2074326/202104/2074326-20210408135711354-1312300059.png)](https://img2020.cnblogs.com/blog/2074326/202104/2074326-20210408135711354-1312300059.png)

这个时候就完了吗？！  
你以为就完了吗？！  
然而并没有完，万一只走了一遍tarjan整个图没有找完怎么办呢？！  
所以。tarjan的调用要在循环里解决，以使每一个点都被访问到。

### 代码

下面是tarjan函数的代码：

```
void tarjan(int v) {       //Tarjan算法
  dfn[v] = low[v] = ++tot; //标记dfn[]访问顺序，还有low[]的初始值
  sta.push(v);             //让点v进栈
  vis[v] = true;           //标记这个点被访问过
  for (int i = head[v]; ~i; i = edge[i].next) { //一直循环这个点每一个出度，直到-1表示没有了，这也是为什么memset head数组时要赋-1
    int u = edge[i].to;               //定义u并把它赋成这条边的终点
    if (!dfn[u]) {                    //如果u没有被访问过
      tarjan(u);                      //找下面这个点
      low[v] = min(low[v], low[u]);   //这个点low[v]的值就是当前low[]的值与找到的u点的low[]值
    } else if (vis[u])                //如果u被访问过了，但是还在队列中
      low[v] = min(low[v], dfn[u]);   //low[v]就取这个点的low值与循环到的点u的dfn[u]的最小值
  }
  if (dfn[v] == low[v]) {   //如果发现v这个点的dfn[]和low[]相等，说明这个点是一个强连通分量的“根”。
    sccnt++;                //scc（Strongly Connected Component）， cnt（count），就是强连通分量的个数
    int u;                  //定义u变量，作为栈顶元素
    do { 
      u = sta.top();        //将u赋值为sta栈的栈顶元素
      vis[u] = false;       //将u弹出
      sta.pop();            //同上
      color[u] = sccnt;     //将u标记为这个强连通分量里的点
    } while (v != u);       //当v == u之后，结束循环
  }
}
```

在主函数里像这样调用tarjan函数：

  ```
  for (int i = 1; i <= n; i++) //循环每一个点
    if (!dfn[i]) tarjan(i);    //如果dfn[i]没有值，即这个点被没有访问过，需要访问；
                               //如果dfn[i]已经有一个值，说明这个点被访问过了，不用担心漏了，
                               //同时也为了节省时间，就不访问了。
```

## 缩点

### 引入

**缩点**的定义：把**强连通分量**看成是一个大点，保留那些不在强连通分量里的边，这样的图就是**缩点**后的图。  
缩点后的图保留了所有不在分量里的边，而且缩点后的图是一个**有向无环图**（DAG），可以进行拓扑排序。

那么，缩点之后的图，就成了这样：

[![](https://img2020.cnblogs.com/blog/2074326/202104/2074326-20210408142101139-1395943988.png)](https://img2020.cnblogs.com/blog/2074326/202104/2074326-20210408142101139-1395943988.png)

### 缩点的解决

现在问题来了，怎么储存缩点后的图？

在一开始调用的tarjan函数中，我们已经把同一个强连通分量中的点标记成了相同的颜色。为了重新建立一个缩点后的图，我们先把`head[]`数组和`edge[]`数组清空，在读入边的时候，我们把边的起点和终点分别存到`from[]`数组和`to[]`数组中。  
在建立邻接表储存缩点后的图时，判断这些输入的点是否在同一个 SCC 中，如果不在就连边。  
用如下代码建立缩点后的图：

  ```
  for (int i = 1; i <= m; i++)              //循环每一条边
    if (color[from[i]] != color[to[i]])     //如果这条边的出发点和终止点不在同一个强连通分量中
      add(color[from[i]], color[to[i]]);    //就连一条边
```

## 【模板】缩点

题目链接：[【模板】缩点](https://www.luogu.com.cn/problem/P3387)

### 审题

这题不仅需要缩点，还要找出一条路径使点权和最大。  
因为 强连通分量（SCC） 中点可以互相到达，所以需要缩点使图变成一个有向无环图（DAG），强连通分量 内所有点权和即为缩点后该点的点权。  
这题可以使用LPFA算法~（Longest Path Fast Algorithm，发明者：沃·兹基硕德）~来解决。  
把松弛改为扩张：

```
  if (dis[v] > dis[u] + siz[v]) dis[v] = dis[u] + siz[v];
->if (dis[v] < dis[u] + siz[v]) dis[v] = dis[u] + siz[v];
```
题目链接：[【模板】缩点](https://www.luogu.com.cn/problem/P3387)

## 前言

这是一道模板题。需要学习[强连通分量和缩点](https://www.cnblogs.com/g-mph/p/14632167.html)，还有[最短路径算法](https://oi-wiki.org/graph/shortest-path/)。

## 审题

给一张图，找一条路径使点权和最大。

## 思路

先用tarjan算法求出这张图中所有的强连通分量，将它们缩成一点，建一个缩点后的图。  
这次题目让我们求这张图上的一条路径，使经过的点权之和最大。看到“最”，就会想到和最短路有关。但是这题求的是**最大**的**点权**之和，就需要考虑把最短路算法魔改一下。

这篇题解使用的是LPFA算法~（Longest Path Fast Algorithm，发明者：沃·兹基硕德）~

把SPFA算法的边权改为点权，松弛改为扩张：

  ```
  if (dis[v] > dis[u] + siz[v]) dis[v] = dis[u] + siz[v];
->if (dis[v] < dis[u] + siz[v]) dis[v] = dis[u] + siz[v];
```

整个LPFA的代码如下：

```
void spfa(int s) {          //LPFA开始
  queue<int> que;           //定义队列que
  que.push(s);              //将s push进队列
  dis[s] = siz[s];          //将s出发的最长路的值初始化为它所在连通块的权值之和
  used[s] = true;           //标记s在队列中
  while (!que.empty()) {    //当队列不为空（即有值）时循环
    int u = que.front();    //把u赋为que的队首元素
    que.pop();              //删除que中的第一个元素
    used[u] = false;        //标记u不在队列中
    for (int i = head[u]; ~i; i = edge[i].next) {  //循环从u出发的所有边
      int v = edge[i].to;                //定义点v并把它赋值为这条边的终点
      if (dis[v] < dis[u] + siz[v]) {    //如果现在的“最长路”没有先走u的长度长
dis[v] = dis[u] + siz[v];        //就对路径进行“扩张”操作
if (!used[v]) {                  //如果点v不在队列中
  used[v] = true;                //标记它加入队列
  que.push(v);                   //把它加入队列
}
      }
    }
  }
  for (int i = 1; i <= sccnt; i++)       //循环每一个强连通分量，找dis（最长路）的最大值
    ans = max(ans, dis[i]);              //如果dis[i]比答案大，就让答案为dis[i]
}
```

因为需要求出权值的最大和，所以tarjan函数里面多了这样一句：

```
siz[sccnt] += val[u];
```

整个`tarjan()`函数如下

```
void tarjan(int v) {       //Tarjan算法
  dfn[v] = low[v] = ++tot; //标记dfn[]访问顺序，还有low[]的初始值
  sta.push(v);             //让点v进栈
  vis[v] = true;           //标记这个点被访问过
  for (int i = head[v]; ~i; i = edge[i].next) { //一直循环这个点每一个出度，直到-1表示没有了，这也是为什么memset head数组时要赋-1
    int u = edge[i].to;               //定义u并把它赋成这条边的终点
    if (!dfn[u]) {                    //如果u没有被访问过
      tarjan(u);                      //找下面这个点
      low[v] = min(low[v], low[u]);   //这个点low[v]的值就是当前low[]的值与找到的u点的low[]值
    } else if (vis[u])                //如果u被访问过了，但是还在队列中
      low[v] = min(low[v], dfn[u]);   //low[v]就取这个点的low值与循环到的点u的dfn[u]的最小值
  }
  if (dfn[v] == low[v]) {   //如果发现v这个点的dfn[]和low[]相等，说明这个点是一个强连通分量的“根”。
    sccnt++;                //scc（Strongly Connected Component）， cnt（count），就是强连通分量的个数
    int u;                  //定义u变量，作为栈顶元素
    do { 
      u = sta.top();        //将u赋值为sta栈的栈顶元素
      vis[u] = false;       //将u弹出
      sta.pop();            //同上
      color[u] = sccnt;     //将u标记为这个强连通分量里的点
      siz[sccnt] += val[u]; //这个强连通分量的权值加上u这个点的权值
    } while (v != u);       //当v == u之后，结束循环
  }
}
```

为了使每一个点都被访问到，`tarjan()`的调用在循环中进行：

  ```
  for (int i = 1; i <= n; i++) //循环每一个点
    if (!dfn[i]) tarjan(i);    //如果dfn[i]没有值，即这个点被没有访问过，需要访问；
                               //如果dfn[i]已经有一个值，说明这个点被访问过了，不用担心漏了，
                               //同时也为了节省时间，就不访问了。
```

建缩点后的图时，将原来的`head[]`和`edge[]`数组清空，循环每一条边，如果它的起点和终点不在一个强连通分量中，则连一条边：

  ```
  for (int i = 1; i <= m; i++)              //循环每一条边
    if (color[from[i]] != color[to[i]])     //如果这条边的出发点和终止点不在同一个强连通分量中
      add(color[from[i]], color[to[i]]);    //就连一条边
```

## 代码

完整代码如下：

```
#include <bits/stdc++.h>   //万能头文件
using namespace std;       //名字空间，具体我也不知有啥用，但是有用到了iostream就得加这个，否则就得std::
const int maxN = 2e5 + 3;  //数组大小

struct Edge {
  int next, to;            //用结构体存邻接表
} edge[maxN];
int head[maxN], dfn[maxN], low[maxN];
int color[maxN], val[maxN], siz[maxN];
int from[maxN], to[maxN], dis[maxN];
bool vis[maxN], used[maxN];
int cnt, tot, sccnt, ans, n, m;
stack<int> sta;

//以上定义了一包变量和数组

template<typename Tp> void read(Tp &x) {
  char c = getchar();      //先输入一个字符
  x = 0;                   //定义x并初始化为0，用来累计输入的数
  while (!isdigit(c)) c = getchar(); //如果这个字符不是数字的话，就一直读下一个字符（本题没有负数情况）
  do {
    x = x * 10 + (c ^ 48); //先把x×10，然后与c^48相加
                           //十进制48转换为二进制为110000，而'0'到'9'都是11****，异或会使不同的位为1，相同的位为0
    c = getchar();         //读下一个字符
  } while (isdigit(c));    //循环条件为读到的为数字，则一直循环到读入的不是数字为止
}

void add(int from, int to) {
  edge[++cnt].next = head[from], edge[cnt].to = to, head[from] = cnt;
}

void tarjan(int v) {       //Tarjan算法
  dfn[v] = low[v] = ++tot; //标记dfn[]访问顺序，还有low[]的初始值
  sta.push(v);             //让点v进栈
  vis[v] = true;           //标记这个点被访问过
  for (int i = head[v]; ~i; i = edge[i].next) { //一直循环这个点每一个出度，直到-1表示没有了，这也是为什么memset head数组时要赋-1
    int u = edge[i].to;               //定义u并把它赋成这条边的终点
    if (!dfn[u]) {                    //如果u没有被访问过
      tarjan(u);                      //找下面这个点
      low[v] = min(low[v], low[u]);   //这个点low[v]的值就是当前low[]的值与找到的u点的low[]值
    } else if (vis[u])                //如果u被访问过了，但是还在队列中
      low[v] = min(low[v], dfn[u]);   //low[v]就取这个点的low值与循环到的点u的dfn[u]的最小值
  }
  if (dfn[v] == low[v]) {   //如果发现v这个点的dfn[]和low[]相等，说明这个点是一个强连通分量的“根”。
    sccnt++;                //scc（Strongly Connected Component）， cnt（count），就是强连通分量的个数
    int u;                  //定义u变量，作为栈顶元素
    do { 
      u = sta.top();        //将u赋值为sta栈的栈顶元素
      vis[u] = false;       //将u弹出
      sta.pop();            //同上
      color[u] = sccnt;     //将u标记为这个强连通分量里的点
      siz[sccnt] += val[u]; //这个强连通分量的权值加上u这个点的权值
    } while (v != u);       //当v == u之后，结束循环
  }
}
void spfa(int s) {          //LPFA开始
  queue<int> que;           //定义队列que
  que.push(s);              //将s push进队列
  dis[s] = siz[s];          //将s出发的最长路的值初始化为它所在连通块的权值之和
  used[s] = true;           //标记s在队列中
  while (!que.empty()) {    //当队列不为空（即有值）时循环
    int u = que.front();    //把u赋为que的队首元素
    que.pop();              //删除que中的第一个元素
    used[u] = false;        //标记u不在队列中
    for (int i = head[u]; ~i; i = edge[i].next) {  //循环从u出发的所有边
      int v = edge[i].to;                //定义点v并把它赋值为这条边的终点
      if (dis[v] < dis[u] + siz[v]) {    //如果现在的“最长路”没有先走u的长度长
dis[v] = dis[u] + siz[v];        //就对路径进行“扩张”操作
if (!used[v]) {                  //如果点v不在队列中
  used[v] = true;                //标记它加入队列
  que.push(v);                   //把它加入队列
}
      }
    }
  }
  for (int i = 1; i <= sccnt; i++)       //循环每一个强连通分量，找dis（最长路）的最大值
    ans = max(ans, dis[i]);              //如果dis[i]比答案大，就让答案为dis[i]
}
int main() {
  memset(head, -1, sizeof(head));            //把head[]数组初始化为-1，具体原因见tarjan函数
  read(n), read(m);                          //输入n、m
  for (int i = 1; i <= n; i++) read(val[i]); //输入每一个点的权值
  for (int i = 1; i <= m; i++) {
    read(from[i]), read(to[i]);              //输入每一条边的起点和重点
    add(from[i], to[i]);                     //把起点和终点连一条边
  }
  for (int i = 1; i <= n; i++) //循环每一个点
    if (!dfn[i]) tarjan(i);    //如果dfn[i]没有值，即这个点被没有访问过，需要访问；
                               //如果dfn[i]已经有一个值，说明这个点被访问过了，不用担心漏了，
                               //同时也为了节省时间，就不访问了。
  memset(head, -1, sizeof(head));           //将原来的head[]数组清空，以便重新建图
  memset(edge, 0, sizeof(edge));            //将原来的edge[]数组清空，以便重新建图
  for (int i = 1; i <= m; i++)              //循环每一条边
    if (color[from[i]] != color[to[i]])     //如果这条边的出发点和终止点不在同一个强连通分量中
      add(color[from[i]], color[to[i]]);    //就连一条边
  for (int i = 1; i <= sccnt; i++) spfa(i); //循环每一个连通块（单独的一个点也是一个连通块），因为每一个连通块已经缩成一个点，所以就可以当作一个点来对待
  printf("%d\n", ans);                      //输出答案
}
```


### - $T1$ ： $Tarjan$ 求大小大于 $1$ 的 $SCC$
```cpp
/*
 * @Author    FrankWKD (wangkedi01)
 * @Date      2025-06-21
 * @Source    "--"
 * @License   GNU General Public License 2.0
 * @FileName  Template.cpp
 * @FilePath  /media/frank/FrankW/_default/_Mine!/Working/code-spaces/czos/Tarjan_SCC/Template.cpp
 * @Solution  Tarjan求SCC模板，同T1
 */

// #pragma GCC optimize(3)
#include <bits/stdc++.h>
using namespace std;
const int N = 1e4 + 10, M = 5e4 + 10;
struct node {
    int to, nxt;
} a[M];

int pre[N], k = 0;
int dfn[N], low[N], times = 0;
stack<int> st;
bool inst[N];
int sccnt = 0;
int si[N];  // 每个SCC内部的节点数
int id[N];  // 每个点属于哪个SCC
int n, m, x, y;
void tarjan(int x) {  // 以x为起点进行SCC的查找(只负责这一层)
    dfn[x] = low[x] = ++times;
    st.push(x);
    inst[x] = true;
    for (int i = pre[x]; i; i = a[i].nxt) {
        int to = a[i].to;
        if (!dfn[to]) {
            tarjan(to);  // 逐层向下递归相邻节点
            low[x] = min(low[x], low[to]);

        } else if (inst[to])
            low[x] = min(low[x], low[to]);  // 找到了闭环！更新SCC！
    }
    if (low[x] == dfn[x]) {
        sccnt++;
        // 找到SCC入口，SCC数量++ 开始弹栈
        int t;
        do {
            t = st.top();
            st.pop();
            inst[t] = false;
            si[sccnt]++;
            id[t] = sccnt;
        } while (t != x);
    }
}
void add(int u, int v) {
    a[++k] = {v, pre[u]};
    pre[u] = k;
}
int main() {
    // ios::sync_with_stdio(false);
    // cin.tie(0); cout.tie(0);
    cin >> n >> m;
    for (int i = 1; i <= m; i++) {
        cin >> x >> y;
        add(x, y);
    }
    for (int i = 1; i <= n; i++) {
        if (!dfn[i]) {
            tarjan(i);
        }
    }
    int ans = 0;
    for (int i = 1; i <= sccnt; i++) {
        if (si[i] > 1)
            ans++;  // 因为题目要求大小大于1的SCC的数量，所以这里我们再次统计一下
    }
    cout << ans;
    return 0;
}
```