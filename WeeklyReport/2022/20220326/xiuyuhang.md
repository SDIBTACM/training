# 周报

这周还是重复之前的工作，补题，打比赛。

# 补题

### 比赛

### C - Collision 2

只需要判断每一行会不会出现相反方向，以及找出向右的最小的和向左最大的，判断横坐标即可。

```c++
#include<iostream>
#include<algorithm>
#include<math.h>
#include<string.h>
#include<string>
#include<map>
using namespace std;
typedef long long ll;
const int N=2e5+5;
int ans1,ans2;
map<ll,ll> maxn,minn;
map<ll,ll> visl,visr;
map<ll,ll> vis;
pair<ll,ll> a[N];
vector<ll> p;
signed main()
{
     ll n;cin>>n;
     for(ll i=1;i<=n;i++){
         cin>>a[i].first>>a[i].second;
     }
     string s;
     cin>>s;
     for(ll i=1;i<=s.size();i++){
          if(!vis[a[i].second]){ vis[a[i].second]=1;p.push_back(a[i].second);}
          if(s[i-1]=='L'){
               if(!visl[a[i].second]){visl[a[i].second]=1; maxn[a[i].second]=a[i].first;}
               else maxn[a[i].second]=max(maxn[a[i].second],a[i].first);  
          }
          else {
               if(!visr[a[i].second]){visr[a[i].second]=1; minn[a[i].second]=a[i].first;}
               else minn[a[i].second]=min(minn[a[i].second],a[i].first);
          }
     }
     for(ll i=0;i<p.size();i++){
          if(!visr[p[i]]||!visl[p[i]]) continue;
          if(maxn[p[i]]>=minn[p[i]]){
             cout<<"Yes";return 0;}
     }
     cout<<"No";
}



```

### D - Moves on Binary Tree

就是R就*2+1，L就乘2，U就除2，但是数字比较大，直接做不优化的化ll存不下，第一反应就是用高精度写，但是，不太会。。。然后看了看大佬的，知道可以把u和R L抵消从而优化。

```c++
#include<iostream> 
#include<algorithm>
#include<string>
#include<math.h>
#include<vector>
using namespace std;
typedef long long ll;
vector<char> q;
signed main()
{
    ll n,x;cin>>n>>x;
    string s;cin>>s;
    for(int i=0;i<s.size();i++){
        if(s[i]=='U'){
            if(!q.empty()&&q.back()!='U')  q.pop_back();
            else q.push_back(s[i]);
        
        }
        else{
            q.push_back(s[i]);
        } 
    }
    for(int i=0;i<q.size();i++){
        if(q[i]=='U')
            x/=2;
        else if(q[i]=='L') x*=2;
        else x=x*2+1;
    }
    cout<<x;
}


```

