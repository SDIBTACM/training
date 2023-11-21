# 乱看乱学：

## 上上（上）周：
扩展欧几里得（逆元，一次同余方程）区间素数筛 欧拉函数（质因数分解）费马小定理 欧拉定理 扩展中国剩余定理（同余方程组）  
### 程度： 
多少会点  

## 上周：
splay树森林合并  树链剖分+线段树 组合数计算（1e6内取质数模 快速幂+费马小定理 预处理逆元 后 快速计算 || 1e18内取质数模 基础卢卡斯）  
### 程度：
没板子能写，有板子更快  

## 本周：
（没学会 扩展卢卡斯）C++简单重载运算符 计算几何（向量存储，多边形面积，线段交点，凸包）  
### 程度：
刚学会点基础    

# ICPC 网络赛第二场 e题
### 题意：
n个点树上结构，m条巴士路线（两端点最短路），中途站点可下车，端点可上车，问点1 可否到任意点，能到那要乘几次巴士；  
### 思路：
比赛时候没想到方便的正解；但想到了花里胡哨的复杂解法，而且好像无题可做的样子，于是硬着头皮上了  
  
因为问的是 点1 到任意点，所以考虑 点1 的等价点 （ 点1 可换乘的点就是 点1 的等价点 ） ----- 用并查集维护等价性  
所有巴士端点如果是等价点 那么所有这些端点会构成由巴士路组成的图 点都是等价点 ----- 再次存图  
等价点图 建好后 可以算出每个等价点 的乘坐巴士的次数 即 点1 到各等价点的 最短路----- bfs 跑图  
对于每条巴士路 如果是有等价点的路 两个端点间的最短路径上的所有点标记尝试更新为 两个端点的乘坐巴士的次数的小者 ----- 线段树的区间更新  
树上路径化成数组才能用线段树 ----- 树链剖分（重链剖分）  
线段树懒标全打好后 ----- dfs 把懒标推到叶子节点 并记录答案  
判断并输出答案完事 --------------------------------------当然以上说的不知是不是对的  
  
然而 ，（可能是忘了打印板子），整套写下来耗费近两小时（也算是打发时间）； 中间记录的stack忘了pop，卡了25分钟 ； tr[a].n 写成 int n ；bfs 写成了 dfs ；  
赛后一听正解可以秒杀，  啊！？  
  
刚试了 好像过不了啊？？？ 不知道哪里搞错了， 哭死~   
  
比赛收获：分解问题能力++ ，头皮硬度++ ，脑子寄存器+++  
经验总结：可以开拓思路，但是如果 代码难度过大 考虑换思路 而不是顶硬上 浪费时间 或 得不偿失  
    
### 未AC代码：
```ruby
#include <stdio.h>
#include <iostream>
#include <bitset>
#include <stack>
#include <queue>
using namespace std;
struct E
{
	int to, next;
}edge[200100];
struct H
{
	int p, hs, fl, fa, lk, tm, dep;
}head[100010];

int fa[100100];
struct BE
{
	int to, next;
}br[200010];
struct BH
{
	int n, p;
}bus[100010];
int find(int x)
{
	if (fa[x] == x)
		return x;
	return fa[x] = find(fa[x]);
}
bitset <100010> hx;
void bfs(int a)
{
	
	queue <int> que;
	que.push(1);
	hx[1] = 1;
	bus[1].n = 1;
	while (que.size())
	{
		int qf = que.front();
		que.pop();
		int k = bus[qf].p;
		while (k)
		{
			BE key = br[k];
			if (hx[key.to])
			{
				k = key.next;
				continue;
			}
			que.push(key.to);
			hx[key.to] = 1;
			bus[key.to].n = bus[qf].n + 1;
			k = key.next;
		}
	}
}

int dfsw(int a)
{
	hx[a] = 1;
	int fl = 0;
	int mx = 0, kh = 0;
	int sum = 0;
	int k = head[a].p;
	while (k)
	{
		E key = edge[k];
		if (hx[key.to])
		{
			k = key.next;
			continue;
		}
		fl = 1;
		head[key.to].dep = head[a].dep + 1;
		head[key.to].fa = a;

		int wth = dfsw(key.to);
		sum += wth;
		if (wth > mx) mx = wth, kh = key.to;
		
		k = key.next;
	}
	if (fl == 0)
		return 1;
	head[kh].fl = 1;
	head[a].hs = kh;
	
	return sum;
}
//	int p, hs, fl, fa, lk, tm;
int link = 0;
int dfn = 0;
int haxi[100010];
void dfsd(int a)
{
	hx[a] = 1;
	if (head[a].fl == 0)
		link = a;
	head[a].lk = link;
	head[a].tm = ++dfn;
	haxi[dfn] = a;
	if (head[a].hs == 0) return;
	dfsd(head[a].hs);
	int k = head[a].p;
	while (k)
	{
		E key = edge[k];
		if (hx[key.to] || key.to == head[a].hs)
		{
			k = key.next;
			continue;
		}
		dfsd(key.to);
		k = key.next;
	}
}
struct SM
{
	int l, r;
};
stack <SM> stk;
void fd(int a, int b)
{
	if (head[a].lk == head[b].lk)
	{
		int l = head[a].tm, r = head[b].tm;
		if (l > r) swap(l, r);
		stk.push({ l,r });
		return;
	}
	if (head[head[a].lk].dep < head[head[b].lk].dep)swap(a, b);
	int l = head[a].tm, r = head[head[a].lk].tm;
	if (l > r) swap(l, r);
	stk.push({ l,r });
	fd(head[head[a].lk].fa, b);
}
struct T
{
	int n;
	int lz;
	int l, r;
}tr[400100];
void inherit(int a)
{
	if (tr[a].lz== 0x3f3f3f3f || tr[a].l == tr[a].r) return;
	tr[a * 2].lz = min(tr[a * 2].lz, tr[a].lz);
	tr[a * 2 + 1].lz = min(tr[a * 2 + 1].lz, tr[a].lz);
	tr[a].lz = 0x3f3f3f3f;
}
void build(int a, int l, int r)
{
	tr[a].l = l, tr[a].r = r;
	tr[a].lz = 0x3f3f3f3f;
	if (l == r)
	{
		tr[a].n = haxi[l];
		return;
	}
	int m = (l + r) >> 1;
	build(a * 2, l, m);
	build(a * 2 + 1, m + 1, r);
}
void modify(int a, int l, int r, int v)
{
	if (tr[a].l == l && tr[a].r == r)
	{
		tr[a].lz = min(tr[a].lz, v);
		return;
	}
	inherit(a);
	int m = (tr[a].l + tr[a].r) >> 1;
	if (r <= m) modify(a * 2, l, r, v);
	else if (l > m) modify(a * 2 + 1, l, r, v);
	else
	{
		modify(a * 2, l, m, v);
		modify(a * 2 + 1, m + 1, r, v);
	}
	return;
}
int res[100100];
void dfsres(int a)
{
	if (tr[a].l == tr[a].r)
	{
		res[tr[a].n] = tr[a].lz;
		return;
	}
	inherit(a);
	dfsres(a * 2);
	dfsres(a * 2 + 1);
}
struct Q
{
	int x, y;
}qq[100100];
int main()
{
	int n, m;
	cin >> n >> m;
	int cnt = 1;
	for (int i = 2; i != n + 1; i++)
	{
		int x;
		scanf("%d", &x);
		edge[cnt].to = x;
		edge[cnt].next = head[i].p;
		head[i].p = cnt++;
		edge[cnt].to = i;
		edge[cnt].next = head[x].p;
		head[x].p = cnt++;
	}
	for (int i = 1; i != n + 1; i++)
		fa[i] = i;
	int cc = 1;
	for (int i = 1; i != m + 1; i++)
	{
		int x, y;
		scanf("%d %d", &x, &y);
		qq[i].x = x, qq[i].y = y;
		int fx = find(x), fy = find(y);
		fa[fy] = fx;

		br[cc].to = y;
		br[cc].next = bus[x].p;
		bus[x].p = cc++;
		br[cc].to = x;
		br[cc].next = bus[y].p;
		bus[y].p = cc++;
	}
	bfs(1);
	hx = 0;
	dfsw(1);
	hx = 0;
	dfsd(1);
	build(1, 1, n);
	for (int i = 1; i != m + 1; i++)
	{
		if (find(1) != find(qq[i].x))
			continue;
		fd(qq[i].x, qq[i].y);
		int mn = min(bus[qq[i].x].n, bus[qq[i].y].n);
		while (stk.size())
		{
			modify(1, stk.top().l, stk.top().r, mn);
			stk.pop();
		}
	}
	dfsres(1);
	for (int i = 2; i != n + 1; i++)
	{
		if (res[i] == 0x3f3f3f3f)
			printf("%d ", -1);
		else
			printf("%d ", res[i]);
	}
	return 0;
	/*
	10 8
1 2 1 4 4 6 7 2 9
1 4
3 6
3 7
9 4
5 10
3 4
3 9
7 9
2 2 1 -1 3 3 -1 2 -1
		*/
}
```
