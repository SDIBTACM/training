# 本周感想
- 做了4套题，1套div2，3套从cf上抓的题，分数都是1400-1800之间的，坚持下去收获还是挺明显的，至少目前做题速度已经快了不少了
- 复习了一下强连通分量、线段树、树状数组，理解的过程看可视化的步骤能简单不少，不过对于听众来说还是逃不了自学的命运
### [题目](https://atcoder.jp/contests/abc305/tasks/abc305_e)
### 题意
- 给定一张N个点(编号为1～N)，M 条边的无向图，保证无重边无自环。现在有K个被标记的点，其中第i个被标记的点的编号为pi，任何从pi
出发经过不超过hi;条边能到达的点都会被染色（包括pi自身）。你需要求出这张图最终有哪些点被染色。
### 思路
- 贪心 & 层序遍历
- 每次取出水流流量最大的水源进行遍历
- 每次遍历只遍历一层，将终点放进水源集合
- 这样就能保证每个点都只访问一次，时间复杂度nlogn
### 代码
~~~c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
using ull = unsigned long long;
using PLL = pair<ll, ll>;
#define mem(a, b) memset(a, b, sizeof(a));
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
const int N = 2e5 + 5;
////////////////////////////////////////////////////////////////
vector<ll> a[N];
ll vis[N];

void solve()
{
    ll n, m, k; cin >> n >> m >> k;
    while (m--)
    {
        ll x, y; cin >> x >> y;
        a[x].push_back(y);
        a[y].push_back(x);
    }
    priority_queue<PLL> q;
    set<ll> st;
    while (k--)
    {
        ll u, p; cin >> u >> p;
        q.push({ p + 1,u });
        vis[u] = p + 1;
        st.insert(u);
    }

    while (q.size())
    {
        PLL t = q.top(); q.pop(); 
        ll u = t.second, si = t.first;
        if (vis[u] != si) continue;
        st.insert(u);

        --si;
        if (si == 0) continue;
        for (int i = 0; i < a[u].size(); ++i)
        {
            ll d = a[u][i];
            if (si <= vis[d]) continue;
            q.push({ si, d });
            st.insert(d);
            vis[d] = si;
        }
    }

    cout << st.size() << endl;
    for (auto I : st) cout << I << " ";
}

int main()
{
    IOS;
    //int T; cin >> T; while (T--)
        solve();

    return 0;
}
~~~
