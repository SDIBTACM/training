# 周报
### 本周
- 补题（上周的一个省赛题）的时候很魔幻的写了一个 二维vector模拟线段树（不如说是一个二叉树），代码量不长，主要是因为不想写线段树板子，但效果出奇。末尾附代码
- 2020沈阳：开四出二，第三个数学题，比赛想好久都推不出来怎么做，题解短短两行，自闭；第四题是模拟，比赛时看题面那些公式给看傻了，没读懂。
- 2020上海：开六出四，第五题是个数学题，区域赛的好多数学题啊，还都是做不出来的那种。
- 2021上海：开四出三，第四个是一个双塔DP题，不久前刚看过类似的，但是比赛遇上还是没想起来。第二个是个数学题，但是做起来不如第三个简单，数学很重要。
- 趣味编程题目弄了好久了，把数据上传到oj上，然后测试题目，再检查一下就ok了

### 代码
- vector线段树：本题是从前向后，顺序查找第一个大于等于 x 的数，有查询区间(1~root)最大值，单点修改，增加新节点操作。
~~~cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
using ull = unsigned long long;
using PLL = pair<ll, ll>;
#define endl '\n'
#define mem(a, b) memset(a, b, sizeof(a));
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
const int N = 1e6 + 5;
//////////////////////////////////////////////////////////////
vector<vector<ll>> g;	// 二维 vector 模拟线段树

ll find(ll x, ll t, ll rt)	// 查找孩子
{  
	if (t == 0) return rt;
	if (g[t - 1][rt << 1] >= x) return find(x, t - 1, rt << 1);	// 优先左孩子
	return find(x, t - 1, rt << 1 | 1);	// 其次右孩子
}

inline void addnew(ll c)	// 增加新节点
{
	g[0].push_back(c);
	ll t = 0;
	while (g[t].size() > 1)	// 整合队形
	{
		if (g.size() - 1 == t) g.push_back({ 0 });
		if (g[t].size() > g[t + 1].size() * 2) g[t + 1].push_back(0);
		++t;
	}
	update(g[0].size() - 1 >> 1, 1);// 更新父亲
}

void update(ll rt, ll t)	// 维护区间
{
	g[t][rt] = g[t - 1][rt << 1];
	if ((rt << 1 | 1) < g[t - 1].size()) g[t][rt] = max(g[t][rt], g[t - 1][rt << 1 | 1]);
	if (g[t].size() == 1) return;	// 没父亲
	update(rt >> 1, t + 1);			// 更新父亲
}

void solve()
{
	ll n, c; cin >> n >> c;
	g.clear();
	g.push_back({ c }), g.push_back({ c });

	multiset<ll> st;
	st.insert(c);
	while (n--)
	{
		ll x; cin >> x;

		if (g[g.size() - 1][0] < x) addnew(c);
		ll rt = find(x, g.size() - 1, 0);
		g[0][rt] -= x;
		update(rt >> 1, 1);

		auto p = st.lower_bound(x);
		if (p == st.end()) p = st.insert(c);
		st.insert(*p - x);
		st.erase(p);
	}
	cout << g[0].size() << " " << st.size() << endl;
}

signed main()
{
	IOS;
	ll _; cin >> _; while (_--)
		solve();

	return 0;
}
~~~
双塔dp也附上吧
~~~cpp
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
    ll n, k; cin >> n >> k;
    const ll p = 3000;
    vector<vector<ll>> f(110, vector<ll>(6000));
    for (int i = 0; i <= k; ++i)
        for (int w = p - 2700; w <= p + 2700; ++w)
            f[i][w] = -0x3f3f3f3f3f;
    f[0][p] = 0;
    vector<vector<ll>> d = f;
    while (n--)
    {
        ll v, t; cin >> v >> t;
        // 0
        for (int i = 0; i <= k; ++i)
            for (int w = p - 2600; w <= p + 2600; ++w)
                f[i][w] = max({ d[i][w], d[i][w - t] + v, d[i][w + t] + v });
        // 1
        t <<= 1;
        for (int i = 1; i <= k; ++i)
            for (int w = p - 2600; w <= p + 2600; ++w)
                f[i][w] = max({ f[i][w], d[i - 1][w - t] + v, d[i - 1][w + t] + v });
        d = f;
    }
    ll ans = 0;
    for (int i = 0; i <= k; ++i)
        ans = max(ans, f[i][p]);
    cout << ans << endl;
}
signed main()
{
    IOS;
    //ll _; cin >> _; while (_--)
    solve();
    return 0;
}
~~~
