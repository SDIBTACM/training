# 做出来的题
比赛只做出来了B、D题，其余只看懂了E题，无奈写的思路乱了些，赛后整理了一下还是能过得。
## 题解
- B
- 思路：仅现在晴天且目的地有伞时不用带伞，模拟输出即可
- 代码： 
```
#include <bits/stdc++.h>
using namespace std;
#define mem(a, b) memset(a, b, sizeof(a));
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0); //勿混用
typedef long long ll;
typedef unsigned long long ull;
const int inf = 0x3f3f3f3f;
const int mod = 1e9 + 7;
const int N = 1e5 + 5;
const int M = 1e6 + 5;
//////////////////////////////////////////////////////////////
ll n, m; 
char a[6];
inline void solve()
{
    ll p;
    cin >> p >> n >> m;
    while (p--)
    {
        string a, b;
        cin >> a >> b;
        char x = a[0], y = b[0];
        if (x == 'N' && m) cout << "N ";
        else
        {
            cout << "Y ";
            --n;
            ++m;
        }
        if (y == 'N' && n) cout << "N" << endl; 
        else
        {
            cout << "Y" << endl;
            ++n;
            --m;
        }
    }
}
int main()
{
    IOS;
    //int T; cin >> T; while (T--)
        solve();
    return 0;
}  
```
- D
- 思路：枚举凹陷的低地做起点，遍历四周的点加入优先队列
- 代码：
```
#include <bits/stdc++.h>
using namespace std;
#define mem(a, b) memset(a, b, sizeof(a));
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0); //勿混用
typedef long long ll;
typedef unsigned long long ull;
const int inf = 0x3f3f3f3f;
const int mod = 1e9 + 7;
const int N = 1e5 + 5;
const int M = 1e6 + 5;
//////////////////////////////////////////////////////////////
ll n, m;
int a[110][110];
char vis[110][110];
int dir[4][2] = { {-1,0},{0,1},{1,0},{0,-1} };
struct node
{
    int x;
    int y;
    int v;
};
int operator <(const struct node& a, const struct node& b)
{
    return a.v > b.v;
}
priority_queue <struct node> q;
inline int bfs(int sx, int sy) {
    mem(vis, 0);
    int ans = 1;
    int p = a[sx][sy];
    q.push({ sx, sy,a[sx][sy] });
    vis[sx][sy] = 1;
    while (q.size() > 0)
    {
        node temp = q.top();
        q.pop();
        p = temp.v;
        for (int i = 0; i <= 3; i++)
        {
            int dx = temp.x + dir[i][0], dy = temp.y + dir[i][1];
            if (vis[dx][dy] || a[dx][dy] < p) continue;
            q.push({ dx, dy,a[dx][dy] });
            vis[dx][dy] = 1;
            ++ans;
        }
    }
    return ans;
}
inline void solve()
{
    cin >> n >> m;
    for (int i = 1; i <= n; ++i) for (int j = 1; j <= m; ++j) cin >> a[i][j];
    int ma = 0;
    for (int i = 1; i <= n; ++i) for (int j = 1; j <= m; ++j)
    {
        if (a[i][j] > a[i][j - 1] && a[i][j - 1]) continue;
        if (a[i][j] > a[i - 1][j] && a[i - 1][j]) continue;
        if (a[i][j] > a[i + 1][j] && a[i + 1][j]) continue;
        if (a[i][j] > a[i][j + 1] && a[i][j + 1]) continue;
        ma = max(ma, bfs(i, j));
    }
    cout << ma;
}
int main()
{
    IOS;
    //int T; cin >> T; while (T--)
    solve();
    return 0;
}
```
# 比赛后补的题
- A
- 思路：反向建边，枚举所有顶点：如果这个点和它的两个子节点都遍历过某点，那么说明某点可以激活顶点的两个子节点，然后再激活这个顶点
- 代码：
```
#include <bits/stdc++.h>
using namespace std;
#define mem(a, b) memset(a, b, sizeof(a));
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0); //勿混用
typedef long long ll;
typedef unsigned long long ull;
const int inf = 0x3f3f3f3f;
const int mod = 1e9 + 7;
const int N = 1e3 + 5;
const int M = 1e6 + 5;
//////////////////////////////////////////////////////////////
ll n, m;
int head[N];
int ed[N << 1], nex[N << 1], idx;
int a[N], b[N];
int siz[N];
int vis[N];
void add(int x, int y)
{
    ed[idx] = y;
    nex[idx] = head[x];
    head[x] = idx++;
}
int dfs(int x, int p)
{
    mem(vis, 0);
    vis[x] = vis[p] = 1;
    ++siz[x];
    queue<int> q;
    q.push(x);
    while (q.size())
    {
        int u = q.front();
        q.pop();
        if (siz[u] == 3) return 1;
        for (int i = head[u]; i; i = nex[i])
        {
            int d = ed[i];
            if (vis[d]) continue;
            ++siz[d];
            vis[d] = 1;
            q.push(d);
        }
    }
    return 0;
}
inline void solve()
{
    idx = 1;
    cin >> n;
    for (int i = 1; i <= n; ++i)
    {
        cin >> a[i] >> b[i];
        add(b[i], i);
        add(a[i], i);
    }
    for (int i = 1; i <= n; ++i)
    {
        mem(siz, 0); 
        dfs(i, i);
        dfs(a[i], i);
        if (dfs(b[i], i)) cout << 'Y';
        else cout << 'N';
    }
}
int main()
{
    IOS;
    //int T; cin >> T; while (T--)
    solve();
    return 0;
}
```
- C
- 思路：发现只要是7以上的数字，不论另一个数和e是什么，一定能够组合成功————因为7以上的数字至少有4种组合方式。然后枚举小于7的情况（没想到能写这么长，害！）
- 代码
```
#include <bits/stdc++.h>
using namespace std;
#define mem(a, b) memset(a, b, sizeof(a));
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0); //勿混用
typedef long long ll;
typedef unsigned long long ull;
const int inf = 0x3f3f3f3f;
const int mod = 1e9 + 7;
const int N = 1e5 + 5;
const int M = 1e6 + 5;
//////////////////////////////////////////////////////////////
ll n, m;
ll k, e;
int vis[1001];
int main()
{
    cin >> n >> k >> e;
    vis[e] = 1;
    int a = k, b = n - k - e;
    if (a < b) swap(a, b);
    if (a != b && a != e && b != e) { cout << 0; return 0; }
    if (b >= 7) { cout << 0; return 0; }// 7 以上的数字，随机少任何 3 个数都能 ac
    if (a >= 7)// b < 7 <= a
    {
        if ((e == 1 || e == 2) && b == e) { cout << 1; return 0; }
        cout << 0; return 0;
    }
    if (a == b && a == e) // a == b == c
    {
        if (a == 1 || a == 4) { cout << 2; return 0; }
        if (a == 2 || a == 3) { cout << 3; return 0; }
        cout << 0; return 0;
    }
    if (a == b)// b == a < 7
    {
        switch (a)
        {
        case 0:
            cout << 0;
            return 0;
        case 1:
            if (e == 1) cout << 2;
            else cout << 1;
            return 0;
        case 2:
            if (e == 1) cout << 2;
            else if (e == 2) cout << 3;
            else cout << 1;
            return 0;
        case 3:
            if (e <= 3) cout << e;
            else cout << 0;
            return 0;
        case 4:
            if (e == 1 || e == 3) cout << 1;
            else if (e == 4) cout << 2;
            else cout << 0;
            return 0;
        case 5:
            cout << 0;
            return 0;
        case 6:
            cout << 0;
            return 0;
        default:
            cout << 0;
            return 0;
        }
        return 0;
    }
    if (a == e)// b < a < 7
    {
        if (a >= 5) { cout << 0; return 0; }
        if (a == 4)
        {
            if (b == 1 || b == 3) { cout << 1; return 0; }
            else { cout << 0; return 0; }
        }
        if (a == 2)
        {
            if (b == 1) { cout << 2; return 0; }
            else { cout << 1; return 0; }
        }
        if (a == 1) { cout << 1; return 0; }
        cout << b; return 0;
    }
    // b == e
    if (b == e)
    {
        if (b == 1 || b == 2) { cout << 1; return 0; }
        else { cout << 0; return 0; }
    }
    cout << 0;
    return 0;
}
```
- E
- 思路：深搜，除了标记路径以外 对于每个位置的check也标记，不然会time limit，然后翻过来再跑一边
- 代码
```
#include <bits/stdc++.h>
using namespace std;
#define mem(a, b) memset(a, b, sizeof(a));
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0); //勿混用
typedef long long ll;
typedef unsigned long long ull;
const int inf = 0x3f3f3f3f;
const int mod = 1e9 + 7;
const int N = 1e5 + 5;
const int M = 1e6 + 5;
//////////////////////////////////////////////////////////////
ll n, m;
int ans;
char a[1010][110];
int s[110], siz;
char vis[1010][110];
int che[1010][110];
int check(int t, int p)
{
    if (che[t][p] != -1) return che[t][p];
    for (int i = 1; i <= siz; ++i) if (a[t][(p + s[i]) % m] == '1') return che[t][p] = 0;
    return che[t][p] = 1;
}
int dfs(int ans, int t)
{
    if (t == n + 1) return 1;
    vis[t][ans] = 1;
    int l = (ans - 1 + m) % m;
    int r = (ans + 1) % m;
    while (vis[t][l] == 0) if (check(t, l)) vis[t][l] = 1, l = (l - 1 + m) % m;  else break;
    l = (l + 1) % m;
    while (vis[t][r] == 0) if (check(t, r)) vis[t][r] = 1, r = (r + 1) % m; else break;
    r = (r - 1 + m) % m;
    while (1)
    {
        if (vis[t + 1][l] == 0 && check(t + 1, l) && dfs(l, t + 1)) return 1;
        if (vis[t - 1][l] == 0 && check(t - 1, l) && dfs(l, t - 1)) return 1;
        vis[t + 1][l] = vis[t - 1][l] = 1;
        if (l == r) break;
        l = (l + 1) % m;
    }
    return 0;
}
int solve()
{
    mem(vis, 0);
    mem(che, -1);
    for (int i = 0; i <= m; ++i) vis[0][i] = 1;
    ans = 0;
    while (ans < m) if (vis[1][ans] == 0 && check(1, ans) && dfs(ans, 1)) return 1; else ++ans;
    return 0;
}
int main()
{
    cin >> n >> m;
    string sc; cin >> sc;
    for (int i = 0; sc[i]; ++i) if (sc[i] == '1') s[++siz] = i;
    for (int i = 1; i <= n; ++i) scanf("%s", a[i]);
    if (solve()) { cout << 'Y' << endl; return 0; }
    for (int i = 1; i <= siz; ++i) s[i] = m - s[i]; //翻过来 
    if (solve()) { cout << 'Y' << endl; return 0; }
    cout << 'N';
    return 0;
}
```
