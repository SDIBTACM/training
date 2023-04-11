# 比赛总结

---
赛时只做了签到题, 赛后补了c, d, 学习了一下别人的a, 题面真的很长根本看不懂...真想好好学学英语
---
### 比赛地址：[sdtbu选拔赛3 - Virtual Judge (vjudge.net)](https://vjudge.net/contest/552058)

### 题解地址：[2022-2023 ICPC Latin American Regional Programming Contest — Unofficial editorial - Codeforces](https://codeforces.com/blog/entry/114211)

### 题解：

##### A

思路：

以为只有不被要钱和可以要到钱时才满足条件, 现在才发现忘了考虑父节点

代码：

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
#define endl '\n'
// #define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0); //勿混用
// #define deBug
#define fr(qa, ws, ed) for (int qa = (ws); qa <= (ed); ++qa)
#define mem(z, o) memset(z, o, sizeof(z))
template<class T> inline void Swap(T &z, T &o) { z^=o, o^=z, z^=o; }
template<class T> inline T Max(const T &z, const T &o) { return z > o ? z : o; }
template<class T> inline T Min(const T &z, const T &o) { return z < o ? z : o; }
#ifdef deBug
//++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

namespace DEBUG_ {
#define debug(...) DEBUG_::_debug(#__VA_ARGS__, __VA_ARGS__)
// #define debuga(z) DEBUG_::_debug(z)
    inline void _debug(){cerr << "-------------------" << endl; }
    template <typename T> inline void _debug(const char* name, T _) 
    {cerr << name << '=' << _ << endl;}
    template <typename T> inline void _debug(const char* name, vector<T> &VV) 
    {cerr<<name<<'='<<"[";for(const auto &vv:VV)cerr<<vv<<",";cerr<<"]"<<endl;}
    //  template <typename T> inline void _debug(T &aa) 
    // {cerr << '{';for (auto &i : aa)cerr << i << ",";cerr << '}' << endl;}
    template <class First, class... Rest>
    inline void _debug(const char* name, First first, Rest... rest)
    { while (*name != ',') cerr << *name++;cerr << '=' << first << ",";
    _debug(name + 1, rest...); }
    // template <typename T> ostream& operator<<(ostream& os, const vector<T>& V)
    // { os << "[";for (const auto& vv : V) os << vv << ",";os << "]";return os; }
}
#endif
#ifndef IOS
namespace Read{
    template<class T> inline void rd(T &xxx){ xxx = 0;char fff = 0,crh = getchar();
        while (!isdigit(crh)) fff |= (crh == '-'), crh = getchar();
        while (isdigit(crh)) xxx=(xxx<<1)+(xxx<<3)+(crh&15), crh = getchar();
        xxx = fff?-xxx:xxx; 
    }
    template<typename Firt,typename...Rest>
    inline void rd(Firt &x, Rest&...y){rd(x), rd(y...);}
    template<class T> inline void pr(T x){ 
        if(x < 0) {putchar('-'); x = - x; }
	    if(x >= 10) pr(x / 10); putchar('0' + x % 10); 
    }
    template<typename T> inline void ps(T *x){while(*x) putchar(*(x++));}
    #define Pr(z, o) pr(z), putchar(o)
    #define pu(z) putchar(z)
} using namespace Read;
#endif
//+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

const int N = 1e4+105, M = 1e9;

using pii = pair<int, int>;
unordered_map<int, int>mp;
int vis[1010];
vector<int>ve[1010];

void dfs(int i, int fa)
{
    vis[i] = 1;
    ++mp[i];
    for (auto &it : ve[i])
    {
        if (!vis[it] && it != fa) dfs(it, fa);
    }
}
void solve()
{
    int n; rd(n);
    int x, y;
    vector<pii>a(n+1);
    fr (i, 1, n)
    {
        rd(x, y); a[i] = {x, y};
        ve[x].emplace_back(i);
        ve[y].emplace_back(i);
    }

    fr (i, 1, n)
    {
        {
            mem(vis, 0);
            dfs(a[i].first, i);
            mem(vis, 0);
            dfs(a[i].second, i);
            mem(vis, 0);
            dfs(i, i);

            bool lp = 0;
            for (auto &it : mp)
            {
                if (it.second >= 3)
                {
                    lp = 1;
                    break;
                }
            }
            putchar(lp ? 'Y' : 'N');
        }
        mp.clear();
    }
    putchar('\n');
}
signed main()
{
    {solve();}
    return 0;
}


```

##### C

思路：

读题的时候就没理解题目什么意义, 后来读懂了发现只需要先挑一段出来讨论记录状态, 在01背另外一段, 讨论两段处理顺序取最优就可

代码：

```cpp
#include <bits/stdc++.h>
using namespace std;
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0); //勿混用
template<class T> inline T Min(const T &z, const T &o) { return z < o ? z : o; }
template<class T> inline T Max(const T &z, const T &o) { return z > o ? z : o; }

unordered_map<int, bool> mp;
int dp[1010];

int Dp(int k)
{
    for (int i = 1; i <= k; ++i)
    {
        if (mp[i]) continue;
        for (int j = k; j >= i; --j) dp[j] = Max(dp[j], dp[j - i] + i);
    }
    return dp[k];
}
void solve()
{
    int n, k, e; cin >> n >> k >> e;
    int k2 = n - k - e;
    auto f = [=](int k, int k2)->int
    {
        mp.clear();
        mp[e] = 1;
        memset(dp, 0, sizeof dp);

        int a = k;
        if (k != e) mp[k] = 1;
        else if (!mp[k - 1])
        {
            mp[k-1] = 1;
            if (!mp[1]) mp[1] = 1;
            else a = k-1;
        }
        int b = Dp(k2);
        return n - e - a - b;
    };
    int ans = Min(f(k, k2), f(k2, k));
    
    cout << ans << endl;
}
signed main()
{
    IOS
    {solve();}
    return 0;
}


```

##### D

思路：

暴力枚举每个点作为起始点的答案, 题目意思其实就是可扩展的满足条件的总步数
多写几种方法让自己长长记性

代码：

```dfs + 记忆化
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0); //勿混用
#define deBug
#define fr(qa, ws, ed) for (int qa = (ws); qa <= (ed); ++qa)
#define mem(z, o) memset(z, o, sizeof(z))
template<class T> inline void Swap(T &z, T &o) { z^=o, o^=z, z^=o; }
template<class T> inline T Max(const T &z, const T &o) { return z > o ? z : o; }
template<class T> inline T Min(const T &z, const T &o) { return z < o ? z : o; }

int d[][2] = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
int n, m;
int a[110][110], vis[110][110];

inline bool check(int x, int y)
{
    if (x < 1 || y < 1 || x > n || y > m) return 0;
    return 1;
}
int dfs(int x, int y)
{
    vis[x][y] = 1;
    fr (i, 0 ,3)
    {
        int dx = x + d[i][0], dy = y + d[i][1];
        if (check(dx, dy) && a[dx][dy] > a[x][y] && !vis[dx][dy])
            vis[x][y] += dfs(dx, dy);
    }

    return vis[x][y];
}
inline void solve() 
{
    cin >> n >> m;
    fr (i, 1, n) fr (j, 1, m) cin >> a[i][j];

    int ans = 0;
    fr (i, 1, n) fr (j, 1, m)
    {
        mem(vis, 0);
        ans = Max(ans, dfs(i, j));
    }

    cout << ans << endl;
}
signed main()
{
    IOS
    // int t; cin >> t;
    // fr (i, 1, t)
    {solve();}
    return 0;
}

```
``` bfs
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0); //勿混用
#define deBug
#define fr(qa, ws, ed) for (int qa = (ws); qa <= (ed); ++qa)
#define mem(z, o) memset(z, o, sizeof(z))
template<class T> inline void Swap(T &z, T &o) { z^=o, o^=z, z^=o; }
template<class T> inline T Max(const T &z, const T &o) { return z > o ? z : o; }
template<class T> inline T Min(const T &z, const T &o) { return z < o ? z : o; }

int d[][2] = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
int n, m;
int a[110][110], vis[110][110];

inline bool check(int x, int y)
{
    if (x < 1 || y < 1 || x > n || y > m) return 0;
    return 1;
}
struct A{
    int x, y, w;
    bool operator < (const A &z)const{
        return this->w < z.w;
    }
};
int bfs(int x, int y)
{
    priority_queue<A> que;
    int res = 0;
    que.push({x, y, a[x][y]});
    vis[x][y] = 1;
    while (!que.empty())
    {
        ++res;
        auto t = que.top(); que.pop();
        fr (i, 0, 3)
        {
            int dx = t.x + d[i][0], dy = t.y + d[i][1];
            if (check(dx, dy) && !vis[dx][dy] && a[dx][dy] > t.w)
            {
                que.push({dx, dy, a[dx][dy]});
                vis[dx][dy] = 1;
            }
        }
    }

    return res;
}
inline void solve() 
{
    cin >> n >> m;
    fr (i, 1, n) fr (j, 1, m) cin >> a[i][j];

    int ans = 0;
    fr (i, 1, n) fr (j, 1, m)
    {
        mem(vis, 0);
        ans = Max(ans, bfs(i, j));
    }

    cout << ans << endl;
}
signed main()
{
    IOS
    // int t; cin >> t;
    // fr (i, 1, t)
    {solve();}
    return 0;
}

```
``` 按条件建图 找 最长路径
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0); //勿混用
#define deBug
#define fr(qa, ws, ed) for (int qa = (ws); qa <= (ed); ++qa)
#define mem(z, o) memset(z, o, sizeof(z))

const int N = 1e4+105, M = 1e9;

int d[][2] = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
int n, m;
int a[110][110], vis[N], st[110][110];
vector<int> ve[N];

inline int Hash(int x, int y){ return (x * 100 + y); };
int dfs(int x)
{
    int res = 1;
    vis[x] = 1;
    for (auto &i : ve[x])
        if (!vis[i]) res += dfs(i);
    return res;
}
void solve() 
{
    cin >> n >> m;
    fr (i, 1, n) fr (j, 1, m) cin >> a[i][j];

    fr (i, 1, n) fr (j, 1, m)
    {
        fr (k, 0, 3)
        {
            int dx = i + d[k][0], dy = j + d[k][1];
            if (a[dx][dy] > a[i][j]) ve[Hash(i, j)].emplace_back(Hash(dx, dy)), st[dx][dy];
        }
    }
    // debug(ve[Hash(1, 1)]);
    int ans = 0;
    fr (i, 1, n) fr (j, 1, m)
    {
        if (!st[i][j])
        {
            mem(vis, 0);
            ans = max(ans, dfs(Hash(i, j)));
        }
    }
    cout << ans << endl;
}
signed main()
{
    IOS
    // int t; cin >> t;
    // fr (i, 1, t)
    {solve();}
    return 0;
}

```
##### E
熬不动了下次再补
