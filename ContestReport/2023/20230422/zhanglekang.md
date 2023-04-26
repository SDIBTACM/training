# 比赛总结

---

在读题和debug上浪费了太多的时间 总是不能注意到一些题目描述的细节 写的代码也总是莫名奇妙的wa debug的时间比想题的时间长就很烦

### 比赛地址：[sdtbu选拔赛5 - Virtual Judge (vjudge.net)](https://vjudge.net/contest/554049)

### 题解：

##### A

思路：
一开始一直看不懂题 真心感觉这题描述真的好难理解 读懂了题就是签到了 统计字符串中有几段连读且个数大于等于2的只含a的子串

代码：

```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0); //勿混用
#define fr(qa, ws, ed) for (int qa = (ws); qa <= (ed); ++qa)

const int N = 1e5 + 10, M = 29;
int a[N];
void solve()
{
    int n; cin >> n;
    string s; cin >> s; s = " " + s;

    fr (i, 1, n) if (s[i] == 'a') a[i] = a[i-1] + 1, a[i-1] = 0;
    int ans = 0;
    fr (i, 1, n) if (a[i] >= 2) ans += a[i];

    cout << ans << endl;
}
signed main()
{
    IOS
    {solve();}
    return 0;
}

```

##### B

思路：

这题感觉难在建图 图建好了就是一道很基础的求最大连通块问题 把切割线的两边都填充格子 这样n*n的切割图可建成2n * 2n 的图 然后再去统计行列为奇数的格子

代码：

```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
#define fr(qa, ws, ed) for (int qa = (ws); qa <= (ed); ++qa)

const int N = 2e3 + 10;
int d[][2] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
int st[N][N];
int mx, my, mix, miy;
using pii = pair<int, int>;

inline bool check(int x, int y){ return (x < mix || y < miy || x > mx || y > my); }
int bfs(int x, int y)
{
    queue<pii>que;
    int res = 0;
    que.push({x, y});
    st[x][y] = 1;
    if (x%2 && y%2) ++res;
    while (!que.empty())
    {
        auto t = que.front(); que.pop();
        fr (i, 0, 3)
        {
            int dx = t.first + d[i][0], dy = t.second + d[i][1];
            if (!check(dx, dy))
            {   
                if (st[dx][dy] == -1) return INT32_MIN;
                if (st[dx][dy]) continue;
                que.push({dx, dy});
                st[dx][dy] = 1;
                if (dx%2 && dy%2) ++res;
            }
        }
    }
    return res;
}
void solve()
{
    int n; cin >> n;
    int x, y, lx, ly;
    cin >> lx >> ly;

    lx <<= 1, ly <<= 1;
    fr (i, 1, n)
    {
        cin >> x >> y;
        x <<= 1, y <<= 1;
        if (x == lx)
        {
            int l = min(y, ly), r = max(y, ly);
            fr (j, l, r) st[x][j] = 1;
        }
        else 
        {
            int l = min(x, lx), r = max(x, lx);
            fr (j, l, r) st[j][y] = 1;
        }
        lx = x, ly = y;
    }
    mix = miy = 0, my = mx = 2000;
    fr (i, mix, mx)
    {
        if (!st[i][miy]) st[i][miy] = -1;
        if (!st[i][my]) st[i][my] = -1;
    }
    fr (i, miy, my)
    {
        if (!st[mix][i]) st[mix][i] = -1;
        if (!st[mx][i]) st[mx][i] = -1;
    }
    int ans = 0;
    fr (i, mix, mx) fr (j, miy, my)
        if (st[i][j] == 0) ans = max(ans, bfs(i, j));

    cout << ans << endl;
}
signed main()
{
    IOS
    {solve();}
    return 0;
}
```

##### C

思路：
设 k = 2^n, a = (1, 1) -> (n, n) 的最小步数 = n, b = (1, 1) -> (x, y)
从 (k/2，k/2) 到(x, y) 的最小步数即从(k, k) 到(x, y)的最小步数减一
则(k/2, k/2) - > (x, y) = (k, k) -> (x,y) - 1 = n - b - 1

代码：

```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0); //勿混用
#define fr(qa, ws, ed) for (int qa = (ws); qa <= (ed); ++qa)

const int N = 1e5 + 10, M = 29;
void solve()
{
    int n, x, y; cin >> n >> x >> y;
    int nn = 1 << n;

    int st = nn >> 1, res = 0, ans = 0;
    while (x%2 == 0) x >>= 1, ++res;
    ans = n - res - 1, res = 0;
    while (y%2 == 0) y >>= 1, ++res;
    ans = min(ans, n - res - 1);
    
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
统计 h 的出现次数, 遍历到 h 时, 若 h 出现过, 则 h 出现次数-1表示一个高度为 h 的气球被打破, 否则满足条件的答案个数+1, 然后将 ai - 1 次数+1, 表示默认将 h - 1 的气球打破(不管是否存在), 这样就可不重不漏的统计满足 h(i-1) = h(i) + 1 = h(i+1) + 2 + .... 的序列个数

代码：

```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0); //勿混用
#define deBug
#define fr(qa, ws, ed) for (int qa = (ws); qa <= (ed); ++qa)

const int N = 1e5 + 10, M = 29;
void solve()
{
    int n; cin >> n;
    vector<int>a(n);
    for (auto &i : a) cin >> i;
    
    unordered_map<int, int> mp;
    int ans = 0;
    for (auto &i : a)
    {
        if (!mp[i]) ++ans;
        else mp[i]--;
        mp[i-1]++;
    }

    cout << ans << endl;
}
signed main()
{
    IOS
    {solve();}
    return 0;
}

```

##### E

思路：

遍历每个字符串的每一个字符，如果当前字符是*，则枚举 * 变成 a~z 的每一种情况，并把这个新的字符串放到map中统计出现次数, 输出次数出现最多的且字典序最小字符串即可
当时写的时候没注意题目中字符串的长度, 故把每个字符串又哈希了一下放入map, 在用另一个 map 记录哈希值对应的字符串, 这样做还需要另外考虑次数出现最多的字符串的字典序, 属实麻烦了


代码：

```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0); //勿混用
#define fr(qa, ws, ed) for (int qa = (ws); qa <= (ed); ++qa)

const int N = 5e5 + 10, M = 29;
using ull = unsigned long long;
ull Hash(const string &s)
{
    ull p = 1, hash = 0;
    for (int i = 0; i < s.size(); ++i)
    {
        hash = hash * M + s[i];
        p = p * M;
    }
    return hash;
}
void solve()
{
    int k, n; cin >> k >> n;
    vector<string> ve(k);
    unordered_map<ull, int> mp;
    unordered_map<ull, string> _mp;
    for (auto &s : ve) cin >> s;
    for (auto &s : ve)
    {
        int p = -1;
        fr (i, 0, n-1) if (s[i] == '*') { p = i; break; }
        if (p == -1)
        {
            ull pp = Hash(s);
            ++mp[pp];
            if (!_mp.count(pp) || s < _mp[pp]) _mp[pp] = s;
            continue;
        }
        fr (i, 0, 25)
        {
            string t = s;
            t[p] = 'a' + i;
            ull pp = Hash(t);
            ++mp[pp];
            if (!_mp.count(pp) || t < _mp[pp]) _mp[pp] = t;
        }
    }
    int mx = -1;
    string ans;
    for (auto &it : mp)
        if (mx < it.second) mx = it.second, ans = _mp[it.first];
        else if (mx == it.second && ans > _mp[it.first]) ans = _mp[it.first];
        
    cout << ans << " " << mx << endl;
}
signed main()
{
    IOS
    {solve();}
    return 0;
}


```

##### F

思路：

若输入的数字中有9就输出”F“，否则输出”S“。

代码：

```
#include <bits/stdc++.h>
using namespace std;
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
#define fr(qa, ws, ed) for (int qa = (ws); qa <= (ed); ++qa)
void solve()
{
    int n, lp = 0;
    fr (i, 1, 8) cin >> n, lp |= (n == 9);
    cout << (lp ? "F\n" : "S\n");
}
signed main()
{
    IOS
    {solve();}
    return 0;
}

```

##### G

思路：

模拟题，就是题目比较难读, 细节也比较多 1 ~10 分别用1~10表示, 分数也分别为1~10, J、Q、K分别用11、12、13表示，分数都为 10。
用map统计一种牌已用了多少张, 若当前牌有剩余的则用掉并将其张数+1，若加上当前点数Mary的点数正好是23或者加上当前点数John的点数超过23了但是Mary没有超过23就满足条件，无解的话输出-1。

代码：

```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0); //勿混用
#define fr(qa, ws, ed) for (int qa = (ws); qa <= (ed); ++qa)


void solve()
{
    int n; cin >> n;
    int a, b, c, d; cin >> a >> b >> c >> d;
    int n1 = min(a, 10) + min(b, 10);
    int n2 = min(c, 10) + min(d, 10);
    int mp[20] = {0};
    mp[a]++, mp[b]++, mp[c]++, mp[d]++;
    fr (i, 1, n)
    {
        int x; cin >> x;
        n1 += min(x, 10), n2 += min(x, 10);
        mp[x]++;
    }
    int cc = 24 - n1, dd = 23 - n2;
    if (cc > 10 && dd > 10) return void(cout << "-1\n");
    while (mp[cc] >= 4) ++cc;
    while (mp[dd] >= 4) ++dd;

    if (n2 + min(10, cc) > 23 && n2 + min(10, dd) > 23) return void(cout << "-1\n");
    cout << min({10, cc, dd}) << endl;
}
signed main()
{
    IOS
    {solve();}
    return 0;
}

```
