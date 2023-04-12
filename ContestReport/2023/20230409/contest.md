# 20、21、22第三次个人赛(2023.04.09 14:00-18:00)

## [比赛地址](https://vjudge.net/contest/552058) 密码：acm2023

## 标程

### A
```c++
    #include <bits/stdc++.h>
    #include <ext/pb_ds/assoc_container.hpp>
    #include <ext/pb_ds/tree_policy.hpp>
    using namespace std;
    using namespace __gnu_pbds;

    template <class T>
    using ordered_set = tree<T, null_type, less<T>, rb_tree_tag, tree_order_statistics_node_update>;

    #define inf long long int
    #define endl '\n'
    #define pb push_back
    #define pi pair<int, int>
    #define pii pair<pi, int>
    #define fir first
    #define sec second
    #define MAXN 1005
    #define mod 1000000007

    int a, b, ii;
    pi v[MAXN];
    bool vis[MAXN];
    int mask[MAXN];
    vector<int> adj[MAXN];

    void dfs(int x, int val)
    {
      vis[x] = 1;
      mask[x] |= val;
      for (auto const &y : adj[x])
      {
        if (!vis[y] && y != ii)
          dfs(y, val);
      }
    }
    void dfs2(int x, int val)
    {
      vis[x] = 1;
      mask[x] |= val;
      for (auto const &y : adj[x])
      {
        if (!vis[y])
          dfs(y, val);
      }
    }
    signed main()
    {
      ios_base::sync_with_stdio(false);
      cin.tie(NULL);
      int n;
      cin >> n;
      for (int i = 0; i < n; i++)
      {
        int a, b;
        cin >> a >> b;
        a--, b--;
        v[i] = {a, b};
        adj[a].pb(i);
        adj[b].pb(i);
      }
      for (int i = 0; i < n; i++)
      {
        a = v[i].fir, b = v[i].sec, ii = i;
        for (int k = 0; k < n; k++)
        {
          vis[k] = 0;
          mask[k] = 0;
        }
        dfs(a, 1);
        for (int k = 0; k < n; k++)
        {
          vis[k] = 0;
        }
        dfs(b, 2);
        for (int k = 0; k < n; k++)
        {
          vis[k] = 0;
        }
        dfs2(i, 4);
        bool ok = 0;
        for (int k = 0; k < n; k++)
        {
          if (mask[k] == 7)
            ok = 1;
        }
        (ok) ? cout << "Y" : cout << "N";
      }
      cout << endl;
    }
```
### B

``` c++
    #include <bits/stdc++.h>
    using namespace std;

    #define int long long int

    signed main()
    {
      int n, h, w;
      cin >> n >> h >> w;
      while (n--)
      {
        char a, b;
        cin >> a >> b;
        if (a == 'Y')
        {
          w++;
          h--;
          cout << "Y ";
        }
        else if (w == 0)
        {
          w++;
          h--;
          cout << "Y ";
        }
        else
        {
          cout << "N ";
        }

        if (b == 'Y')
        {
          w--;
          h++;
          cout << "Y ";
        }
        else if (h == 0)
        {
          w--;
          h++;
          cout << "Y ";
        }
        else
        {
          cout << "N ";
        }
        cout << endl;
      }
    }
```
### C
```c++

    #include <bits/stdc++.h>
    using namespace std;

    #define int long long int

    int n, k, e;

    int solve(int a, int b)
    {
      vector<bool> vis(n + 1, 0);
      vis[k] = 1;
      int ans = a + b;
      for (int i = 1; i <= n; i++)
      {
        if (i > a || vis[i])
        {
          continue;
        }

        vis[i] = 1;

        { // preencher com 1
          int curr = a - i;
          ans = min(ans, curr + b);
          for (int x = 1; x <= n; x++)
          {
            int aa = x;
            int bb = b - x;
            if (bb < 0)
              break;
            if (vis[aa] || vis[bb] || aa == bb)
              continue;
            ans = min(ans, curr);
            break;
          }
        }

        for (int j = 1; j <= n; j++)
        {
          if (i + j > a || vis[j])
            continue;
          vis[j] = 1;
          { // preencher com 2
            int curr = a - (i + j);
            ans = min(ans, curr + b);
            for (int x = 1; x <= n; x++)
            {
              int aa = x;
              int bb = b - x;
              if (bb < 0)
                break;
              if (vis[aa] || vis[bb] || aa == bb)
                continue;
              ans = min(ans, curr);
              break;
            }
          }
          vis[j] = 0;
        }

        vis[i] = 0;
      }
      return ans;
    }
    signed main()
    {
      cin >> n >> k >> e;
      int a = e;
      int b = n - (e + k);
      if (a != k && b != k && a != b)
      {
        cout << "0\n";
        return 0;
      }
      int ans = min(solve(a, b), solve(b, a));
      cout << ans << endl;
    }
    // preencher um dos lados com 1 ou dois blocos
```

### D
```c++
    #include <bits/stdc++.h>
    using namespace std;

    #define pi pair<int, int>
    #define pii pair<int, pi>
    #define fir first
    #define sec second

    int dx[] = {0, 0, 1, -1};
    int dy[] = {1, -1, 0, 0};

    int r, c;
    int v[101][101];
    bool vis[101][101];

    int solve(int i, int j)
    {
      for (int i = 0; i < r; i++)
      {
        for (int j = 0; j < c; j++)
          vis[i][j] = 0;
      }
      priority_queue<pii> pq;
      pq.push({-v[i][j], {i, j}});
      vis[i][j] = 1;
      int ans = 0, mx = -1;
      while (!pq.empty())
      {
        int x = pq.top().sec.fir;
        int y = pq.top().sec.sec;
        pq.pop();
        if (v[x][y] <= mx)
          continue;
        mx = v[x][y];
        ans++;
        for (int d = 0; d < 4; d++)
        {
          int ii = x + dx[d], jj = y + dy[d];
          if (ii >= 0 && jj >= 0 && ii < r && jj < c && !vis[ii][jj])
          {
            vis[ii][jj] = 1;
            pq.push({-v[ii][jj], {ii, jj}});
          }
        }
      }
      return ans;
    }
    signed main()
    {
      ios_base::sync_with_stdio(false);
      cin.tie(NULL);
      cin >> r >> c;
      for (int i = 0; i < r; i++)
      {
        for (int j = 0; j < c; j++)
          cin >> v[i][j];
      }
      int ans = 0;
      for (int i = 0; i < r; i++)
      {
        for (int j = 0; j < c; j++)
          ans = max(ans, solve(i, j));
      }
      cout << ans << endl;
    }

```
### E
```c++

    #include <bits/stdc++.h>
    using namespace std;

    #define pi pair<int, int>
    #define pii pair<int, pi>
    #define fir first
    #define sec second
    #define pb push_back

    int n, m;
    string s;
    string t[1001];
    bool can[1001][101];
    bool vis[1001][101];

    void dfs(int i, int j)
    {
      vis[i][j] = 1;
      if (i + 1 < n && !vis[i + 1][j] && can[i + 1][j])
        dfs(i + 1, j);
      if (i - 1 >= 0 && !vis[i - 1][j] && can[i - 1][j])
        dfs(i - 1, j);
      int a = (j + 1) % m;
      int b = (j - 1 + m) % m;
      if (!vis[i][b] && can[i][b])
        dfs(i, b);
      if (!vis[i][a] && can[i][a])
        dfs(i, a);
    }
    bool solve()
    {
      memset(vis, false, sizeof(vis));
      memset(can, false, sizeof(can));
      for (int i = 0; i < n; i++)
      {
        for (int r = 0; r < m; r++)
        {
          int x = r;
          bool ok = 1;
          for (int j = 0; j < m; j++)
          {
            if (s[j] == '1' && t[i][x] == '1')
            {
              ok = 0;
              break;
            }
            x++;
            if (x == m)
              x = 0;
          }
          if (ok)
            can[i][r] = 1;
        }
      }
      for (int i = 0; i < m; i++)
      {
        if (!vis[0][i] && can[0][i])
          dfs(0, i);
      }
      bool ok = 0;
      for (int i = 0; i < m; i++)
        ok |= vis[n - 1][i];
      return ok;
    }
    signed main()
    {
      ios_base::sync_with_stdio(false);
      cin.tie(NULL);
      cin >> n >> m >> s;
      for (int i = 0; i < n; i++)
        cin >> t[i];
      bool ok = solve();
      reverse(s.begin(), s.end());
      ok |= solve();
      (ok) ? cout << "Y\n" : cout << "N\n";
    }

```
## 比赛结果
| Rank | Team                    | Score |
| ---- | ----------------------- | ----- |
| 1    | SDTBU_LY(廖银)          | 3     |
| 2    | SDTBU_LCY(刘骋羽)       | 3     |
| 3    | SDTBU_XYH               | 3     |
| 4    | SDTBU_CWL(陈文龙)       | 3     |
| 5    | STU_YXY(杨新艺)         | 2     |
| 6    | SDTBU_GYM(郭一鸣)       | 2     |
| 7    | SDU_TSK(响当当)         | 2     |
| 8    | rg2021213527(王传瑞)    | 2     |
| 9    | SDTBU_TMQ(唐梦淇)       | 2     |
| 10   | SDTBU_WCK(王成魁)       | 2     |
| 11   | SDTBU_TLH(仝令辉)       | 2     |
| 12   | SDTBU_LX(李旭)          | 2     |
| 13   | SDTBU_XJJ(谢俊杰)       | 2     |
| 14   | js2021230777(田佳璇)    | 2     |
| 15   | rj2022214683(赵丽萍)    | 2     |
| 16   | 13626422329(SDTBU_HS)   | 1     |
| 17   | jk21213389(李业昊)      | 1     |
| 18   | SDTBU_MZW(孟孜文)       | 1     |
| 19   | SDTBU_SY(孙源)          | 1     |
| 20   | SDTBU_WZY(王在媛)       | 1     |
| 21   | SDTBU_xjw(肖俊伟)       | 1     |
| 22   | jk22214600(赵潼)        | 1     |
| 23   | SDTBU_LTY(刘统誉)       | 1     |
| 24   | SDTBU_LSS(李双双)       | 1     |
| 25   | jk22214603(栗子旭)      | 1     |
| 26   | SDTBU_wzw(王智炜)       | 1     |
| 27   | SDTBU_ZLK(康康不想秃头) | 1     |
| 28   | 2019214174({Cu1}...)    | 1     |
| 29   | SDTBU_CH(CHH)           | 1     |
| 30   | SDTBU_SYY(宋玥)         | 1     |
| 31   | SDTBU_LYR               | 1     |
| 32   | SDTBU_ZRT(赵若彤)       | 1     |
| 33   | jk22214598(祝增圆)      | 1     |
| 34   | SDTBU_LQ(SDTBU-LQ)      | 1     |
| 35   | szyaizd(周东)           | 1     |
| 36   | SDTBU_LT(林涛)          | 1     |
| 37   | SDTBU_CJZ(成嘉泽)       | 1     |
| 38   | sm2020234582(是真不会)  | 1     |
| 39   | wl22214941(郑堂堂)      | 1     |
| 40   | wl222121943(付晓龙)     | 1     |
| 41   | SDTBU_MBY(马宝运)       | 1     |
| 42   | SDTBU_GGCX(高晨轩)      | 1     |
