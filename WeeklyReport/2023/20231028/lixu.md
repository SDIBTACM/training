# 周报感想
又迟到了
- 刚刚给这周的每日一题难度调整了一下，1500做着好像太水了。试试几周1600，不行再混几个高分的打乱顺序。
- 操作系统进程管理内容差不多看完了，这周看看内存管理，应该能看完
- 上周的java集合框架看的太慢了，这周抽半天时间看完整理完吧
- 又被老板催了，这周再多打几次组队赛，确实练得少就没提升，既然缴钱了就加把劲。

## [一个二分图匹配问题](https://vjudge.net/contest/590994#problem/I)
### 题意
- n 个英雄打 m 个怪物，每人只能打 自己能打的那部分怪物 的其中一只怪物。每个人可以选择喝一瓶药水，喝了可以多打一只怪物，最多只能喝一瓶，药水共 k 瓶。
- 求最多打多少只怪物
### 思路
- 二分图，开出两倍的英雄，分为前 n 个和后 n 个。每对相同的英雄能打的怪物信息相同。
- 匹配的第 n 个时，意义为：这些英雄在不喝药的情况下，能打的怪物数量。
- 匹配后 n 个人时，意义为：第 i 个英雄在喝药的情况下，能不能多打一只怪物。
- 不存在第 i + n 个人能打怪，但第 i 个不能打怪的情况。因为怪物信息相同，如果能打的话，先匹配第 i 个时就打了。
- 因为后面的人匹配时只会干扰前面匹配的目标怪物，但不会干扰英雄是否有打怪，所以后 n 个不会干扰前 n 个。条件成立

- 代码
```c++

#include <bits/stdc++.h>
using namespace std;
using ll = long long;
using ull = unsigned long long;
using PLL = pair<ll, ll>;
#define endl '\n'
#define mem(a, b) memset(a, b, sizeof(a));
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
const int N = 1e5 + 5;
//////////////////////////////////////////////////////////////
vector<ll> a[1505];

int match[1505], res;
int vis[1505];

int dfs(int x)
{
    for (auto j : a[x]) if (vis[j] == 0)
    {
        vis[j] = 1;
        if (match[j] == 0 || dfs(match[j]))
        {
            match[j] = x;
            return 1;
        }
    }
    return 0;
}

void solve()
{
    ll n, m, k; cin >> n >> m >> k;
    for (int i = 1; i <= n; ++i)
    {
        ll siz; cin >> siz;
        while (siz--)
        {
            ll x; cin >> x;
            a[i].push_back(x);
            a[i + n].push_back(x);
        }
    }

    ll ans1 = 0, ans2 = 0;
    for (int i = 1; i <= n << 1; i++)
    {
        memset(vis, 0, sizeof(vis));
        if (dfs(i))
        {
            if (i <= n) ans1++;
            else ans2++;
        }
    } 
    cout << ans1 + min(ans2, k) << endl;
}

signed main()
{
    IOS;
    //ll _; cin >> _; while (_--)
    solve();

    return 0;
}
```
