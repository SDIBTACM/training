# 周报

这周该比赛比赛，该补题补题，参与了一场服务器很差的比赛，体验感很差。

下一步要思考以后做什么工作，做好打算。

# 补题

### 牛客练习赛 96

C.现在的C都这么难了么orz， 维护一个区间值，利用滑动窗口维护，最大值最小值利用RMQ打ST表，然后需要利用二分来找。

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N=1e6+6;
const ll mod=1e9+7;
int a[N][21],b[N][21],lg[N];
ll ans;
int check(int l,int r)
{
    int z=lg[r-l+1];
    int x=max(a[l][z],a[r-(1<<z)+1][z]);
    int y=min(b[l][z],b[r-(1<<z)+1][z]);
    return x-y;
}
signed main()
{
    cin.tie(0);
	ios::sync_with_stdio(0);
    int n,k;
    cin>>n>>k;
    for(int i=2;i<=n;i++)
      lg[i]=lg[i>>1]+1;
    for(int i=1;i<=n;i++){
      int t;cin>>t;
      a[i][0]=b[i][0]=t;
    }
    for(int i=1;(1<<i)<=n;i++)
      for(int j=1;j+(1<<i)-1<=n;j++)
     {
        a[j][i]=max(a[j][i-1],a[j+(1<<i-1)][i-1]);
        b[j][i]=min(b[j][i-1],b[j+(1<<i-1)][i-1]);
     }
     for(int i=1;i<=n;i++)
     {
         int l=i,r=n;
         while(l<=r)
         {
             int mid=(l+r)>>1;
             if(check(i,mid)<k)l=mid+1;
             else r=mid-1;
         }
         int z=l;
         l=i,r=n;
         while(l<=r)
         {
             int mid=(l+r)>>1;
             if(check(i,mid)<=k)l=mid+1;
             else r=mid-1;
         }
         ans+=r-z+1;
     }
    cout<<ans<<endl;
} // namespace std;

```

## 比赛

C、裸dp

```c++
#include<iostream>
#include<algorithm>
#include<queue>
using namespace std;
typedef long long ll;
const int N=1e6+5;
const ll mod=998244353;
ll n;ll dp[N][10];
signed main()
{
    cin.tie(0);
    ios::sync_with_stdio(0);
    cin>>n;
    for(int i=1;i<=9;i++)
      dp[1][i]=1;
    for(int i=2;i<=n;i++)
      for(int j=1;j<=9;j++)
      {  
         dp[i][j]=(dp[i-1][j]+dp[i][j])%mod;
         if(j-1>=1)
         dp[i][j]=(dp[i-1][j-1]+dp[i][j])%mod;
         if(j+1<=9)
         dp[i][j]=(dp[i-1][j+1]+dp[i][j])%mod;
      }
      ll ans=0;
      for(int i=1;i<=9;i++)
       ans=(ans+dp[n][i])%mod;
      cout<<ans;
}

```

