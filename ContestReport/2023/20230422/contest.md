# 20、21、22第五次个人赛(2023.04.22 14:00-18:00)

## [比赛地址](https://vjudge.net/contest/554049)

## 题解
### A
```cpp
#include <bits/stdc++.h>
using namespace std;

#define int long long int

signed main(){
    int n;
    string s;
    cin >> n >> s;
    int ans = 0;
    for (int i = 0; i < n; i++)
    {
        if (s[i] != 'a')
            continue;
        if (i - 1 >= 0 && s[i - 1] == 'a')
            ans++;
        else if (i + 1 < n && s[i + 1] == 'a')
            ans++;
    } 
    cout << ans << endl;
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

//#define int long long int
#define endl '\n'
#define pb push_back
#define pi pair<int, int>
#define pii pair<int, pi>
#define fir first
#define sec second
#define MAXN 1010
#define mod 1000000007

bool adj[MAXN][MAXN][4];
pi parent[MAXN][MAXN];
int sz[MAXN][MAXN];
bool vis[MAXN][MAXN];
int dx[] = {1, -1, 0, 0};
int dy[] = {0, 0, 1, -1};

int bfs(int x, int y)
{
  queue<pi> q;
  q.push({x, y});
  vis[x][y] = 1;
  int cnt = 1;
  while (!q.empty())
  {
    int i = q.front().fir;
    int j = q.front().sec;
    q.pop();
    for (int d = 0; d < 4; d++)
    {
      if (adj[i][j][d])
      {
        int x = i + dx[d];
        int y = j + dy[d];
        if (!vis[x][y])
        {
          vis[x][y] = 1;
          cnt++;
          q.push({x, y});
        }
      }
    }
  }
  return cnt;
}
pi find_set(pi i)
{
  return parent[i.fir][i.sec] = (parent[i.fir][i.sec] == i) ? i : find_set(parent[i.fir][i.sec]);
}
void make_set(pi x, pi y)
{
  x = find_set(x), y = find_set(y);
  if (x != y)
  {
    if (sz[x.fir][x.sec] > sz[y.fir][y.sec])
      swap(x, y);
    parent[x.fir][x.sec] = y;
    sz[y.fir][y.sec] += sz[x.fir][x.sec];
  }
}
void init()
{
  for (int i = 0; i < MAXN; i++)
  {
    for (int j = 0; j < MAXN; j++)
    {
      parent[i][j] = {i, j};
      sz[i][j] = 1;
    }
  }
}
signed main()
{
  ios_base::sync_with_stdio(false);
  cin.tie(NULL);
  int n;
  cin >> n;
  n++;
  init();
  vector<pi> v;
  for (int i = 0; i < n; i++)
  {
    int x, y;
    cin >> x >> y;
    x += 2, y += 2;
    v.pb({x, y});
  }
  for (int i = 1; i < n; i++)
  {
    if (v[i].fir == v[i - 1].fir)
    {
      int l = min(v[i].sec, v[i - 1].sec);
      int r = max(v[i].sec, v[i - 1].sec);
      for (int j = l; j < r; j++)
        make_set({v[i].fir, j + 1}, {v[i].fir - 1, j + 1});
    }
    else
    {
      int l = min(v[i].fir, v[i - 1].fir);
      int r = max(v[i].fir, v[i - 1].fir);
      for (int j = l; j < r; j++)
        make_set({j, v[i].sec}, {j, v[i].sec + 1});
    }
  }
  for (int i = 0; i < MAXN; i++)
  {
    for (int j = 0; j < MAXN; j++)
    {
      for (int d = 0; d < 4; d++)
      {
        int x = i + dx[d];
        int y = j + dy[d];
        if (x < 0 || y < 0 || y >= MAXN || x >= MAXN)
          continue;
        if (find_set({i, j}) != find_set({x, y}))
          adj[i][j][d] = 1;
      }
    }
  }
  bfs(0, 0);
  int ans = 0;
  for (int i = 0; i < MAXN; i++)
  {
    for (int j = 0; j < MAXN; j++)
    {
      if (!vis[i][j])
        ans = max(ans, bfs(i, j));
    }
  }
  cout << ans << endl;
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
#define MAXN 500001
#define mod 1000000007

signed main()
{
  ios_base::sync_with_stdio(false);
  cin.tie(NULL);
  int n, x, y;
  cin >> n >> x >> y;
  for (int i = 0;; i++)
  {
    if (y & (1 << i))
    {
      cout << n - 1 - i << endl;
      break;
    }
  }
  return 0;
}
```
### D
```cpp
#include <bits/stdc++.h>
using namespace std;

#define int long long int

int s[1000005];

signed main(){
    int n;
    cin >> n;
    int ans = n;
    for (int i = 0; i < n; i++)
    {
        int x;
        cin >> x;
        if (s[x + 1] > 0)
        {
            s[x + 1]--;
            ans--;
        }
        s[x]++;
    }
    cout << ans << endl;
    return 0;
}
```
### E
```cpp
#include <bits/stdc++.h>

using namespace std;
#define int long long int
#define fir first
#define sec second
#define mod 1000000009

const int d = 256;

signed main() {
    int n, c;
    cin >> n >> c;
    vector<string> v(n);
    vector<int> p(n);
    unordered_map<int, int> mp;
    for (int i = 0; i < n; i++)
    {
        cin >> v[i];
        int pos = -1;
        for(int j = 0; j < c; j++)
        {
            if (v[i][j] == '*')
            {
                pos = j;
            }
        }
        for (char j = 'a'; j <= 'z'; j++)
        {
            v[i][pos] = j;
            int h = 0;
            for (auto const &x : v[i])
            {
                h = (h * d) % mod;
                h = (h + x) % mod;
            }
            mp[h]++;
        }
        v[i][pos] = '*';
        p[i] = pos;
    }
    string ans = "";
    int mx = -1;
    for (int i = 0; i < n; i++)
    {
        int pos =  p[i];
        for (char j = 'a'; j <= 'z'; j++)
        {
            v[i][pos] = j;
            int h = 0;
            for (auto const &x : v[i])
            {
                h = (h * d) % mod;
                h = (h + x) % mod;
            }
            if (mp[h] > mx)
            {
                ans = v[i];
                mx = mp[h];
            }
            else if (mp[h] == mx && v[i] < ans)
            {
                ans = v[i];
                mx = mp[h];
            }
        }
    }
    cout << ans << " " << mx << endl;
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

#define int long long int
#define endl '\n'
#define pb push_back
#define pi pair<int, int>
#define pii pair<int, pi>
#define fir first
#define sec second
#define MAXN 1010
#define mod 1000000007

signed main()
{
  ios_base::sync_with_stdio(false);
  cin.tie(NULL);
  bool ans = 1;
  for (int i = 0; i < 8; i++)
  {
    int a;
    cin >> a;
    if (a == 9)
      ans = 0;
  }
  (ans) ? cout << "S\n" : cout << "F\n";
  return 0;
}
```
### G 
```cpp
#include <bits/stdc++.h>
using namespace std;

#define int long long int

int dp[14];

void init()
{
  for (int i = 1; i <= 13; i++)
  {
    dp[i] = 4;
  }
}
int convert(int x)
{
  if (x > 10)
    return 10;
  return x;
}
signed main()
{
  init();
  int n;
  cin >> n;
  int a1 = 0, a2 = 0, sum_j = 0, sum_m = 0;
  cin >> a1 >> a2;
  dp[a1]--;
  dp[a2]--;
  sum_j = convert(a1) + convert(a2);
  cin >> a1 >> a2;
  dp[a1]--;
  dp[a2]--;
  sum_m = convert(a1) + convert(a2);
  for (int i = 1; i <= n; i++)
  {
    int k;
    cin >> k;
    dp[k]--;
    sum_j += convert(k);
    sum_m += convert(k);
  }
  int ans = -1;
  for (int i = 1; i <= 13; i++)
  {
    if (dp[i] > 0)
    {
      if (sum_m + convert(i) == 23 || (sum_j + convert(i) > 23 && sum_m + convert(i) <= 23))
      {
        ans = i;
        break;
      }
    }
  }
  cout << ans << endl;
  return 0;
}
```
### H 
```cpp
#include <bits/stdc++.h>

using namespace std;
#define int long long int
#define fir first
#define sec second
#define pb push_back
#define mod 1000000009
#define pi pair<int, int>
#define MAXN 100005

int n, k, l, sum, sum2;
int a[MAXN];
int b[MAXN];
multiset<int> s1, s2;

void rem(int i)
{
    sum -= a[i];
    if (s1.find(b[i]) != s1.end())
    {
        s1.erase(s1.find(b[i]));
        sum2 -= b[i];
        if (s2.size() > 0)
        {
            int aux = *s2.rbegin();
            s2.erase(s2.find(aux));
            s1.insert(aux);
            sum2 += aux;
        }
    }
    else 
    {
        s2.erase(s2.find(b[i]));
    }
}
void add(int i)
{
    sum += a[i];
    if (s1.size() < l)
    {
        s1.insert(b[i]);
        sum2 += b[i];
    }
    else if (b[i] > (*s1.begin()))
    {
        int aux = *s1.begin();
        s1.erase(s1.find(aux));
        sum2 -= aux;
        s2.insert(aux);
        s1.insert(b[i]);
        sum2 += b[i];
    }
    else 
    {
        s2.insert(b[i]);
    }
}
int solve()
{
    return sum + sum2;
}
signed main() 
{
    cin >> n;
    for (int i = 0; i < n; i++)
        cin >> a[i];
    for (int i = 0; i < n; i++)
        cin >> b[i];
    cin >> k >> l;
    for (int i = 0; i < k; i++)
    {     
        add(i);
    }
    int ans = solve();
    int l = k - 1, r = n - 1;
    while(l >= 0)
    {
        rem(l);
        add(r);
        ans = max(ans, solve());
        l--, r--;
    }
    cout << ans << endl;
}
```
## 比赛排名


| Rank | Team                  | Score |
|------|-----------------------|-------|
| 1    | SDTBU_CWL(陈文龙)        | 7     |
| 2    | SDTBU_LY(廖银)          | 6     |
| 3    | SDTBU_LCY(刘骋羽)        | 6     |
| 4    | SDU_TSK(田始坤)          | 6     |
| 5    | STU_YXY(杨新艺)          | 6     |
| 6    | SDTBU_XYH             | 6     |
| 7    | SDTBU_MZW(孟孜文)        | 5     |
| 8    | SDTBU_ZLK(康康不想秃头)     | 5     |
| 9    | SDTBU_LX(李旭)          | 5     |
| 10   | 2019214174(fyj)       | 5     |
| 11   | SDTBU_TLH(仝令辉)        | 5     |
| 12   | SDTBU_SY(孙源)          | 4     |
| 13   | jk21213389(李业昊)       | 4     |
| 14   | SDTBU_WCK(王成魁)        | 4     |
| 15   | SDTBU_GYM(郭一鸣)        | 4     |
| 16   | SDTBU_LTY(刘统誉)        | 3     |
| 17   | szyaizd(周东)           | 3     |
| 18   | js2021230777(田佳璇)     | 3     |
| 19   | SDTBU_ZRT(赵若彤)        | 3     |
| 20   | 13626422329(SDTBU_HS) | 2     |
| 21   | SDTBU_CH(崔涵)          | 2     |
| 22   | SDTBU_TMQ(唐梦淇)        | 2     |
| 23   | rg2021213527(王传瑞)     | 2     |
| 24   | SDTBU_SYY(宋玥)         | 2     |
| 25   | SDTBU_xjw(肖俊伟)        | 2     |
| 26   | SDTBU_WZY(王在媛)        | 2     |
| 27   | SDTBU_LSS(李双双)        | 2     |
| 28   | SDTBU_XJJ(谢俊杰)        | 2     |
| 29   | SDTBU_GGCX(高晨轩)       | 2     |
| 30   | SDTBU_LQ(SDTBU-LQ)    | 2     |
| 31   | wl222121943(付晓龙)      | 2     |
| 32   | SDTBU_LYR             | 2     |
| 33   | SDTBU_LT(林涛)          | 2     |
| 34   | jk22214598(祝增圆)       | 2     |
| 35   | jk22214603(栗子旭)       | 2     |
| 36   | jk22214600(赵潼)        | 2     |
| 37   | sm2020234582(是真不会)    | 2     |
| 38   | wl22214941(郑堂堂)       | 2     |
| 39   | rj2022214683(赵丽萍)     | 2     |
| 40   | SDTBU_CJZ(成嘉泽)        | 2     |
| 41   | SDTBU_MBY(马宝运)        | 2     |
| 42   | jk22214550(刘贵瑜)       | 1     |
