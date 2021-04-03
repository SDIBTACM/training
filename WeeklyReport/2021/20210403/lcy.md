# 本周任务 #

## 学了…… ##

（1）Bellman_Ford算法

Bellman_Ford算法是一个效率比较低的算法，但它可以判断是否存在负环路。

可通过以下方法实现：

1.初始化图

就是存数据

2.重复对每一条边进行松弛操作

    for(i=1;i<n;i++)//最多进行n-1次循环
        for(j=1;j<=m;j++)//遍历每一条边
            if(dis[v]>dis[u]+w[u][v])//如可松弛则松弛
                dis[v]=dis[u]+w[u][v];

3.检查负权环

    for(i=1;i<=m;i++)
        if(dis[v]>dis[u]+w[u][v])
            return 0;//如还存在可松弛的情况，则存在负环路
    return 1;

（2）SPFA算法

通过百度我们不难发现一个悲伤的故事（以下内容引自百度百科）：

SPFA算法的全称是：Shortest Path Faster Algorithm，是西南交通大学段凡丁于 1994 年发表的论文中的名字。不过，段凡丁的证明是错误的，且在 Bellman-Ford 算法提出后不久（1957 年）已有队列优化内容，所以国际上不承认 SPFA 算法是段凡丁提出的。

SPFA算法也可判断是否存在负环路，是在Bellman_Man算法的基础上通过队列优化实现的，因此算法效率较高。

不难证明每个点最多入队n次，如果超过n次则存在负环路。

核心代码：

    int SPFA(int start)
    {
        int i;
        queue <int> q;
        for(i=1;i<=n;i++)
        {
            dis[i]=inf;
            vis[i]=0;
            cnt[i]=0;
        }
        dis[start]=0,vis[start]=1,cnt[start]=1;
        q.push(start);
        while(!q.empty())
        {
            start=q.front();
            q.pop();
            vis[start]=0;//出队则取消标记
            for(i=1;i<=n;i++)
            {
                if(dis[i]>dis[start]+map[start][i])
                {
                    dis[i]=dis[start]+map[start][i];
                    if(!vis[i])//如果能优化的点不在队列中则入队
                    {
                        vis[i]=1;//入队则添加标记
                        cnt[i]++;
                        if(cnt[i]>n)
                            return 0;//如果入队次数大于n次则存在负环路
                        else
                            q.push(i);
                    }
                }
            }
        }
        return 1;
    }

（3）Floyd算法

Floyd算法可求多源最短路，即从任意点出发的最短路。

核心代码：

    for(k=1;k<=n;k++)
    {
        for(i=1;i<=n;i++)
        {
            if(map[i][k]==inf)//如果i到k就不可走则跳过
                continue;
            for(j=1;j<=n;j++)
                if(map[i][j]>map[i][k]+map[k][j])//如可松弛则松弛
                    map[i][j]=map[i][k]+map[k][j];
        }
    }

总的来说这周学的算法都不难。。。

## 完成了…… ##

完成了vj的A，B，C题（D，E，F，G上周做了，H看不懂。。。）。

[https://acm.sdtbu.edu.cn/vjudge/contest/view.action?cid=2428#problem/A](https://acm.sdtbu.edu.cn/vjudge/contest/view.action?cid=2428#problem/A "Six Degrees of Cowvin Bacon")

这题思路挺直接的，用到了Floyd算法，求n头牛中到除自己外其它牛的平均距离最小的牛。

由于这题让求平均距离，所以得用double型变量，但是我懒得取整了，直接用%.0lf输出的，输出结果看起来一样，结果交了竟是WA！后来我又取整交了一遍就AC了，看来系统在判题时不仅看结果看起来是否一样，还看输出类型相不相同，题目让输出int型数据，你用%.0lf输出了一个double型数据就是不对，学到了，学到了。

[https://acm.sdtbu.edu.cn/vjudge/contest/view.action?cid=2428#problem/B](https://acm.sdtbu.edu.cn/vjudge/contest/view.action?cid=2428#problem/B "Wormholes")

这题要用SPFA算法判断是否存在负环路，如存在则满足题意。

这题我把inf设成了0x7fffffff，结果一直WA！错的我都不敢再提交了，默默地去POJ上照着师哥的代码一点一点改，一遍一遍试看是哪里错了，结果竟错在了inf的值上，把inf改成0x3fffffff就对了！我百度了下，大概意思是为了满足无穷大加无穷大仍为无穷大，不能让数据超过32位上限变成无穷小所以得用0x3fffffff，因为0x7fffffff=0x3fffffff+0x3fffffff+1，又学到了，又学到了。

[https://acm.sdtbu.edu.cn/vjudge/contest/view.action?cid=2428#problem/C](https://acm.sdtbu.edu.cn/vjudge/contest/view.action?cid=2428#problem/C "Silver Cow Party")

这题的意思是求除指定点外的所有点到指定点的距离的最小值及从指定点回来的距离的最小值的和的最大值（我知道你人已经傻了，没事我也傻了）。

我不咋会做，就上博客搜了下，解法就是先用Dijkstra算一遍从指定点到各点的距离，就相当于算了回去的最短路，再把所有路都反向，再用Dijkstra算一遍从指定点到各点的距离，就相当于算了过去的最短路，再相加求最大值，仔细一想好像是这个理儿。。。

## 进行了…… ##

我在补CSDN的博客，师哥说让创个博客我就创了，想着把之前学的所有算法都写成博客顺便复习下，就开始整了，中间由于备考计算机二级耽误了十几天，现在又开始整了，目前整到搜索了，但由于这个周报又耽搁了一个上午。。。不过没事这个周报也挺有用的，将来可以在博客在开个周报的专题。

PS：我博客的内容都没公开，因为整的不咋好，师哥见谅。。。

# 下周计划 #

## 学习…… ##

还能学啥，先把师哥讲的学会了再说。。。

## 复习…… ##

搜索的博客这周末就能整完，下周补二叉树的博客，如果快的话再补下最短路的博客，顺便就当复习了，我喜欢把事往前赶，这样玩的时候就可以放开了。^_^

# 本周感想 #

每一周都差不多，忙而充实，大学的时光有一半多都提供给了代码，既包括专业课还包括ACM（昨天做鹏哥留的作业，让用代码发出一首歌的伴奏，我看不懂简谱还自学了简谱。。。本来不用做很长的，结果上头了把整个明天会更好都敲完了，肝了4个小时，不过效果还不错），没办法，谁让我闲不住呢，也不咋爱玩游戏。。。
