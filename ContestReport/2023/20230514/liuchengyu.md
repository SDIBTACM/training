# 第二周组队赛

上周末去打陕西邀请赛了，第二周组队赛没打，题目是我这两天一个人补的，补的题都没看题解，有的题也是改了好几个做法交了好几遍才交上的，由于是我一个人做的补题报告就不按之前的格式了，随便写了。这段时间有点心神不宁，邀请赛也打铁了，挺难受的，不想那么多了，时间会治愈一切。

### 题目链接

[Dashboard - 2022 ICPC Gran Premio de Mexico 1ra Fecha - Codeforces](https://codeforces.com/gym/103708)

---

### B. Building 5G antennas

思路：

我的做法与网上的题解不同，我的做法是LCA。先将1当做根，之后从1开始深搜，记录每个点的深度。第一次肯定走点1，先把距离点1小于等于k的节点都放进优先队列里，每次弹出序号最小的节点，记为点now，每当弹出一个节点后，更新优先队列，遍历距离点1的距离为dis[now] + 1到dis[now] + k的这些点，只有这些点中存在可能放进优先队列中的点。为什么小于dis[now] + 1的点不行呢，因为如果一个距离小于dis[now] + 1的点是当前点now的祖先，那么它一定已经在优先队列中了；如果一个距离小于dis[now] + 1的点不是当前点now的祖先，那么它们之间的距离一定大于k。为什么大于dis[now] + k的点不行呢，因为对于任意一个距离大于dis[now] + k的点，它与点now之间的距离一定大于k。于是我们只用遍历dis[now] + 1到dis[now] + k这些点即可，在这些点中，并不是所有点都能到达，只有那些与点now距离小于等于k的点才能到达，需要用到LCA快速计算出两点之间的距离。算法用到了数据结构优化，总的时间复杂度不好分析，但是这题给了3000ms的时限，我的代码用了大概1200ms就能跑完，时间复杂度完全够。

代码：

```cpp
#include <iostream>
#include <queue>
#include <vector>
#include <set>
using namespace std;
#define N 100005
int lg[N], dis[N], dep[N], fa[N][20];
priority_queue <int, vector <int> , greater <int> > q;
vector <int> ans, edge[N];
set <int> st[N];
void cal()
{
    for(int i = 1; i < N; i++)
        lg[i] = lg[i / 2] + 1;
}
void dfs1(int u, int f, int d)
{
    st[d].insert(u);
    dis[u] = d;
    for(int i = 0; i < edge[u].size(); i++)
    {
        int v = edge[u][i];
        if(v == f)
            continue;
        dfs1(v, u, d + 1);
    }
}
void dfs2(int u, int f)
{
    for(int i = 1; i <= lg[dep[u]]; i++)
        fa[u][i] = fa[fa[u][i - 1]][i - 1];
    for(int i = 0; i < edge[u].size(); i++)
    {
        int v = edge[u][i];
        if(v == f)
            continue;
        dep[v] = dep[u] + 1;
        fa[v][0] = u;
        dfs2(v, u);
    }
}
int lca(int u, int v)
{
    if(dep[u] < dep[v])
        swap(u, v);
    while(dep[u] > dep[v])
        u = fa[u][lg[dep[u] - dep[v]] - 1];
    if(u == v)
        return u;
    for(int i = lg[dep[u] - 1]; i >= 0; i--)
    {
        if(fa[u][i] != fa[v][i])
        {
            u = fa[u][i];
            v = fa[v][i];
        }
    }
    return fa[u][0];
}
int main()
{
    cal();
    int n, k;
    cin >> n >> k;
    for(int i = 1; i <= n - 1; i++)
    {
        int u, v;
        cin >> u >> v;
        edge[u].push_back(v);
        edge[v].push_back(u);
    }
    dfs1(1, 0, 0);
    for(int i = 0; i <= k; i++)
    {
        for(auto it : st[i])
            q.push(it);
        st[i].clear();
    }
    dfs2(1, 0);
    while(!q.empty())
    {
        int now = q.top();
        q.pop();
        ans.push_back(now);
        for(int i = dis[now] + 1; i <= min(dis[now] + k, N - 1); i++)
        {
            set <int>::iterator it;
            for(it = st[i].begin(); it != st[i].end(); )
            {
                int d = dis[now] + dis[*it] - 2 * dis[lca(now, *it)];
                if(d <= k)
                {
                    q.push(*it);
                    st[i].erase(it++);
                }
                else
                    it++;
            }
        }
    }
    for(int i = 0; i < ans.size(); i++)
    {
        if(i == 0)
            cout << ans[i];
        else
            cout << " " << ans[i];
    }
    cout << endl;
    return 0;
}

```

### D. Different Pass a Ports

思路：

如果这题把M或者K改小一点的话，完全能用dp暴力跑，一维枚举当前步数，二维枚举当前点，三维枚举从哪个点转移过来，是一个很裸的dp，但是时间复杂度太高，为O(KM)，在这题里就是5e8的复杂度，给了1000ms的时限属实极限，我先这样试了下，发现这样做会T在test181，无论再怎样优化都过不去test181这个坎，必须换做法。通过观察可以发现每次的状态转移都是相同的，以样例一举例，点1的状态只能从点2、3、4、5转移而来，无论走了多少步。于是我们就可以构造两个矩阵，第一个矩阵是一个1 * n的状态数矩阵，第二个矩阵是一个n * n的状态转移矩阵，对本题而言就是可达矩阵，之后拿矩阵快速幂加速计算即可，最终的答案就是状态数矩阵的和。

代码：

```cpp
#include <iostream>
#include <cstring>
using namespace std;
#define int long long
#define N 105
#define MOD 1000000007
int res[2][N], base[N][N];
void mul(int x[][N], int y[][N], int n, int m)
{
    int t[n + 5][m + 5];
    memset(t, 0, sizeof(t));
    for(int i = 1; i <= n; i++)
        for(int j = 1; j <= m; j++)
            for(int k = 1; k <= m; k++)
                t[i][j] = (t[i][j] + x[i][k] * y[k][j]) % MOD;
    for(int i = 1; i <= n; i++)
        for(int j = 1; j <= m; j++)
            x[i][j] = t[i][j];
}
void matrix_fpow(int res[][N], int base[][N], int n, int k)
{
    while(k)
    {
        if(k & 1)
            mul(res, base, 1, n);
        mul(base, base, n, n);
        k >>= 1;
    }
}
signed main()
{
    int n, m, k;
    cin >> n >> m >> k;
    for(int i = 1; i <= m; i++)
    {
        int a, b;
        cin >> a >> b;
        base[a][b] = 1;
        base[b][a] = 1;
    }
    res[1][1] = 1;
    matrix_fpow(res, base, n, k);
    int ans = 0;
    for(int i = 1; i <= n; i++)
        ans = (ans + res[1][i]) % MOD;
    cout << ans << endl;
    return 0;
}

```

### H. Hog Fencing

思路：

这题跟李旭给趣味编程赛出的那题不能说一模一样，只能说完全一致。签到题，简单看两眼就能推导出公式，不多说了。

代码：

```cpp
#include <iostream>
using namespace std;
int main()
{
    int n;
    cin >> n;
    int a = n / 4, b = n / 4;
    if(n % 4 >= 2)  
        a += 1;
    int ans = a * b;
    cout << ans << endl;
    return 0;
}

```

### I. Isabel's Divisions

思路：

又是一道签到题，按照题意模拟即可。

代码：

```cpp
#include <iostream>
using namespace std;
int main()
{
    int n;
    cin >> n;
    int ans = 0, t = n;
    while(t)
    {
        int tt = t % 10;
        t /= 10;
        if(tt)
            ans += n % tt == 0;
    }
    cout << ans << endl;
    return 0;
}

```

### J. Jeffrey's ambition

思路：

这题一眼二分图最大匹配板子题，但是一看数据，1e4，好大啊，但是仔细一想二分图最大匹配没别的做法啊，通常就用匈牙利算法，时间复杂度O(NM)，对于这题来说就是1e9，300ms的时限按理说完全不够用。但是想到二分图最大匹配没有时间复杂度更小的做法了，而且O(NM)只是理论上的时间复杂度上限，实际上跑起来可能根本达不到1e9的复杂度，干脆直接一发交了，结果过了，看来这题就是在吓唬我们。网上的题解是用网络流过的，时间复杂度好像更高了？我没仔细看，反正作者也说是卡过的，依我看这题就是吓唬人呢，不是啥卡过的，就是这么做的。

代码：

```cpp
#include <iostream>
#include <cstring>
#include <vector>
using namespace std;
#define N 10005
int n, m, vis[N], belong[N];
vector <int> edge[N];
bool find(int x)
{
    for(int i = 0; i < edge[x].size(); i++)
    {
        if(!vis[edge[x][i]])
        {
            vis[edge[x][i]] = 1;
            if(!belong[edge[x][i]] || find(belong[edge[x][i]]))
            {
                belong[edge[x][i]] = x;
                return true;
            }
        }
    }
    return false;
}
int main()
{
    ios::sync_with_stdio(0);
    cin.tie(0), cout.tie(0);
    cin >> n >> m;
    for(int i = 1; i <= n; i++)
    {
        int k;
        cin >> k;
        for(int j = 1; j <= k; j++)
        {
            int c;
            cin >> c;
            edge[i].push_back(c);
        }
    }
    int ans = 0;
    for(int i = 1; i <= n; i++)
    {
        memset(vis, 0, sizeof(vis));
        if(find(i))
            ans++;
    }
    cout << m - ans << endl;
    return 0;
}

```

### K. Kilo Waste

思路：

裸的背包题，先跑一遍可行性背包，之后二分即可。

代码：

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;
#define N 100005
int r[55], dp[N];
vector <int> v;
int main()
{
    int k, p;
    cin >> k >> p;
    for(int i = 1; i <= p; i++)
        cin >> r[i];
    dp[0] = 1;
    for(int i = 1; i <= p; i++)
        for(int j = r[i]; j <= 100000; j++)
            if(!dp[j] && dp[j - r[i]])
                dp[j] = 1;
    for(int i = 1; i <= 100000; i++)
        if(dp[i])
            v.push_back(i);
    while(k--)
    {
        int n;
        cin >> n;
        int pos = lower_bound(v.begin(), v.end(), n) - v.begin();
        cout << v[pos] - n << endl;
    }
    return 0;
}

```
