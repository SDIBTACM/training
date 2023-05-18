**组队赛第二周**

- lx
- wck
- zlk

**比赛地址**
https://vjudge.csgrandeur.cn/contest/557451

## 比赛总结
这次的组对赛 wck 读题读的好快做题的速度完全跟不上了, 感觉板子题挺多, 大部分板子还没学过, 又要被迫学算法了
## 题解
A : 要求按序号从小到大输出距离当前结点距离小于等于k 的子结点, 显然要从1开始, 用dfs遍历图(dfs遍历树还是不熟, 两个人写都写错好几次), 将与本结点小于等于k的子节点都遍历一遍,
将未访问过的子节点插入set中(或优先队列)以保证输出和遍历下一个结点的子节点时序列最小, 并且记录下访问到当前节点的步数, 若之前如果访问过该点，并且之前记录的步数比目前访问到这
个结点的步数大，则说明目前结点的子节点之前已经访问过了不用在继续往下遍历, 利用这一性质剪枝避免不必要的遍历(不加剪枝一直TL, 气死)
```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
#define fr(qa, ws, ed) for (int qa = (ws); qa <= (ed); ++qa)

const int N = 1e5 + 10;
int vis[N];
int n, m;
vector<int>V[N];
set<int>se;
void dfs(int u, int fa, int k)
{
    if (k < 0 || vis[u] > k) return ;
    if (!vis[u]) se.insert(u);
    vis[u] = k + 1;
    for (auto &it : V[u])
    {
        if (it == u) continue;
        dfs(it, u, k - 1);
    }
}
int main()
{
    IOS
    cin >> n >> m;
    fr (i, 1, n-1)
    {
        int x, y; cin >> x >> y;
        V[x].push_back(y);
        V[y].push_back(x);
    }
    se.insert(1);
    vis[1] = 1;
    while (!se.empty())
    {
        int t = *se.begin(); se.erase(se.begin());
        cout << t << " ";
        vis[t] = 1;
        dfs(t, t, m);
    }
    return 0;
}
```
B: 求从图上所有点走 k 步的方案数, 即求该图邻接矩阵的 k 次幂
矩阵快速幂板子(矩阵运算 + 快速幂, 之前没学过还以为多高级~)
```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
#define fr(qa, ws, ed) for (int qa = (ws); qa <= (ed); ++qa)

const int N = 1e6 + 1000, mod = 1e9+7;
using ll = long long;
struct matrix{
    int x, y;
    ll MM[110][110];
    matrix(int xx, int yy, int aa = 0) : x(xx), y(yy)
    {
        for (int i = 1; i <= xx; ++i)
            for (int j = 1; j <= yy; ++j)
                MM[i][j] = (i == j) * aa;
    }
    matrix operator * (const matrix &A) const {
        matrix temp(x, A.y);
        for (int i = 1; i <= x; ++i)
            for (int j = 1; j <= A.y; ++j)
                for (int k = 1; k <= y; ++k)
                    temp.MM[i][j] = (temp.MM[i][j] + (MM[i][k] * A.MM[k][j] % mod)) % mod;
        return temp;
    }
    matrix operator ^ (ll k) {
        matrix res(this->x, this->y, 1);
        matrix tt = *this;
        while (k)
        {
            if (k & 1) res = res * tt;
            k >>= 1, tt = tt * tt;
        }
        return res;
    }
};
int main()
{
    IOS
    int n, m, k; cin >> n >> m >> k;
    matrix a(n, n);
    fr (i, 1, m)
    {
        int x, y; cin >> x >> y;
        a.MM[x][y] += 1;
        a.MM[y][x] += 1;
    }
    a = a ^ k;
    ll ans = 0;
    fr (i, 1, n) ans = (ans + a.MM[1][i]) % mod;

    cout << ans << endl;
    return 0;
}
```
D : 答案即被分割的块的方案数的乘积, 难在如何快速求斐波那契数列
离 ac 只差一个 log(n) 求斐波那契的公式呜呜呜, 加上快速幂和hash优化可以更快
```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
#define fr(qa, ws, ed) for (int qa = (ws); qa <= (ed); ++qa)

const int N = 1e6 + 1000, mod = 1e9+7;
using ll = long long;
using pii = pair<ll, ll>;
unordered_map<int, int> Hash;
ll Pow(ll n, int k)
{
    ll res = 1;
    while (k)
    {
        if (k & 1) res = (res * n) % mod;
        n = (n * n) % mod, k >>= 1;
    }
    return res;
}
pii f(ll n)
{
    if (!n) return {0, 1};
    pii A = f(n/2);
    ll c = (A.first * ((A.second  * 2 - A.first + mod) % mod)) % mod;
    ll d = (A.first * A.first % mod + (A.second * A.second) % mod) % mod;
    if (n&1) return {d, (c + d) % mod};
    else return {c, d};
}
int main()
{
    IOS
    ll n, m; cin >> n >> m;
    ll *a = new ll[m+2];
    fr (i, 1, m) cin >> a[i];
    a[0] = -1;
    sort(a, a + 1 + m);
    fr (i, 1, m) ++Hash[a[i] - a[i-1] - 2];
    ++Hash[n - a[m] - 1];
    delete []a;
    ll ans = 1;
    for (auto &it : Hash)
    {
        ll a = it.second, b = it.first;
        if (b < 0) return cout << 0 << endl, 0;
        ans = (ans * Pow(f(b + 1).first, a)) % mod;
    }

    cout << ans << endl;
    return 0;
}
```
E: 签到题, 推公式
```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
#define fr(qa, ws, ed) for (int qa = ws; qa <= ed; ++qa)

const int N = 1e5 + 10;

int main()
{
    IOS
    int n; cin >> n;
    n >>= 1;
    cout << n/2 * ((n+1)/2) << endl;
    return 0;
}
```
F: 签到题
```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
#define fr(qa, ws, ed) for (int qa = ws; qa <= ed; ++qa)

const int N = 1e5 + 10;

int main()
{
    IOS
    int n; cin >> n;
    int t = n, cnt = 0;
    while (t)
    {
        int c = t % 10;
        if (c && n %c  == 0) ++cnt;
        t /= 10;
    }
    cout << cnt << endl;
    return 0;
}
```
G: 听队友说是匈牙利求最大匹配板子, 但是却一直超时, 加了个快读还是一直TL, 后来 lx 又优化了下终于 ac
赛后又学习了一下匈牙利, 发现一直 TL 的原因是频繁清标记数组过于浪费时间, 把标记数据 vis 开成 bool 类型或者二进制压位bitset(效率最好)就可
```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
// #define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
#define fr(qa, ws, ed) for (int qa = (ws); qa <= (ed); ++qa)

const int N = 1e4 + 10, M = 1e5 + 10;
#ifndef IOS
namespace Read{
    template<class T> inline void rd(T &xxx){ xxx = 0;char fff = 0,crh = getchar();
        while (!isdigit(crh)) fff |= (crh == '-'), crh = getchar();
        while (isdigit(crh)) xxx=(xxx<<1)+(xxx<<3)+(crh&15), crh = getchar();
        xxx = fff?-xxx:xxx; 
    }
    template<typename Firt,typename...Rest>
    inline void rd(Firt &x, Rest&...y){rd(x), rd(y...);}
} using namespace Read;
#endif
int h[N], e[M], ne[M], idx;
int match[N];
bitset<N> vis;

inline void add(int a, int b)
{
    e[++idx] = b, ne[idx] = h[a], h[a] = idx;
}
bool find(int x)
{
    for (int i = h[x]; ~i; i = ne[i])
    {
        int j = e[i];
        if (!vis[j])
        {
            vis[j] = 1;
            if (match[j] == 0 || find(match[j]))
            {
                match[j] = x;
                return true;
            }
        }
    }
    
    return false;
}
void solve()
{
    int n, m; rd(n, m);
    memset(h, -1, sizeof h);
    fr (i, 1, n)
    {
        int k; rd(k);
        while (k--)
        {
            int v; rd(v);
            add(i, v);
        }
    }

    int res = 0;
    fr (i, 1, n)
    {
        vis.reset();
        if (find(i)) ++res;
    }
    cout << m - res << endl;
}
int main()
{
    // IOS
    { solve(); }
    return 0;
}
```
H : 完全背包
队友的记忆化搜索做法
```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
#define fr(qa, ws, ed) for (int qa = ws; qa <= ed; ++qa)

const int N = 1e5 + 100;
int w[55];
int f[N], a[N];
queue<int>que;
int main()
{
    IOS
    int n, m; cin >> n >> m;
    fr (i, 1, m) cin >> w[i], que.push(w[i]), f[w[i]] = w[i];
    int mx = -1;
    fr (i, 1, n) cin >> a[i], mx = max(a[i], mx);
    mx += 100;
    while (!que.empty())
    {
        int t = que.front(); que.pop();
        fr (i, 1, m)
        {
            int v = t + w[i];
            if (v > mx || f[v]) continue;
            que.push(v);
            f[v] = v;
        }
    }
    while (mx--){ if (!f[mx]) f[mx] = f[mx+1]; }

    fr (i, 1, n) cout << f[a[i]] - a[i] << endl;
    return 0;
}
```
普通完全背做法
```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
#define fr(qa, ws, ed) for (int qa = ws; qa <= ed; ++qa)

const int N = 1e5 + 100;

int w[55];
int f[N];

int main()
{
    IOS
    int n, m; cin >> n >> m;
    fr (i, 1, m) cin >> w[i];
    
    fr (i, 1, m)
        fr (j, w[i], N) f[j] = max(f[j], f[j-w[i]]+w[i]);
    while (n--)
    {
        int k; cin >> k;
        fr (i, f[k], f[k] + 100) if (f[i] >= k)
        {
            cout << f[i] - k << endl;
            break;
        }
    }
    return 0;
}
```
