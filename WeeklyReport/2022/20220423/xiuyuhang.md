# 周报

这周应付党课了，没怎么敲代码，下周多补补了

# 补题

### 比赛

**D - 2-variable Function** 

一开始以为是对公式的化简，看了题解，发现想多了，就是一个暴力，x^3优化一个最大值，从1e6开始算就可以了，简单题不会，好菜

```c++
#include<iostream>
#include<algorithm>
#include<string.h>
#include<string>
#include<map>
#include<vector>
#include<queue>
#include<set>
#include<math.h>
using namespace std;
typedef long long ll;
const int N=1505;
ll dp[N][N][4];
string a[N];
ll ans=1e18;
ll sub(ll x,ll y){
   return x*x*x+x*x*y+x*y*y+y*y*y;
}
signed main(){
   ll n;cin>>n;
   ll p=1e6;
   for(int i=0;i<=1e6;i++){
        while(sub(i,p)>=n&&p>=0){
           ans=min(ans,sub(i,p));
           p--;
        }
   }
   cout<<ans;
}
```

### 建筑抢修

先根据每个建筑的最多能持续的时间进行排序，从小到大，再开一个大根堆的优先队列，这样每一次读一个建筑消耗的时间，如果加上这个时间超过了最大时间，就不能加，就把他删掉即可。

```c++

#include<iostream>
#include<algorithm>
#include<string.h>
#include<string>
#include<map>
#include<vector>
#include<queue>
#include<set>
#include<math.h>
using namespace std;
typedef long long ll;
const int N=2e5;
struct ss{
   ll x,y;
}a[N];
priority_queue<ll> q;
bool cmp(ss a,ss b){
   return a.y<b.y;
}
signed main(){
   ll n;cin>>n;
   for(int i=1;i<=n;i++){
      cin>>a[i].x>>a[i].y;
   }
   sort(a+1,a+1+n,cmp);
   ll la=0,sum=0;
   for(int i=1;i<=n;i++){
      la+=a[i].x;
      q.push(a[i].x);
     if(la>a[i].y){
        la-=q.top();
        q.pop();
     }
   }
   cout<<q.size();
}

```

**E - Fraction Floor Sum**

看了看题解 ，学到一个结论，[x/i](1<=i<=n)的和不超过2*sqrt(n);

很明显，在某些区间内，x/i的值是相同的，所以可以把区间分块，然后逐个区间求和。

n/l=n/r => r=n/(n/l);

```c++
#include<iostream>
#include<algorithm>
#include<string.h>
#include<string>
#include<map>
#include<vector>
#include<queue>
#include<set>
#include<math.h>
using namespace std;
typedef long long ll;
typedef pair<ll,ll> pll;
const int N=1501;
int vis[N];
int a[N];
signed main(){
   ll n;cin>>n;
   ll l,r,ans=0;
   for(l=1,r=1;l<=n;l=r+1){
        r=n/(n/l);
        ans+=(r-l+1)*(n/l);
   }
   cout<<ans;
}

```

**E - Bishop 2** 

一开始没想搜索，感觉这种棋盘题很大可能是dp，后来仔细想了想，这个题的起点不固定，dp就不可以，然后后面搜索的时候要优化一下，开一个vis的三维数组来表示某一个点的某个方向有没有走过，走过就没必要再走一遍了。

```c++
#include<iostream>
#include<algorithm>
#include<string.h>
#include<string>
#include<map>
#include<vector>
#include<queue>
#include<set>
#include<math.h>
using namespace std;
typedef long long ll;
typedef pair<ll,ll> pll;
const int N=1501;
char a[N][N];
int vis[N][N][5],ans[N][N];
ll x[3],y[3],n;
queue<pll> q;
ll dx[4]={1,1,-1,-1},dy[4]={1,-1,1,-1};
void bfs(){
   fill(ans[0],ans[0]+N*N,0x3f3f3f3f);
   ans[x[1]][y[1]]=0;
    while(!q.empty()){
    pll t=q.front();q.pop();
    for(int i=0;i<4;i++)
      for(int j=1;j<=n;j++){
          ll px=t.first+dx[i]*j;ll py=t.second+dy[i]*j;
          if(px<1||px>n||py<1||py>n||a[px][py]=='#'||vis[px][py][i]) break;
          vis[px][py][i]=1;
          q.push(make_pair(px,py));
          ans[px][py]=min(ans[px][py],ans[t.first][t.second]+1);
      }
    }
}
signed main(){
   cin>>n;
   cin>>x[1]>>y[1]>>x[2]>>y[2];
   for(int i=1;i<=n;i++)
     for(int j=1;j<=n;j++){
        cin>>a[i][j];
     }
   q.push(make_pair(x[1],y[1]));
   bfs();
   if(ans[x[2]][y[2]]==0x3f3f3f3f) cout<<"-1";
   else cout<<ans[x[2]][y[2]];
}

```

C-牛客推荐系统开发之选飞行棋子

dp没dp明白，模拟了一下，一共四列，不能每一列都循环，四个for，直接爆，可以先判断两行，提前预处理好两行的1的数目，这样，遍历其他的两行，然后通过这两行来约束剩下的两行，注意两个数同一列的情况

```c++
#include<iostream>
#include<algorithm>
#include<string.h>
#include<string>
#include<map>
#include<vector>
#include<queue>
#include<set>
#include<math.h>
using namespace std;
typedef long long ll;
typedef pair<ll,ll> pll;
const int N=5005;
const ll mod=998244353;
int dp[N][N];
char a[5][N];
int d[5];
int sum,ans;
signed main(){
   int n;cin>>n;
  for(int i=1;i<=4;i++)
    scanf("%s",a[i]+1);
  for(int i=3;i<=4;i++)
    for(int j=1;j<=n;j++)
     if(a[i][j]=='1') d[i]++;
 for(int i=1;i<=n;i++){
    if(a[3][i]==a[4][i]&&a[3][i]=='1') sum++;
 }
 for(int i=1;i<=n;i++){
    if(a[1][i]=='0') continue;
    for(int j=1;j<=n;j++){
       if(i==j||a[2][j]=='0') continue;
       int x=d[3]-(a[3][i]-'0')-(a[3][j]-'0');
       int y=d[4]-(a[4][i]-'0')-(a[4][j]-'0');
       int p=sum-(a[3][i]-'0')*(a[4][i]-'0')-(a[3][j]-'0')*(a[4][j]-'0');
       ans+=x*y-p;
    }
 }
 cout<<ans;
}
/*orz*/

```

