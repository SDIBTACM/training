**组队赛第一周**

- 杨新艺
- 田始坤
- 修于航

**比赛地址**

https://vjudge.net/contest/556687#overview

**做出题目**

F

**题意**: 签到题 略 **思路**: 略

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<ll,ll> pll;
const int N=1e6+5;
const int bis=137;
const ll mod=1000000007;
const int inf=0x3f3f3f3f;

map<int,map<int,int>> mp;

int n, m;
int a[N];
vector<int> v[N];

signed main(){
  ios::sync_with_stdio(false);
  cin.tie(0);cout.tie(0);
  

  cin >> n >> m;

  for (int i = 1; i <= m; i ++) v[i].push_back(1);

  for (int i = 1; i<= n; i ++)
  {
      int x, y;
      cin >> x >> y;
      v[y].push_back(x);
      a[i] = y;
      //v[i][0] ++;
  }

  for(int i = 1; i<= m; i ++) sort(v[i].begin() +1, v[i].end());

  for (int i = 1; i <= n; i ++) 
  {
    int x = a[i];
    int y = v[x][0];
    a[i] = v[x][y];
    v[x][0] ++;
  }

  for (int i = 2; i <= n; i ++) 
    if (a[i] <= a[i - 1]) 
    {
      cout << "N" << endl;
      return 0;
    }

  cout << "Y" << endl;
  return 0;
}
```

------

E

**题意**: 有 n 个盒子，每个盒子有一个球和一个编号，每个球只有一种颜色，所有球只会有 m 种颜色，在只交换相同颜色小球的前提下，问能否将所有盒子的编号按升序排列。

**思路**: 记录每种颜色的小球来自哪些盒子，记录每个盒子应该放什么颜色的小球。将所有盒子中的小球拿出，将所有盒子按编号升序排序。此时所有盒子升序且不含小球，开始从第一个盒子开始，依次向盒子中放小球，每次放目标颜色中编号最小的球，如果最后所有盒子都有小球，说明成功。

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<ll,ll> pll;
const int N=1e6+5;
const int bis=137;
const ll mod=1000000007;
const int inf=0x3f3f3f3f;

int a[100005];
int main()
{
  int t, d, m;
  cin >> t >> d >> m;
  
  a[0] = 0;
  for (int i = 1; i <= m; i ++) cin >> a[i];
  a[m + 1] = d;

  sort(a, a + m + 2);

  for (int i = 0; i < m + 1; i ++)
  {
    if (a[i + 1] - a[i] >= t) 
    {
      cout << "Y";
      return 0;
    }
  }
  cout << "N";
  return 0;
}
```



------

B

**思路** ：

原始：用```f[i]```存取第```i```数位的取值，因为是要求得`（b+1）`最小的倍数，所以数位要从前往后跑，并且每一数位的取值要从```0->f[i]-1```，如果满足```%（b+1)==0```,则结束此程序，按照这个思路实现程序，这时你会发现```Time limit```,因为此时的时间复杂度最坏为```N*B==2e9```，因此只有优化此算法.

优化：上面的思路是先逐一改变第i位的值，从`0->f[i]-1`,问题也正是出在此地方.那么就在此处优化，尝试是否可以先求解出满足答案的解，再判断是否在`0->f[i]-1`当中，假设第```i```的数位改变```x```,第i的权值是`quickpow(b,n-i,b+1)`这时可以可以抽象出问题的实质:`r==x*a(mod(b+1))`,分析到这里就是一道扩展欧几里得的裸题了，得出满足条件的最小正整数，判断`f[i]-x`是否在`0->f[i]-1`当中。

````cpp
#include<iostream>
#include<algorithm>

#define int long long

using namespace std;
const int N=200010;

int f[N];

int quickpow(int a,int k,int mod)
{
    int res=1;
    while(k)
    {
        if(k&1) res=res*a%mod;
        k>>=1;
        a=a*a%mod;
    }
    return res;
}

int exgcd(int a,int b,int &x,int &y)
{
    if(!b)
    {
        x=1,y=0;
        return a;
    }
    int d=exgcd(b,a%b,x,y);
    int temp=x;
    x=y;
    y=temp-a/b*y;
    return d;
}

signed main()
{
    ios::sync_with_stdio(false);
    cin.tie(0),cout.tie(0);
    int b,n;
    cin>>b>>n;
    int r=0;
    for(int i=1;i<=n;i++)
    {
        cin>>f[i];
        r=(r*b%(b+1)+f[i])%(b+1);
    }
    if(r==0) 
    {
        cout<<0<<' '<<0<<endl;
        return 0;
    }
    for(int i=1;i<=n;i++)
    {
        int a=quickpow(b,n-i,b+1);
        int x=0,y=0;
        int d=exgcd(a,b+1,x,y);
        if(r%d) cout<<-1<<' '<<-1<<endl;
        else 
        {
            int k=(x*r/d%((b+1)/d)+(b+1)/d)%((b+1)/d);
            if(k<=f[i]&&k>=0) 
            {
                cout<<i<<' '<<f[i]-k<<endl;
                return 0;
            }
        }
    }
    cout<<-1<<' '<<-1<<endl;
}

````



**********

C

**题解** : 

从第一个人开始,假设第一个人在```T```时上电梯，判断后面与他同方向的人，判断 ```t(下一个上电梯的人的时间)<=T+10```是否成立。

+ 成立则证明电梯不会停下，结束时间变为```t+10```，继续判断下一个人
+ 不成立判断两个方向的已经在等待的人，找到其中最先等待的人(结束时间s)，如果是与现在同方向的，结束时间变为```s+10```,如果相反方向，要判断有多少个人已经开始等待了，这些人可以同时进入电梯，所以结束时间为```自身+10```即可
+ 然后重复判断即可

``` cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<ll,ll> pll;
const int N=1e6+5;
const int bis=137;
const ll mod=1000000007;
const int inf=0x3f3f3f3f;

priority_queue<ll,vector<ll>,greater<ll> > q[2];

signed main(){
  ios::sync_with_stdio(false);
  cin.tie(0);cout.tie(0);
  ll n;cin>>n;
  for(int i=1;i<=n;i++){
    ll x,y;cin>>x>>y;
    q[y].push(x);
  }
  ll x,y;x=0x3f3f3f3f;
  if(q[0].size()&&q[0].top()<x) x=q[0].top(),y=0;
  if(q[1].size()&&q[1].top()<x) x=q[1].top(),y=1;
  q[y].pop();
  x+=10;
  while(1){
    while(!q[y].empty()){
      if(q[y].top()<=x){
        x=q[y].top()+10;q[y].pop();
      }
      else {
        ll th=0x3f3f3f3f,tp=0;
        if(q[0].size()&&q[0].top()<th) th=q[0].top(),tp=0;
        if(q[1].size()&&q[1].top()<th) th=q[1].top(),tp=1;
        y=tp;
        if(th<x){
          ll flag=0;
          while(q[y].size()){
            if(q[y].top()<=x){
              q[y].pop();flag=1;
            }
            else break;
          }
          if(flag) x+=10;
        }
        else x=th+10;
      }
    }
    if(q[y].empty()&&q[1-y].size()){
      y=1-y;
      ll flag=0;
      while(q[y].size()){
        if(q[y].top()<=x){
          q[y].pop();flag=1;
        }
        else break;
      }
      if(flag) x+=10;
    }
    if(q[0].empty()&&q[1].empty()) break;
  }
  cout<<x<<"\n";
}
```

**********

D

**思路** ：

找规律可以看出，此题是一个斐波那契数列。

**注意** ：

1、需要从大往小跑，例如：34  小`->` 分解成 `2`和`17`（凑不出），大`->`小 (本身)

2、需要剪枝

```cpp
#include<iostream>
#include<algorithm>
#include<map>

#define int long long
#define endl '\n'
using namespace std;
const int N=1000010;

int f[N],path[N];

bool success=false;
int cnt;
map<int,int>mp;

void dfs(int u,int ans,int last)
{
    if(success) exit(0);
    if(mp.count(ans)) exit(0);
    if(ans==1)
    {
        success=true;
        for(int i=1;i<=u-1;i++) 
        {
            for(int j=1;j<=path[i];j++) cout<<'A';
            cout<<'B';
        }
        exit(0);
    }
    for(int i=last;i>=1;i--)
    {
        if(ans%f[i]) continue;
        path[u]=i;
        dfs(u+1,ans/f[i],i);
    }
}

signed main()
{
    ios::sync_with_stdio(false);
    cin.tie(0),cout.tie(0);
    
    int n;
    cin>>n;
    
    f[++cnt]=2,f[++cnt]=3;
    for(int i=3;;i++)
    {
        f[++cnt]=f[i-1]+f[i-2];
        if(f[cnt]>=n) break;
    }
    dfs(1,n,cnt);
    if(success==false) puts("IMPOSSIBLE");
}

```



************

C

**题解** : 

首先可以离线判断，因为添加子节点不会对答案有影响。从起始点1开始，如果答案节点被删除，就要遍历子节点，具体遍历方式跟先序遍历一样。注意一点，如果遍历到一个点，这个点没删除了，要继续判断他的子节点，因为本身被删除了，但是子节点仍可能是答案。然后需要优化，如果一个点，他的所有子树全被删除，那就不需要要再遍历。

``` cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<ll,ll> pll;
const int N=1e6+5;
const int bis=137;
const ll mod=1000000007;
const int inf=0x3f3f3f3f;

int res,vis[N],ans,flag,mp[N],fa[N],num[N];
vector<int> q;
vector<int> p[N];

void dfs(int x){
  int sum=0;
  for(int i=num[x];i<p[x].size();i++){//num[x]表示从左到右的第一个左右子树没有全被删除的点
    int it=p[x][i];
    if(mp[it]) continue;
    else sum++;
    if(vis[it]) dfs(it);
    if(flag) return ;
    ans=it;flag=1;break;
  }
  if(!sum) {
    mp[x]=1;num[fa[x]]++;dfs(fa[x]);//自身及所有子树全部被删除，回到父节点
  }
}

signed main(){
  ios::sync_with_stdio(false);
  cin.tie(0);cout.tie(0);
  int n;cin>>n;
  res=1;
  for(int i=1;i<=n;i++){
    int x,y;cin>>x>>y;
    if(x==1){
      p[y].push_back(++res);
      fa[res]=y;
    }
    else {
      q.push_back(y);
    }
  }
  ans=1;
  for(auto it:q){
    vis[it]=1;
    flag=0;
    if(vis[ans]) dfs(ans);
    cout<<ans<<"\n";
  }
}
```

************

