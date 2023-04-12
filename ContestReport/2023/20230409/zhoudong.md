# 比赛总结
### 1.大量的时间花在了读题上面，不能像别人那样读个大概就能知道题意了，自己很容易被某些单词就卡住，读题还得慢慢翻单词书。
### 2.做题很不细心，太粗心了，因此多了很多罚时。
## 题解
A
## 题意
有一个村外的人向村内的人要钱，他就会立即给钱，并向其他两个人要钱，每个人都只会给一次钱。
## 思路
假如村外的人找A要钱，A可以向B,C要钱。假如 D 可以向 A，B,C 要钱，那么最终 A则向B,C要不到钱，因为B,C给了D。因此A就要亏钱。利用链式前向星建边，i 向 A,B则 A->i,B->i;
## 代码
```
// #include<bits/stdc++.h> //除非记不住其他头文件了，否则别用
#include<iostream>
#include<csdio>
#include<cstring>
#include<vector>
using namespace std;
#define int  long long
#define IOS ios::sync_with_stdio(0); cin.tie(0);cout.tie(0)
#define fr(i, a, b) for (int i = a; i <= b; i++)
#define fr1(i, a, b) for (int i = a; i >= b; i--)
#define ll '\n'
typedef pair<int, int> PII;
const int INF = 0x3f3f3f3f;
const int N = 1010;
const int mod1 = 1000000007;
// priority_queue<int,vector<int>,greater<int>>q; //小根堆
// priority_queue<int,vector<int>,less<int>>q1;//大根堆
#define mem(a,b) memset(a,b,sizeof a)
int head[N],nex[N*2],ed[N*2];// head 有多少个点开多大的空间，nex,ed则根据有多少边
int idx;
int vis[N],cnt[N];
void add(int a,int b) //链式前向星
{
    ed[idx]=b;
    nex[idx]=head[a];
    head[a]=idx++;
}
void dfs(int root,int fa)
{
    vis[root]=1;    //标记该点已经走过
    cnt[root]++;    // 统计该点出现了几次
    for(int i=head[root];i!=-1;i=nex[i])
    {
        int j=ed[i];
        if(j!=fa&&!vis[j])
            dfs(j,fa);
    }
}
void zyt()
{
    int n;
    cin >> n;
    mem(head,-1); //链式前向星必干
    int a[n+1],b[n+1];
    fr(i,1,n)
    {
        cin >> a[i] >> b[i];
        add(a[i],i); 
        add(b[i],i);
    }
    fr(i,1,n)
    {
        vector<int>v;
        v.push_back(i);
        v.push_back(a[i]);
        v.push_back(b[i]);
        mem(cnt,0);
        fr(j,0,2)
        {
            mem(vis,0);
            dfs(v[j],i);
        }
        int flog=0;
        fr(j,1,n)
            if(cnt[j]>=3)
            {
                flog=1;
                break;
            }
        if(flog)
            cout << "Y";
        else 
            cout << "N";
    }
}
signed main()
{
    IOS;
    int t = 1;
    // scanf("%lld",&t);
    // cin >> t;
    while (t--)
        zyt();
    return 0;
}
```
B
## 题意
A每天要从家到公司，从公司到家。如果下雨了，则必须带伞，如果没有下雨，但是目的地没有伞的话，则本次坐车也要带伞。
## 思路
每一次进行判断即可
## 代码
```
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<cstring>
#include<cmath>
#include<queue>
#include<map>
#include<stack>
using namespace std;
// #define int  long long
#define IOS ios::sync_with_stdio(0);cin.tie(0);cout.tie(0)
#define fr(i,a,b) for(int i=a;i<=b;i++)
#define fr1(i,a,b) for(int i=a;i>=b;i--)
#define ll  '\n' 
typedef pair<int,int>PII;
const int INF=0x3f3f3f3f;
const int N=10;
const int mod1=1000000007;
/*priority_queue<int,vector<int>,greater<int>>q2; //小根堆
priority_queue<int,vector<int>,less<int>>q1;//大根堆*/


void zyt()
{
    int n,k1,k2;
    cin >> n >> k1 >> k2;
    while(n--)
    {
        char a,b;
        cin >> a >> b;
        if(a=='Y'||!k2)
        {
            k1--,k2++;
            cout << "Y ";
        }
        else
            cout << "N ";
        if(b=='Y'||!k1)
        {
            k1++,k2--;
            cout << 'Y';
        }
        else
            cout << 'N';
        cout << ll;
    }
}
signed main()
{
    IOS;
    int t=1;
    // scanf("%lld",&t);
    // cin >> t;
    while(t--)
        zyt();
    return 0;
}
```
C
## 题意
有一个 N 的方格，有 1,2,3''''N大小的的瓷砖，各一个。 给一个 N，并在上面一个 大小为K 的瓷砖，放完之后最左边有 E 个空格。
## 思路
这个 N 被分成了三部分，中间的那部分题目已经放了，我们只需要放最左边(k1)和最右边的空格(k2)。
我们先遍历min(k1,k2)到1，如果 i 没有被使用，则放在空格上，如果 i 被使用了，我们再判断 i 能否分成两个没有使用的小瓷砖上。
## 代码
```
#include <iostream>
#include <algorithm>
#include <cstdio>
#include <cstring>
#include <cmath>
#include <queue>
#include <map>
#include <stack>
using namespace std;
// #define int  long long
#define IOS ios::sync_with_stdio(0); cin.tie(0);cout.tie(0)
#define fr(i, a, b) for (int i = a; i <= b; i++)
#define fr1(i, a, b) for (int i = a; i >= b; i--)
#define ll '\n'
typedef pair<int, int> PII;
const int INF = 0x3f3f3f3f;
const int N = 10;
const int mod1 = 1000000007;
// priority_queue<int,vector<int>,greater<int>>q; //小根堆
// priority_queue<int,vector<int>,less<int>>q1;//大根堆

map<int,int>mp;
bool cha(int x)
{
    if(mp[x]==0) //没有被使用，则直接放上去
    {
        mp[x]=1; //并标记
        return 1;
    }
    fr(i,1,x/2)
    {
        int m=x-i;  
        if(i==m)   // 每一种只有一个，不能分成两同的两块
            continue;
        if(!mp[m]&&!mp[i]) //两个都没有被使用
        {
            mp[m]=1;
            mp[i]=1;
            return 1;
        }
    }
    return 0;
}
void zyt()
{
    int n,k,f1;
    cin >> n >> k >> f1;
    int f2=n-k-f1;  // 计算出 左边和右边
    mp[k]=1;
    if(f1>f2)
        swap(f1,f2);
    for(int i=f1;i>=1;i--) //遍历
        if(cha(i))
        {
            f1-=i;
            break;
        }
    for(int i=f2;i>=1;i--)
        if(cha(i))
        {
            f2-=i;
            break;
        }
    cout << f1+f2 << ll;
}
signed main()
{
    IOS;
    int t = 1;
    // scanf("%lld",&t);
    // cin >> t;
    while (t--)
        zyt();
    return 0;
}
```
D
## 题意
A 去吃鱼，选择一个起点，每一次只能吃该点上下左右比该点大的鱼。吃过的地方，都可以当做新的起点。问最多能吃多少条鱼。
## 思路
遍历每一个点，取最大值。（假如一个点，比上下左右某一点大的话，那么从比它小的哪一点开始，至少要比该点 多一条），则可以直接排除某一些点。 用普通队列即可。
## 代码
```
#include<iostream>
#include<algorithm>
#include<stdio.h>
#include<string.h>
#include<math.h>
#include<queue>
#include<map>
#include<stack>
// #include<bits/stdc++.h>
using namespace std;
// #define int  long long
#define IOS ios::sync_with_stdio(0);cin.tie(0);cout.tie(0)
#define fr(i,a,b) for(int i=a;i<=b;i++)
#define fr1(i,a,b) for(int i=a;i>=b;i--)
#define ll  '\n' 
typedef pair<int,int>PII;
const int INF=0x3f3f3f3f;
const int N=10;
const int mod1=1000000007;
// priority_queue<int,vector<int>,greater<int>>q; //小根堆
// priority_queue<int,vector<int>,less<int>>q1;//大根堆


int dr[][2]={-1,0,1,0,0,-1,0,1};
typedef struct{
    int x,y,c;
}STU;
/*struct cmp
{
    bool operator()(STU A,STU B)
    {
        return A.c<B.c;
    }
};
priority_queue<STU,vector<STU>,cmp>q; //小根堆*/
bool operator<(const STU &A,const STU &B)
{
    return A.c<B.c;
}
priority_queue<STU>q;
int a[110][110];
int vis[110][110];
int ans=0;
int n,m,k;
void bfs(int x,int y,int w)
{
    q.push({x,y,w});
    vis[x][y]=1;
    while(q.size())
    {
        STU tem=q.top();
        q.pop();
        fr(i,0,3)
        {
            int dx=tem.x+dr[i][0];
            int dy=tem.y+dr[i][1];
            if(vis[dx][dy]==1||dx<1||dx>n||dy<1||dy>m||a[dx][dy]<tem.c)
                continue;
            vis[dx][dy]=1;
            k++;
            q.push({dx,dy,a[dx][dy]});
        }
    }
}
void zyt()
{
    cin >> n >> m;
    fr(i,1,n)
        fr(j,1,m)
            cin >> a[i][j];
    fr(i,1,n)
        fr(j,1,m)
        {
            if(a[i][j-1]&&a[i][j]>a[i][j-1])
                continue;
            if(a[i][j+1]&&a[i][j]>a[i][j+1])
                continue;
            if(a[i-1][j]&&a[i][j]>a[i-1][j])
                continue;
            if(a[i+1][j]&&a[i][j]>a[i+1][j])
                continue;
            k=1;
            memset(vis,0,sizeof vis);
            bfs(i,j,a[i][j]);
            ans=max(ans,k);
        }
    cout << ans << ll;
}
signed main()
{
    IOS;
    int t=1;
    // scanf("%lld",&t);
    // cin >> t;
    while(t--)
        zyt();
    return 0;
}
```
```
/*
typedef struct{
    int x,y,c;
}STU;
bool operator <(const STU &B,const STU &C)
{
    return B.c>C.c;
}
priority_queue<STU>q;*/

/*struct STU
{
    int x,y,c;
    bool operator < (const STU &A)const 
    {
        return this->c > A.c;
    }
};
priority_queue<STU>q;*/


/*struct STU
{
    int x,y,c;
    friend bool operator <(const STU &A,const STU &B)
    {
        return A.c > B.c;
    }
};
priority_queue<STU>q;*/

/*typedef struct{
    int x,y,c;
}STU;
struct cmp
{
    bool operator()(STU A,STU B)
    {
        return A.c<B.c;
    }
};
priority_queue<STU,vector<STU>,cmp>q; //小根堆*/
优先队列，四种写法
```
E
## 题意
就是把螺母转进去，但螺母在外面的时候可以翻转。 当同一个位置都是1的时候则螺母转不动了。螺母可以左转，右转。
## 思路
暴力跑，把每一种情况跑一遍。走过的点，标记一下，节省时间。
## 代码
```
#include<bits/stdc++.h>  //GUN G++ 20 11.2.0 会编译错误，直接踩雷，以后用 GUN G++ 17
/*#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>*/
using namespace std;
#define int  long long
#define IOS ios::sync_with_stdio(0); cin.tie(0);cout.tie(0)
#define fr(i, a, b) for (int i = a; i <= b; i++)
#define fr1(i, a, b) for (int i = a; i >= b; i--)
#define ll '\n'
typedef pair<int, int> PII;
const int INF = 0x3f3f3f3f;
const int N = 1010;
const int mod1 = 1000000007;
// priority_queue<int,vector<int>,greater<int>>q; //小根堆
// priority_queue<int,vector<int>,less<int>>q1;//大根堆
#define mem(a,b) memset(a,b,sizeof a)
int n,m;
string s;
char a[1010][110];
int vis[1010][110];
bool flog;
bool check(int &d,int &st)
{
    fr(i,1,m)   //判断这一次，M个位置是否有都是1的
    {
        if(a[d][i]=='1'&&s[st+i-1]=='1')
            return false;
    }
    return true;
}
void dfs(int d,int st)
{
    if(st==0)   st=m;  //螺母到最左边了
    if(d==0)    return ; // 螺母都出去了
    if(st==m+1) st=1;  // 转到头了，从第一个开始
    if(!check(d,st))   return ; //标记
    if(vis[d][st]) return ;
    vis[d][st]=1;
    if(d==n)
    {
        flog=1;
        return ;
    }
    dfs(d-1,st);  //要么回一行，要么继续下去，要么螺母左转，要么螺母右转。
    dfs(d+1,st);
    dfs(d,st-1);
    dfs(d,st+1);
}
void zyt()
{
    cin >> n >> m;
    cin >> s;
    s=' '+s+s;
    fr(i,1,n)
        cin >> a[i]+1;
    fr(i,1,m)
        dfs(1,i);
    reverse(s.begin()+1,s.end());
    mem(vis,0);
    fr(i,1,m)
        dfs(1,i);
    flog?cout << 'Y':cout << 'N';
}
signed main()
{
    IOS;
    int t = 1;
    // scanf("%lld",&t);
    // cin >> t;
    while (t--)
        zyt();
    return 0;
}
```
