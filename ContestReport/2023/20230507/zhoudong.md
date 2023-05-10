# 组队赛第一周
- 周东
- 马宝运
- 王智炜
- 林涛

## 比赛地址
- https://vjudge.net/contest/556687#overview

# 题解

## B题
- 题意： 有一个长度为n的B进制数，问你能否减小某一位上的数，使得其可以整除B+1，输出修改的位置和修改后的数，如果不能满足条件输出−1,−1，不用修改则输出0,0 
- 思路：对任意一个数 x ^ n % (x + 1)的值只为1和x，其中当n为奇位时为x，否则为1，利用这个性质可以简单的写出代码，要保证是最小的，则从最高位开始遍历。
```
// #include<bits/stdc++.h>
#include<iostream>
#include<cstring>
#include<algorithm>
#include<map>
#include<vector>
#include<queue>
#define ll '\n'
using namespace std;
// #define int long long
#define IOS ios::sync_with_stdio(false);cin.tie(NULL);cout.tie(NULL)
#define fr(i,a,b) for(int i=a;i<=b;i++)
#define fr1(i,a,b) for(int i=a;i>=b;i--)
typedef pair<int,int>PII;
const int INF=0x3f3f3f3f;
const int N=100010;
// priority_queue<int,vector<int>,less<int>>q;//大根堆
// map<int,int>mp;
// vector<vector<int>>v(10,vector<int>(10,0)); //二维数组
void zyt()
{
    int b,n;
    cin >> b >> n;
    int a[n+1];
    int yu=0;
    fr(i,1,n)
    {
        cin >> a[i];
        yu=(yu*b%(b+1)+a[i])%(b+1); // 大数求余,从高位开始
    }
    if(yu==0)
    {
        cout << "0 0" << ll;
        return ;
    }     
    for(int i=1,j=n;i<=n;i++,j--)
    {
        if(j&1)     //该位为偶数位，下标从1开始的
        {
            if(a[i]>=yu)
            {
                cout << i << ' ' << a[i]-yu << ll;
                return ;
            }
        }
        else    // 该位为奇数位
        {
            if(a[i]>=b+1-yu)
            {
                cout << i << ' ' << a[i]-b-1+yu << ll;
                return ;
            }
        }
    }
    cout << "-1 -1" << ll;
}
signed main()
{
    IOS;
    int t=1;
    // cin >> t;
    while(t--)
        zyt();
    return 0;
}
```

## C题
- 题意：电梯两端有乘客, 电梯一次只能向一个方向运行, 并且在当乘客登上电梯后, 若是在 10s 内依旧有同方向乘客登上电梯, 10s 倒计时重新计时, 否则就按照两端首位乘客的到达时间决定电梯先向哪个方向运行. 计算两方向乘客都互相做过电梯后的消耗时间.
- 思路：开两个队列，把相同方向的，放到同一个队列里面。然后模拟：比较两个队列，那个队头先到。判断该方向10S内，是否还有乘客能登上电梯，若有再继续判断其后10s后的情况。
```
// #include<bits/stdc++.h>
#include<iostream>
#include<cstring>
#include<algorithm>
#include<map>
#include<vector>
#include<queue>
#define ll '\n'
using namespace std;
// #define int long long
#define IOS ios::sync_with_stdio(false);cin.tie(NULL);cout.tie(NULL)
#define fr(i,a,b) for(int i=a;i<=b;i++)
#define fr1(i,a,b) for(int i=a;i>=b;i--)
typedef pair<int,int>PII;
const int INF=0x3f3f3f3f;
const int N=100010;
// priority_queue<int,vector<int>,less<int>>q;//大根堆
// map<int,int>mp;
// vector<vector<int>>v(10,vector<int>(10,0)); //二维数组
void zyt()
{
    int n;
    cin >> n;
    queue<int>q[2];
    fr(i,1,n)
    {
        int a,b;
        cin >> a >> b;
        q[b].push(a); //  同方向的，放到相同队列里面。
    }
    int ed=0;
    while(q[1].size()||q[0].size())
    {
        if(q[0].size()==0)  //当某一个方向没有人了
        {
            ed=max(ed,q[1].front())+10; //大循环保证必有一个队列有人
            q[1].pop();                 // 只要上来人，就会加 10S
            while(q[1].size())
            {
                ed=max(ed,q[1].front()+10);
                q[1].pop();
            }
            break;
        }
        if(q[1].size()==0)
        {
            ed=max(ed,q[0].front())+10;
            q[0].pop();
            while(q[0].size())
            {
                ed=max(ed,q[0].front()+10);
                q[0].pop();
            }
            break;
        }
        if(q[0].front()<q[1].front())
        {
            ed=max(ed,q[0].front())+10;
            q[0].pop();
            while(q[0].size()&&q[0].front()<=ed) //保证上来的人是在连续的时间内
            {
                ed=max(ed,q[0].front()+10);
                q[0].pop();
            }
        }
        else
        {
            ed=max(ed,q[1].front())+10;
            q[1].pop();
            while(q[1].size()&&q[1].front()<=ed)
            {
                ed=max(ed,q[1].front()+10);
                q[1].pop();
            }
        }
    }
    cout << ed << ll;
}
signed main()
{
    IOS;
    int t=1;
    // cin >> t;
    while(t--)
        zyt();
    return 0;
}
```
## D题
- 题意：给一个数字n，问能不能组成一个训练方式AB序列，A可以到达相邻的两个字母，B只能到达相邻的字母。
- 思路：通过列举可以得知 AB的构造，可以构造菲波那切数列。AB 可以增加2种方式，AAB 可以增加3种，AAAB 可以增加5种。我们就可以判断这个n，有哪些菲波那切数相乘构成。先确定这些数，然后再构造AB序列。
```
// #include<bits/stdc++.h>
#include<iostream>
#include<cstring>
#include<algorithm>
#include<unordered_map>
// #include<map>
#include<vector>
#include<queue>
#define ll '\n'
using namespace std;
#define int long long int
#define IOS ios::sync_with_stdio(false);cin.tie(NULL);cout.tie(NULL)
#define fr(i,a,b) for(int i=a;i<=b;i++)
#define fr1(i,a,b) for(int i=a;i>=b;i--)
typedef pair<int,int>PII;
const int INF=0x3f3f3f3f;
const int N=1e5+10;
// priority_queue<int,vector<int>,less<int>>q;//大根堆
// map<int,int>mp;
// vector<vector<int>>v(10,vector<int>(10,0)); //二维数组
vector<int>v;
unordered_map<int,int>dp;
unordered_map<int,int>vis;
unordered_map<int,int>mp;
int tt(int n)    //判断这个n有哪些菲波那切数构成
{
    if(n==1)
        return 1;
    if(vis[n])  //当这个数已经判断过了，则可以返回了
        return dp[n];
    int idx=lower_bound(v.begin(),v.end(),n)-v.begin(); //找到集合中，第一个大于等于n的数
    if(idx==v.size())
        idx--;
    for(int i=idx;i>=0;i--)
    {
        if(n%v[i]==0) //如果这个数是n的因数，则标记
        {
            int res=tt(n/v[i]);
            if(res==1) //当这些数相乘==n时，才证明找到了答案，则返回。
            {            //要是算到一半，发现这几个因数，最终的结果！=n，则返回重新找。
                vis[n]=1;     // 因为要保证字典序最下，所以先找大的，因为优先考虑了大数则不能保证答案是正确的
                mp[n]=v[i];
                return dp[n]=1;
            }
        }
    }
    vis[n]=1;
    return dp[n]=0;
}
void zyt()
{
    int n;
    cin >> n;
    v.push_back(2);
    v.push_back(3);
    while(1)     //斐波那切数的构造，因为数比较大，用容器比较好点
    {
        int m=v.size();
        int x=v[m-1]+v[m-2];
        if(x<=n)
            v.push_back(x);
        else
            break;
    }
    if(tt(n)==0)
    {
        cout << "IMPOSSIBLE" << ll;
        return ;
    }
    vector<int>ans;
    while(n>1)   
    {
        ans.push_back(mp[n]); //确定那些因数构成了这个 n
        n/=mp[n];
    }
    string s;
    for(auto &i:ans)
    {
        int idx=lower_bound(v.begin(),v.end(),i)-v.begin();
        string t="";   // 2 AB 3 AAB 5 AAAB 8 AAAAB
        fr(i,0,idx)
            t+='A';
        t+='B';
        s+=t;
    }
    cout << s << ll;
}
signed main()
{
    IOS;
    int t=1;
    // cin >> t;
    while(t--)
        zyt();
    return 0;
}
```

## E题
- 题意：有n块，m种颜色，给你n块，你可以交换任意两个相同颜色的块，保证最后的循序是 自然数循序
- 思路：开两个数组，使内容相同，将第一个数组根据第一个数字排序，然后再与第二个数组比较，如果该位置的颜色不同，则输出“N”
```
// #include<bits/stdc++.h>
#include<iostream>
#include<cstring>
#include<algorithm>
#include<map>
#include<vector>
#define ll '\n'
using namespace std;
// #define int long long
#define IOS ios::sync_with_stdio(false);cin.tie(NULL);cout.tie(NULL)
#define fr(i,a,b) for(int i=a;i<=b;i++)
#define fr1(i,a,b) for(int i=a;i>=b;i--)
typedef pair<int,int>PII;
const int INF=0x3f3f3f3f;
const int N=100010;
// priority_queue<int,vector<int>,less<int>>q;//大根堆
// map<int,int>mp;
// vector<vector<int>>v(10,vector<int>(10,0)); //二维数组
void zyt()
{
    int n,m;
    cin >> n >> m;
    vector<PII> v1(n),v2(n);
    fr(i,0,n-1)
    {
        cin >> v1[i].first >> v1[i].second;
        v2[i]=v1[i];
    }
    sort(v1.begin(),v1.end());
    fr(i,0,n-1)
    {
        if(v1[i].second!=v2[i].second)
        {
            puts("N");
            return ;
        }
    }
    puts("Y");
}
signed main()
{
    IOS;
    int t=1;
    // cin >> t;
    while(t--)
        zyt();
    return 0;
}
```

## F题
- 题意：给t 睡觉时间 d总的时间 n餐食的次数 给出每次餐食的时间，问能否既全部把餐食吃完，还要睡觉。
- 思路：给餐食时间排个序，看看有没有差值能满足睡觉。排序的末尾要加入总的时间。
```
// #include<bits/stdc++.h>
#include<iostream>
#include<cstring>
#include<algorithm>
#include<map>
#include<vector>
#define ll '\n'
using namespace std;
// #define int long long
#define IOS ios::sync_with_stdio(false);cin.tie(NULL);cout.tie(NULL)
#define fr(i,a,b) for(int i=a;i<=b;i++)
#define fr1(i,a,b) for(int i=a;i>=b;i--)
typedef pair<int,int>PII;
const int INF=0x3f3f3f3f;
const int N=100010;
// priority_queue<int,vector<int>,less<int>>q;//大根堆
// map<int,int>mp;
// vector<vector<int>>v(10,vector<int>(10,0)); //二维数组
void zyt()
{
    int t,d,n;
    cin >> t >> d >> n;
    if(d<t)
    {
        puts("N");
        return ;
    }
    int v[n+2];
    fr(i,1,n)
        cin >> v[i];
    sort(v+1,v+n+1);
    v[n+1]=d;
    int s=0;
    fr(i,1,n+1)
    {
        if(v[i]-s>=t)
        {
            puts("Y");
            return ;
        }
        s=v[i];
    }
    puts("N");
}
signed main()
{
    IOS;
    int t=1;
    // cin >> t;
    while(t--)
        zyt();
    return 0;
}
```

## G题
- 题意：继承王位，当t=1,则说明多了一个儿子，t=2则说明该值得人没了。输出每次有人去世后，王位的继承人是谁。
- 思路：将自己的儿子放到自己的队列中。通过dfs，将会按顺序将王位继承人放到一个队列中。当有人去世后，判断当前王位继承人是否去世，如果去世了，则从队列中弹，一直找到还没有去世的王位继承人。
```
// #include<bits/stdc++.h>
#include<iostream>
#include<cstring>
#include<algorithm>
#include<map>
#include<vector>
#include<queue>
#define ll '\n'
using namespace std;
// #define int long long
#define IOS ios::sync_with_stdio(false);cin.tie(NULL);cout.tie(NULL)
#define fr(i,a,b) for(int i=a;i<=b;i++)
#define fr1(i,a,b) for(int i=a;i>=b;i--)
typedef pair<int,int>PII;
const int INF=0x3f3f3f3f;
const int N=1e5+10;
// priority_queue<int,vector<int>,less<int>>q;//大根堆
// map<int,int>mp;
// vector<vector<int>>v(10,vector<int>(10,0)); //二维数组
vector<int>v1[N],v2;
queue<int>q;
void dfs(int u)
{
    for(auto &v:v1[u])    //将王位继承人按顺序放入队列中
    {
        q.push(v);
        dfs(v);
    }
}
int vis[N];
void zyt()
{
    int n;
    cin >> n;
    int cnt=1;
    fr(i,1,n)
    {
        int a,b;
        cin >> a >> b;
        if(a==1)
            v1[b].push_back(++cnt);  
        else
            v2.push_back(b);
    }
    dfs(1);
    int now=1;
    for(auto &i:v2)
    {
        vis[i]=1;
        if(vis[now]==0)
        {
            cout << now << ll;
            continue;
        }
        while(vis[q.front()]&&q.size())
            q.pop();
        now=q.front();
        cout << now << ll;
    }
}
signed main()
{
    IOS;
    int t=1;
    // cin >> t;
    while(t--)
        zyt();
    return 0;
}
```
