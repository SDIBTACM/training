# 周报
## 本周
- 组队打了一场去年的CCPC深圳邀请赛，只A了两道题，真的难
- 周六蓝桥杯省赛
- 周天组队赛，一共8道题，最后两个小时没A，倒是被double精度整无语了。
## 下周计划
- 打几场组队赛
## 感悟
- nginx，前两周立的flag现在还没开始呢
- 嘿早起就是好啊，晚上想熬夜了都坚持不下去。
## [组队赛链接：华中农业大学第十三届程序设计竞赛](https://ac.nowcoder.com/acm/contest/78309)
### A、C、G、L题略了
### J题
#### [题意: 自己看吧](https://ac.nowcoder.com/acm/contest/78309/J)
#### 思路
- 判断所有 k 的可能情况，取最大值。
- 对山体高度排序，然后外层循环自上而下遍历 k 的高度，内层模拟算出答案。k 每次下降 1 高度，有些山就减小 1 点权值，累加"有些山"即可。
- 时间复杂度 $O(n)$
#### 代码
```cpp
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

void solve()
{
    ll n; cin >> n;
    vector<PLL> a(n);
    for (PLL& x : a) cin >> x.first;
    for (PLL& x : a) cin >> x.second;
    sort(a.begin(), a.end(), greater<PLL>());
    
    ll ans = 0, cnt = 0, it = 0;
    for (ll k = a[0].first + 1; k >= 0; --k)
    {
        while (it < n && a[it].first > k) cnt += a[it++].second;
        cnt -= it;
        ans = max(ans, cnt);
    }

    cout << ans << endl;
}

signed main()
{
    IOS;
    // int _; cin >> _; while (_--)
        solve();

    return 0;
}
```
### I题
#### 题意：
- 给 n 个点，求这 n 个点能构成的直线数量，如果有重复的点输出"inf".
- 数据范围： $1 \leq n \leq 10^{3}$ , $x_{i}$ , $y_{i}$ 整数且区间范围 $[-10^{9}, 10^{9}]$
#### 思路：
- 先特判重复点, 有则结束
- 对每对点的直线信息都暴力一遍找一遍，存进 $set$ 里，自动过滤重复的直线， $size$ 即答案.
- 记录直线的唯一信息：用double类型存斜率和截距容易爆精度，不过根据LCY的邪门做法，所有的点都加上1e9之后，精度就过了，我和ZLK不懂但大受震撼。
- 以下正解：
- 方向向量替代斜率：存一个pair，两点之间的 x、y 轴之差。替代截距：代入一个点与方向向量，解出的是关于截距的倍数。
- 然后对上面的三个数取掉公因数以保证唯一性，即唯一的方向向量与截距倍数，以替代斜率与截距。
#### 代码
```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
using ull = unsigned long long;
using PLL = pair<ll, ll>;
#define endl '\n'
#define mem(a, b) memset(a, b, sizeof(a));
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
const int N = 1e5 + 10;
//////////////////////////////////////////////////////////////

inline pair<PLL, ll> work(PLL a, PLL b)
{
    ll dx = a.first - b.first, dy = a.second - b.second;
    ll cp = a.second * dx - a.first * dy;
    ll g = __gcd(__gcd(dx, dy), cp);
    return {{dx / g, dy / g}, cp / g};
}

void solve()
{
    ll n; cin >> n;
    set<PLL> st;
    for (int i = 0; i < n; ++i)
    {
        ll x, y; cin >> x >> y;
        st.insert({x, y});
    }
    if (st.size() != n) return cout << "inf" << endl, void();
    set<pair<PLL, ll>> ans;
    for (auto a : st)
        for (auto b : st) if (a != b)
            ans.insert(work(a, b));

    cout << ans.size() << endl;
}

signed main()
{
    IOS;
    // int _; cin >> _; while (_--)
        solve();

    return 0;
}
```
### H题
#### [题意: 自己看吧](https://ac.nowcoder.com/acm/contest/78309/H)
#### 思路：
- 外层二分寻找最小能量，内层最短路找当前能量下，最小耗费体力。
- 整体时间复杂度 $O(n\log_{}{n}\log_{}{n})$
#### 代码
```cpp
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
ll n, m, s, t, c;
ll v[N];
vector<PLL> a[N];
bool vis[N];
int dist[N];


inline bool check(const ll k)
{
    mem(vis, 0);
    mem(dist, 0x3f);
    priority_queue<PLL, vector<PLL>, greater<PLL>> q;
    q.push({v[s], s});
    dist[s] = v[s];

    while (q.size())
    {
        auto u = q.top().second; q.pop();
        if (vis[u]) continue;
        vis[u] = 1;
        for (auto [d, w] : a[u])
        {
            if (!vis[d] && w <= k && dist[d] > dist[u] + v[d])
            {
                dist[d] = dist[u] + v[d];
                q.push({dist[d], d});
            }
        }
    }
    return dist[t] <= c; 
}

void solve()
{
    cin >> n >> m >> s >> t >> c;
    for (int i = 1; i <= n; ++i) cin >> v[i];
    while (m--)
    {
        ll x, y, w; cin >> x >> y >> w;
        a[x].push_back({y, w});
    }
    ll l = 0, r = 1e9, ans = 1e9;
    while (l <= r)
    {
        ll mid = l + r >> 1;
        if (check(mid)) ans = mid, r = mid - 1;
        else l = mid + 1;
    }
    cout << ans << endl;
}

signed main()
{
    IOS;
    // int _; cin >> _; while (_--)
        solve();

    return 0;
}
```
### D题
#### 题意：
- 给定一个正整数序列 $a_{i}$ 。
- 设 $R_{i}$ 表示 $a_{max(1, i-ra+1)},...,a_{i}$ 中的最大的 $min(i, rb)$ 个数之和。
- 设 $T_{i}$ 表示 $a_{1)},...,a_{i}$ 中的最大的 $min(i, b)$ 个数之和。
- 对于 $1 \leq i \leq n$ , 求 $R_{i} + T_{i}$ 的最大值。
- 数据范围： $1 \leq rb \leq ra \leq n \leq 2e^{6}$ , $1 \leq b \leq n \leq 2e^{5}$ , $0 \leq a_{i} \leq 10^{9}$
#### 思路
- 大体思路上对 $R_{i}$ 与 $T_{i}$ ，分别维护，从左到右求出所有结果。
- T 维护比较简单：遍历 $a_{i}$ 时，用容器把每个数都放进去，然后维护容器大小，溢出时就删除最小值。期间记录答案即可。
- 以下是 R 的维护：
- 遍历 $a_{i}$ 时，用一个容器A维护选中的值；再用一个容器B维护没选中但是在可选区间的值。
- 对于每个 $a_{i}$ , 判断可不可以放入A，不可以的话就放入B，同时维护A的容器大小。
- 对于过期时间线上的值D：判断在不在容器A里面，在的话就拿出过期的D，此时容器A大小有缺，用容器B取最大值维护A。期间记录答案即可。
- 时间复杂度: $O(n\log_{}{n})$
#### 代码
```cpp
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

void solve()
{
    ll n; cin >> n;
    ll b; cin >> b;
    ll ra, rb; cin >> ra >> rb;
    vector<ll> a(n);
    for (ll& x: a) cin >> x; 
    
    ll sum = 0;
    vector<ll> T;
    multiset<ll> st;
    for (ll u : a)
    {
        sum += u;
        st.insert(u);
        while (st.size() > b)
        {
            ll x = *st.begin();
            st.erase(st.begin());
            sum -= x;
        }
        T.push_back(sum);
    }

    sum = 0;
    vector<ll> R;
    set<PLL> vis;
    priority_queue<PLL> q;
    for (int i = 0, j = -ra; i < a.size(); ++i, ++j)
    {
        sum += a[i];
        vis.insert({a[i], i});

        while (vis.size() > rb)
        {
            PLL x = *vis.begin();
            q.push(x);
            vis.erase(x);
            sum -= x.first;
        }

        if (j >= 0)
        {
            if (vis.count({a[j], j}))
            {
                vis.erase({a[j], j});
                sum -= a[j];
            }
            while (vis.size() < rb)
            {
                while (q.size() && q.top().second <= j) q.pop();
                PLL x = q.top(); q.pop();
                vis.insert(x);
                sum += x.first;
            }
        }
        R.push_back(sum);
    }
    ll ans = 0;
    for (int i = 0; i < a.size(); ++i) ans = max(ans, R[i] + T[i]);
    cout << ans << endl;

}

signed main()
{
    IOS;
    // int _; cin >> _; while (_--)
        solve();

    return 0;
}
```cpp
