# 周报

这个周平时还是补补题，临近考试周了，还有国赛，事情好多

# 补题

题意:给定一个字符串，每次操作可以选择这个字符串中的一种字符，将他们全部都减1，最多K次操作，问可以形成的字典大小最小的字符串。

题解:首先我们观察这个字符串，很明显，如果两个字符不相同，我们把大的减小到和小的相等，然后再两个一起减小，就省去了一部分的操作，从而减少操作次数，因此，我们先把每种字符按照第一个先后出现的顺序存储，然后遍历这个数组，判断你当前读入的字符是不是到现在为止的最大字符，如果不是，就证明这个字符比前面的小，这样这个字符肯定就已经被连带减小了

例如：c b 在减小c的时候，当c等于了b，就可以同时减小两个。 所以当该字符不是最大字符时，就不需要管它。如果是，就判断该字符与上一个最大字符的差值，即你要变成上一个字符需要几次操作，然后与前面需要的操作相加，看看是否超过最大操作数，如果超过就能减少几个减少几个，没超过就加到总操作数里继续看下一个字符。

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<ll,ll> pll;
const int N=4e4+5;
const ll mod=1e9+7;
ll dp[N],vis[26];
vector<ll> q[N],p;
signed main(){
  ios::sync_with_stdio(false);
  cin.tie(0);
  ll t;cin>>t;
  while(t--){
    ll n,m;cin>>n>>m;
    string s;cin>>s;
    p.clear();
    memset(vis,0,sizeof(vis));
    for(ll i=0;i<=25;i++) q[i].clear();
    for(ll i=0;i<s.size();i++){
        q[s[i]-'a'].push_back(i);//记录该字符出现的位置
        if(!vis[s[i]-'a']){
          vis[s[i]-'a']=1;
          p.push_back(s[i]-'a');//记录每种字符出现的先后顺序
        }
    }
    ll sum=0;
    ll maxn=-1;
    ll last=0;
    for(ll i=0;i<p.size();i++){
         maxn=max(maxn,p[i]);
         if(p[i]!=maxn) continue;//判断是否最大值
         if(p[i]-last+sum>m){
           for(ll j=0;j<q[p[i]].size();j++){
             s[q[p[i]][j]]-=(m-sum);//将每个位置的字符能减少几减少几
           }
           for(ll j=p[i]-(m-sum);j<p[i];j++){
             for(ll z=0;z<q[j].size();z++){
               s[q[j][z]]=p[i]-(m-sum)+'a';//减少过程中，判断有没有比它小的字符被连带减小，有就更新
             }
           }
           break;
         }
         for(ll z=0;z<=p[i];z++){
           for(ll j=0;j<q[z].size();j++){
              s[q[z][j]]='a';//如果操作数足够，就肯定会减少到 a
           }
           q[z].clear();//该字符减少完后就没有了，所以要清零
         }
         sum+=p[i]-last;
         last=p[i];
    }
    cout<<s<<endl;
  }
}
```



 题意:一棵树n个点，m条路， di表示1-i的距离，问怎么选择边可以使得d2+...dn最短。

题解: 很明显，就是直接套最短路板子，判断每一个点的最短路，题目里没有负权值，直接dijkstra就可以了。边如何记录，与每个点相连的那个最短路的边，每次更新出来储存，然后直接输出即可。

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<ll,ll> pll;
const int N=5e5+5;
ll vis[N],pre[N];
ll dis[N],cnt,head[N];
ll ans[N];
struct ss{
  ll next,to,w,id;
}e[N];
void add(ll x,ll y,ll w,ll id){
  e[++cnt].to=y;
  e[cnt].w=w;
  e[cnt].id=id;
  e[cnt].next=head[x];
  head[x]=cnt;
}
void dij(){
   priority_queue<pll,vector<pll>,greater<pll> >q;
   memset(dis,0x3f,sizeof(dis));
   memset(vis,0,sizeof(vis));
   q.push({0,1});
   dis[1]=0;
   while(!q.empty()){
     ll v=q.top().first;
     ll u=q.top().second;
     q.pop();
     if(vis[u]) continue;
     vis[u]=1;    
     for(ll i=head[u];i;i=e[i].next){
       ll j=e[i].to;
       if(dis[j]>v+e[i].w){
         dis[j]=dis[u]+e[i].w;
         q.push({dis[j],j});
         ans[j]=e[i].id;
       }
     }
   } 
}
signed main(){
  ios::sync_with_stdio(false);
  cin.tie(0); 
  ll n,m;cin>>n>>m;
  for(ll i=1;i<=m;i++){
    ll x,y,w;cin>>x>>y>>w;
    add(x,y,w,i);
    add(y,x,w,i);
  }
  dij();
  for(int i=2;i<=n;i++){
    cout<<ans[i]<<" ";
  }
} 
```
