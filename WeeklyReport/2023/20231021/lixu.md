# 周报感想
- 这周训练也稀稀松松，除了日常水个每日一题, 每周个人训练赛之外，打了一次牛客小白，以在第四题莫名其妙的数据上卡住告终。然后周日晚上和cwl，lzx打了一场组队赛，2020年的南京区域站，结果只A了一题，，惨
- 组队赛的第二题在思维的方向上错了，虽然换了几次但是全错了。第三题是概率求期望，一开始把价值算错了（固定代价还需要算入每次的成本代价和概率做运算）于是样例就没看懂，后面也没想出来。
- 思维题有待加强，概率期望的话，应该有规律，可以看看。

## [补个第七题的题解（连着三周都是补第七题...）](https://vjudge.net/contest/589155)
### 题意
- 一共 n 个票，每个票的信息有 得票人 和 买断这张票的代价
- 有些票是投给自己的，对于那些投给别人的票，你可以选择买断它
- 让自己票数第一（不能和其他人并列），求需要的最小代价。
- 数据范围：票数最多 100，价值最大 1000
### 思路
- 本来想的是 二分 + 贪心 ，外层枚举自己和其他人票数的分界线（让自己票数大于等于v，其他人票数小于v），内层贪心求出花费的最小价值。但是答案一直错，看了下题解是 枚举 + 贪心 ，思路差不多，但是外层遍历（数据比较小，可以不用二分）
- 详细：
- 对票排个序，价值大的优先。
- 贪心方面：
- 首先选择 all in，所有的票全买走
- 然后卖票，优先卖出价值高的
- 同时保证其他人的票数要低于贪心的目标答案 v，自己的票数要大于等于 v
- 返回剩余的代价

- 后来想了一下为什么外层二分不对呢。本来自觉票数越少，代价越小。但是这不对，自己票数变少的话，对于降低的分界线，需要多买入票数高的其他人A，这些票的代价会高于卖给其他人B的代价。于是不成立

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
///////////////////////////////////////////////////////////////

bool cmp(PLL a, PLL b) {
    return a.second > b.second;
}

ll check(ll v, vector<PLL>& a, ll sum, ll p)
{
    vector<ll> siz(p + 4);
    for (auto& x : a) if (p - 1 >= v && siz[x.first] + 1 < v)
    {
        --p;
        ++siz[x.first];
        sum -= x.second;
    }
    if (p == v) return sum;
    return -1;
}

void solve()
{
    ll n; cin >> n;
    vector<PLL> u(n - 1);
    for (auto& x : u) cin >> x.first;
    for (auto& x : u) cin >> x.second;

    vector<PLL> a;
    for (auto& x : u) if (x.first != 1) a.push_back(x);
    sort(a.begin(), a.end(), cmp);

    ll sum = 0, p = n - 1 - a.size();
    for (auto& x : a) sum += x.second;

    ll ans = sum;
    while (p <= n - 1)
    {
        ll w = check(p, a, sum, n - 1);
        if (w != -1) ans = min(ans, w);
        ++p;
    }
    cout << ans << endl;
}

int main()
{
    IOS;
    ll _; cin >> _; while (_--)
        solve();

    return 0;
}
```
