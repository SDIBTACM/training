### Info
- nothing to say
- 一切尽在不言中
- 李业昊
- 陈文龙
- 刘统誉
### 比赛地址
[https://vjudge.net/contest/556687](https://vjudge.net/contest/556687)
### 做出来的题目
#### B
- **题意** 给一个任意进制(B)的数(Di)，求让哪一位减小后使得结果可以被B+1整除。
- **思路** 如例一：
```
10 5
2 3 4 5 6
```
已知10进制数的任意一位减小后的结果可以被11整除。‘6’是个位数，若被11整除，则需在十位找‘6’凑。
题中十位是‘5’，不够凑‘6’，所以向前借‘10’，得到：
```
2 3 3 15 6
```
此时将个位的‘6’和十位的‘6’消去得：
```
2 3 3 9 0
```
以此类推
```
2 2 13 9 0
2 2 4 0 0
1 12 4 0 0
1 8 0 0 0
0 18 0 0 0
```
显然将原‘2 3 4 5 6’的千位减去18后的结果就可以被11整除，但千位的3不够减18，所以将18向后借1得：
```
0 17 10 0 0
0 7 0 0 0
```
显然7还是不能减
```
0 6 10 0 0
0 0 4 0 0
```
此时百位的4可以减4，于是答案就是‘3 0’。在向前看因为结果要的是结果最小，所以要从后往前找。
```cpp
#include<bits/stdc++.h>
#define ll long long
using namespace std;
void solve()
{
  int b,l;
  cin>>b>>l;
  vector<int>d(l),x;
  for(auto &i:d)cin>>i;
  x=d;
  for(int i=l-1;i>=1;--i)
  {
    if(x[i]==-1)
    {
      x[i]+=b;
      x[i-1]--;
    }
    if(x[i]<=x[i-1])
      x[i-1]-=x[i],x[i]=0;
    else
    {
      if(i-2>=0)
      {
        x[i-2]--;
        x[i-1]+=b-x[i];
        x[i]=0;
      }
      else
      {
        x[i]-=x[i-1];
        x[i-1]=0;
        break;
      }
    }
  }
  int t=0;
  while(t<l&&x[t]==0)t++;
  if(t==l)
  {
    cout<<0<<' '<<0<<endl;
    return ;
  }
  while(t<l-1)
  {
    if(d[t]>=x[t])
    {
      cout<<t+1<<' '<<d[t]-x[t]<<endl;
      return ;
    }
    x[t+1]=b-(x[t]-1);
    t++;
  }
  if(d[t]>=x[t])
   {
     cout<<t+1<<' '<<d[t]-x[t]<<endl;
     return ;
   } 
   cout<<-1<<' '<<-1<<endl;
}
int main()
{
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    int t=1;
    // cin>>t;
    while(t--)
        solve();
    return 0;
}
```
队友提供了另一种思路，~~好像~~比这个简单[liutongyu.md](https://github.com/SDIBTACM/training/blob/master/ContestReport/2023/20230507/liutongyu.md)
#### E
- **签到题不想写了**
```cpp
#include<bits/stdc++.h>
using namespace std;
void solve()
{
    int n,k;
    cin>>n>>k;
    vector<int>a(n+1),b(n+1);
    for(int i=1;i<=n;++i)cin>>a[i]>>b[i];
    bool flag=0;
    for(int i=1;i<=n;++i)
    {
        if(b[a[i]]!=b[i])
        {
            flag=1;
            break;
        }
    }
    if(flag)cout<<'N'<<endl;
    else cout<<'Y'<<endl;
}
int main()
{
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    int t=1;
    // cin>>t;
    while(t--)
        solve();
    return 0;
}
```
#### F
- **题意** 问这个人能不能在飞行途中睡够一觉
- **思路** 暴力找所有睡觉的时间够不够他想要睡一觉的，有就‘Y’，没有就‘N’。
```cpp
#include<bits/stdc++.h>
using namespace std;
void solve()
{
    int t,d,m;
    cin>>t>>d>>m;
    vector<int>a(m+1);
    for(int i=1;i<=m;++i)cin>>a[i];
    a[0]=0;
    a.push_back(d);
    bool flag=0;
    for(int i=1;i<=m+1;++i)
    {
        if(a[i]-a[i-1]>=t)
        {
            flag=1;
            break;
        }
    }
    if(flag)cout<<'Y'<<endl;
    else cout<<'N'<<endl;
}
int main()
{
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    int t=1;
    // cin>>t;
    while(t--)
        solve();
    return 0;
}
```
### 补的题目
#### C
- **题意** 一个双向电梯，有许多人想从一头到另一头，如果他到电梯时电梯的方向和他想去的方向一样，就上电梯，否则就等电梯停。
- **思路** 变长数组wait存等待的人，对于每一个人，如果到达的时间小于电梯停止的时间，人要去的方向与电梯方向(d)相同，电梯停止时间延长，否则人加入等待队列。到达时间大于停止时间，如果有人等，就让等的人走，直到所有人走完。
```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;
void solve()
{
  int n, x, y;
  cin >> n;
  vector<int> wait;
  int ans = 0, d = -1;
  for (int i = 1; i <= n; ++i)
  {
    cin >> x >> y;
    if (x < ans)
    {
      if (d == y)
        ans = x + 10;
      else
        wait.push_back(x);
    }
    else
    {
      if (wait.size())
      {
        wait.clear();
        ans += 10;
        d ^= 1;
      }
      if (x > ans)
        ans = x + 10, d = y;
      else
      {
        if (d == y)
          ans = x + 10;
        else
          wait.push_back(x);
      }
    }
  }
  if (wait.size())
    ans += 10;
  cout << ans << endl;
}
int main()
{
  solve();
  return 0;
}
```
#### D
- **题意** 一串字符只包含'A'和'B'且必须以'B'结尾，B只能向后走一位，A可以选择向后走一位或者跳过下一位到下下一位。已知所有可能的从头到尾的方法的数量，求字典序最小的字符串。
- **思路** 显然连续的B可以化简为一个B，对于串‘AB’，‘AAB’，‘AAAB’……来说，显然是对应情况的最优解，经计算的这个数列是以2，3开头的斐波那契数列。而其他情况的最优解则由若干个斐波那契数相乘得到，因为是字典序最小，所以要从最大的斐波那契数来找。
```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;
vector<unsigned ll> a;
string s;
unsigned ll n;
bool dfs(int x = a.size() - 1)
{
  if (x == -1)
  {
    if (n == 1)
      return 1;
    else
      return 0;
  }
  if (n % a[x] == 0)
  {
    s.append(x + 1, 'A');
    s.push_back('B');
    n /= a[x];
    if (dfs(x))
      return 1;
    n *= a[x];
    s.erase(s.size() - x - 2);
  }
  if(dfs(x-1))return 1;
  return 0;
}
void solve()
{
  cin >> n;
  a.push_back(2);
  a.push_back(3);
  for (int i = 2; a[i - 1] + a[i - 2] <= n; ++i)
  {
    a.push_back(a[i - 1] + a[i - 2]);
  }
  if (dfs())
    cout << s << endl;
  else
    cout << "IMPOSSIBLE" << endl;
}
int main()
{
  ios::sync_with_stdio(0);
  cin.tie(0);
  cout.tie(0);
  int t = 1;
  // cin>>t;
  while (t--)
    solve();
  return 0;
}
```
#### G
- **题意** 王位继承，如果当前的王死了，则由他的最早出生，即序号最小的儿子当国王。
- **思路** 用树做简单，首先构建这棵树，即祖父关系，然后对于每一次的死亡，从头深搜这棵树，找到第一个没有死的人。~~不是很会，没想出来，应该是这样吧？~~
```cpp
#include <bits/stdc++.h>
#define ll long long
#define endl '\n'
using namespace std;
int idx=1;
vector<int>ed[100005];
queue<int>Q;
queue<int>king;
bool vis[100005];
void dfs(int rt=1)
{
  king.push(rt);
  for(auto &i:ed[rt])
    dfs(i);
}
void solve()
{
  int q,x,y;
  cin>>q;
  while(q--)
  {
    cin>>x>>y;
    if(x==1)ed[y].push_back(++idx);
    else Q.push(y);
  }
  dfs();
  while(Q.size())
  {
    vis[Q.front()]=1;
    Q.pop();
    while(vis[king.front()])king.pop();
    cout<<king.front()<<endl;
  }
}
int main()
{
  ios::sync_with_stdio(0);
  cin.tie(0);
  cout.tie(0);
  solve();
  return 0;
}
```  
