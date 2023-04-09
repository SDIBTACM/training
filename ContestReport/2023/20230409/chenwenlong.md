# 比赛总结
整体感觉还行，不足的还是读题，和细节处理，猪脑容易过载！
## 做出来的题
### B
赛时看错题了，没看到是小镇外的人先向小镇的人要钱，然后就在错误的题意上wa，赛后补题
#### 思路
根据题意直接模拟
#### 代码
```h
void solve()
{
  int n,h,w;
  cin>>n>>h>>w;
  for(int i=1;i<=n;++i)
  {
    char a,b;
    cin>>a>>b;
    if((a=='Y'&&h)||!w)
    {
      cout<<'Y';
      h--;
      w++;
    }
    else cout<<'N';
    if((b=='Y'&&w)||!h)
    {
      cout<<" Y\n";
      h++;
      w--;
    }
    else cout<<" N\n";
  }
}
```
### C
刚开始想的先满足一边，然后二分查找去枚举另一边，感觉不太对，试了一下不行就做D去了，后面仔细想了一下还是决定分类讨论
#### 思路
将 e，k,n-e-k 三个值的大小进行分类讨论，特别注意有相等的情况。
#### 代码
```h
void solve()
{
  int n, k, e, d, ans = 0;
  cin >> n >> k >> e;
  d = n - k - e;
  if (e > d)
    swap(e, d);
  if(d<k||e>k)
  {
    if(d==e)
    {
      if(d<k)
      {
        if(d==1||d==2)ans=1;
      }
      else
      {
        if(d==2)ans=2;
        else if(d==3)ans=k;
        else if(d==4)ans=k%2;
      }
    }
    cout<<ans<<endl;
    return ;
  }
  if (d > k)
  {
    if (e == k)
    {
      if (k == 1 || k==2)
        ans = 1;
    }
  }
  else
  {
    if (e == 0)
    {
      if (k == 1 || k == 2)
        ans = 1;
    }
    else if (e < k)
    {
      if (k == 2)
        ans = 2;
      else if (k == 3)
        ans = e;
      else if (k == 4)
        ans = e % 2;
    }
    else
    {
      if (k == 1)
        ans = 2;
      else if (k == 2)
        ans = 3;
      else if (k == 3)
        ans = 3;
      else if (k == 4)
        ans = 2;
    }
  }
  cout << ans << endl;
}
```
### D
刚开始想的并查集，然后发现并查集会重复合并，后面考虑将可行的数都连成链，然后答案就是最长的链，交了两次没去掉相邻时不用枚举大的数的情况TL了。
#### 思路
用链式前向星存边，a<b就存边a->b，然后枚举点dfs找最长链的长度。
#### 代码
```h
const int N = 1e5;
int dir[4][2] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
int head[N], ed[N * 2], nex[N * 2], idx;
set<int> st2;
int vis[N];
inline void add(int a, int b)
{
  ed[++idx] = b;
  nex[idx] = head[a];
  head[a] = idx;
}
inline int dfs(int x)
{
  int ans = 1;
  vis[x] = 1;
  for (int i = head[x]; i; i = nex[i])
    if (!vis[ed[i]])
      ans+=dfs(ed[i]);
  return ans;
}
void solve()
{
  int n, m;
  cin >> n >> m;
  vector<vector<int>> a(n, vector<int>(m));
  for (int i = 0; i < n; ++i)
    for (int j = 0; j < m; ++j)
      cin>>a[i][j], st2.insert(i * m + j);
  for (int i = 0; i < n; ++i)
    for (int j = 0; j < m; ++j)
    {
      for (int k = 0; k < 4; ++k)
      {
        int dx = i + dir[k][0], dy = j + dir[k][1];
        if (dx < 0 || dx >= n || dy < 0 || dy >= m || a[dx][dy] < a[i][j])
          continue;
        add(i * m + j, dx * m + dy);
        st2.erase(dx * m + dy);
      }
    }
  int ans = 0;
  for (auto i : st2)
  {
    memset(vis, 0, (n * m + 5) * sizeof(vis[0]));
    ans = max(ans, dfs(i));
  }
  cout << ans << endl;
}
```
## 补的题
### A
赛时题看错了，没注意到要先有镇子外的人作为初始点，就一直在错误的题意下wa，后面补题时想了一个思路：向上枚举点x可能得父节点以及父节点的父节点......，然后
从枚举的“父节点”向下枚举子节点，看看能否把x的子节点夺取过来，若可以的话，则Y，否则N。感觉加上各种记忆化应该不会T，然后一直wa on 3。应该还是有我想不到bug。后面看题解做，又因为一些小毛病一直wa，最气的是**关了同步流后用putchar输出竟然有问题，不理解，很玄学**
#### 思路
分别从x和它的两个儿子a，b开始dfs遍历，如果有点rt在这三次遍历时都能被遍历到，那么x就可能成为以rt为根的二叉树的叶子节点，即亏钱。
建边：a向b要钱，建边b->a
#### 代码
```h
const int N = 2005;
vector<int> ed[N];
int vis[N], temp, vis2[N];
int a[N][2];
void dfs(int &rt)
{
  vis[rt] = 1;
  vis2[rt]++;
  for (auto i : ed[rt])
  {
    if (vis[i])
      continue;
    dfs(i);
  }
}
void solve()
{
  int n, x, y;
  cin >> n;
  for (int i = 1; i <= n; ++i)
  {
    cin >> a[i][0] >> a[i][1];
    ed[a[i][0]].push_back(i);
    ed[a[i][1]].push_back(i);
  }
  for (int i = 1; i <= n; ++i)
  {
    if (!ed[i].size())
    {
      cout << 'N';
      continue;
    }
    memset(vis2, 0, (n + 5) * sizeof(int));
    memset(vis, 0, (n + 5) * sizeof(int));
    vis[i] = 1;
    dfs(a[i][0]);
    memset(vis, 0, (n + 5) * sizeof(int));
    vis[i] = 1;
    dfs(a[i][1]);
    memset(vis, 0, (n + 5) * sizeof(int));
    dfs(i);
    int t=0;
    for (int j = 1; j <= n; ++j)
      if (vis2[j] == 3)
      {
        t=1;
        break;
      }
    if(t)cout<<'Y';
    else cout<<'N';
  }
}
```
### E
#### 思路
直接暴力dfs！走过的点不重复走
对于匹配的循环串,double一下，就避免了复杂的处理

读题小心，螺母可以移到最外面翻转一下，最后一个样例看了半天！！！
还有就是用exit(1)结束程序会提示运行错误
#### 代码
```h
#include <iostream>
#include <string>
#include<cstring>
#include<algorithm>
using namespace std;
bool vis[255][1105];
int n, m;
char g[1105][205];
bool flag;
string s;
inline bool check(int &d, int &st)
{
  for (int i = 1; i <= m; ++i)
  {
    if (g[d][i] == '1' && s[i + st - 1] == '1')
      return false；
  }
  return true;
}
void dfs(int dep, int start)
{
  if(start==0)start=m;
  if(start==m+1)start=1;
  if (!dep)
    return;
  if (vis[start][dep])
    return;
  vis[start][dep] = 1;
  if (!check(dep, start))
    return;
  if (dep == n)
  {
    flag=1;
    return ;
  }
  dfs(dep + 1, start);
  dfs(dep - 1, start);
  dfs(dep,start-1);
  dfs(dep,start+1);
}
int main()
{
  cin >> n >> m;
  cin >> s;
  s = " " + s + s;
  for (int i = 1; i <= n; ++i)
  {
    cin >> g[i] + 1;
  }
  for (int i = 1; i <= m; ++i)
    dfs(1, i);
  reverse(s.begin(),s.end());
  s=" "+s;
  memset(vis,0,sizeof vis);
  for (int i = 1; i <= m; ++i)
    dfs(1, i);
  flag?cout<<'Y':cout<<'N';
  return 0;
}
```