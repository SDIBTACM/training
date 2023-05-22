**组队赛第二周**

- 杨新艺
- 田始坤
- 修于航

**比赛地址**

https://vjudge.csgrandeur.cn/contest/557451#overview

**做出题目**

D. Different Pass a Ports

**题解** :  从一个点出发，走固定的步数/路程，问有多少种走的方式，矩阵快速幂的模板。
代码:

``` cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<ll,ll> pll;
const int N=1e5+5;
const int bis=137;
const ll mod=1e9+7;
const int inf=0x3f3f3f3f;
 
struct matrix
{
  int a[110][110];
  int N;
  matrix(int num){
    N=num;
    memset(a,0,sizeof(a));
  }
  void build(){
    for(int i=1;i<=N;i++) a[i][i]=1;
  }
  int *operator[](int i){
    return a[i];
  }
  matrix operator*(matrix& x){
    matrix temp(N);
    for(int i=1;i<=N;i++)
      for(int j=1;j<=N;j++)
        for(int k=1;k<=N;k++){
          temp[i][j]=(temp[i][j]+(1ll*a[i][k]*x[k][j])%mod)%mod;
        }
    return temp;
  }
  matrix operator^(ll p){
    matrix res(N);res.build();
    matrix A=*this;
    while(p>0){
      if(p&1) res=res*A;
      A=A*A;
      p>>=1;
    }
    return res;
  }
};
 
signed main(){
  ios::sync_with_stdio(false);
  cin.tie(0);cout.tie(0);
  int n,m,k;
  cin>>n>>m>>k;
  matrix res(n);
  for(ll i=1;i<=m;i++){
    ll x,y;cin>>x>>y;
    res[x][y]++;res[y][x]++;
  }
  res=res^k;
  ll ans=0;
  for(int i=1;i<=n;i++) ans=(ans+res[1][i])%mod;
  cout<<ans;
}
```

------



E. Erudite of words

**题解*** : 分为一下几步

+ 首先从m个单词中选择k个单词，C(m,k)
+ 判断选择了k个单词一共可以构成多少种字符串，我们假设f[i]为选择了i个单词构成的字符串种类数，可以发现直接求不好求，很容易超时，所以我们可以采用递推的方式求，设f[i]的初始值为pow(i,n),这里肯定包含一些用的单词数不足i的情况，所以需要减去单词数为i-1,i-2,i-3....1的情况
+ 最后f[k]*C(m,k); 
+ 注意开long long
  代码:

``` cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N=1e6+5;
const ll mod=1e9+7;

int a[N];
ll fact[N],infact[N],ans[N];

ll qsm(ll x,ll y){
  ll res=1;
  while(y){
    if(y&1) res=(res*x)%mod;
    y>>=1;
    x=(x*x)%mod;
  }
  return res;
}

void init(){
  fact[0]=1;
  for(int i=1;i<N;i++){
    fact[i]=fact[i-1]*i%mod;
  }
  infact[N-1]=qsm(fact[N-1],mod-2)%mod;
  for(int i=N-1;i>=1;i--) infact[i-1]=infact[i]*i%mod;
}

ll solve(ll x,ll y){
  return fact[x]*infact[y]%mod*infact[x-y]%mod;
}

signed main(){
  ios::sync_with_stdio(false);
  cin.tie(0);cout.tie(0);
  init();
  int n,m,k;cin>>n>>m>>k;
  for(int i=1;i<=k;i++){
    ans[i]=qsm(i,n);
    for(int j=1;j<i;j++){
      ans[i]=(ans[i]-(solve(i,j)*ans[j])%mod+mod)%mod;
    }
  }
  cout<<(ans[k]*solve(m,k)%mod+mod)%mod;
}
```

------



F. Froginald the frog

**题解** : 每一个位置i只与i-1,i-2这两个位置有关，所以f[i]=f[i-1]+f[i-2],标准的斐波那契数列，数据范围为1e9,所以要用矩阵快速幂加速

代码：

``` cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<ll,ll> pll;
const int N=1e5+5;
const int bis=137;
const ll mod=1e9+7;
const int inf=0x3f3f3f3f;

vector<ll> q;

struct matrix
{
  ll a[4][4];
  int N;
  matrix(){
    memset(a,0,sizeof(a));
  }
  void build(){
    for(int i=1;i<=2;i++) a[i][i]=1;
  }
  ll *operator[](int i){
    return a[i];
  }
  matrix operator*(matrix& x){
    matrix temp;
    for(int i=1;i<=2;i++)
      for(int j=1;j<=2;j++){
        temp[i][1]=(temp[i][1]+(x[i][j]*a[j][1])%mod)%mod;
      }
    return temp;
  }

  matrix solve(matrix p,matrix q){
    matrix res;
    for(int i=1;i<=2;i++)
      for(int j=1;j<=2;j++)
        for(int k=1;k<=2;k++){
          res[i][j]=(res[i][j]+(p[i][k]*q[k][j])%mod)%mod;
        }
    return res;
  }

  matrix operator^(ll p){
    matrix res;res.build();
    matrix A=*this;
    while(p>0){
      if(p&1) res=solve(res,A);
      A=solve(A,A);
      p>>=1;
    }
    return res;
  }
};

signed main(){
  ios::sync_with_stdio(false);
  cin.tie(0);cout.tie(0);
  ll n,m;cin>>n>>m;
  matrix res,ans;
  res[1][1]=res[1][2]=res[2][1]=1;
  res[2][2]=0;
  ans[1][1]=2;ans[2][1]=1;
  for(int i=1;i<=m;i++){
    ll x;cin>>x;q.push_back(x);
  }
  sort(q.begin(),q.end());
  for(auto x:q){
    if(x==1) ans[2][1]=0,ans[1][1]--;
    if(x==2) ans[1][1]=0;
  }
  q.push_back(n+1);
  ll th=0;
  while(th<q.size()-1&&q[th]<=2){
    th++;
  }
  ll beg=3;
  if(q[th]<3){
    cout<<"0";return 0;
  }
  while(1){
    matrix tmp;
    if(q[th]-beg>0){
      tmp=res^(q[th]-beg);
      ans=ans*tmp;
    }
    if(th==q.size()-1) break;
    beg=q[th]+1;th++;
    ans[2][1]=ans[1][1];
    ans[1][1]=0;
  }
  cout<<ans[1][1];
}
```

************

B. Building 5G antennas

**题解** ：

朴素：给定一种情况：当一个节点上一层连接n/2个节点，下一层也连接n/2个节点，此时的时间复杂度为n*n/4(不可取)。

优化：可以设一个数组dist[]，用来存储每个节点最早从第几步开始遍历，当再次遍历到该节点时的开始步数>=dist[v],则退出dfs()，因为在k确定的情况下，dist[v]越小则代表着可以遍历到更深的层次中。此时的时间复杂度为n*k。

```c++
#include<iostream>
#include<algorithm>
#include<queue>
#include<vector>
#include<cstring>

#define int long long 
#define x first
#define y second

using namespace std;
typedef pair<int,int> PII;
const int N=100010,Inf=0x3f3f3f3f3f3f3f3f;

priority_queue<int,vector<int>,greater<int> >q;

vector<int>v[N];
int n,k;
int dist[N];

void dfs(int u,int fa,int ans)
{
    if(ans>k) return ;
    if(dist[u]<=ans) return ;
    if(dist[u]==Inf) q.push(u);
    dist[u]=ans;
    for(auto i:v[u])
    {
        if(i==fa) continue;
        dfs(i,u,ans+1);
    }
}

void solve()
{
    for(int i=1;i<=n;i++) dist[i]=-1;
    memset(dist,0x3f,sizeof dist);
    q.push(1);
    dist[1]=2;
    while(q.size())
    {
        int res=q.top();
        q.pop();
        cout<<res<<' ';
        dfs(res,-1,0);
    }
}

signed main()
{
    ios::sync_with_stdio(false);
    cin.tie(0),cout.tie(0);
    
    cin>>n>>k;
    for(int i=1;i<=n-1;i++)
    {
        int a,b;
        cin>>a>>b;
        v[a].push_back(b),v[b].push_back(a);
    }
    
    solve();
    
    return 0;
}
```

****************

G. Jeffrey's ambition

**题解** ：最大匹配数的裸题：匈牙利算法

```c++
#include<iostream>
#include<algorithm>
#include<cstring>
#include<queue>

using namespace std;
const int N=100010,M=N*2;

int h[N],ne[M],e[M],idx;
int match[N];
bool st[N];

vector<int>v;

void add(int a,int b)
{
    e[idx]=b,ne[idx]=h[a],h[a]=idx++;
}

bool find(int u)
{
    for(int i=h[u];i!=-1;i=ne[i])
    {
        int j=e[i];
        if(!st[j])
        {
            st[j]=true;
            v.push_back(j);
            if(!match[j]||find(match[j]))
            {
                match[j]=u;
                return true;
            }
        }
    }
    return false;
}

signed main()
{
    ios::sync_with_stdio(false);
    cin.tie(0),cout.tie(0);
    
    int n,m;
    cin>>n>>m;
    
    memset(h,-1,sizeof h);
    
    for(int i=1;i<=n;i++)
    {
        int k;
        cin>>k;
        for(int j=1;j<=k;j++)
        {
            int x;
            cin>>x;
            add(i,x);
        }
    }
    
    int res=0;
    
    for(int i=1;i<=n;i++)
    {
        for(int j=v.size()-1;j>=0;j--)
        {
            st[v[j]]=0;
            v.pop_back();
        }
        if(find(i)) res++;
    }
    cout<<m-res<<endl;
}
```

**********

K. Kilo Waste

**题解** ：完全背包的裸题

```c++
#include<iostream>
#include<algorithm>

#define int long long 

using namespace std;
const int N=51000;

int v[110],w[110];
int dp[N],f[N];

signed main()
{
    int n,m;
    cin>>m>>n;
    for(int i=1;i<=n;i++) cin>>v[i],w[i]=v[i];
    for(int i=1;i<=m;i++) cin>>f[i];
    //f[i][j]=max(f[i-1][j],f[i-1][j-v[i]]);
    dp[0]=1;
    for(int i=1;i<=n;i++)
    for(int j=v[i];j<=50100;j++) dp[j]=dp[j]|dp[j-v[i]];
    
    for(int i=1;i<=m;i++)
    {
        for(int j=f[i];;j++)
        {
            if(dp[j])
            {
                cout<<j-f[i]<<endl;
                break;
            }
        }
    }
    return 0;
}
```

------

H.Kilo Waste

**题解** ：完全背包裸题，需要注意的是完全背包求的是“不超过体积的最大价值”，而本题求的是最小浪费，需要特判什么时候不小于目标重量，本题时间宽裕可以暴力，如果要求严格可以提前存好然后二分。

```cpp
#include <iostream>
#include <algorithm>
#include <cstring>
#include <queue>
#include <vector>
#include <stack>
#include <map>

using namespace std;

typedef pair<int, int> PII;

const int N = 55, M = 1e5 + 10;

int k, p;
int a[N], f[M]; //f[i]表示凑出的重量不大于i的最大值

int main()
{
    cin >> k >> p;
    for (int i = 1; i <= p; i ++) cin >> a[i];

    for (int i = 1; i <= p; i ++)
    {
        for (int j = a[i]; j < M; j ++)
            f[j] = max(f[j], f[j - a[i]] + a[i]);
    }

    while (k -- )
    {
        int b;
        cin >> b;
        for (int i = b; ; i ++)
        {
            if (f[i] >= b) 
            {
                cout << f[i] - b << endl;
                break;
            }
        }
    }
    system("pause");
    return 0;
}
```

