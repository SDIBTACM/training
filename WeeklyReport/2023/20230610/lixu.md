# 周报
## 本周学到了
关于[这个DP题](https://codeforces.com/problemset/problem/1741/E)，可以用搜索做(不过既然有边，那么应该属于图论)。
（这题在这周的每日周报我给抓上了，结果gym给这周的个人赛也抓进去了，不过昨晚我做的时候没交上，比赛交上了）
- 题意: 自己看吧
- dp原解: 
- 线性dp，从前往后跑，第 i 个可以选择包含后面或者前面的 a[i] 个数字
- 包含后面的情况下: 如果前面那个(第i-1个)为真的话，则第 i + a[i] 位置也为真(完成的区间接在一起了嘛)
- 包含前面的情况下: 如果第 i - a[i] 的左端(第i - a[i] - 1)为真的话，则第 i 位置也为真
- 最后判断第 n 个位置是否为真就可以了
- 搜索解法:
- 第 i 个可以包含两种区间，一个是 {i-a[i], i}另一个是 {i, i + a[i]}
- 把这两种区间看作两条路径，起点为区间左端，终点是区间右端
- 然后以1为起点跑，看能否达到终点n
- 时间复杂度：因为每条边最多只需要跑一次，所以时间复杂度是 O(n)，和dp原解是一样的，不过常数较大，所以速度慢了一点(46ms : 78ms)

代码如下:
~~~
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
using ull = unsigned long long;
#define mem(a, b) memset(a, b, sizeof(a));
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
const int N = 1e5 + 5;
//////////////////////////////////////////////////////////////////////////////
vector<ll> r[N << 1];
queue<ll> q;
void solve()
{
    ll n; cin >> n;
    for (int i = 1; i <= n; ++i) r[i].clear();
    for (int i = 1; i <= n; ++i)
    {
        ll x; cin >> x;
        if (i + x <= n) r[i].push_back(i + x);
        if (i - x >= 1) r[i - x].push_back(i);
    }
    while (q.size()) q.pop();
    for (int i = 0; i < r[1].size(); ++i) q.push(r[1][i]);
    r[1].clear();
    while (q.size())
    {
        ll temp = q.front(); q.pop();
        if (temp == n) return void(cout << "YES" << endl);
        ++temp;
        for (int i = 0; i < r[temp].size(); ++i) q.push(r[temp][i]);
        r[temp].clear();
    }
    cout << "NO" << endl;
}

int main()
{
    IOS;
    int T; cin >> T; while (T--)
        solve();
    return 0;
}
~~~
做了很多二分、贪心题，做贪心题时有一个清晰的思路很重要，不然总有找不到的bug。
## 下周
大一算法最后一讲数论了，我也找些题做做吧，话说数论都是一些数学知识的集合，应该有点难理解
## 感想
昨天打完蓝桥杯打的是差到没话说，wck问我一题我错一题(太贴心了)。数三角那道题虽然难写倒是写出来了，但是代码用的auto不知道能不
能编辑(好歹人均比赛报名费比icpc高不少的，编辑器用这么古老的版本真是不知道怎么吐槽了)。一个搜索题，比赛时草草看了一眼想做完简单题再写的，然
后写了一个不是很简单的题目后我竟然给忘了。   明年还来。
