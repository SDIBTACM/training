# 本周感想
- 首先复习了搜索树。理解的越深入一些就越觉得搜索树的代码量可能也没那么长，毕竟每个整体代码函数多了一些，查找、修改、更新、插入、删除等，但是每个函数的代码量都不多，而且函数之间还有甚多重复的部分，比如 修改、插入、删除 函数都要用到 查找，所以在写代码的时候甚至比想象中的要容易。
- 每天除了复习算法之外，当然就是做题了。做习题的时候差不多就是难度上升到 cf1800-1900分的时候速度就会明显降下来，按时间来说 做出来的时候很多时候算是补题时间了。不过cf的题目分数又是一个很迷的事情，或许是因为有不擅长或者擅长的题目类型吧。
### [一个交互题](https://codeforces.com/contest/1856/problem/D)
### 题意
- 有一个长度为 n 的排列 a，每次你可以询问区间 $[l,r]$  之间的逆序对个数，花费 $(r−l)^{2}$ 的代价，你需要在花费不超过 $5n^{2}$ 的代价下找出最大值所在位置。
### 思路
- 假设， $[l,r]$ 逆序对有 a 个， $[l+1,r]$ 的逆序对有 b 个。刚开始认定 $a[r]$ 元素最大， $l$ 从 $r-1$ 向左枚举。
- 如果 $a - b == r - l$ ( $[l+1,r]$ 的元素数量)，说明 $a[l]$ 是比 $[l+1,r]$ 之间所有元素要大的, 并更新 $r = l$。
- 否则 $a[r]$ 依旧是最大的。
- 于是有了下面这段代码
~~~c++
void solve()
{
    ll n; cin >> n;
    ll l = n - 1, r = n;
    ll q = 0;
    while (l)
    {
        cout << "? " << l << " " << r << endl; 
        ll now; cin >> now;
        if (now - q == r - l)
        {
            now = 0;
            r = l;
        }
        --l;
        q = now;
    }
    cout << "! " << r << endl; 
}
~~~
- 但是这样过不了。推测原因是因为代价太高了，题目不接受。（最坏情况的代价是 $\sum_{i=1}^{n}{n^{2}}$ ）
- 然后发现这题可以通过 二分 来降低代价：
- 每次把区间分成左右两部分
- 用左半部分区间的最大值与 右半部分区间的最大值比较，就能得出整段区间的最大值。如此递归，比较 $\log_{2}{n}$ 次就可以了。
### AC代码
~~~c++
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
using ull = unsigned long long;
using PLL = pair<ll, ll>;
#define mem(a, b) memset(a, b, sizeof(a));
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0);
const int N = 2e5 + 5;
////////////////////////////////////////////////////////////////

inline ll check(ll l, ll r) 
{
    if (l == r) return 0;
    cout << "? " << l << " " << r << endl;
    ll res; cin >> res;
    return res;
}

ll work(ll l, ll r)
{
    if (l == r) return l;
    ll mid = l + r >> 1;
    ll lm = work(l, mid);
    ll rm = work(mid + 1, r);

    if (check(lm, rm) - check(lm + 1, rm) == rm - lm) return lm;
    return rm;
}

void solve()
{
    ll n; cin >> n;
    ll m = work(1, n);
    cout << "! " << m << endl;
}

int main()
{
    IOS;
    int T; cin >> T; while (T--)
    solve();

    return 0;
}
~~~
