# [第四次组队赛比赛报告](https://vjudge.csgrandeur.cn/contest/560008#problem)
### 成员:
- 李旭
- 王成魁
- 张乐康
### 感悟：
代码尽可能写的简短，检查、修改越容易，在不经意中出现bug的可能性也越小。代码写的美观一些还是很有用的。
## A - Broken Keyboard
### 题意:
给定一字符串，如果字符串字母的数量按照单双单双...形式排列，输出YES，otherwise输出NO
### 思路:
从前往后，每三个字母的格式为abb，判断符合与否。需要注意的是：
- a、b可以相同
- 如果只能凑够2个字符则一定不满足abb的情况
### 代码:
~~~
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
#define fr(qa, ws,ed) for (int qa = (ws); qa <= (ed); ++qa)
#define ll int
const int N = 1e3 + 10, mod = 1e9 + 7;

void solve()
{
    int n; cin >> n;
    string a; cin >> a;
    a = "1" + a;
    for(int i = 3; i < a.size(); i += 3)
    {
        if(a[i] != a[i - 1])
        {
            cout << "NO" << endl;
            return;
        }
    }
    if(n % 3 == 2) cout << "NO" << endl;
    else cout << "YES" << endl;
}

signed main()
{
    IOS
    int t;cin >> t; while(t--)
        solve();

    return 0;
}
~~~
## B - Watch the Videos
### 题意:
求看完所有n个视频的最小时间，看之前需要将视频下载到存储空间为m的磁盘中，对于第i个视频，下载需要ai个时间；对于所有视频，看视频需要1个单位的时间。在看视频的同时，可以下载其他视频。
### 思路:
- 首先，不论看视频的先后顺序如何，视频的下载时间是必须的。于是，对于固定n个视频，利用观看时间下载的次数越多，总时间越短。
- 第二，如果两个视频可以同时在磁盘中存在，即ai + aj <= m，则可以利用其中一个视频的观看时间下载视频另一个视频。
- 第三，因为每个视频只能下载、观看一次，所以可以把第二点看成拥有不同权值的点，连成的一条线。其中，线的两个端点i，j满足ai + aj <= m且每个点最多只有一个出度和入度。
而每个线段的结尾都需要1单位时间来观看视频，而不能下载。

由上述三点可以得出，n个点最后连接生成的线段数量越少，观看视频时浪费的时间越少，总时间就越短。最后求出最少线段匹配数量即可。
### 代码:
~~~
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
const int N = 5e5 + 1000;

ll a[N];

int main()
{
    IOS
    ll ans = 0, n, m; cin >> n >> m;
    ll l = 1, r = n, flog = 0;
    for (int i = 1; i <= n; ++i) cin >> a[i], ans += a[i];
    sort(a + 1, a + 1 + n);
    while (r > l)
    {
        if (a[r] + a[l] <= m)
        {
            if (flog == 0) --r;
            else ++l;
            flog ^= 1;
        }
        else --r, ++ans;
    }
    cout << ans + 1 << endl;
    return 0;
}
~~~
## C - Exchange
### 题意:
一个任务可以得到1枚金币，1枚金币可以换a枚银币，b枚银币可以换1枚金币。问最少几个任务可以凑出n枚银币。
### 思路:
如果可以从交换金币和银币中赚取差价，那么仅需要最开始的一个任务得到启动资金。否则，要老老实实地做任务并换成银币，直到所需银币，即n/a并向上取整。
### 代码:
~~~
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
const int N = 1e5 + 1000;

void solve()
{
    ll n, a, b; cin >> n >> a >> b;
    if (a > b) cout << 1 << endl;
    else cout << (n + a - 1) / a << endl;
}

int main()
{
    IOS
    int t; cin >> t; while(t--)
    solve();
    
    return 0;
}
~~~
## E - Torus Path
### 题意:
在n*n的网格中，每个格子有一定权值，你的起点在左上角，终点在右下角，你可以左右下移动，并且触碰到边缘会从对面的边缘出现，你不能在任一个格子出现两次，求最后经过的所有格子的权值总和最大。
### 思路:
不论如何移动，斜线中总要有一个格子无法到达，而其他的格子都可以到达，否则无法到达终点————求出所有格子的总和，再减去斜线格子的最小值即可。
### 代码:
~~~
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
const int N = 1e5 + 1000;

int n;
ll a[300][300];
ll ans[300];

int main()
{
    IOS
    cin >> n;
    ll sum = 0;
    for(int i = 1; i <= n; ++i)
        for(int j = 1; j <= n; ++j)
            cin >> a[i][j],sum += a[i][j];
    
    ll mi = 0x3f3f3f3f;
    for(int i = n; i>=1;--i) 
        mi = min(mi,a[i][n-i+1]);
    cout << sum - mi <<endl;    
    
    return 0;
}
~~~
## G - Minimum LCM
### 题意:
将n分为任意两数a、b的和，求当lcm(a,b)最小时的a、b
### 思路:
n范围是1e9，但是求n最小的公因数只需要遍历到sqrt(n)即可，范围小于4e4，1e2个输入，暴力跑完全可以。
### 代码:
~~~
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
#define fr(qa, ws,ed) for (int qa = (ws); qa <= (ed); ++qa)
#define ll int
const int N = 1e3 + 10, mod = 1e9 + 7;

int a[N];

void solve()
{
    int n; cin >> n;
    int m = sqrt(n);
    for (int i = 2; i <= m; ++i)
        if (n % i == 0) 
            return void(cout << n / i << " " << n - n / i << endl);

    cout << 1 << " " << n - 1 << endl;
}

signed main()
{
    IOS
    int t;cin >> t; while(t--)
        solve();

    return 0;
}
~~~
## H - Number Reduction
### 题意:
一个位数不超过5e5的数字x，删除任意k位，在没有前导零的情况下，求x的最小值。
### 思路:
位数为n的x中，淘汰掉k位，得到的n-k位即使答案。
- 最高位的确认还是很重要的：因为最多只能删除k位，所以前k+1位至少会保存一位，保留前k+1位除零外数字最小的一位，最小的同时保留原位数尽可能高的即可。
- 优先队列中存放目前为止，被淘汰掉的位。
- 对于剩下的第t位：第t位及之前的位数放入优先队列中，选出最小，且比前一位位数的下标要大的即可。循环n-k-1次。
### 代码:
~~~
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
#define IOS ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
#define fr(qa, ws,ed) for (int qa = (ws); qa <= (ed); ++qa)
const int N = 1e3 + 10, mod = 1e9 + 7;

struct cmp{
    char v;
    int i;
};
bool operator < (const cmp& A, const cmp &B){
    if (A.v == B.v) return A.i > B.i;
    return A.v > B.v;
}
void solve()
{
    priority_queue<cmp> q;
    string s; cin >> s;
    int k; cin >> k;
    int p = 0;
    for(int i = 0; i <= k; ++i)
        if(s[i] != '0' && s[p] > s[i]) p = i;
    cout << s[p];
    for(int i = 0; i <= k; ++i) q.push({s[i], i});

    for(int i = k + 1; i < s.size(); ++i)
    {
        q.push({s[i],i});
        while(1)
        {
            auto t = q.top(); 
            q.pop();
            if(t.i > p)
            {
                cout << t.v;
                p = t.i;
                break;
            }
            //cerr << t.i << "   " << p << endl;
        }
    }
    cout << endl;    
}

signed main()
{
    IOS
    int t;cin >> t; while(t--)
        solve();

    return 0;
}
~~~
