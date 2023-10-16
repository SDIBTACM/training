# 周报感想
- 比赛第六题因为多输出了个空格，报时间超限。于是想优化方式想了好久没想到，好好好
- 做了几个难题但没感到什么收获，可能是太难了，就算看题解也悟不出什么
- 这周还是 Java 和 操作系统
- 稍微期待一下大二的下阶段的算法吧，希望好好学，不过需要这些算法需要暑假算法的基础，考虑到有些人基础没打好，，，加油吧
- 百团大战一天协会人数多了一倍，真厉害

## [补个第七题的题解](https://pintia.cn/problem-sets/91827364500/exam/problems/91827368255?type=7&page=0)
题意不写了，反正有链接
- 双塔dp，之前没做过双塔，比赛时做的时候想过很多状态方程也没想到这么刁钻的。
- 塔高代表时间长短。
- f[i][j] 中 i 代表第 i 个， j 是两者的时间差，f[i][j] 是前 i 个时间差为 j 的最小时间
- 因为时间差可能是负的，所以 j 开两倍的数组。
- 当前的物品放在高塔时：因为两个塔必须是连续的（两塔中间不能有未完成的），所以更新短的塔到原高塔的位置，时间差为当前的物品高度，然后高塔更新代价。
- 当前的物品放在低塔时：更新后的塔高如果低于高塔的话，时间代价为0，否则时间代价为高出去的那一小截。同时更新时间差，比较原先的时间差进行做差更新，
- 需要注意，上面的操作可能会在同一个时间差进行多次比较，所以不能直接赋值。

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
ll f[110][215]; // 第 i 个，时间差为 j

void solve()
{
    ll n; cin >> n;
    for (int i = 0; i <= n; ++i) for (int j = 0; j <= 210; ++j) f[i][j] = INT64_MAX;
    ll  p = 105;
    f[0][p] = 0;
    for (int i = 1; i <= n; ++i)
    {
        ll a, b; cin >> a >> b;

        // A > B
        for (int j = 0; j <= 100; ++j) if (f[i - 1][p + j] != INT64_MAX)
        {
            // 加在 a 后的时间差为 p + a
            f[i][p + a] = min(f[i][p + a], f[i - 1][p + j] + a);
            // 加在 b 后的时间差为 j - b
            f[i][p + j - b] = min(f[i][p + j - b], f[i - 1][p + j] + max(0LL, b - j));
        }

        // A < B
        for (int j = 1; j <= 100; ++j) if (f[i - 1][p - j] != INT64_MAX)
        {
            f[i][p - b] = min(f[i][p - b], f[i - 1][p - j] + b); // p - b
            f[i][p - j + a] = min(f[i][p - j + a], f[i - 1][p - j] + max(0LL, a - j)); // - j + a
        }
    }
    ll ans = INT64_MAX;
    for (int i = 0; i <= 210; ++i) ans = min(ans, f[n][i]);
    cout << ans << endl;
}

signed main()
{
    IOS;
    ll _; cin >> _; while (_--)
        solve();

    return 0;
}
```
