# 20、21、22第一次组队赛(2023.05.07 13:00-18:00)

## [比赛地址](https://vjudge.net/contest/556687)

## 题解


### A
```cpp
#include <bits/stdc++.h>
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>
using namespace std;
using namespace __gnu_pbds;

template <class T>
using ordered_set = tree<T, null_type, less<T>, rb_tree_tag, tree_order_statistics_node_update>;

#define int long long int
#define endl '\n'
#define pb push_back
#define pi pair<int, int>
#define pii pair<int, pi>
#define fir first
#define sec second
#define MAXN 200001

const int d = 257, mod = 8675309;
const int d2 = 619, mod2 = 1000000007;
int n, m;
string ss;
string vv[MAXN];

struct sh
{
  vector<int> pref;
  vector<int> pot;
  vector<int> pref2;
  vector<int> pot2;

  int mul(int x, int y, int m)
  {
    return (x * y) % m;
  }
  int sub(int a, int b, int m)
  {
    return (a - b < 0) ? a - b + m : a - b;
  }
  int sum(int a, int b, int m)
  {
    return (a + b >= m) ? a + b - m : a + b;
  }
  sh(string &s)
  {
    pref.resize(s.size() + 1);
    pot.resize(s.size() + 1);
    pref2.resize(s.size() + 1);
    pot2.resize(s.size() + 1);
    pref[0] = 0, pref2[0] = 0;
    for (int i = 0; i < s.size(); i++)
    {
      int val = mul(pref[i], d, mod);
      pref[i + 1] = sum(s[i], val, mod);
      int val2 = mul(pref2[i], d2, mod2);
      pref2[i + 1] = sum(s[i], val2, mod2);
    }
    pot[0] = 1, pot2[0] = 1;
    for (int i = 1; i <= s.size(); i++)
    {
      pot[i] = mul(pot[i - 1], d, mod);
      pot2[i] = mul(pot2[i - 1], d2, mod2);
    }
  }
  sh() {}
  int get(int l, int r)
  {
    return sub(pref[r + 1], mul(pref[l], pot[r - l + 1], mod), mod) * sub(pref2[r + 1], mul(pref2[l], pot2[r - l + 1], mod2), mod2);
  }
};

sh s;
sh v[MAXN];

bool can(int k) // se todos os shifts possuem uma substring de tamanho k
{
  if (k == 0)
    return true;
  gp_hash_table<int, bool> has;
  vector<pi> inter;
  bool can = false;
  for (int i = 0; i < m; i++)
  {
    int l = 0, r = k - 1;
    while (r < vv[i].size())
    {
      can = 1;
      has[v[i].get(l, r)] = 1;
      l++, r++;
    }
  }
  if (!can)
    return false;
  int l = 0, r = k - 1, sz = n + k;
  while (r < sz)
  {
    if (has[s.get(l, r)])
    {
      int ll = l + 1, rr = r;
      ll -= (ll >= n) ? n : 0;
      rr -= (rr >= n) ? n : 0;
      if (ll <= rr && k > 1)
        inter.pb({0, ll - 1}), inter.pb({rr + 1, n - 1});
      else if (k > 1)
        inter.pb({rr + 1, ll - 1});
      else
        inter.pb({0, n - 1});
    }
    l++, r++;
  }
  sort(inter.begin(), inter.end());
  for (int i = 0, j = 0; i < n; i++)
  {
    pi curr = {i, 1e6};
    while (j < inter.size() && inter[j] <= curr)
      j++;
    if (j == 0)
      return false;
    j--;
    if (!(i >= inter[j].fir && i <= inter[j].sec))
      return false;
  }
  return true;
}
signed main()
{
  ios_base::sync_with_stdio(false);
  cin.tie(NULL);
  cin >> n >> m >> ss;
  ss = ss + ss;
  s = sh(ss);
  for (int i = 0; i < m; i++)
  {
    cin >> vv[i];
    v[i] = sh(vv[i]);
  }
  int l = 0, r = n;
  while (l < r)
  {
    int mid = (l + r + 1) >> 1;
    (can(mid)) ? l = mid : r = mid - 1;
  }
  cout << l << endl;
  return 0;
}
```

### B
```cpp
#include <bits/stdc++.h>
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>
using namespace std;
using namespace __gnu_pbds;

template <class T>
using ordered_set = tree<T, null_type, less<T>, rb_tree_tag, tree_order_statistics_node_update>;

#define int long long int
#define endl '\n'
#define pb push_back
#define pi pair<int, int>
#define pii pair<int, pi>
#define fir first
#define sec second
#define MAXN 500001
//#define mod 1000000007

signed main()
{
  ios_base::sync_with_stdio(false);
  cin.tie(NULL);
  int base, n;
  cin >> base >> n;
  int mod = base + 1;
  vector<int> pot(n + 1);
  pot[0] = 1;
  for (int i = 1; i <= n; i++)
  {
    pot[i] = (pot[i - 1] * base) % mod;
  }
  vector<int> v(n);
  for (int i = 0; i < n; i++)
    cin >> v[i];
  reverse(v.begin(), v.end());
  int curr = 0;
  for (int i = 0; i < n; i++)
  {
    int at = v[i];
    at = (at * pot[i]) % mod;
    curr += at;
    curr %= mod;
  }
  if (curr == 0)
  {
    cout << 0 << " " << 0 << endl;
    return 0;
  }
  for (int i = n - 1; i >= 0; i--)
  {
    int past = curr;
    int at = v[i];
    int idx = (n - 1) - i;
    at = (at * pot[i]) % mod;
    curr -= at;
    curr += mod;
    curr %= mod;
    if (curr < v[i])
    {
      int x = curr;
      int past2 = curr;
      int at2 = (x * pot[i]) % mod;
      curr += at2;
      curr %= mod;
      if (curr == 0)
      {
        cout << n - i << " " << x << endl;
        return 0;
      }
      curr = past2;
    }
    if (past < v[i])
    {
      int x = v[i] - past;
      int past2 = curr;
      int at2 = (x * pot[i]) % mod;
      curr += at2;
      curr %= mod;
      if (curr == 0)
      {
        cout << n - i << " " << x << endl;
        return 0;
      }
      curr = past2;
    }
    curr = past;
  }
  cout << -1 << " " << -1 << endl;
  return 0;
}
```

### C
```cpp
#include <bits/stdc++.h>
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>
using namespace std;
using namespace __gnu_pbds;

template <class T>
using ordered_set = tree<T, null_type, less<T>, rb_tree_tag, tree_order_statistics_node_update>;

#define int long long int
#define endl '\n'
#define pb push_back
#define pi pair<int, int>
#define pii pair<int, pi>
#define fir first
#define sec second
#define MAXN 300005

signed main()
{
  ios_base::sync_with_stdio(false);
  cin.tie(NULL);
  int n;
  cin >> n;
  int st = -1, to_go = 0;
  vector<int> nxt;
  for (int i = 0; i < n; i++)
  {
    int a, b;
    cin >> a >> b;
    if (a > to_go)
    {
      if (nxt.size())
      {
        st ^= 1;
        to_go += 10;
        nxt.clear();
      }
      else
      {
        st = -1;
      }
    }
    if (st == -1 || st == b)
    {
      st = b;
      to_go = a + 10;
    }
    else
    {
      nxt.pb(a);
    }
  }
  if (nxt.size())
  {
    to_go += 10;
    nxt.clear();
  }
  cout << to_go << endl;
  return 0;
}
```

### D
```cpp
#include <bits/stdc++.h>
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>
using namespace std;
using namespace __gnu_pbds;

template <class T>
using ordered_set = tree<T, null_type, less<T>, rb_tree_tag, tree_order_statistics_node_update>;

#define int long long int
#define endl '\n'
#define pb push_back
#define pi pair<int, int>
#define pii pair<int, pi>
#define fir first
#define sec second
#define MAXN 500001
#define mod 1000000007

const int LIM = 1e17;

vector<int> v;
unordered_map<int, int> dp;
unordered_map<int, int> choose;
unordered_map<int, int> vis;

int solve(int n)
{
  if (n == 1)
    return 1;
  if (vis[n])
    return dp[n];
  int ret = 0;
  for (int i = v.size() - 1; i >= 0; i--)
  {
    if (n % v[i] == 0)
    {
      int curr = solve(n / v[i]);
      if (curr == 1)
      {
        vis[n] = true;
        choose[n] = v[i];
        return dp[n] = 1;
      }
    }
  }
  vis[n] = true;
  return dp[n] = ret;
}
signed main()
{
  ios_base::sync_with_stdio(false);
  cin.tie(NULL);
  v.clear();
  v.pb(2);
  v.pb(3);
  while (1)
  {
    int n = v.size();
    int x = v[n - 1] + v[n - 2];
    if (x <= LIM)
      v.pb(x);
    else
      break;
  }
  int n;
  cin >> n;
  if (solve(n) == 0)
  {
    cout << "IMPOSSIBLE\n";
    return 0;
  }
  vector<int> ans;
  while (n > 1)
  {
    ans.pb(choose[n]);
    n /= choose[n];
  }
  string s;
  for (auto const &i : ans)
  {
    int idx = lower_bound(v.begin(), v.end(), i) - v.begin();
    string t = "";
    for (int j = 0; j <= idx; j++)
      t.pb('A');
    t.pb('B');
    s += t;
  }
  cout << s << endl;
  return 0;
}
```

### E
```cpp
#include <bits/stdc++.h>
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>
using namespace std;
using namespace __gnu_pbds;

template <class T>
using ordered_set = tree<T, null_type, less<T>, rb_tree_tag, tree_order_statistics_node_update>;

//#define int long long int
#define endl '\n'
#define pb push_back
#define pi pair<int, int>
#define pii pair<int, pi>
#define fir first
#define sec second
#define MAXN 300005

signed main()
{
  ios_base::sync_with_stdio(false);
  cin.tie(NULL);
  int n, k;
  cin >> n >> k;
  vector<pi> v(n);
  vector<pi> o(n);
  for (int i = 0; i < n; i++)
  {
    cin >> v[i].fir >> v[i].sec;
    o[i] = v[i];
  }
  sort(v.begin(), v.end());
  bool can = true;
  for (int i = 0; i < n; i++)
    if (v[i].sec != o[i].sec)
      can = false;
  (can) ? cout << "Y\n" : cout << "N\n";
  return 0;
}
```

### F
```cpp
#include <bits/stdc++.h>
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>
using namespace std;
using namespace __gnu_pbds;

template <class T>
using ordered_set = tree<T, null_type, less<T>, rb_tree_tag, tree_order_statistics_node_update>;

//#define int long long int
#define endl '\n'
#define pb push_back
#define pi pair<int, int>
#define pii pair<int, pi>
#define fir first
#define sec second
#define MAXN 300005

signed main()
{
  ios_base::sync_with_stdio(false);
  cin.tie(NULL);
  int t, d, m;
  cin >> t >> d >> m;
  if (d < t)
  {
    cout << "N\n";
    return 0;
  }
  vector<int> v(m);
  for (int i = 0; i < m; i++)
    cin >> v[i];
  sort(v.begin(), v.end());
  v.pb(d);
  bool at = false;
  int sum = 0;
  int last = 0;
  for (int i = 0; i < v.size(); i++)
  {
    if (v[i] - last >= t)
      at = true;
    last = v[i];
  }
  (at) ? cout << "Y\n" : cout << "N\n";
  return 0;
}
```

### G
```cpp
#include <bits/stdc++.h>
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>
using namespace std;
using namespace __gnu_pbds;

template <class T>
using ordered_set = tree<T, null_type, less<T>, rb_tree_tag, tree_order_statistics_node_update>;

//#define int long long int
#define endl '\n'
#define pb push_back
#define pi pair<int, int>
#define pii pair<int, pi>
#define fir first
#define sec second
#define MAXN 300005

int id = 1;
vector<int> adj[MAXN];
bool vis[MAXN];
queue<int> qq;

void dfs(int s)
{
  qq.push(s);
  for (auto const &i : adj[s])
    dfs(i);
}
signed main()
{
  ios_base::sync_with_stdio(false);
  cin.tie(NULL);
  int q;
  cin >> q;
  vector<int> queries;
  while (q--)
  {
    int a, b;
    cin >> a >> b;
    b--;
    if (a == 1)
      adj[b].pb({id++});
    else
      queries.pb(b);
  }
  dfs(0);
  for (auto const &i : queries)
  {
    vis[i] = 1;
    while (vis[qq.front()])
      qq.pop();
    cout << qq.front() + 1 << endl;
  }
  return 0;
}
```

### H
```cpp
#include <bits/stdc++.h>
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>
using namespace std;
using namespace __gnu_pbds;

template <class T>
using ordered_set = tree<T, null_type, less<T>, rb_tree_tag, tree_order_statistics_node_update>;

//#define int long long int
#define endl '\n'
#define pb push_back
#define pi pair<int, int>
#define pii pair<int, pi>
#define fir first
#define sec second
#define MAXN 300005

int n;
vector<vector<int>> seg;

void build(vector<int> &v)
{
  n = 1;
  while (n < v.size())
    n <<= 1;
  seg.resize(n << 1);
  for (int i = 0; i < v.size(); i++)
    seg[i + n].pb(v[i]);
  for (int i = n - 1; i; i--)
    merge(seg[i << 1].begin(), seg[i << 1].end(), seg[(i << 1) | 1].begin(), seg[(i << 1) | 1].end(), back_inserter(seg[i]));
}
int qry(int l, int r, int x)
{
  int ans = 0;
  for (l += n, r += n + 1; l < r; l >>= 1, r >>= 1)
  {
    if (l & 1)
      ans += lower_bound(seg[l].begin(), seg[l].end(), x) - seg[l].begin(), l++;
    if (r & 1)
      r--, ans += lower_bound(seg[r].begin(), seg[r].end(), x) - seg[r].begin();
  }
  return ans;
}
signed main()
{
  ios_base::sync_with_stdio(false);
  cin.tie(NULL);
  int n, q;
  cin >> n >> q;
  vector<int> v(n);
  for (int i = 0; i < n; i++)
    cin >> v[i];
  build(v);
  while (q--)
  {
    int a, p, f;
    cin >> a >> p >> f;
    a--;
    (v[a] >= p) ? cout << 0 << endl : cout << f - qry(a + 1, a + f, p) << endl;
  }
  return 0;
}
```
## 比赛排名
| Rank | Team                  | Score |
|------|-----------------------|-------|
| 1    | SDTBU_LCY(刘骋羽)        | 5     |
| 2    | SDTBU_XJJ(谢俊杰)        | 4     |
| 3    | SDTBU_CWL(陈文龙)        | 3     |
| 4    | SDTBU_LX(李旭)          | 3     | 
| 5    | SDTBU_TLH(仝令辉)        | 2     |
| 6    | 13626422329(SDTBU_HS) | 2     |   
| 7    | SDTBU_SY(孙源)          | 2     | 
| 8    | SDTBU_XYH             | 2     |   
| 9    | SDTBU_MBY(马宝运)        | 2     |
| 10   | SDTBU_CH(崔涵)          | 2     | 
| 11   | SDTBU_WZY(王在媛)        | 2     |
| 12   | SDTBU_CJZ(成嘉泽)        | 2     |
| 13   | SDTBU_GGCX(高晨轩)       | 2     |
| 14   | SDTBU_wzw(王智炜)        | 0     |

