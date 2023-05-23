## 比赛报告

## H

**题意:**

分别给出两个点上一次到正确距离现在的时间, 再给出两个点的周期, 问下一次这两个点在正确点相遇的时间.

**思路:**

刚开始想的是 LCM, 后来修修补补就过了, 不过当时看的很迷, 后来发现, 只需要一步一步加起来取模就行... 签到题

```c++
#include <bits/stdc++.h>

using namespace std;

const int N = 1e3 + 5;

int ds, ys, dm, ym;

int main()
{
    ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
    cin >> ds >> ys >> dm >> ym;
    int res = 0;
    for (int i = 1; ; i++) {
        res++;
        if ((ds + i) % ys == (dm + i) % ym) break;
        
    }
    cout << res << endl;
    return 0;
}
```

## F

**题意:** 签到题 略

**思路:** 略

```c++
#include <iostream>

int main()
{
    int r, f;
    std::cin >> r >> f;
    int turn = f / r, res = f % r;
    if (turn % 2) {
        if (res > r / 2) {
			std::cout << "up" << std::endl;
        } else {
            std::cout << "down" << std::endl;
        }
    } else {
        
        if (res > r / 2) {
			std::cout << "down" << std::endl;
        } else {
            std::cout << "up" << std::endl;
        }
    }
    return 0;
}
```

## A

**题意:** 略

**思路:**

跟着题意模拟即可

```c++
#include <bits/stdc++.h>

using namespace std;

int h, k, v, s, res = 0;

int main()
{
    cin >> h >> k >> v >> s;
    while (h > 0) {
        v += s;
        v -= max(1, v / 10);
        if (v >= k) 
            h++;
       	else if (v > 0) {
            if (--h == 0) v = 0;
        } else if (v == 0)
            h = v = 0;
        if (s > 0) s--;
        res += v;
    }
    cout << res << endl;
    return 0;
}
```

## G

**题意:**

签到题

**思路:**

把每一个点一下, 然后求两个点的映射值的差 -1

```c++
#include <bits/stdc++.h>

using namespace std;

const int N = 1e5 + 5;

int n, q;

int h[N], e[N << 1], nex[N << 1], idx;
map<string, int> ht;

void add(int a, int b, int c) {
    e[++idx] = b, nex[idx] = h[a], h[a] = idx;
}

int main()
{
    ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
    cin >> n >> q;
    for (int i = 1; i <= n; i++) {

        string s; cin >> s;
        ht[s] = i;
    }

    for (int i = 1; i <= q; i++) {
        string a, b; cin >> a >> b;
        cout << abs(ht[a] - ht[b]) - 1 << endl;

    }
    return 0;
}

/**

3 10
1 2


**/

```

## B

**题意:**

把 $n$ 个整数, 分成每一段都不超过 $k (l \le k \le h)$ 的部分, 然后计算每一部分的和, 求所有分块大小下块为正值的最大数量和最小数量

**思路:**

我们可以发现, 只有最后一块和第一块的块长度可以小于给定的 $k$, 故可以发现, 当第一块的长度确定后, 这个划分就确定了,  所以我们首先枚举块长, 然后再枚举第一块的长度, 然后计算当前划分中每一个块的值, 取最大最小即可, 这样的话时间复杂度为 $O((h - l) * k * \frac{n}{k} * k)$ 其中 $h - l$ 为枚举的块长的数量, $k$ 是枚举第一块的长度, $\frac{n}{k}$ 是这个划分将被划分的块的数量, $k$ 表示计算这个块内和需要的数量, 不过这个 $k$ 可以使用前缀和优化掉, 所以最终时间复杂度为 $O((h - l) * k * \frac{n}{k}) = O((h - l) * n)$

```c++
#include <iostream>
using namespace std;
#define N 30005
int a[N], sum[N];
int main()
{
    int n, l, h;
    cin >> n >> l >> h;
    for(int i = 1; i <= n; i++)
    {
        cin >> a[i];
        sum[i] = sum[i - 1] + a[i];
    }
    int mn = 1e9, mx = 0;
    for(int i = l; i <= h; i++)
    {
        for(int j = 0; j < i; j++)
        {
            int cnt = sum[j] > 0;
            for(int k = j + 1; k <= n; k += i)
            {
                int ed = min(k + i - 1, n);
                // cout << i << " " << j << " " << k << " " << ed << endl;
                if(sum[ed] - sum[k - 1] > 0)
                    cnt++;
            }
            mn = min(mn, cnt);
            mx = max(mx, cnt);
        }
    }
    cout << mn << " " << mx << endl;
    return 0;
}

```

## C

**题意:**

给出一个二维矩阵, 问你每一次扩展可以把每一个＃周围的 - 号给扩展为＃, 问需要几次

**思路:**

这个题其实挺迷的, 没大看懂, 猜样例发现可以每次都把边界这一圈的＃全都变成 - , 直到所有＃都变成了 - 位置, 也就是每次都把边界的这一圈＃变成 -. 本质就是从最终状态倒推起点需要几步

```c++
#include <iostream>
#include <queue>
using namespace std;
#define N 1005
char mp[N][N];
int n, m, flag[N][N], dir[4][2] = {{-1, 0}, {0, -1}, {0, 1}, {1, 0}};
queue <pair <int, int> > q[2];
bool check(int x, int y)
{
    for(int i = 0; i < 4; i++)
    {
        int xx = x + dir[i][0];
        int yy = y + dir[i][1];
        if(xx < 1 || xx > n || yy < 1 || yy > m)
            continue;
        if(mp[xx][yy] == '-' || flag[xx][yy])
            continue;
        return true;
    }
    return false;
}
int bfs()
{
    int res = 0, now = 0;
    while(!q[now].empty())
    {
        res++;
        while(!q[now].empty())
        {
            int x = q[now].front().first;
            int y = q[now].front().second;
            q[now].pop();
            for(int i = 0; i < 4; i++)
            {
                int xx = x + dir[i][0];
                int yy = y + dir[i][1];
                if(xx < 1 || xx > n || yy < 1 || yy > m)
                    continue;
                if(mp[xx][yy] == '-' || flag[xx][yy])
                    continue;
                flag[xx][yy] = 1;
                q[now ^ 1].push({xx, yy});
            }
        }
        now ^= 1;
    }
    return res - 1;
}
int main()
{
    cin >> n >> m;
    for(int i = 1; i <= n; i++)
        cin >> mp[i] + 1;
    for(int i = 0; i <= m + 1; i++)
        mp[0][i] = mp[n + 1][i] = '-';
    for(int i = 0; i <= n + 1; i++)
        mp[i][0] = mp[i][m + 1] = '-';
    for(int i = 0; i <= n + 1; i++)
    {
        for(int j = 0; j <= m + 1; j++)
        {
            if(mp[i][j] == 'X')
                continue;
            if(check(i, j))
                q[0].push({i, j});
        }
    }
    int ans = bfs();
    cout << ans << endl;
    return 0;
}

```

## E

**思路:**

首先如果出现的字符数大于 $18$ 则不可以, 然后暴力枚举, 每种情况, 通过重新排列当前字符串来实现这个字符串的字符被分配到不同集合里,讲道理这种情况应该是会 T 的但是可能是剪枝的情况, 导致它过了... 运气吧

```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;
#define N 1005
char s[N][5];
int n, vis[30], flag[30];
vector <char> ans[5];
bool dfs(int now)
{
    if(now > n)
        return true;
    int f[5] = {0, 0, 0, 0, 0};
    vector <char> v1;
    vector <int> v2;
    for(int i = 1; i <= 3; i++)
    {
        if(flag[s[now][i] - 'a' + 1])
        {
            if(f[flag[s[now][i] - 'a' + 1]])
                return false;
            f[flag[s[now][i] - 'a' + 1]] = 1;
        }
        else
            v1.push_back(s[now][i]);
    }
    for(int i = 1; i <= 3; i++)
        if(!f[i] && ans[i].size() < 6)
            v2.push_back(i);
    if(v1.size() != v2.size())
        return false;
    sort(v1.begin(), v1.end());
    sort(v2.begin(), v2.end());
    if(v1.size())
    {
        do {
            for(int i = 0; i < v1.size(); i++)
            {
                flag[v1[i] - 'a' + 1] = v2[i];
                ans[v2[i]].push_back(v1[i]);
            }
            if(dfs(now + 1))
                return true;
            for(int i = 0; i < v1.size(); i++)
            {
                flag[v1[i] - 'a' + 1] = 0;
                ans[v2[i]].pop_back();
            }
        } while(next_permutation(v2.begin(), v2.end()));
        return false;
    }
    else
        return dfs(now + 1);
}
int main()
{
    cin >> n;
    for(int i = 1; i <= n; i++)
        cin >> s[i] + 1;
    int f = 1, cnt = 0;
    for(int i = 1; i <= n; i++)
    {
        for(int j = 1; j <= 3; j++)
        {
            if(!vis[s[i][j] - 'a' + 1])
            {
                vis[s[i][j] - 'a' + 1] = 1;
                cnt++;
            }
        }
        if(s[i][1] == s[i][2] || s[i][1] == s[i][3] || s[i][2] == s[i][3])
        {
            f = 0;
            break;
        }
    }
    if(cnt > 18)
        f = 0;
    if(!f)
    {
        cout << 0 << endl;
        return 0;
    }
    if(dfs(1))
    {
        int pos = 1;
        for(int i = 1; i <= 3; i++)
        {
            while(ans[i].size() < 6)
            {
                while(vis[pos])
                    pos++;
                ans[i].push_back((char)('a' + pos - 1));
                vis[pos] = 1;
            }
        }
        for(int i = 1; i <= 3; i++)
        {
            if(i != 3)
            {
                for(int j = 0; j < ans[i].size(); j++)
                    cout << ans[i][j];
                cout << " ";
            }
            else
            {
                for(int j = 0; j < ans[i].size(); j++)
                    cout << ans[i][j];
                cout << endl;
            }
        }
    }
    else
        cout << 0 << endl;
    return 0;
}

```

## 补题 D

**题意:**

**思路:**

贪心, 由于当前天的垃圾只能由未来的日期收拾, 所以贪心的想, 当前垃圾由可以选择今天以后的某几天收拾, 如果前几天的某几天已经被选中清理未来的垃圾, 这样就可以省一次打扫, 也就是选择未来已经选择打扫的天中剩余的最大值, 如果没有的话就找没有选择打扫的天中选择最大值. 如果从当前天到检查日期的一天中选择后依旧剩余了垃圾, 则说明收拾不完. 说的不清楚自己意会一下

```c++
#include <bits/stdc++.h>

using namespace std;

const int N = 1e3 + 5;

int n, d;
int a[N], sum[N], b[N], v[N];

priority_queue<pair<int, int>> heap;

int main()
{
    ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
    cin >> n >> d;
    for (int i = 1; i <= n; i++) {
        cin >> a[i] >> b[i];
    }
    for (int i = 1; i <= d; i++) cin >> v[i];
    int res = 0;
    for (int i = 1; i <= d; i++) {
        while (heap.size()) heap.pop();
        int t = 0;
        for (int j = v[i]; j > v[i - 1]; j--) {
            
            if (b[j]) {
                heap.push({0, b[j]});
            }
            t = a[j];
            while (t > 0 && heap.size()) {
                auto cur = heap.top();
                heap.pop();
                if (!cur.first) res++;
                int m = cur.second;
                cur.second -= min(t, cur.second);
                t -= min(t, m);
                if (cur.second) heap.push({1, cur.second});
            }
            if (t > 0) {
                cout << -1 << endl;
                return 0;
            }
        } 
    }
    cout << res << endl;
    return 0;
}
```

