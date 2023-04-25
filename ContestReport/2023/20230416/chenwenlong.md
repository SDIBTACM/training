# 比赛总结
## 做出来的题
### A
再也不看一半就开始敲了,刚开始理解的题意就是有向图里找环，直接就敲tarjan的板子，结果不对，然后读了一遍发现是找环里有多少边，直接暴力遍历，交了又不对，又读了一遍题才知道是找最大的环。
#### 思路
把每个物品都看成点，每人需求看成有向边，然后从每个点遍历，找到从这个点开始能找出的环的大小，输出最大的那个就行。
#### 代码
```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;
const int N = 1e5 + 5;
map<string, int> mp;
int head[205], ed[405], nex[405], idx;
bool vis[1000];
int xxx;
vector<int> a;
void add(int a, int b)
{
  ed[++idx] = b;
  nex[idx] = head[a];
  head[a] = idx;
}
int dfs(int &rt, int &val)
{
  int t = -1e8;
  for (int i = head[rt]; i; i = nex[i])
  {
    if (vis[i])
      continue;
    vis[i] = 1;
    if (ed[i] == val)
    {
      return 1;
    }
    t = max(t, dfs(ed[i], val));
  }
  return t + 1;
}
void solve()
{
  int n;
  string s1, s2;
  cin >> n;
  for (int i = 1; i <= n; ++i)
  {
    cin >> s1 >> s1 >> s2;
    if (mp.find(s1) == mp.end())
      mp[s1] = ++xxx;
    if (mp.find(s2) == mp.end())
      mp[s2] = ++xxx;
    add(mp[s1], mp[s2]);
  }
  int ans = 0;
  for (int i = 1; i <= xxx; ++i)
  {
    memset(vis, 0, sizeof vis);
    int t = dfs(i, i);
    if (t > 0)
      ans = max(ans, t);
  }
  if (ans == 0)
    cout << "No trades possible" << endl;
  else
    cout << ans << endl;
}
int main()
{
  int t = 1;
  // cin>>t;
  while (t--)
    solve();
  return 0;
}
```
### B
这个根据题意直接模拟就行，赛时没注意输入的数字可能不是个位数，wa了两次。
#### 代码
```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;
const int N = 1e5 + 5;
map<char, int> mp;
map<int, char> mp1;
void init()
{
  int idx = -1;
  for (int i = 'A'; i <= 'Z'; ++i)
  {
    mp[i] = ++idx;
    mp1[idx] = i;
  }
  for (int i = '0'; i <= '9'; ++i)
  {
    mp[i] = ++idx;
    mp1[idx] = i;
  }
  mp[' '] = ++idx;
  mp1[idx] = ' ';
}
void solve()
{
  init();
  string s;
  int n, len;
  cin >> n;
  vector<vector<int>> a(n, vector<int>(n));
  vector<vector<int>> g;
  vector<int> x;
  for (int i = 0; i < n; ++i)
    for (int j = 0; j < n; ++j)
      cin >> a[i][j];
  getchar();
  getline(cin, s);
  len = s.size();
  while (len % n != 0)
  {
    s.push_back(' ');
    len++;
  }
  for (int i = 0; i < len; ++i)
  {
    x.push_back(mp[s[i]]);
    if (i % n == n - 1)
    {
      g.push_back(x);
      x.clear();
    }
  }
  for (int i = 0; i < len / n; ++i)
  {
    x.clear();
    for (int j = 0; j < n; ++j)
    {
      int cnt = 0;
      for (int k = 0; k < n; ++k)
      {
        cnt += a[j][k] * g[i][k]%37;
      }
      x.push_back(mp1[cnt % 37]);
    }
    g[i] = x;
  }
  s.clear();
  for (int i = 0; i < len / n; ++i)
    for (int j = 0; j < n; ++j)
    {
      s.push_back(g[i][j]);
    }
  cout << s;
}
int main()
{
  int t = 1;
  // cin>>t;
  while (t--)
    solve();
  return 0;
}
```
### C
这个题也是直接模拟就行
#### 代码
```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;
const int N = 1e5 + 5;
class a
{
public:
  int idx;
};
void solve()
{
  int n, m;
  cin >> n >> m;
  a tem[n + 1];
  int mp[n + 1];
  for (int i = 1; i <= n; ++i)
  {
    tem[i].idx = i;
    mp[i] = i;
  }
  int s1, s2;
  char c;
  for (int i = 1; i <= m; ++i)
  {
    cin>>c>>s1>>c>>s2;
    if (mp[s1] > mp[s2])
    {
      int c = mp[s2];
      a x = tem[mp[s2]];
      mp[s2] = mp[s1];
      for (int j = c + 1; j <= mp[s1]; ++j)
      {
        mp[tem[j].idx]--;
        tem[j - 1] = tem[j];
      }
      tem[mp[s2]] = x;
    }
  }
  for (int i = 1; i <n; ++i)
  {
    cout << 'T' << tem[i].idx <<' ';
  }
  cout<<'T'<<tem[n].idx;
}
int main()
{
  int t = 1;
  // cin>>t;
  while (t--)
    solve();
  return 0;
}
```
## 补的题
### D
赛时没看，赛后看了也不会，情况考虑不全，后面看了师哥的题解，用了4个check，跟我的对比了一下，emm，我把一些check都柔和在一起了，然后人不人鬼不鬼的，估计就漏了一些点。
#### 代码
```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;
const int N = 1e5 + 5;
set<int> hang[9], lie[9], ss[3][3], st1;
set<pair<int, int>> st;
int a[9][9];
bool h[9][10], l[9][10];
vector<int> b[3][3];
inline bool check(int &x, int &y)
{
  set<int> sst = st1;
  for (auto i = hang[x].begin(); i != hang[x].end(); ++i)
    sst.erase(*i);
  for (auto i = lie[y].begin(); i != lie[y].end(); ++i)
    sst.erase(*i);
  for (auto i = ss[x / 3][y / 3].begin(); i != ss[x / 3][y / 3].end(); ++i)
    sst.erase(*i);
  if (sst.size() == 1)
  {
    int t = *sst.begin();
    hang[x].insert(t);
    lie[y].insert(t);
    ss[x / 3][y / 3].insert(t);
    a[x][y] = t;

    return true;
  }
  return false;
}
inline bool check1(int &x, int &val)
{
  int flag = false;
  int sum = 0, pos;
  for (int i = 0; i < 9; ++i)
    if (!a[x][i] && lie[i].find(val) == lie[i].end() && ss[x / 3][i / 3].find(val) == ss[x / 3][i / 3].end())
      sum++, pos = i;
  if (sum == 1)
  {
    flag = true;
    lie[pos].insert(val);
    hang[x].insert(val);
    ss[x / 3][pos / 3].insert(val);
    a[x][pos] = val;
    st.erase({x, pos});
  }
  return flag;
}
inline bool check2(int &y, int &val)
{
  int flag = false;
  int sum = 0, pos;
  for (int i = 0; i < 9; ++i)
    if (!a[i][y] && hang[i].find(val) == hang[i].end() && ss[i / 3][y / 3].find(val) == ss[i / 3][y / 3].end())
      sum++, pos = i;
  if (sum == 1)
  {
    flag = true;
    hang[pos].insert(val);
    lie[y].insert(val);
    ss[pos / 3][y / 3].insert(val);
    a[pos][y] = val;
    st.erase({pos, y});
  }
  return flag;
}
inline bool check3(int &x, int &y, int &val)
{
  bool flag = false;
  int sum = 0, pos;
  for (auto &i : b[x][y])
  {
    if (!a[i / 9][i % 9] && hang[i / 9].find(val) == hang[i / 9].end() && lie[i % 9].find(val) == lie[i % 9].end())
    {
      sum++;
      pos = i;
    }
  }
  if (sum == 1)
  {
    a[pos / 9][pos % 9] = val;
    hang[pos / 9].insert(val);
    lie[pos % 9].insert(val);
    ss[x][y].insert(val);
    flag = true;
    st.erase({pos / 9, pos % 9});
  }
  return flag;
}

void solve()
{
  for (int i = 0; i < 9; ++i)
  {
    st1.insert(i + 1);
    for (int j = 0; j < 9; ++j)
    {
      cin >> a[i][j];
      if (a[i][j])
      {
        hang[i].insert(a[i][j]);
        lie[j].insert(a[i][j]);
        ss[i / 3][j / 3].insert(a[i][j]);
      }
      else
        st.insert({i, j});
      b[i / 3][j / 3].push_back(i * 9 + j);
    }
  }
  bool flag = true;
  while (flag == true)
  {
    flag = false;
    for (auto i = st.begin(); i != st.end();) // 每个位置
    {
      int x = i->first, y = i->second;
      if (check(x, y))
      {
        flag = true;
        ++i;
        st.erase({x, y});
      }
      else
        ++i;
    }
    // 行
    for (int i = 0; i < 9; ++i)
    {
      for (int j = 1; j <= 9; ++j)
      {
        if (hang[i].find(j) == hang[i].end() && check1(i, j))
          flag = true;
      }
    }
    // 列
    for (int i = 0; i < 9; ++i)
    {
      for (int j = 1; j <= 9; ++j)
      {
        if (lie[i].find(j) == lie[i].end() && check2(i, j))
          flag = true;
      }
    }
    // 子矩阵
    for (int i = 0; i < 3; ++i)
      for (int j = 0; j < 3; ++j)
        for (int k = 1; k <= 9; ++k)
          if (ss[i][j].find(k) == ss[i][j].end() && check3(i, j, k))
            flag = true;
  }
  if (st.size())
    puts("Not easy");
  else
    puts("Easy");
  for (int i = 0; i < 9; ++i)
  {
    for (int j = 0; j < 9; ++j)
      if (a[i][j])
        cout << a[i][j] << ' ';
      else
        cout << ". ";
    cout << '\n';
  }
}
int main()
{
  int t = 1;
  // cin>>t;
  while (t--)
    solve();
  return 0;
}
```
### E
第一次比赛的 扔鸡蛋问题！！！！！当时学了后还去找了两道练习题做了，结果这次就忘了，也没耐心去推一下，浮躁了。
#### 思路
定义`f[i][j]`表示用i个蛋测试j层楼需要的最佳情况下的最坏次数(听绕口的，看看题意理解就行)
假设楼高n，有两种情况：
1. 从第j层扔下去碎了->转变为求i-1个鸡蛋测试j-1层楼:f[i][j]=f[i-1][j-1]+1。
2. 没碎->转变为求用i个鸡蛋测试n-i层楼：f[i][j]=f[i][n-j]+1;

只有一个鸡蛋时 f[1][j]=j;
只有一层楼时 f[i][1]=1;
根据转移方程，每一次转移只从上一次和本层转移过来，所以f[i][j]可以优化为f[2][j];
最后找区间，遍历上一次可能转移到答案的点，记录一下。
#### 代码
```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;
const int N = 1e5 + 5;
int f[2][5005];
void solve()
{
  int t,a,b;
  cin>>a>>b;
  f[0][1]=1;
  for(int i=1;i<=a;++i)f[1][i]=i;
  for(int i=2;i<=b;++i)
  {
    for(int j=2;j<=a;++j)
    {
      f[i&1][j]=1e9;
      for(int k=1;k<=j;++k)
        f[i&1][j]=min(f[i&1][j],max(f[(i-1)&1][k-1],f[i&1][j-k])+1);
    }
  }
  int l=1e8,r=0;
  cout<<f[b&1][a];
  for(int i=1;i<=a;++i)
  {
    if(max(f[(b-1)&1][i-1],f[b&1][a-i])+1==f[b&1][a])
    {
      l=min(l,i);
      r=max(r,i);
    }
  }
  if(l==r)cout<<' '<<l;
  else cout<<' '<<l<<'-'<<r;
}
int main()
{
  int t = 1;
  // cin>>t;
  while (t--)
  solve();
  return 0;
}
```