# 比赛总结

---

### 比赛地址：[sdtbu选拔赛3 - Virtual Judge (vjudge.net)](https://vjudge.net/contest/552058)

### 题解地址：[2022-2023 ICPC Latin American Regional Programming Contest — Unofficial editorial - Codeforces](https://codeforces.com/blog/entry/114211)

### 题解：

##### A

思路：

刚开始以为只有入度为0的点满足条件，结果发现想的太简单了，后面发现a可以到达b、c，而a、b、c又可以由某一个点到达，满足这个条件，a就会亏钱，但是暴力去判断的话时间复杂度是O(n^3)，会T。此题的关键是建反向图，这样的话只用判断从a、b、c能到达哪些点，如果有一个点被访问了3次，那就说明从这个点出发可以到达a、b、c，即a会亏钱。当问题较复杂时，有时考虑建反图不失为一种处理的思路。

代码：

```cpp
#include <iostream>
#include <cstring>
#include <vector>
using namespace std;
#define N 1005
char ans[N];
int flag[N], cnt[N];
vector <int> edge1[N], edge2[N];
void dfs(int u, int f)
{
    flag[u] = 1, cnt[u]++;
    for(int i = 0; i < edge2[u].size(); i++)
    {
        int v = edge2[u][i];
        if(v == f || flag[v])
            continue;
        dfs(v, f);
    }
}
int main()
{
    int n;
    cin >> n;
    for(int i = 1; i <= n; i++)
    {
        int u, v;
        cin >> u >> v;
        edge1[i].push_back(i);
        edge1[i].push_back(u);
        edge1[i].push_back(v);
        edge2[u].push_back(i);
        edge2[v].push_back(i);
    }
    for(int i = 1; i <= n; i++)
    {
        memset(cnt, 0, sizeof(cnt));
        for(int j = 0; j < edge1[i].size(); j++)
        {
            int u = edge1[i][j];
            memset(flag, 0, sizeof(flag));
            dfs(u, i);
        }
        ans[i] = 'N';
        for(int j = 1; j <= n; j++)
        {
            if(cnt[j] == 3)
            {
                ans[i] = 'Y';
                break;
            }
        }
    }
    cout << ans + 1 << endl;
    return 0;
}

```

##### B

思路：

签到题，按题意模拟即可。

代码：

```cpp
#include <iostream>
using namespace std;
#define N 10005
char ans[N][3];
int main()
{
    int n, h, w;
    cin >> n >> h >> w;
    for(int i = 1; i <= n; i++)
    {
        char c1, c2;
        cin >> c1 >> c2;
        if(c1 == 'Y' || !w)
        {
            ans[i][1] = 'Y';
            h--, w++;
        }
        else
            ans[i][1] = 'N';
        if(c2 == 'Y' || !h)
        {
            ans[i][2] = 'Y';
            h++, w--;
        }
        else
            ans[i][2] = 'N';
    }
    for(int i = 1; i <= n; i++)
        cout << ans[i][1] << " " << ans[i][2] << endl;
    return 0;
}

```

##### C

思路：

这题我是分清楚情况跑的可行性背包。首先考虑最复杂的情况，即三段相等的情况，会发现n绝对是3的倍数，这个时候直接手写一下，当n == 3 || n == 12时，输出2，当n == 6 || n == 9时，输出3，其余情况均是0。其它的情况，当两段等长时，将其中的一段直接用长度正好的那段覆盖，剩下的那段跑可行性背包。当长的那段没用时，直接用长度正好的那段覆盖，短的那段跑可行性背包。当短的那段没用时，直接用长度正好的那段覆盖，长的那段跑可行性背包。当两段都没用时，根据题意，直接输出0。

代码：

```cpp
#include <iostream>
using namespace std;
#define N 1005
int flag[N], dp[N];
void cal()
{
    dp[0] = 1;
    for(int i = 1; i <= 1000; i++)
    {
        if(flag[i])
            continue;
        for(int j = 1000; j >= i; j--)
            if(!dp[j] && dp[j - i])
                dp[j] = 1;
    }
}
int main()
{
    int n, k, e;
    cin >> n >> k >> e;
    int l = e, r = n - e - k;
    if(l == k && r == k)
    {
        if(n == 3 || n == 12)
            cout << 2 << endl;
        else if(n == 6 || n == 9)
            cout << 3 << endl;
        else
            cout << 0 << endl;
    }
    else
    {
        flag[k] = 1;
        int mx = max(l, r);
        int mn = min(l, r);
        if(mx == mn)
        {
            flag[mx] = 1;
            cal();
            int ans = 0;
            for(int i = mn; i >= 0; i--)
            {
                if(dp[i])
                {
                    ans = mn - i;
                    break;
                }
            }
            cout << ans << endl;
        }
        else
        {
            if(flag[mx])
            {
                flag[mn] = 1;
                cal();
                int ans = 0;
                for(int i = mx; i >= 0; i--)
                {
                    if(dp[i])
                    {
                        ans = mx - i;
                        break;
                    }
                }
                cout << ans << endl;
            }
            else if(flag[mn])
            {
                flag[mx] = 1;
                cal();
                int ans = 0;
                for(int i = mn; i >= 0; i--)
                {
                    if(dp[i])
                    {
                        ans = mn - i;
                        break;
                    }
                }
                cout << ans << endl;
            }
            else
                cout << 0 << endl;
        }
    }
    return 0;
}

```

##### D

思路：

看到这题的时限是5s，直接暴力判断。暴力枚举每个点作为起始点的答案，最终的答案就是能吃到的最多的那个。在跑一个点时，根据贪心的思想可以知道每次弹出队列中最小的那个数，所以需要用到优先队列，我也是这样想的，但是帆哥拿普通队列也过了，出队列的顺序跟数的大小无关，这一点我还是没太想清楚，容我再想想。

代码：

```cpp
#include <iostream>
#include <cstring>
#include <queue>
using namespace std;
#define N 105
int r, c, mp[N][N], flag[N][N];
int dir[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
struct Node
{
    int number;
    int x;
    int y;
    friend bool operator < (const Node &x, const Node &y)
    {
        return x.number > y.number;
    }
};
int bfs(int st_x, int st_y)
{
    int res = 0;
    memset(flag, 0, sizeof(flag));
    priority_queue <Node> q;
    q.push({mp[st_x][st_y], st_x, st_y});
    flag[st_x][st_y] = 1;
    while(!q.empty())
    {
        Node node = q.top();
        q.pop();
        res++;
        for(int i = 0; i < 4; i++)
        {
            int x = node.x + dir[i][0];
            int y = node.y + dir[i][1];
            if(x < 1 || x > r || y < 1 || y > c)
                continue;
            if(mp[x][y] < node.number || flag[x][y])
                continue;
            q.push({mp[x][y], x, y});
            flag[x][y] = 1;
        }
    }
    return res;
}
int main()
{
    cin >> r >> c;
    for(int i = 1; i <= r; i++)
        for(int j = 1; j <= c; j++)
            cin >> mp[i][j];
    int ans = 0;
    for(int i = 1; i <= r; i++)
    {
        for(int j = 1; j <= c; j++)
        {
            int num = bfs(i, j);
            ans = max(ans, num);
        }
    }
    cout << ans << endl;
    return 0;
}

```

##### E

看着像是个模拟题，等有空再搞搞吧。
