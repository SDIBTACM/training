# 周报感想
- 这周的个人赛找的09年浙江省赛打的，以后也想用省赛当个人赛比赛用，反正那么多省份，也举办了好多年。
- 比赛时做了六道题，第七道wa了十发，补题时发现思路就不对，AC时和我比赛写的代码就没什么联系了。也是头一次做这么多道题，这套简单题有点多。
- 还是得再练dp，这次第七题dp就一直wa，前几周网络赛的dp也没写出来。
- 国庆后半场一不小心没坚持住，学习停滞了。现在进度上一半操作系统、一半java，继续补吧。

## [补个第七题的题解吧](https://vjudge.net/contest/586144#problem/J)
- 题意： 一个人要砍 $m$ 天树，一共 $n$ 个树， $m\leq n$ 。每天只能砍一棵树，活着的树每天都会长大，树的初始大小和成长速度给你，求砍最大的质量。
- 思路：  
- 首先假设已经选出来要砍哪 $m$ 课树了：为了保证最后质量最大，这 $m$ 个树按照成长速度小到大排序，先砍成长慢的，最后砍成长快的。  
- 接下来就是选出这 $m$ 课树：先把成长速度最小的 $m$ 个树当作原始数据，后面按照成长速度  依次添加一个新的候选树，然后对这 $m+1$ 个树都假设单独删掉，并求出剩下 $m$ 个树的质量，保留最大的哪个选项并删掉那课树。ps: 按照成长速度添加树，后续操作时就不用排序了，可以省掉 $nlogn$ 倍的时间
- 虽然蛮暴力的，好在数据范围只有 $0 < m \leq n \leq 250$

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

inline ll check(vector<PLL>& a)
{
    ll ans = 0, p = 0;
    for (PLL x : a) if (x.first != -1)
        ans += x.second + x.first * p++;
    return ans;
}

inline void work(vector<PLL>& a)
{
    ll ans = 0, it = 0;
    for (int i = 0; i < a.size(); ++i)
    {
        ll x = a[i].first;
        a[i].first = -1;
        ll cnt = check(a);
        a[i].first = x;
        if (cnt > ans) ans = cnt, it = i;
    }
    a.erase(a.begin() + it);
}

void solve()
{
    ll n, m; cin >> n >> m;
    vector<PLL> r(n);
    for (PLL& x : r) cin >> x.second;
    for (PLL& x : r) cin >> x.first;
    sort(r.begin(), r.end());
    vector<PLL> a;
    for (int i = 0; i < m; ++i) a.push_back(r[i]);
    for (int i = m; i < n; ++i)
    {
        a.push_back(r[i]);
        work(a);
    }
    cout << check(a) << endl;
}

signed main()
{
    IOS;
    ll _; cin >> _; while (_--)
        solve();
    return 0;
}
```
