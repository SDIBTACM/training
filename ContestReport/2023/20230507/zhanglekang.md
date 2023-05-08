**组队赛第一周**

- lx
- wck
- zlk

**比赛地址**

https://vjudge.net/contest/556687#overview

## 比赛总结
好久没打组队赛了, 感觉现在交流有点费劲, 总是听不懂别人说啥, 不过配合还可, 每个人都有自己的事做
比赛一开始就开错了题, 对着最难的 a 题一做就 2 小时, 真不知道说啥了, 感觉读题方面也比较慢, 毕竟只有一个人读题, 要是能会英语就好了,还能替我可怜的队友分担一下
这次比赛只做了两道签道题 e,f 和一道模拟题 c, b 读错题了, d不理解题面, 剩下的 g, h 都没有时间看, 浪费了太多的时间在 a
## 题解
B：
有一个长度为n的B进制数，问你能否减小某一位上的数，使得其可以整除B+1，输出修改的位置和修改后的数，如果不能满足条件输出−1,−1，不用修改则输出0,0
对任意一个数，x ^ n % (x + 1)的值只为1和x，其中当n为奇数时为x，否则为1，利用这个性质可以简单的写出代码，注意遍历顺序和位数就好。
```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
#define fr(qa, ws, ed) for (int qa = (ws); qa <= (ed); ++qa)

const int N = 2e5 + 10;

int a[N];
void solve()
{
    int b, l; cin >> b >> l;
    int res = 0;
    fr (i ,1, l) 
    {
        cin >> a[i];
        res = (res * b % (b + 1) + a[i]) % (b + 1);
    }
    if (!res) return void(cout << "0 0\n");
    for (int i = 1, j = l; i <= l; ++i, --j)
    {
        if (j & 1) 
        {
            if (a[i] >= res) return void(cout << i << " " << a[i] - res << endl);    
        }
        else if (a[i] >= b + 1 - res)
            return void(cout << i << " " << a[i] - (b + 1 - res) << endl);
    }
    cout << "-1 -1\n";
}
int main()
{
    IOS
    { solve(); }
    return 0;
}
```
C :有一个电梯，每次同时只能向一边运行，但可以载无数个人。从左到右运行一次要用 10 分钟，只要电梯上有人，就会持续运行，后到达且要去的方向不同的人只能等着。给出每个人到达的时刻和要去的方向，求电梯停下的时刻。
 设当前运行完成时刻和等待时刻。对于每一个到来的人，如果要去的方向与当前相同，时刻大于当前时刻且反方向有人等，方向改变；否则运行方向不变。在当前基础上加十分钟。反之亦然。模拟即可
 ```
 #include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
#define fr(qa, ws, ed) for (int qa = (ws); qa <= (ed); ++qa)

const int N = 1e5 + 10;

queue<int>que, que2;

void solve()
{
    int n; cin >> n;
    fr (i, 1, n)
    {
        int t, d; cin >> t >> d;
        if (d) que2.push(t);
        else que.push(t);
    }
    int ans = 0;
    while (!que.empty() && !que2.empty())
    {
        ans += 10;
        if (que.front() < que2.front())
        {
            while (!que.empty() && que.front() < ans)
            {
                ans = max(ans, que.front() + 10);
                que.pop();
            }
        }
        else
        {
            while (!que2.empty() && que2.front() < ans)
            {
                ans = max(ans, que2.front() + 10);
                que2.pop();
            }
        }
    }
    ans += 10;
    while (!que.empty()) ans = max(ans, que.front() + 10), que.pop();
    while (!que2.empty()) ans = max(ans, que2.front() + 10), que2.pop();

    cout << ans << endl;
}
int main()
{
    IOS
    { solve(); }
    return 0;
}
 ```
 by Lx
 ```
 
 ```
D:有两种练习A和B，B表示做完后继续做一下个，A表示做完后可以选择继续做下一个或者跳过下一个直接做下下个，现在让你构造一个练习的序列，序列的最后以B结尾，使得做练习的所有情况刚好为n
题解：因为B不会产生任何多余的贡献，贡献只和A有关
发现一串a加一个B的方案数就是斐波那契数列(6)用一个b还是多个b隔开，效果一样，又因为题目要保证字典序最小，所以一个b就行了
然后就是多个"一串a加一个b""的组合了，方案数为累乘, 先预处理斐波那契数列，在 dfs 寻找方案就可
```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
#define fr(qa, ws, ed) for (int qa = (ws); qa <= (ed); ++qa)

const int N = 2e5 + 10;
using ll = long long;
template<typename T> inline void ps(T *x){while(*x) putchar(*(x++));}

ll f[100];
vector<ll>res, ans;
bool lp = 0;

int init()
{
    f[1] = f[2] = 1;
    int i = 3;
    for (; i < 100; ++i)
    {
        f[i] = f[i-1] + f[i-2];
        if (f[i] > 1e15) break;
    }
    return i;
}
void dfs(int i, ll n)
{
    if (lp) return ;
    if (n == 1)
    {
        lp = 1;
        ans = res;
        return ;
    }
    for (int j = i; j > 2; --j)
    {
        if (lp) return ;
        if (n % f[j] == 0)
        {
            res.push_back(j-2);
            dfs(j, n / f[j]);
            res.pop_back();
        }
    }
}
void solve()
{
    int i = init();
    ll n; scanf("%lld", &n);
    dfs(i, n);
    if (!lp) ps("IMPOSSIBLE\n");
    else
    {
        for (auto &j : ans)
        {
            fr (i, 1, j) ps("A");
            ps("B");
        }
    }
}
int main()
{
    // IOS
    { solve(); }
    return 0;
}
```
E:保证现位置的颜色和排好序后本位置的颜色一致即可
```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
#define fr(qa, ws, ed) for (int qa = (ws); qa <= (ed); ++qa)

const int N = 1e5 + 10;

int a[N], mp[N];
void solve()
{
    int n, k; cin >> n >> k;
    fr (i, 1, n)
    {
        int x, y; cin >> x >> y;
        a[i] = x, mp[x] = y;
    }
    fr (i, 1, n)
        if (mp[i] != mp[a[i]]) return void(cout << "N\n");
    cout << "Y\n";
}
int main()
{
    IOS
    { solve(); }
    return 0;
}
```
F:找差值
```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
#define fr(qa, ws, ed) for (int qa = (ws); qa <= (ed); ++qa)

const int N = 1e5 + 10;

void solve()
{
    int t, d, m; cin >> t >> d >> m;
    int st = 0;
    while (m--)
    {
        int x; cin >> x;
        if (x - st >= t) return void(cout << "Y\n");
        st = x;
    }
    if (d - st >= t) cout << "Y\n";
    else cout << "N\n";
}
int main()
{
    IOS
    { solve(); }
    return 0;
}
```
G:题面太长了有用的话就一句, 就是要选择新的国王，选举策略是，优先选孩子，如果没孩子，就选最近的兄弟，然后再看兄弟的儿子
将所以询问离线建树, 然后dfs遍历一遍数存入队列保证顺序即可
```
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
#define fr(qa, ws, ed) for (int qa = (ws); qa <= (ed); ++qa)

const int N = 1e5 + 10;

vector<int> v[N], g;
int vis[N];
int idx = 1;
queue<int>que;

void dfs(int u)
{
    for (auto &i : v[u])
    {
        que.push(i);
        dfs(i);
    }
}
void solve()
{
    int q; cin >> q;
    fr (i, 1, q)
    {
        int t, x; cin >> t >> x;
        if (t == 1) v[x].push_back(++idx);
        else g.push_back(x);
    }
    que.push(1);
    dfs(1);
    int now = 1;
    for (auto &i : g)
    {
        vis[i] = 1;
        if (vis[now])
        {
            while (!que.empty() && vis[que.front()]) que.pop();
            now = que.front();
        }
        cout << now << endl;
    }
}
int main()
{
    IOS
    { solve(); }
    return 0;
}
```
