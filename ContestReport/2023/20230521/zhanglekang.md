**组队赛第三周**

- lx
- wck
- zlk

**比赛地址**
https://vjudge.csgrandeur.cn/contest/558996#overview

## 比赛总结
这次的组队赛感觉大部分题读懂题都可做， 真是省赛之前找自信了
## 题解
A. 根据题面模拟
```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);

void solve()
{
    int h, k, v, s; cin >> h >> k >> v >> s;
    int res = 0;
    while (h > 0)
    {
        v += s, v -= max(1, v / 10);
        if (v >= k) h++;
        if (v > 0 && v < k)
        {
            --h;
            if (h == 0) v = 0;
        }
        if (v <= 0)
        {
            h = 0, v = 0;
            break;
        }
        res += v;
        if (s > 0) --s;
    }

    cout << res << endl;
}
int main()
{
    IOS
    { solve(); }
    return 0;
}
```
B.根据题目所描述, 枚举开始的起点和固定的长度即可
```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);

using ll = long long;
void solve()
{
    int n, l, r; cin >> n >> l >> r;
    vector<ll> a(n+1, 0);
    int mx = -1, mi = INT32_MAX;
    for (int i = 1; i <= n; ++i) cin >> a[i], a[i] += a[i-1];
    
    for ( ;l <= r; ++l)
    {
        for (int i = 1; i <= l; ++i)
        {
            int cnt = (a[i] > 0), j;
            for (j = i + l; j < n; j += l)
                cnt += (a[j] - a[j - l] > 0);
            cnt += (a[n] - a[j - l] > 0);
            mx = max(mx, cnt), mi = min(mi, cnt);
        }
    }

    cout << mi << " " << mx << endl;
}
int main()
{
    IOS
    { solve(); }
    return 0;
}
```
D. 从需要检查的当天倒序模拟, 把可清理的量压入优先队列, 优先用最大贡献去清理垃圾, 再从清理到的那天依次倒序清理
```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);

using pii = pair<int, int>;
void solve()
{
    int n, d; cin >> n >> d;
    vector<pii> a(n);
    for (auto &i : a) cin >> i.first >> i.second;

    int ans = 0, r;
    auto itl = a.begin();
    while (d--)
    {
        cin >> r; --r;
        int res = 0;
        priority_queue<int> que;
        for (auto itr = a.begin() + r; itr >= itl; itr = prev(itr))
        {
            que.push(itr->second);
            itr->first -= res;
            while (!que.empty() && itr->first > 0)
            {
                itr->first -= que.top(); que.pop();
                ++ans;
            }
            if (itr->first > 0) return void(cout << -1 << endl);
            res = -itr->first;
        }
        itl = a.begin() + r;
    }

    cout << ans << endl;
}
int main()
{
    IOS
    { solve(); }
    return 0;
}
```
E. 无论多少组字符串中, 出现的字符是固定的, 最多出现 18 个小写字母, 考虑枚举每个出现的字符, dfs去搜索可行性答案, 注意去掉不能同时出现在同一集合的字符
最多存在 18 种不同字符, 每种字符尝试放入 3 种集合, 最坏的时间复杂度远小于 3^18, 可过 
```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(fasle), cin.tie(0), cout.tie(0);

bool vis[30][30], st[30], lp = 0;
vector<int> a;
string ans[26];
unordered_map<char, bool> _mp;
bool check(string &s, int ch)
{
    for (auto &c : s) if (vis[c - 'a'][ch]) return 0;
    return 1;
}
int dfs(int i, int n)
{
    if (i >= n) return lp = 1;
    for (int j = 0; j < 3; ++j)
    {
        if (ans[j].size() < 6 && check(ans[j], a[i]))
        {
            ans[j].push_back(a[i] + 'a');
            dfs(i + 1, n);
            if (lp) return 0;
            ans[j].pop_back();
        }
    }
    return 0;
}
void solve()
{
    int t; cin >> t;
    while (t--)
    {
        string s; cin >> s;
        if (s[0] == s[1] || s[1] == s[2] || s[0] == s[2]) return void(cout << "0\n");
        for (auto &ch : s)
        {
            if (_mp.count(ch) == 0) _mp[ch] = 1, a.push_back(ch - 'a');
            for (auto &crh : s)
                vis[ch - 'a'][crh - 'a'] = 1;
        }
    }
    int n = a.size();
    if (n > 18) return void(cout << "0\n");
    dfs(0, n);
    if (!lp) return void(cout << "0\n");
    auto f = [](int i){
        cout << ans[i];
        int k = 6 - ans[i].size();
        for (char crh = 'a'; k && crh <= 'z'; ++crh)
            if (_mp.count(crh) == 0)
                cout << crh, --k, _mp[crh] = 1;
        cout << " \n"[i == 2];
    };
    f(0), f(1), f(2);
}
int main()
{
    solve();
    return 0;
}
```
F. 签到题
```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);

void solve()
{
    int n, m; cin >> n >> m;
    bool lp = (m + n / 2) / n % 2;
    cout <<  (lp ? "down" : "up") << endl;
}
int main()
{
    IOS
    { solve(); }
    return 0;
}
```
G.签到
```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);

void solve()
{
    int n, q; cin >> n >> q;
    unordered_map<string, int> Hash;
    string s, a;
    for (int i = 1; i <= n; ++i) cin >> s, Hash[s] = i;
    while (q--)
    {
        cin >> s >> a;
        cout << max(0, abs(Hash[a] - Hash[s]) - 1) << endl;
    }
}
int main()
{
    IOS
    { solve(); }
    return 0;
}
```
H. 感觉是扩展欧几里得, 但数据小得可怜, 直接模拟就可
```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);

void solve()
{
    int a, b, c, d, cnt = 0; cin >> a >> b >> c >> d;
    while (1)
    {
        if (a % b == 0) a = 0;
        if (c % d == 0) c = 0;
        if (a == c) break;
        ++cnt, ++a, ++c;
    }

    cout << cnt << endl;
}
int main()
{
    IOS
    { solve(); }
    return 0;
}
```
