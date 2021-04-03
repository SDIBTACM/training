## 周记

### 1.*Bellman-Ford算法*

##### 此算法是用于负权图中，对于负权图dijkstra无法进行 ，而此算法可以通过判断是否有负环来进行返回 某一点（起始点）到其他点的最短路，时间复杂度为O（mn）m为边数，n为节点数 。 

进行 n-1次松弛，所有情况都被找到，从而得到最短路 

> // Bellman-Ford 算法代码实现
> // 时间复杂度：O(nm) n为节点数，m为边数
> // 用来判断是否存在负环，没有负环找到起始点到其他点的最短路
> // bellman_ford 的 基本思路如下：
> // 首先需要理解"存在负环就没有最短路"，原因,你可以一直在这个负环打圈，路径权值会不断减小，不断地趋近于负无穷大。
> // 理解了存在负环就没有最短路，那么如果存在最短路，通过 至多 n-1轮松弛操作即可得到初始点到其他点的最短路。
> // 如果还可以进行 第 n 轮 松弛，那么存在负权回路。

> \#include <iostream>
> \#include <vector>
> using namespace std;

> const int N = 110;
> const int INF = 1000000007;

> // dist[i] 起始点到点 i 的最短距离
> int dist[N];
> // 边，s 边的起点编号，e 边的终点编号，v 边的长度
> typedef struct Edge {
>   int s, e, v;
> }Edge;

> vector <Edge> E;

> // n 个节点， 起始点为 start
> // 函数 return false ；有负权回路；return true： 没有负权回路
> bool bellman_ford(int n, int start) {
>   // 初始化
>   for(int i=1; i<=n; i++) {
>     dist[i]=INF;
>   }
>   dist[start]=0;

> // n-1轮松弛操作
>   for(int i=0; i<n; i++) {
>     bool flag = false;
>     for(int j=0; j<E.size(); j++) {
>       int s = E[j].s;
>       int e = E[j].e;
>       int v = E[j].v;

      if(dist[e] > dist[s]+v) {
        dist[e] = dist[s]+v;
        flag = true;
      }
    }
    if(!flag) return true;
> }

> // 第 n 轮 松弛 
>   for(int j=0; j<E.size(); j++) {
>     if(dist[E[j].e] > dist[E[j].s]+E[j].v)
>       return false;
>   }

>  return true;
> }

> void addEdge(Edge edge) {
>   E.push_back(edge);
> }
> int main()
> {
>   // n 个节点， m 条边
>   int n, m;
>   cin >> n >> m;
>   int s, e, v;
>   Edge edge;
>   for(int i=1; i<=m; i++) {
>     cin >> s >> e >> v;
>     edge.s = s;
>     edge.e = e;
>     edge.v = v;
>     addEdge(edge);
>   }
>   int start = 1;
>   if(!bellman_ford(n, start)) {
>     cout << "有负权回路" << endl;
>   }
>   else {
>     cout << "没有负权回路" << endl;
>   }
>   return 0;
> }

> /* Test:
> 3 3
> 1 2 1
> 2 3 2
> 3 1 -2
> 3 3
> 1 2 1
> 2 3 2
> 3 1 -3
> 3 3
> 1 2 1
> 2 3 2
> 3 1 -4
> */

### 2.SPFA算法

> •1.初始时，只有把起点放入队列中。queue

> •2.遍历与起点相连的边，如果可以松弛就更新距离dis[],然后判断如果这个点没有在队列中就入队标记。

> •3.出队队首，取消标记，循环，直至队为空。

> •4.所有能更新的点都更新完毕，dis[]数组中的距离就是，起点到其他点的最短距离。

![image-20210403102431687](#周记.assets/image-20210403102431687.png)

######  判断负环

•SPFA算法保证每个顶点至多入队列n次（n为顶点总数）更新出到所有点的最短距离

•

•如果某个点入队列次数大于n次表示图中有负环

##   算法复习

### 1.栈

##### 先进后出  类似拿盘子

stack<i>a;//i  为你定义的数据类型

stack.push 进栈

stack.pop  出栈

stack.sizeof(a); 栈a的长度

stack.empty 判断是否为空

stack.top  返回栈尾的数据，但不改变

### 2.队列

##### 先进先出  类似排队买饭

queue<i>a;//同上

queue.push 进队列

queue.pop  出队列

queue.size（a）;队列a的长度

queue.empty  判断是否为空

queue.front  返回队列头的数据，但不改变

## 本周感想

##### 丰富而又充实，需要学的东西还很多，趁着假期可以学习一点算法，算法还是硬伤







 

 

