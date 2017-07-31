#最小生成树
## Dijkstra
```cpp
void Dijkstra()
{
    memset(vis, 0, sizeof(vis));
    for(int i = 1; i <= n; i++)
        dis[i] = edge[1][i];

    dis[1] = 0;
    vis[1] = 1;

    int pos = 1;
    for(int i = 1; i < n; i++)
    {
        int min = INF;
        pos = 1;

        for(int j = 1; j <= n; j++)
        {
            if(!vis[j] && dis[j] < min)
            {
                min = dis[j];
                pos = j;
            }
        }
        
        vis[pos] = 1;
        for(int j = 1; j <= n; j++)
            if(!vis[j] && dis[pos] + edge[pos][j] < dis[j])
                dis[j] = dis[pos] + edge[pos][j];
    }
}
```
Dijkstra算法计算单源最短路<br>
也可以用来判断能否生成最短路<br>
Dijkstra算法的精髓在于松弛计算<br>
要注意的是无法计算负权图的最短路<br><br><br>

## Floyd
```cpp
void floyd()
{
    for(int k = 1; k <= n; k++)
        for(int i = 1; i <= n; i++)
            for(int j = 1; j <= n; j++)
                if(edge[i][k] + edge[k][j] < edge[i][j])
                    edge[i][j] = edge[i][k] + edge[k][j];
}
```
Floyd算法计算多源最短路<br>
典型的DP算法<br>
时间复杂都稍高 O(n^3 )<br>
Floyd可以用来传递闭包<br>
确定元素在集合中的位置、关系<br><br><br>

## Bellman_Ford
```cpp
bool Bellman()
{
    for(int i = 1; i <= n; i++)
        dis[i] = INF;
    dis[1] = 0;

    for(int i = 0; i < n-1; i++)
        for(int j = 0; j < v.size(); j++)
            if(dis[v[j].v] > dis[v[j].u] + v[j].w)
                dis[v[j].v] = dis[v[j].u] + v[j].w;

    for(int i = 0; i < v.size(); i++)
        if(dis[v[i].u] + v[i].w < dis[v[i].v])
            return 0;

    return 1;
}
```
Bellman_Ford算法可以判断是否存在负权回路<br>
正权回路可以用这个来判别<br>
感觉这个算法有些麻烦，不好理解<br><br><br>

---
## SPFA
稍微的看了一下，也是求单源最短路<br>
不同的地方在于使用了队列优化<br>
也可以用来判断负权回路<br>
只会套模板<br>

---
<br><br><br>

- 希望以后能变得更强<br>
- いじょうです

