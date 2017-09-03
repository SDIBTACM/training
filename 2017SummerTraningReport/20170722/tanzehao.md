## 二叉树
```cpp
struct Tree
{
    char data;
    
    tree *lchild;
    tree *rchild;
};
```
感觉没什么能说的<br>
掌握先序中序后序遍历、建树<br>
感觉没怎么遇到过二叉树的题<br>
注意一下层次遍历建树<br><br><br>

## 并查集
```cpp
void unite(int x, int y)
{
    x = find_fa(x);
    y = find_fa(y);
    
    if(x == y)
        return ;
    
    if(h[x] < h[y])
        fa[x] = y;
    else
    {
        fa[y] = x;
        if(h[x] == h[y])
            h[x]++;
    }
}
```
经典的畅通工程(HDU 1232)<br>
检查多个点是否在一个集合内<br>
注意路径压缩与按秩排序<br>
感觉大多数都是模板题，也可能我做这方面的题比较少<br>
最难的可能就是食物链这种的了吧(POJ 1182)<br><br><br>

## 最小生生成树
```cpp
int prime()
{
    memset(f, 0, sizeof(f));
    for(int i = 1; i <= n; i++)
        d[i] = map[1][i];
    f[1] = 1;

    int sum = 0;
    for(int i = 1; i < n; i++)
    {
        int maxn = -INF, p = -1;
        for(int j = 1; j <= n; j++)
        {
            if(!f[j] && d[j] > maxn)
            {
                maxn = d[j];
                p = j;
            }
        }

        if(maxn == 0)
            return -1;

        sum += maxn;
        f[p] = 1;

        for(int j = 1; j <= n; j++)
            if(!f[j] && d[j] < map[p][j])
                d[j] = map[p][j];
    }
    return sum;
}
```
Prim算法用的比Kruskal算法熟练<br>
但是个人感觉Kruskal用并查集会更快一些<br>
dalao说与是否是稠密图有关<br>
对于最短路挺感兴趣的，可能是因为没学过<br>
<br><br><br>

- 希望以后能变得更强<br>
- いじょうです

